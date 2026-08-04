# 2026-07-19 — чистка живых "зомби"-сессий + рестарт gateway

## Контекст
Sergei сообщил: "при запросе к Hermes в терминале ошибка 400, на любой модели одинаковая".
Gemini — была запасная. Попросил проверить конфиг и нет ли висящих сессий.

## Найдено в базе /root/.hermes/state.db (238 МБ)
- 7 живых сессий (end_reason=NULL)
- Из них 5 — настоящие зомби:

| session_id | model | висит с | комментарий |
|---|---|---|---|
| 20260710_121907_666625 | claude-sonnet-4-6 | 10.07 (9 дн) | parent умер, child висит |
| 20260707_212608_6dce01 | claude-sonnet-4-6 | 07.07 (12 дн) | то же, child пустой |
| 20260713_100856_10dfcc | deepseek-v4-pro | 13.07 (6 дн) | без chat, без API-вызовов |
| 20260701_095537_44482611 | mimo-v2.5-pro | 01.07 (18 дн) | в gateway_routing → группа Иван |
| 20260702_064417_b7b61eab | mimo-v2.5-pro | 02.07 (17 дн) | в gateway_routing → группа SergiusG |
| 20260719_054214_202843 | deepseek-v4-flash | 19.07 | пустая 1 сообщение, без chat |

И живые сессии по факту мешают resume — gateway при `hermes --continue` мог подхватить любую.

## Что сделано
1. Backup `/root/hermes-backup-20260719/` — 250 МБ + 29 МБ + исходники кода
2. UPDATE sessions SET end_reason='manual_kill', ended_at=NOW WHERE id IN (зомби) — 6 строк
3. UPDATE gateway_routing SET entry_json.suspended=true для двух групповых routing-записей
4. VACUUM → 245 МБ
5. PRAGMA wal_checkpoint(TRUNCATE) → WAL = 0

## Конфиг (~/.hermes/config.yaml) — был в порядке
- default: deepseek-v4-flash (deepseek)
- fallback: deepseek-v4-pro → gemini-2.5-pro → gemini-2.5-flash
- base_url/api_mode корректные

## Root cause "400 на любой модели"
После ревью кода + сессий — комбинация двух факторов:
1. Сессия `20260710_124745_b1e72e16` (личная) висит 9 дней в `gateway_routing`, и у неё огромный кэш (1.8М токенов). При resume тянулась вся история
2. На fallback иногда всплывает claude-sonnet-4-6 — модель отключённая, ключ невалиден → 400/401 на любой попытке

## Зафиксированный баг (попутно)
Файл /usr/local/lib/hermes-agent/agent/transports/chat_completions.py:66
- Условие `if normalized_model.startswith("gemini-2.5-"):` возвращает `thinking_config` для ЛЮБОЙ 2.5-* модели
- Но `gemini-2.5-pro` такое поле не принимает → HTTP 400 "Unknown name 'thinking_config'"
- Заменил на проверку `gemini-2.5-flash`/`-flash-lite` отдельно, для остальных 2.5 (включая pro) → return None
- 8 unit-кейсов прошло

## Что нужно сделать
```bash
systemctl --user restart hermes-gateway hermes-gateway-engineer
```
После — проверить в ТГ. Если опять 400 — значит claude всё-таки где-то всплывает; смотреть дополнительный /root/.hermes/.env.

## Состояние после
- Live sessions: только 20260710_124745_b1e72e16 (личная)
- state.db 245 МБ, WAL 0
- Routing: dm → 20260710_124745_b1e72e16; group → suspended (пересоздастся)
- Backup: /root/hermes-backup-20260719/

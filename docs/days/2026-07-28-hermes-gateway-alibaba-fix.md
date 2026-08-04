---
title: Hermes gateway debug — 2026-07-28
date: 2026-07-28
topic: hermes-config
tags: [hermes, gateway, alibaba, debug, env-files]
status: resolved-and-improved
---

# Hermes gateway debug — 28 июля 2026

## Проблема

Сергей жаловался что **Hermes отвечает с задержкой 30-60 секунд**, а иногда вообще "спит" — присылает ответ только после второго сообщения.

## Что нашёл

### 1. Два gateway процесса (НОРМАЛЬНО, не баг)

Из Wiki `hermes-config.md` (7 июля): **три профиля работают параллельно через systemd**:
- `hermes-gateway.service` (default, PID 461848, 17:58) — этот чат @SergiusG
- `hermes-gateway-engineer.service` (engineer, PID 351083, 23 июля) — @EngVrnBot
- `hermes-accountant-handler.service` (accountant) — бухгалтер-бот

Это **нормальная архитектура**. Не трогать engineer-gateway.

### 2. Лимит GLM-5.2 исчерпан

Логи gateway показали:
```
provider=zai, model=glm-5.2: HTTP 429 Weekly/Monthly Limit Exhausted
Your limit will reset at 2026-07-30 13:19:38
```

GLM-5.2 через z.ai платный провайдер — месячная квота кончилась. Сбросится 30 июля.

### 3. Fallback на Gemini тоже сломан

```
provider=google, model=gemini-2.5-flash: HTTP 400
"message": "Invalid JSON payload received. Unknown name \"thinking_config\": Cannot find field."
```

Hermes шлёт `thinking_config` (формат Anthropic) в Google API — не поддерживается.

### 4. КЛЮЧЕВАЯ ПРОБЛЕМА: ALIBABA ключ не в .env

`~/.hermes/credentials.env` имеет `ALIBABA_API_KEY=sk-sp-H.XEEY.ki0t...` (113 chars).
`~/.hermes/.env` — **НЕ имел** этого ключа.

Hermes-gateway читает `~/.hermes/.env`. Поэтому провайдер `alibaba-token` получал HTTP 401 (No API-key provided).

## Решение (сделано)

**Скопировал в `~/.hermes/.env` отсутствующие ключи из `credentials.env`:**
- ALIBABA_API_KEY (113 chars)
- KLING_API_KEY
- ELEVENLABS_VOICE_RU, ELEVENLABS_VOICE_EN
- GH_PAT
- YOUTUBE_API_KEY, YOUTUBE_OAUTH
- TELEGRAM_ALLOWED_USERS

`credentials.env` не тронут (он остался как было).

## Что нужно сделать вручную

⚠️ **Hermes-gateway нельзя перезапустить изнутри сессии** (SIGTERM kill всю команду). Нужен отдельный shell.

**Команды для Сергея в новом shell:**
```bash
systemctl --user restart hermes-gateway
```

После этого проверить что ключ подхватился:
```bash
journalctl --user -u hermes-gateway --since "30 seconds ago" --no-pager | grep -iE "alibaba|provider="
```

Если видишь `provider=alibaba-token` без 401 ошибок — починено.

## Что доступно через Alibaba (после починки)

```
qwen3.8-max-preview       — LLM
deepseek-v4-pro           — LLM (fallback)
deepseek-v4-flash         — LLM (быстрый)
glm-5.2                   — LLM (для субагентов через MoA)
qwen3.7-plus             — LLM
qwen-audio-3.0-tts-plus   — TTS! (полезно для видео)
wan2.7-image, wan2.7-image-pro — генерация изображений
happyhorse-1.1-i2v, -t2v, -r2v — генерация видео
```

## Текущая конфигурация моделей

```yaml
model:
  default: glm-5.2          # через alibaba-token (после рестарта заработает)
  fallback:
    - deepseek-v4-flash    # alibaba-token
    - glm-5.2              # alibaba-token
    - MiniMax-M3           # minimax ← сейчас работает здесь (fallback через цепочку)
    - gemini-2.5-flash     # google (сломан thinking_config)
```

После рестарта gateway должно сразу попадать в `glm-5.2` через `alibaba-token` (без 401).

## Обновление 28 июля 19:15

Alibaba подхватился (алиас GLM_API_KEY=ALIBABA_API_KEY), но квота token-plan 1-week исчерпана до 30.07 09:23 UTC. Переключились на DeepSeek primary. См. `2026-07-28-gateway-swap-deepseek.md`.

## TODO

- [ ] Сергей перезапускает gateway вручную (`systemctl --user restart hermes-gateway`)
- [ ] Проверить логи — нет ли 401 на alibaba
- [ ] Если есть 401 — ключ мог истечь, нужно обновить через Alibaba Cloud console
- [ ] После починки — тест: отправить сообщение и засечь время ответа (должно быть <3 сек)
- [ ] Возможно стоит починить Gemini fallback (убрать `thinking_config` для Google API)

## Связанные документы

- `/root/fravart-ru/docs/topics/hermes-config.md` — конфиг моделей
- `/root/fravart-ru/docs/days/2026-07-07-hermes-recovery.md` — прошлый recovery gateway
- `/root/.hermes/credentials.env` — основное хранилище ключей
- `/root/.hermes/.env` — копия для Hermes-gateway (обновлено)

---

*Документ создан 2026-07-28 перед ручным рестартом gateway.*

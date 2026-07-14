---
date: 2026-07-13
topic: hermes-config
title: "Hermes: тюнинг GLM-5.2"
tags:
  - hermes
  - config
  - glm
  - тюнинг
  - token-plan
session_ids: []
artifacts:
  - name: "/root/.hermes/profiles/engineer/.env"
    path: "/root/.hermes/profiles/engineer/.env"
    type: local
  - name: "/root/.hermes/config.yaml"
    path: "/root/.hermes/config.yaml"
    type: local
---

# Что делали

Тюнинг конфигов Hermes после покупки MiniMax Token Plan и финального перехода на GLM-5.2.

## Что изменилось

### MiniMax Token Plan

- **Было:** $80 за 4 дня — неустойчивая модель расходов
- **Стало:** Token Plan $50/мес
- **Включено:** ~5.1B токенов M3, TTS, изображения, музыка, 3 Hailuo-видео в день
- **Получили новый API-ключ** `sk-cp-...` (Token Plan использует другой префикс)

### GLM-5.2 как основа делегации

| Компонент | Модель | Провайдер |
|---|---|---|
| Delegation (субагенты) | glm-5.2 | zai |
| MoA aggregator | glm-5.2 | zai |
| Main модель | MiniMax-M3 | minimax |
| Fallback | DeepSeek → Gemini Pro → Flash | — |

GLM-5.2 — июнь 2026, топ-1 на Vending Bench 2 среди open-source. Совместим с Claude Code CLI.

### Engineer-профиль

- В профиле инженера был **старый** MiniMax ключ `sk-api-...` (пустой баланс)
- Заменили на Token Plan `sk-cp-...`
- Модель осталась `MiniMax-M3` (совместима)
- Инженер-бот получил команду `/restart` для перечитывания `.env`

## Сравнение моделей делегации

| Модель | Плюсы | Минусы |
|---|---|---|
| Claude Sonnet (было) | отличное качество | $0.50/запуск, нет бюджета |
| GLM 2.7 | дёшево | зацикливается на tool-calls |
| GLM 4.7 | стабильнее | среднее качество |
| **GLM 5.2** (стало) | топ-1 на бенчмарках | в подписке z.ai |

## Что ещё сделали

- Обновили `/root/.hermes/.env` — новый MiniMax ключ
- Убрали упоминания Claude из конфигов (но оставили в skill-доках как reference)
- Claude Code через GLM-прокси продолжает работать для инженера
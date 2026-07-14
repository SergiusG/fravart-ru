---
title: Hermes — конфиг
slug: hermes-config
period: 2026-07-12 — настоящее время
tags:
  - hermes
  - config
  - glm
  - claude
  - minimax
---

# Hermes — конфиг

## Описание

Настройка AI-агента Hermes: модели, провайдеры, прокси, fallback-цепочки.

## Текущий стек моделей

| Роль | Модель | Провайдер | Бюджет |
|---|---|---|---|
| Основная | MiniMax-M3 | minimax | Token Plan $50/мес (5.1B токенов) |
| Делегация субагентов | glm-5.2 | zai | Месячная подписка |
| MoA aggregator | glm-5.2 | zai | Месячная подписка |
| Fallback | DeepSeek → Gemini Pro → Flash | — | Прямой API |

**Claude полностью выключен** — нет бюджета. Удалён из всех 4 конфигов (main + 3 профиля).

## Записи по дням

- [2026-07-12 — миграция Claude → GLM](days/2026-07-12-hermes-glm-migration.md)
- [2026-07-13 — тюнинг GLM-5.2](days/2026-07-13-hermes-glm-tuning.md)

## Профили

- `default` — личка (@SergiusG)
- `accountant` — бухгалтер-бот (расходы по объектам)
- `engineer` — инженер-бот (Claude Code через GLM-прокси)
- `videobooks` — отдельный для видеопродакшна

## Модели для видеопродакшна

| API | Что используется | Бюджет |
|---|---|---|
| MiniMax TTS | SergiusVoice01 (RU), SergG En2 (EN) | Token Plan |
| MiniMax Hailuo | видео-клипы (3/день) | Token Plan |
| MiniMax image | обложки | Token Plan |
| ElevenLabs SergG En2 | EN-голос | прямой API, ~$5-10/мес |

## Pitfall-заметки

- `hermes config set` не блокирует изменение чувствительных полей, но требует явного `cross_profile: true` для редактирования другого профиля
- В профиле `engineer` раньше был старый MiniMax API-ключ (`sk-api-...`) — заменили на Token Plan (`sk-cp-...`)
- GLM-5.2 работает стабильнее чем 4.7 (меньше зацикливается на tool-calls)
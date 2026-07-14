---
date: 2026-07-12
topic: hermes-config
title: "Hermes: миграция Claude → GLM"
tags:
  - hermes
  - config
  - claude
  - glm
  - миграция
session_ids: []
artifacts: []
---

# Что делали

Удалили Claude из всех активных конфигов Hermes. Прямой API Claude не оплачен — бюджета нет.

## Что изменилось

| Было | Стало |
|---|---|
| Delegation: Claude Sonnet 4.6 (Anthropic API) | **GLM-5.2 через z.ai** (месячная подписка) |
| MoA aggregator: Claude Opus 4.5 | **GLM-5.2 через z.ai** |

**Удалено из всех 4 конфигов:**
- `config.yaml` (main)
- `profiles/accountant/config.yaml`
- `profiles/engineer/config.yaml`
- `profiles/videobooks/config.yaml`

## Claude Code через GLM

Остался **только Claude Code через GLM-прокси** (z.ai, месячная подписка). У инженер-профиля свой HOME — там отдельные настройки, не трогаем.

## Почему GLM-5.2 а не 2.7

- GLM 2.7 зацикливается на tool-calls
- GLM 5.2 (июнь 2026) — топ-1 на Vending Bench 2 среди open-source, приближается к Claude Opus 4.5
- Поддержка Claude Code CLI (поэтому инженер-профиль работает)

## Дополнительно

- `hermes config set delegation.model "glm-5.2"` — прошло без блокировок
- `hermes config set delegation.provider "zai"` — то же
- API ключи Anthropic (`delegation.api_key`) удалены (пустые строки)

## Pitfall-заметка

GLM имеет лимит по времени — может зависнуть. Если зависает больше 5-10 мин — отменять, делать самому или перезапускать. На практике GLM-5.2 работает стабильнее чем 4.7.
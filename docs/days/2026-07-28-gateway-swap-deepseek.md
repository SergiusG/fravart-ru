---
title: Gateway swap to DeepSeek primary — 2026-07-28
date: 2026-07-28
topic: hermes-config
tags: [hermes, gateway, alibaba, deepseek, quota, cron]
status: resolved
---

# Gateway swap — 28 июля 2026 (вечер)

## Проблема
Alibaba token-plan (zai провайдер) исчерпал недельную квоту. Сброс 30.07 09:23 UTC (= 12:23 МСК).

## Что поменял

**primary** провайдер → `deepseek` (модель `deepseek-v4-flash`):
- `model.provider: deepseek`
- `model.default: deepseek-v4-flash`
- `model.base_url: https://api.deepseek.com/v1`

**fallback chain** (новый порядок):
1. `MiniMax-M3` (minimax) — самый надёжный fallback
2. `deepseek-v4-pro` (deepseek) — резерв внутри провайдера
3. `gemini-2.5-flash` (google) — сломан (thinking_config) — Hermes починит
4. `glm-5.2` (zai) — 429 quota до 30.07

**DEEPSEEK_BASE_URL** добавлен в `.env`.

## Cron авто-возврат

Job ID: `52ca184a66a9`
Когда: **30 июля 09:30 UTC** (= 12:30 МСК)
Что: запускает `/root/.hermes/scripts/restore_alibaba_primary.sh` (без systemctl restart — Hermes hot-reload).

Скрипт восстанавливает:
- `model.provider: zai`
- `model.default: glm-5.2`
- `model.base_url: https://token-plan...`
- старый fallback chain

## Ключевая находка

`zai` провайдер в Hermes ищет ключ в `GLM_API_KEY` / `ZAI_API_KEY` / `Z_AI_API_KEY`.
А НЕ в `ALIBABA_API_KEY` (как было настроено).
Alias `GLM_API_KEY=ALIBABA_API_KEY` теперь в `.env`.

## TODO для пользователя
- [ ] Ручной `systemctl --user restart hermes-gateway` (hot-reload может не подхватить провайдера мгновенно)
- [ ] 30.07 после 12:30 МСК проверить что gateway вернулся на zai

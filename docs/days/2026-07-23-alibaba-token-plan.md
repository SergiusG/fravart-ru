---
date: 2026-07-23
topic: ai-models-experiments
title: "Диск чистка + Alibaba Token Plan + HappyHorse тест"
tags:
  - ai-models
  - alibaba
  - kling
  - server
session_ids:
  - "20260723_054314_4ca281b8"
  - "20260723_092519_680b586b"
artifacts: []
---

# Alibaba Token Plan + чистка диска + HappyHorse

## Что делали

Сервер забился (2.2 ГБ свободно из 38). Почистили ~7 ГБ кэша и старых файлов. Настроили Alibaba Cloud Token Plan ($18/мес) как потенциальную замену раздельных подписок. Протестировали HappyHorse 1.1 для генерации видео.

## Результаты

### Чистка диска (2.2 → 8.7 ГБ свободно)

Удалено: pip cache (2.7G), playwright browsers (1.3G), buried-blueprints старьё (930M), ambient project1 (589M), kimi-code (311M), hermes-backup (275M), npm cache (243M), huggingface cache (142M), uv/electron/node-gyp (~350M).

### Alibaba Cloud Token Plan

| Параметр | Значение |
|---|---|
| План | Standard $18/мес |
| Эндпоинт | `token-plan.ap-southeast-1.maas.aliyuncs.com/compatible-mode/v1` |
| Модели | GLM-5.2, DeepSeek-V4-Pro, Qwen3.8-Max-Preview |
| Видео | HappyHorse 1.1 (I2V, T2V, R2V) |
| Картинки | Wan2.7-Image |
| Провайдер | `alibaba-token` |

- Было: ~$35/мес (DeepSeek + GLM + Kling отдельно)
- Стало: $18/мес всё включено

### HappyHorse 1.1 — НЕ пригоден

Жёсткий rate limit (HTTP 429) при batch генерации. Kling MCP остаётся основным инструментом для видео.

## Решения

- Alibaba Token Plan — для текстовых моделей (дешевле раздельных). HappyHorse — не использовать для batch.
- Kling MCP — стабильный инструмент, продолжаем использовать
- I2V с музейных фото = статика; для динамики использовать T2V

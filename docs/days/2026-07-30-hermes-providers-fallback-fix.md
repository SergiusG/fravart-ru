---
tags: [hermes, config, providers, fallback, alibaba, deepseek, gemini]
---

# Настройка провайдеров и fallback (30.07.2026)

## Проблема
Fallback-цепочка в Hermes **не работала** — оба ключа `fallback_providers` в `config.yaml` были записаны как YAML-строки вместо YAML-списков. `hermes fallback list` показывал «No fallback providers configured».

Дополнительно: `custom_providers` был в формате YAML-словаря (dict), а Hermes ожидает список (list).

## Что сделано

### 1. Починен fallback
- Удалены битые `fallback_providers` (под `model:` и на корневом уровне)
- Добавлен корректный YAML-список

### 2. Настроены модели и провайдеры

**Default (основной):**
- Primary: `qwen3.8-max-preview` → alibaba-token (monthly, 5h quota)
- Fallback 1: `deepseek-v4-flash` → deepseek (pay-per-token)
- Fallback 2: `gemini-2.5-flash` → google

**Engineer (бот Инженер — сметы, таблицы, отчёты):**
- Primary: `deepseek-v4-pro` → alibaba-token
- Fallback 1: `deepseek-v4-flash` → deepseek
- Fallback 2: `gemini-2.5-flash` → google

### 3. Исправлен custom_providers
- Конвертирован из dict в list (YAML-формат)
- Провайдеры: `alibaba-token` и `deepseek`

### 4. Модели Alibaba token-plan
- `qwen3.8-max-preview` — топ (primary для default)
- `qwen3.7-max`, `qwen3.7-plus`, `qwen3.6-flash`
- `glm-5.2`, `deepseek-v4-pro`
- `wan2.7-image`, `wan2.7-image-pro`
- `qwen-audio-3.0-tts-plus`

### 5. Gateway
- Обновлён Hermes (`hermes update`)
- Gateway перезапущен — работает без ошибок

---
title: "AI-модели и сервисы — эксперименты"
slug: ai-models-experiments
period: 2026-07-04 — present
tags: [ai, models, kling, minimax, suno, alibaba, deepseek, qwen, glm, pricing]
---

# AI-модели и сервисы — эксперименты

## Описание

Сравнение и тестирование AI-моделей для задач видеопродакшна, озвучки, музыки и текста. Цены, качество, выводы. Что выбрали и почему.

## Модели для текста (LLM)

### Текущий пул (июль 2026)

| Модель | Провайдер | Цена Input/1M | Цена Output/1M | Контекст | Назначение |
|---|---|---|---|---|---|
| **GLM-5.2** | ZAI | недорого | недорого | 128K | Основная (Hermes) |
| **DeepSeek V4 Flash** | DeepSeek | $0.20 | $0.50 | 128K | Быстрые задачи |
| **DeepSeek V4 Pro** | DeepSeek | выше | выше | 128K | Сложные задачи |
| **MiniMax-M3** | MiniMax | $1.50 | $4.50 | 1M | Длинный контекст |
| **Qwen3.8-Max** | Alibaba | $1.25 | $3.75 | 1M | Флагман Alibaba |

### Alibaba Cloud Token Plan

**Стандарт $18/мес — всё включено:** GLM-5.2, DeepSeek-V4-Pro, Qwen3.8-Max-Preview.
Эндпоинт: `https://token-plan.ap-southeast-1.maas.aliysuncs.com/compatible-mode/v1`
Провайдер в Hermes: `alibaba-token`

Qwen3.5 Flash — **в 3 раза дешевле** DS V4 Flash ($0.07 vs $0.20 input) с 1M контекстом, но для наших задач разницы в качестве нет.

## Модели для видео

| Модель | Что умеет | Цена | Качество | Статус |
|---|---|---|---|---|
| **Kling v2_6** (MCP) | t2v, i2v, 10с, 1080p | 50 кредитов/клип | 7/10 CGI | ✅ Основная |
| **MiniMax/Hailuo 2.3** | t2v, i2v, 5с, 720p | ~$0.05/клип | 8.5/10 фотореализм | ✅ Доступна |
| **HappyHorse 1.1** (Alibaba) | i2v, t2v, r2v | включено в $18 | топ-1 рейтинги | ❌ Rate limit 429 |
| Google Veo 2 | t2v, 1080p, 8с | ~$0.35/сек | высокое | через Gemini |
| Runway Gen-3 | t2v, i2v | нужна подписка | высокое | нет доступа |

**Вывод:** Kling MCP — стабильный рабочий инструмент. Hailuo — лучше для фотореализма, но короче клип. HappyHorse не пригоден для batch из-за 429.

## Модели для озвучки (TTS)

| Модель | Что умеет | Цена | Качество | Статус |
|---|---|---|---|---|
| **MiniMax TTS** | клонирование, RU+EN | кредиты | хорошее | ✅ SergiusVoice01 |
| **ElevenLabs** | клонирование, RU+EN | подписка | хорошее | ✅ 2 голоса |
| **Edge BrianNeural** | EN, нет клонирования | бесплатно | среднее | ✅ для EN |
| **speech-2.6-turbo** (MiniMax) | 40 языков, 7 эмоций | кредиты | хорошее | ✅ |

## Модели для музыки

| Модель | Что умеет | Цена | Качество | Статус |
|---|---|---|---|---|
| **Suno Pro** | генерация музыки, v5.5 | $10/мес | **лучшее на рынке** | ✅ ручная ген. |
| **MiniMax music-2.6** | генерация музыки | кредиты | среднее | ✅ fallback |
| Suno (через API) | автоматизация | $0.01-0.04/песня | хорошее | планируем |

**Suno Pro** — значительно лучше MiniMax для ambient. Нет официального API — только ручная генерация на suno.com. Hermes пишет промпты, Сергей генерирует вручную.

## Kling MCP — технические детали

- **Client ID:** `mcp-hermes-agent-99TXJbKeXJsN0VLgidRkMK`
- **Токены:** `/root/ambient/kling_tokens.json`
- **Refresh token grant** работает
- **Токен истекает за 1ч** — refresh перед каждым клипом
- **50 кредитов/клип** (v2_6, 10с, 1080p)
- **generationId** — string, НЕ ids

## Записи по дням

- [2026-07-04: Инструменты для «Книги» (чёрные дыры)](days/2026-07-04-daily-auto.md)
- [2026-07-05: Kling v7 — прогресс генерации](days/2026-07-05-kling-v7-progress.md)
- [2026-07-09: MiniMax API — все модели, тест Hailuo+music](days/2026-07-09-daily-auto.md)
- [2026-07-10: Videobooks — MiniMax EN revoice + YouTube upload](days/2026-07-10-videobooks-youtube-upload.md)
- [2026-07-17: Suno Pro —pricing, API исследование](days/2026-07-17-daily-auto.md)
- [2026-07-20: Инструментарий ambient + videobook — всё есть](days/2026-07-20-daily-auto.md)
- [2026-07-21: Suno промпт для Buried Blueprints](days/2026-07-21-daily-auto.md)
- [2026-07-23: Alibaba Token Plan + HappyHorse тест](days/2026-07-23-daily-auto.md)

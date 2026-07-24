---
date: 2026-07-20
topic: ai-models-experiments
title: "Инструментарий: ambient + videobook — всё есть. Qwen comparison"
tags:
  - ai-models
  - ambient
  - videobooks
  - qwen
session_ids:
  - "20260720_162744_8630e6b9"
artifacts: []
---

# Инструментарий: ambient + videobook — всё есть

## Что делали

Полный аудит инструментария для проектов Misty Horizons (ambient) и Videobooks. Параллельно — исследование Qwen как альтернативы текущему пулу моделей.

## Результаты

### Ambient (Misty Horizons)
Полный стек: Imagen 4.0 (картинки), Kling i2v (анимация), ffmpeg (сборка), Soundraw + MiniMax music (фоновая музыка). **Всё есть, докупать не нужно.** Опционально — звуки природы с FreeSound/Pixabay (бесплатно).

### Videobook
Полный стек: ElevenLabs + MiniMax TTS (озвучка), Replicate Flux (иллюстрации), pymupdf4llm (текст), ffmpeg (сборка), Suno (музыка). **Всё закрыто.** ComfyUI не нужен — нет GPU.

### Qwen (Alibaba)
| Модель | Input/1M | Output/1M | Контекст |
|---|---|---|---|
| Qwen3.5 Flash | **$0.07** | $0.26 | 1M |
| Qwen3.7 Max | $1.25 | $3.75 | 1M |

Qwen3.5 Flash в 3 раза дешевле DS V4 Flash, но для наших задач (сметы, документация, чат) — разницы в качестве нет. Отложено.

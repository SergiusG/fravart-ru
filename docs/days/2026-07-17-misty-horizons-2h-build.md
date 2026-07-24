---
date: 2026-07-17
topic: misty-horizons
title: "Misty Horizons: сборка 2h ambient + Suno Pro"
tags:
  - video
  - ambient
  - kling
  - suno
session_ids:
  - "20260717_003530_09d572d7"
  - "20260717_081511_df262d6e"
artifacts:
  - name: "ambient_misty_lake_2h.mp4"
    type: drive
    url: "Drive Ambient/1/"
---

# Misty Horizons: сборка 2h ambient + Suno Pro

## Что делали

Собрали первый полноценный 2-часовой ambient-ролик для YouTube-канала Misty Horizons. Начали с генерации Kling-анимации туманного озера, закончили сборкой через ffmpeg.

## Результаты

- Kling-клип «misty lake» сгенерирован (i2v, 10с, 1080p) — туман движется, вода анимирована
- 2 версии 2-часового видео: `ambient_stream_kling_2h.mp4` (5.97 ГБ) и `ambient_misty_lake_2h.mp4` (2.06 ГБ)
- 5 ambient-треков через MiniMax music-2.6 (deep_drone, piano_nature, cinematic_warm, mystic_forest, music_part1)
- Рендер 2ч на 4 ядрах: 40-90 мин

## Решения

- **Suno Pro $10/мес** — оформлен. Качество ambient значительно выше MiniMax. Нет официального API — ручная генерация на suno.com
- Сторонние API (Goapiai $0.01/песня) — на будущее для автоматизации
- ffmpeg pipeline: stream_loop 720× (видео) + stream_loop 400× (аудио), CRF 23

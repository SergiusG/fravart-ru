---
title: "Видеопродакшн — пайплайн"
slug: videobooks
period: 2026-07-05 — present
tags: [videobooks, kling, minimax, youtube, video-production, tts]
---

# Видеопродакшн — пайплайн

## Описание

End-to-end пайплайн производства научпоп-видео для YouTube-канала «Архитектор Сознания»: от сценария до загрузки на YouTube. Включает генерацию голоса (MiniMax TTS), AI-клипов (Kling, MiniMax/Hailuo), субтитров, сборку через ffmpeg и публикацию.

## Компоненты

- **MiniMax TTS** — клонированный голос `SergiusVoice01` (RU), `SergG En2` (EN)
- **Kling AI** — генерация видеоклипов (v7)
- **MiniMax Video API** — генерация клипов напрямую
- **ffmpeg** — сборка, цикличное растяжение клипов под озвучку, hard-burn субтитров
- **YouTube API** — загрузка и публикация

## Записи по дням

- [2026-07-05: Kling v7 — прогресс генерации](days/2026-07-05-kling-v7-progress.md)
- [2026-07-10: MiniMax EN revoice + YouTube загрузка](days/2026-07-10-videobooks-youtube-upload.md)

---
title: "Misty Horizons — Ambient YouTube"
slug: misty-horizons
period: 2026-07-16 — present
tags: [video, youtube, ambient, kling, suno, minimax, lo-fi]
---

# Misty Horizons — Ambient YouTube

## Описание

EN YouTube-канал с ambient-видео (2-часовые лупы): туман, вода, природа. Музыка + анимированная сцена. Монетизация через YouTube Partner Program.

## Технологический стек

| Компонент | Инструмент | Статус |
|---|---|---|
| 🎬 Анимация | Kling AI (i2v, t2v) | ✅ 50 кредитов/сцена |
| 🎵 Музыка | Suno Pro ($10/мес) + MiniMax music-2.6 | ✅ |
| 🔊 Звуки природы | FreeSound/Pixabay (бесплатно) | планируем |
| 🎞 Сборка | ffmpeg (2h loop, crossfade, 4K upscale) | ✅ |
| 📺 YouTube | Brand Account UCklNjUu1BHrPIv0DDxBBREA | ✅ |

## Drive-структура

- `/Ambient/` — корневая папка (`1ZniANfr4L0kvxiKgZgGSz7AcTv9Axvmr`)
- `/Ambient/1/` — первый пилот (`1A1KQLvcbABJlKXFtZMu4a0DMVUAbOEod`)
- `/Ambient/2/` (`18H9GoTU29...`)
- `/Ambient/3/` (`1bWWAeqsn7...`)

## Pipeline

Пайплайн подробно описан в skill `ambient-loop-video`.

**Схема:**
1. Сгенерировать картинку (Kling/MiniMax) — атмосферная сцена
2. Анимировать через Kling (туман, вода, листва)
3. Сгенерировать ambient-музыку (Suno/MiniMax)
4. ffmpeg: зациклить анимацию 10с → 2 часа, наложить музыку
5. Залить на YouTube

## Что сделано

- Пилот: `ambient_misty_lake_2h.mp4` (2.06 ГБ) — туманное озеро, Kling анимация, зацикленная музыка
- Музыка: 5 треков через MiniMax music-2.6
- Версии: Ken Burns пилот, stream_kling_2h, misty_lake_2h
- Рендер 2-часового видео: ~40-90 мин на 4 ядрах, CRF 23, итог ~4.5 ГБ

## Уроки и решения

- **YouTube "Inauthentic Content" Policy** — простые лупы отклоняются. Каждое видео должно быть уникальным (AI-генерация + оригинальная музыка). Публиковать 1-2 в неделю, не пачками.
- **Suno Pro** — значительно лучше MiniMax для ambient. Нет официального API — только ручная генерация на suno.com. Сторонние API (Goapiai $0.01/песня) — для автоматизации.
- **Kling i2v** — анимация воды/тумана работает хорошо; для каждого нового видео нужна свежая генерация

## Записи по дням

- [2026-07-16: Концепт ambient-видео + пилот](days/2026-07-16-session-summary.md)
- [2026-07-17: Сборка 2h видео + Suno Pro](days/2026-07-17-daily-auto.md)
- [2026-07-20: Инструментарий ambient + videobook — всё есть](days/2026-07-20-daily-auto.md)

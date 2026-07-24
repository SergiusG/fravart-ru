---
title: "Buried Blueprints"
slug: buried-blueprints
period: 2026-07-21 — present
tags: [video, youtube, documentary, kling, tts, suno, bilingua]
---

# Buried Blueprints

## Описание

YouTube-канал в жанре документального видео о забытой инженерии (forgotten engineering). Билингва RU+EN. Первый эпизод — KM Ekranoplan («Каспийский монстр»). Второй — VVA-14 Бартини (в работе).

## Технологический стек

| Компонент | Инструмент | Статус |
|---|---|---|
| 🎬 Видеоклипы | Kling AI (MCP, v2_6, 10с, 1080p) | ✅ 50 кредитов/клип |
| 🎤 EN озвучка | Edge BrianNeural | ✅ бесплатно |
| 🎤 RU озвучка | MiniMax SergiusVoice01 | ✅ |
| 🎵 Музыка | Suno Pro ($10/мес) | ✅ ручная генерация |
| 🎞 Сборка | ffmpeg (montage, crossfade, sub-burn) | ✅ |
| 🖼 Апскейл | Real-ESRGAN | планируем |
| 📺 Публикация | YouTube API | ✅ |

## Эпизод 1: KM Ekranoplan (Каспийский монстр)

**Статус:** завершён. 10 сцен, ~11 мин, билингва RU+EN.

**Drive:** `1KqAjQ_V03_0E9JjO6YJHExq8czExrAhs`

**Что сделано:**
- Сценарий утверждён (англ + рус версия)
- Музыка: основная тема (интро) + фоновая (повествование) через Suno
- 12 Kling-клипов (180 МБ): заставка, КМ взлёт (3 ракурса), бутылка о нос, Алексеев за штурвалом, «Лунь» над морем, ракеты, турбулентность, крушение, обломки, современный концепт
- TTS-озвучка: 10 сцен через Edge BrianNeural (EN)
- Первая сборка: попытка монтажа через ffmpeg — Сергей не принял, переделываем
- Архивные материалы: CIA satellite photos, Wikimedia Commons (KM, Lun MD-160, Алексеев), реставрация 2020

## Эпизод 2: VVA-14 Бартини

**Статус:** в работе.

**Drive:** `1jEEGhAQx4m1I0MFon6zbrfRQBBv19WA9`

**Исходники Drive:** `1101Gp_cqJFV_qdG46vG-_VOQ9mIkKhE7`

## Уроки и решения

- **Kling вотермарк** — убирается через `ffmpeg delogo` (x=W-150:y=H-60:w=140:h=50)
- **i2v с музейных фото** = статика; для динамики использовать t2v
- **HappyHorse 1.1 (Alibaba)** — жёсткий rate limit (429), НЕ пригоден для batch генерации. Kling MCP работает стабильно
- **Kling MCP токены** истекают за 1ч — refresh перед каждым клипом. `generationId` (string), НЕ ids

## Записи по дням

- [2026-07-21: Suno промпт для Buried Blueprints](days/2026-07-21-daily-auto.md)
- [2026-07-22: Сбор материалов для визуала + Kling клипы + вотермарк](days/2026-07-22-daily-auto.md)
- [2026-07-23: Диск чистка + тест Kling + Alibaba Token Plan](days/2026-07-23-daily-auto.md)

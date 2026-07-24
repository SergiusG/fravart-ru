---
date: 2026-07-22
topic: buried-blueprints
title: "Buried Blueprints: сбор визуала + Kling клипы + вотермарк"
tags:
  - video
  - buried-blueprints
  - kling
  - ffmpeg
session_ids:
  - "20260722_041856_227882c0"
  - "20260722_145304_9b35a53a"
artifacts:
  - name: "12 Kling клипов (180 МБ)"
    type: drive
    url: "Drive Buried Blueprints"
---

# Buried Blueprints: сбор визуала + Kling клипы + вотермарк

## Что делали

Собрали архивные материалы (фото КМ, Lun MD-160, Алексеев, CIA satellite photos) с Wikimedia Commons и других открытых источников. Сгенерировали 12 Kling-клипов для эпизода 1. Решили проблему водяного знака Kling.

## Результаты

### 12 Kling-клипов (180 МБ, все на Drive)

| # | Клип | Сцена | MB |
|---|------|-------|----|
| 1 | `00_blueprint_title` | Заставка Buried Blueprints | 20 |
| 2 | `04_km_takeoff_wide` | КМ взлёт (широкий) | 16 |
| 3 | `04_km_takeoff_front` | КМ взлёт (спереди) | 17 |
| 4 | `04_km_takeoff_side` | КМ взлёт (сбоку) | 15 |
| 5 | `05_champagne_ceremony` | Бутылка о нос | 9 |
| 6 | `05_alexeyev_cockpit` | Алексеев за штурвалом | 14 |
| 7 | `06_lun_cruise` | «Лунь» над морем | 16 |
| 8 | `06_missile_closeup` | Ракеты Москит | 15 |
| 9 | `07_turbulence` | Турбулентность | 16 |
| 10 | `07_crash_impact` | Крушение | 17 |
| 11 | `07_wreckage` | Обломки (новый) | 15 |
| 12 | `09_china_ekranoplan` | Современный концепт | 15 |

### Архивные материалы

7 категорий: cia-photos, km-ekranoplan, lun-md160, alexeyev, hydrofoils, restoration-2020, infographics

### Первая сборка

ffmpeg-монтаж всех 12 клипов + аудио + музыка → загружено на Drive. **Сергей не принял** — переделываем. Файл удалён с Drive.

## Решения

- **Kling вотермарк** убирается через `ffmpeg delogo`: `delogo=x=W-150:y=H-60:w=140:h=50`
- Альтернатива: `drawbox` (заливка цветом) или обрезка
- Нейросетевой inpainting (ProPainter, Lama) — слишком медленный для 10с клипов

## Что не зашло (для переделки)

- Визуал: чёрные экраны вместо фото, недостаточно динамики
- Голос: BrianNeural — под вопросом
- Музыка/темп/монтаж — ждать фидбэка

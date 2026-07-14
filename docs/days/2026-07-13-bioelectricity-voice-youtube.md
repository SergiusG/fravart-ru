---
date: 2026-07-13
topic: bioelectricity-levin
title: "Bioelectricity / Levin: RU + EN версии на YouTube"
tags:
  - bio
  - levin
  - научпоп
  - видео
  - youtube
  - ru
  - en
session_ids: []
artifacts:
  - name: "SCENARIO.md"
    path: "/root/projects/videobooks/био/SCENARIO.md"
    type: local
  - name: "SCENARIO_EN.md"
    path: "/root/projects/videobooks/био/SCENARIO_EN.md"
    type: local
  - name: "final_with_subs.mp4 (RU)"
    url: "https://drive.google.com/file/d/1yz3DpLHLgKfCJfoiSCLchqaKIEdr-u1C/view"
    type: gdrive
  - name: "final_en.mp4 (EN)"
    url: "https://drive.google.com/file/d/1wJE3FmVIx8EjKpuZ-aYoe4tifsjUFPIP/view"
    type: gdrive
  - name: "RU YouTube"
    url: "https://www.youtube.com/watch?v=gMHAceSpiOU"
    type: youtube
  - name: "EN YouTube"
    url: "https://www.youtube.com/watch?v=2didzeQkNzs"
    type: youtube
---

# Что делали

Создали научпоп-видео «Тело думает без мозга» — про биоэлектричество по работам Michael Levin. Сделали две версии: RU и EN.

## Концепция

Тело — машина. Мозг — ПЛК. Биоэлектрическое поле — программа. Сознание — автопилот, который не даёт Наблюдателю проснуться.

Финальная формулировка: «Архитектор твоего сознания — это ты».

## Pipeline

1. **Сценарий RU** (8 сцен, ~7 мин) — написали на основе 4-пунктовой модели Сергея
2. **TTS SergiusVoice01** (MiniMax, один POST, 3393 символа, 4:48 голоса)
3. **3 Hailuo-клипа** по 6 сек, зациклены до 288.6 сек
4. **Drone-фон** music_v3_v3_drone2.mp3 (5% volume, зациклен)
5. **SRT-субтитры** (75 cue) hard-burned через ffmpeg
6. **Перевод на английский** + тот же pipeline для EN
7. **Обложки** через MiniMax image: лицо + ДНК + часовые шестерни
8. **YouTube загрузка** через google_token.json

## Что пошло не по плану

- Сначала баланс MiniMax был пустой → купил Token Plan $50/мес, получил новый ключ `sk-cp-...`
- ffmpeg hard-burn субтитров несколько раз обрывался по таймауту 60с — пришлось использовать `background: true`
- GLM-5.2 субагент сделал более качественный парсер чем я сам
- Пришлось сначала самому сделать всё, потом субагент перезаписал мою версию своей (лучшей)

## Результаты

| Параметр | RU | EN |
|---|---|---|
| Длительность | 4:48 (288.6 сек) | 4:36 (276.6 сек) |
| YouTube (private) | https://www.youtube.com/watch?v=gMHAceSpiOU | https://www.youtube.com/watch?v=2didzeQkNzs |
| Custom thumbnail | ✅ | ✅ |
| Голос | SergiusVoice01 (RU) | SergiusVoice01 (EN, лёгкий RU акцент — принят) |

## Решения

- Один POST для TTS (не chunks) — работает быстрее
- 3 клипа × 96 сек зациклены до полной длины голоса (видео-каркас)
- Обложки без текста → текст наложили через PIL
- Файлы разложены по подпапкам: сценарии/, исходники/, финал/, обложки/
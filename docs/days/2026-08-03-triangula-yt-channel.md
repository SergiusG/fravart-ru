---
date: 2026-08-03
topic: triangula
tags: [triangula, youtube, seo, shorts]
---

# Triangula: YouTube-канал + SEO-посадочные + Alibaba-правило

## YouTube бренд-канал создан

Сергей создал бренд-канал Triangula (brand account внутри основного аккаунта):

- **Channel ID:** `UCOcTFGjgurI2PN03gqrCkZw`
- https://www.youtube.com/channel/UCOcTFGjgurI2PN03gqrCkZw
- Создание: https://www.youtube.com/channel/create (забытая ссылка)

Оформление сгенерировано через **Alibaba wan2.7-image** (арт) + программный текст (JetBrains Mono):
- `/tmp/triangula-video/yt_avatar_800.png` — аватарка 800×800
- `/tmp/triangula-video/yt_banner_2560x1440.jpg` — баннер 2560×1440
- `/tmp/triangula-video/yt_banner_safezone_preview.jpg` — превью safe-zone 1546×423
- Исходники артов: `avatar_art.png`, `banner_art.png` (2048×2048, wan2.7-image)

Решение: отдельный полноценный канал пока не вести; бренд-канал = вывеска для Shorts. Развивать при 1–5K подписчиков.

## SEO-посадочные (Phase 2 #7)

7 страниц задеплоены на triangula.app (тот же Worker `triangula-api`, папка public/):

- `/face-shape-test/`, `/16-face-archetypes/`
- Этнические: `/face-reading-asian/`, `-african/`, `-latin/`, `-indian/`, `-european/`
- Каждая: уникальные title/desc, OG, canonical, FAQ JSON-LD schema
- Индийская добавлена по вопросу Сергея — опора на Samudrika Shastra, «from Kashmir to Kerala»
- `sitemap.xml` обновлён (8 URL), отдаётся 200
- Генератор: `/tmp/triangula/scripts/gen_seo_pages.py`

## CF-токен

Рабочий токен с правами D1: `cfut_47JFW…` (в `.env.cf`). Старый `cfut_NuIDu…` без прав D1. `cfat_LoDW…` мёртв.

## Правило: Alibaba первым

Установлено правило (записано в memory): любую генерацию картинок/медиа всегда начинать с Alibaba token-plan (оплачено), не рисовать программно и не искать других провайдеров.

## Проверка Resend

За выходные 0 новых подписчиков. 42 письма в истории — все тестовые (31.07). Доставка здорова: delivered у реальных, жалоб нет. Трафика нет — ожидаемо до запуска каналов.

## D1

`customers`: 2 записи (senega@me.com, d1test@test.com — обе тестовые). `feedback`: 0. Реальных клиентов пока нет.

## Дальше

- Подготовить метаданные 6 шортсов (заголовки/описания/хэштеги) → ручная загрузка Сергеем с телефона
- TikTok App Review — ждать (сабмит 02.08)
- Канбан Phase 2: отметить прогресс

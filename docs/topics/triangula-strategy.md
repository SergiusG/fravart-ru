---
title: Triangula — Стратегия и план реализации
date: 2026-07-30
topic: project
tags: [triangula, strategy, growth, email, marketing]
status: live
domain: triangula.app
repo: github.com/SergiusG/triangula
kanban: github.com/SergiusG/triangula/issues
---

# Triangula — Стратегия и план реализации

## Что у нас есть

Сайт **triangula.app**: человек загружает фото лица → проходит опрос из 12 вопросов → получает описание своего архетипа (кто он по характеру, какая профессия подходит). Бесплатно — краткий результат. Полный отчёт (PDF) — за $14.99 через Gumroad.

Технически: Cloudflare Worker + face-api.js (детекция лица в браузере) + 16 архетипов + DeepSeek для нарративов + Gumroad для оплаты + PDF-генерация.

## Что хотим

1. Люди приходят, проходят тест, покупают отчёт, рекомендуют друзьям
2. Собираем базу клиентов (email + архетип + дата покупки)
3. Растём органически → потом платно

---

## Phase 1 — Email и база клиентов (issues #1–#5)

Цель: автоматическая отправка PDF-отчёта на email + сбор базы покупателей.

### Что делаем
1. **Регистрируемся на Resend** (resend.com) — бесплатный email-сервис, 100 писем/день, 3000/мес. Привязываем домен triangula.app (DNS-записи SPF, DKIM, DMARC).
2. **Добавляем поле email** в страницу результата (перед оплатой). Человек вводит email → нажимает «Get full report — $14.99» → Gumroad checkout.
3. **Worker endpoint /api/send-report**: принимает данные → проверяет лицензию → генерирует PDF → отправляет через Resend API.
4. **База клиентов в Cloudflare D1** (SQLite): email, архетип, license_key, дата. Bindings в wrangler.toml.
5. **Gumroad webhook**: когда человек платит → Gumroad автоматически дёргает наш webhook → Worker отправляет письмо с PDF → записывает в базу.

### Результат Phase 1
- Юзер платит → автоматически получает красивое письмо с PDF
- Все покупатели попадают в базу
- Мы видим: кто, когда, какой архетип, сколько заработали

---

## Phase 2 — Привлечение клиентов (issues #6–#12)

Цель: трафик на сайт. Начинаем бесплатно → добавляем платное.

### Бесплатные каналы

**SEO (issue #6).** Правим страницу под поисковики: заголовок «Face Archetype Test — Discover Your Personality Type», meta description, schema.org markup, sitemap.xml. Цель: ранжироваться по «face personality test», «face archetype», «face reading test».

**Статьи (issue #7).** Пять SEO-статей (1500-2000 слов):
1. «What Your Face Shape Says About Your Personality»
2. «16 Face Archetypes — Which One Are You?»
3. «Face Reading vs Astrology — What Actually Works»
4. «How to Find Your Career Path Using Facial Geometry»
5. «The Science Behind Face-Based Personality Typing»

Публикуем на Medium, Substack. Каждая ведёт на triangula.app.

**Короткие видео (issue #8).** 10-15 клипов (15-60 сек) для TikTok, Reels, Shorts:
- «How to read faces — 3 signs» (образовательные)
- «Which archetype are you?» (CTA на тест)
- «Celebrity face archetypes» (разбор лиц знаменитостей)
- POV: «When you discover your archetype...»

Хэштеги: #facereading #archetype #personalitytest. Постинг 3-5/неделю. CTA: «Free test → triangula.app».

**Reddit (issue #9).** Soft-promotion в r/typology, r/mbti, r/Enneagram. Не спамить — рассказывать, отвечать, показывать. Пост «I built a face archetype test — here is how it works».

**Инфлюенсеры (issue #10).** 20-30 микро-блогеров (1K-50K подписчиков): YouTube/TikTok/Instagram про face reading, астрологию, типологию. Оффер: бесплатный доступ + 30% с продаж. Каждый приводит 50-200 юзеров.

### Платные каналы

**ProductHunt + AI-директории (issue #11).** Запуск на Product Hunt, theresanaiforthat.com, futurepedia.io. Цель: 500-2000 посетителей за день запуска → 30-80 покупок.

**Реклама (issue #12).** $5/день: Google Search Ads («face personality test») + Meta (Instagram Stories/Reels ads). Цель: CAC < $5, конверсия > 3%. Если работает — масштабируем.

---

## Phase 3 — Удержание и рост (issues #13–#17)

Цель: больше денег с каждого клиента, виральность, удержание.

1. **Feedback-кнопки (#13).** После результата: «This is me» / «Not quite». Запись в D1. 50+ сигналов → пересчёт весов архетипов.

2. **A/B тесты (#14).** Тестируем цену ($9.99 / $14.99 / $19.99), заголовок paywall, CTA-кнопку. 100 юзеров на вариант → выбираем лучший.

3. **Рефералка (#15).** После покупки — ссылка `?ref=ABC`. Друг получает скидку 10%. Если друг покупает — первый получает бесплатное парное чтение. Вирусный коэффициент > 0.5.

4. **Подписка (#16).** $9.99/мес: безлимитные чтения + совместимость партнёров + ежемесячные рассылки. LTV растёт с $15 до $30-50.

5. **Аналитика (#17).** Дашборд: посетители → тесты → email-и → покупки → рефералы. Воронка на каждом этапе.

---

## Порядок выполнения

1. Phase 1 (email + база) — без этого теряем покупателей
2. Phase 2 бесплатное (SEO + видео + статьи + Reddit) — первый трафик
3. Phase 2 платное (блогеры + ProductHunt + реклама) — когда воронка работает
4. Phase 3 (удержание + виральность + подписка) — масштабирование

## Канбан

Все задачи со статусом: https://github.com/SergiusG/triangula/issues

## Документы

- Архитектура: `/root/fravart-ru/docs/topics/triangula.md`
- Технология: `/root/fravart-ru/docs/topics/triangula-tech.md`
- Handoff: `/root/fravart-ru/docs/topics/triangula-session-handoff.md`
- Стратегия (этот документ): `/root/fravart-ru/docs/topics/triangula-strategy.md`
- Скилл Hermes: `triangula-project-workflow`

---

*Документ создан 2026-07-30. Стратегия согласована с Сергеем.*

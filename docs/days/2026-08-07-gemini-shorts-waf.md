# 2026-08-07 — Дневная сессия: Gemini, Shorts #05, защита сайта

**Время:** ~10:00–14:00 МСК. **Сессия:** Telegram DM с SergiusG (много голосовых, транскрипция faster-whisper /tmp/whisenv).

## 1. Shorts #05-Eyes опубликован на YouTube ✅
- Видео `eyes_FINAL.mp4` → YouTube: https://youtube.com/shorts/DgiuQBz7ud8 (ID `DgiuQBz7ud8`)
- Канал `UCOcTFGjgurI2PN03gqrCkZw`
- Статус обновлён в `/root/fravart-ru/docs/topics/triangula-shorts-status.md`
- Instagram Reels и Threads — публикация вручную (текст в `/tmp/triangula-video/publish_05-Eyes.md`)

## 2. Cloudflare Worker: защита от сканеров ✅
- В `/tmp/triangula/src/worker.js` добавлена блокировка 403 в `fetch()`: `/.git`, `/.env`, `/.aws`, `/.svn`, `/wp-*`, `/phpmyadmin`
- Причина: CF-токен без прав на редактирование WAF Zone Rulesets → сделали на уровне кода
- Деплой: `triangula-api` версия `bca576b6`. Проверено: /.git/config→403, /.env→403, /→200

## 3. ИИ-разведка (cron 94b559df39ce, 10:30 МСК) ✅
- Добавлены RSS: The Decoder, TechCrunch AI, VentureBeat AI, Hacker News
- Формат адаптирован под не-технаря: без теории/кода, «💡 Тренд дня», оценка «что это даёт пользователю»

## 4. Gemini API: разбор и починка ✅
- Старый GOOGLE_API_KEY (в .env, строки 36 и 542) был привязан к проекту с предоплатой → кредиты исчерпаны (429)
- Сергей: есть подписка Google AI Pro ($19.99/мес) → ключ должен быть в проекте БЕЗ оплаты
- Новый ключ `AQ.Ab8RN6IqH6s6...` работает (текст, vision). Пропиcан в .env (бэкап `.env.bak_gemini_20260807`)
- Модель `gemini-2.5-flash` устарела для новых проектов → заменена на `gemini-flash-latest` в config.yaml (fallback, vision, compression; бэкап `config.yaml.bak_gemini_20260807`)
- Vision-анализ проверен вживую — работает
- **Генерация картинок через Gemini API НЕ входит в подписку** (квота free tier = 0). Картинки — только Alibaba wan2.7-image.

## 5. Сравнение генерации (Alibaba vs Gemini)
- Промпт: elderly wise man, Rembrandt lighting, 85mm. Alibaba сгенерировала (хорошо), Gemini — 429 quota
- Решение Сергея: качество Alibaba устраивает, остаёмся на ней

## 6. Obsidian: полное удаление ✅
- По решению Сергея Obsidian НЕ используется
- Удалено: cron «Obsidian Drive Sync» (job 21b5cf304fb9), скрипты sync_obsidian.py/.sh
- Память обновлена

## 7. Мониторинг «печатает…» в Telegram
- Сергей видел индикатор печати. Объяснение: фоновые cron-задачи (disk watchdog 30м, indexer 60м и т.д.)
- После удаления Obsidian-синка фоновой возни меньше

## Недоделано / передать
- TikTok App Review: проверить вручную на https://developers.tiktok.com/apps/ (ключ awgtfu3ykvh1x8s0)
- Сессия закрыта по запросу (разрослась в монстр-сессию, 8 тем)

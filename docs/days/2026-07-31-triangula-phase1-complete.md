# 2026-07-31 — Triangula Phase 1 завершена

## Что сделано

### Баги починены
- `/api/send-report` — два бага:
  1. `dedupKey` использовался до определения → ReferenceError 500
  2. `purchase` объявлялся внутри `if (license_key)` → недоступен ниже
- `/api/pdf` — эндпоинт не существовал (404). Добавлен `handlePdf` — принимает archetype/scores/narrative, возвращает HTML-отчёт.

### Отчёт переделан (buildReportHtml)
Было: бары + нарратив + теги карьер + совместимость. Пустое письмо при тестовых данных.

Стало:
- Брендинг △ Triangula + номер отчёта (TRG-XXXXX) + дата
- **Your Archetype** — описание типа (абзац, все 16 архетипов в ARCH_DESC)
- **Бары** в процентах с градиентами (A/B/C)
- **Your Reading** — нарратив от DeepSeek (intro/body/close)
- **Strengths & Blind Spots** — два блока по 4 пункта каждый
- **Career Fit** — теги профессий (перевод через PROFESSION_EN)
- **Compatibility Matrix** — топ-5 + анти-топ-3 с барами
- **Personal Growth** — совет по доминирующей оси
- Футер с дисклеймером
- Нарратив: поддержка и объекта {intro,body,close}, и строки

### D1 проверена
- Таблицы: customers, feedback (миграция 001_init.sql)
- Записи пишутся через send-report и webhook
- CLI `wrangler d1` не работает (auth 7403, токен без прав D1) — но Worker binding работает

### Gumroad webhook
- Валидация: product_name содержит "triangula", email + license_key обязательны
- verifyGumroadLicense → api.gumroad.com/v2/licenses/verify
- Запись в D1 customers (ON CONFLICT email → UPDATE)
- Невалидный ключ → 200 {error: "invalid_license"} (не 500)

## Статус Phase 1

| # | Задача | Статус |
|---|--------|--------|
| 1 | Resend (email) | ✅ |
| 3 | /api/send-report | ✅ починен |
| 4 | D1 database | ✅ |
| 5 | Gumroad webhook | ✅ (полный цикл без реальной покупки не тестирован) |
| — | /api/pdf | ✅ добавлен |
| — | Шаблон отчёта | ✅ переделан |

## TODO
- Дедупликация D1 (записи без UNIQUE по archetype+email)
- Phase 2: SEO, блог, TikTok, Reddit, ProductHunt, инфлюенсеры, реклама
- Полный цикл Gumroad → webhook → D1 → письмо (нужна реальная покупка)

## Файлы
- `/tmp/triangula/src/worker.js` — все изменения
- Деплой: `cd /tmp/triangula && . /root/.hermes/.env.cf && npx wrangler deploy`

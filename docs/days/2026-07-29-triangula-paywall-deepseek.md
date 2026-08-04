---
title: Triangula — paywall, DeepSeek, face-api.js CDN (2026-07-29)
date: 2026-07-29
topic: project
tags: [triangula, deepseek, paywall, face-api]
status: live
domain: triangula.app
repo: github.com/SergiusG/triangula
---

# Triangula — 29 июля 2026

## Что сделано

### DeepSeek LLM
- DeepSeek V4 Flash подключён через `api.deepseek.com/v1/chat/completions`
- Ключ: `sk-92a…8b49` (новый, старый истекал). Занесён в Cloudflare Secrets + `/root/.hermes/credentials.env`
- Модель: `deepseek-v4-flash`
- max_tokens: 800, temperature: 0.7
- Таймаут 8 сек через AbortController → фолбэк на английский шаблон
- Промпт: 3-4 абзаца, professions, growth edge
- In-memory кэш нарративов (narrativeCache)

### face-api.js (детекция лица)
- **Убрана** inline вставка через Worker (`faceapi-inline.js` удалён, был причиной error 1101)
- Теперь грузится с **unpkg CDN** через `<script>` в HTML
- Модели tinyFaceDetector + faceLandmark68 грузятся с GitHub raw
- `script.js` адаптирован — ждёт глобальный `faceapi`, не создаёт `<script>` сам

### Worker.js (текущий стабильный)
- Без `import rules.json` — RULES захардкожены (16 архетипов + профессии)
- Без `import faceapi-inline.js`
- Полностью самодостаточный, не падает при деплое через Dashboard
- DeepSeek + фолбэк + CORS + assets routing
- Секрет: `DEEPSEEK_API_KEY` в Cloudflare Secrets

### Paywall (заглушка)
- После бесплатного результата показывается блок «Full report»
- Список: career fit, compatibility matrix, personal strategies, PDF
- Кнопка `$14.99` (disabled) + «Coming soon — Stripe being set up»

### Git / CI/CD
- Последний коммит: `2665778 feat: paywall placeholder after free reading`
- Worker деплоится через `wrangler deploy` с сервера (токен из `/root/.hermes/.env.cf`)
- `source /root/.hermes/.env.cf` — содержит `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`, `CLOUDFLARE_ZONE_ID`
- ✅ Текущий main совпадает с продакшеном

## Не сделано
- Stripe — учётка не активирована (не может завершить onboard)
- PDF-генерация
- Email-доставка отчёта
- GitHub Actions — падает на smoke test (не критично, Worker деплоится)

## Файлы
- Worker: `/tmp/triangula/src/worker.js` (без импортов, самодостаточный)
- Лендинг: `/tmp/triangula/public/` (index.html + styles.css + script.js)
- Репо: `github.com/SergiusG/triangula`

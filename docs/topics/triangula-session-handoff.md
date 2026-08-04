---
title: Triangula — session state (2026-07-29)
date: 2026-07-29
topic: project
tags: [triangula, session-handoff, status, deepseek]
status: live
domain: triangula.app
repo: github.com/SergiusG/triangula
---

# Triangula — состояние проекта (handoff для следующей сессии)

> Это handoff-документ. Сергей сказал "?" (видимо сессия разрослась и Hermes начал "тупить"). Сергей перезапустит сессию. Этот документ + memory дадут новой сессии полный контекст.

---

## Что работает в проде (✅ проверено)

**Live:** https://triangula.app (CF Zone `38e06bdaec702e8035942026d8a462b3`)

### Архитектура

**Один Cloudflare Worker** `/opt/triangula-api/` раздаёт **всё**:
- Статика (HTML/CSS/JS) через Workers Static Assets
- API (`/api/health`, `/api/archetypes`, `/api/reading`)
- face-api.js + модели через `/fapi/` путь

CF token: `cfut_NuIDu...` (нужны 4 права: Pages:Edit, Workers Scripts:Edit, Workers Routes:Edit, DNS:Edit).

### Стек

- **CV**: face-api.js в браузере (TensorFlow.js WebGL backend, не WASM)
- **Bundle**: 1.6 MB inline base64 (face-api.js 664KB + tinyFaceDetector 190KB + faceLandmark68 357KB)
- **Scoring**: cosine similarity в 3D A/B/C пространстве + 16 правил из rules.json
- **Worker cold start**: < 1 сек (в отличие от MediaPipe 8.5 MB который был 30-60 сек)

### GitHub репо (приватный)

`github.com/SergiusG/triangula`

**CI/CD:** `.github/workflows/deploy.yml` — auto-deploy на push в main

**Secrets (SENEGA1/2/3):**
- SENEGA1 = `CLOUDFLARE_API_TOKEN` (53 chars, начинается с `cfut_NuIDu7P`)
- SENEGA2 = `CLOUDFLARE_ACCOUNT_ID` (`ab7b2c8bdab9cd9f9e0cd6235dfe0659`, 32 chars)
- SENEGA3 = `CLOUDFLARE_ZONE_ID` (`38e06bdaec702e8035942026d8a462b3`, 32 chars)

**GH PAT**: `[redacted]` в `~/.config/gh/hosts.yml` (scopes: repo, workflow, ...)

---

## Файлы и документы

### В репо (github.com/SergiusG/triangula)

```
.github/workflows/deploy.yml  # CI/CD
.gitignore                     # исключает src/faceapi-inline.js
README.md                      # инструкция deploy
docs/TECHNOLOGY.md             # tech report (12KB)
rules.json                     # 16 архетипов + компас-правила (English)
scripts/fetch-faceapi.sh      # regenerates src/faceapi-inline.js (будет переименовано)
src/
├── worker.js                 # routing + scoring engine
└── faceapi-inline.js         # 1.6 MB base64, регенерируется скриптом
public/
├── index.html                # 3 шага: upload → survey → result
├── script.js                 # face-api.js + 12 questions + API call
└── styles.css                # Editorial Depth style (Cormorant + Inter)
```

### На сервере

- `/opt/triangula-api/` — сам Worker
- `/root/triangula-tech-report.md` (12KB) — полный tech report
- `/root/fravart-ru/docs/topics/triangula.md` — архитектура wiki
- `/root/fravart-ru/docs/topics/triangula-tech.md` — технология/монетизация wiki

### На Google Drive

- **Folder Triangula**: `1yjVP9fhcFylocBPB2ZnlD34ZgQqtY-h9`
- **Folder Documents**: `1Sw_NGZdq5HgkPyHSoUBDDWWyFyrAVXq5`
- **triangula-tech-report.md**: `1O4Y9LtghtiDKhC7aqj4IcD-gDsqhkQ1I`
- Drive token валиден после re-auth 2026-07-28 (gmail, drive, contacts, sheets, docs, calendar, youtube)

---

## Архитектура детекции (face-api.js)

```
Browser
  ↓ upload photo
script.js: loadFaceApi() → /fapi/face-api.min.js (664KB)
  ↓ load models
faceapi.nets.tinyFaceDetector + faceLandmark68 → /fapi/*.bin
  ↓ detect
faceapi.detectSingleFace() → 68 landmarks
  ↓
computeMetrics() → 5 face metrics (face_shape, nose_shape, mouth_width, duchen_smile, asymmetry)
  ↓
POST /api/reading → Worker
  ↓
scoreArchetype() → cosine similarity в 3D A/B/C
  ↓
{ archetype, scores: {A,B,C}, confidence, narrative }
```

### Worker endpoints

| Method | Path | Returns |
|---|---|---|
| GET | `/` | HTML landing |
| GET | `/styles.css`, `/script.js` | Static assets |
| GET | `/fapi/face-api.min.js` | JS library |
| GET | `/fapi/*-weights_manifest.json` | Model manifests (JSON) |
| GET | `/fapi/*.bin` | Model weights |
| GET | `/api/health` | `{ok, service, version, archetypes_loaded, assets}` |
| GET | `/api/archetypes` | 16 archetypes |
| POST | `/api/reading` | `{archetype, scores, confidence, narrative}` |

---

## История решений (для контекста)

### 26 июля — старт
- Бренд **Triangula** (геометрия лица = треугольники)
- Домен `triangula.app` через Cloudflare Registrar
- Лендинг `/root/triangula-landing/`
- Playbook: `/root/fravart-ru/docs/topics/face-method-startup-playbook.md`

### 27 июля — деплой v1
- Лендинг через Wrangler Pages (production `fe332fb3`)
- Трек 2 (Aftershock validation) — заблокирован DDoS-Guard, 26 пар реально распарсены в `/tmp/triangula-validation/scripts/extract_pairs.py`

### 28 июля — v0.2.0 → v0.3.0
- Worker создан, 3 эндпоинта + scoring engine
- **Сначала Pages + Worker** (конфликт с routing)
- **Перешли на 1 Worker** (статика + API + assets)
- **MediaPipe FaceMesh** 8.5 MB — таймаутил 60 сек на Samsung S24+
- **Заменили на face-api.js** 1.6 MB — работает за 5-10 сек
- **Перевели rules.json на English** (был на русском из KB Тополова)
- **GitHub** репо создан + CI/CD настроен
- **Google Drive** auth + tech report залит

---

## Что НЕ сделано / открытые вопросы

### 🔴 Блокирующее

1. **Stripe не подключен** — нужны ключи (publishable + secret). Без этого нет монетизации ($14.99).
2. **Email** — Сергей задал вопрос "?" перед перезапуском. Возможно он имеет в виду:
   - **Email юзеров** для отправки PDF-отчёта после оплаты (Resend / Cloudflare Email Workers)
   - **Email уведомлений** ему о новых пользователях (webhook)
   - **hello@triangula.app** на лендинге (CF Email Routing — должен работать автоматически)
   - **Stripe receipts** (автоматические)

### 🟡 Важное

3. **LLM-нарратив** — пока шаблонный (English). Подключить когда обсудим Anthropic/OpenRouter/локальный.
4. **Implicit feedback кнопки** в result-card — «Это про меня» / «Не про меня» → логирование в KV → пересчёт весов правил.
5. **Трек 2 validation Aftershock** — отложен, DDoS-Guard блокирует скачивание фото.

### 🟢 Потом

6. Email-доставка PDF отчёта через R2
7. Подписка $9.99/мес (v2.0)
8. SEO-лендинги под "face archetype test", "personality by face"

---

## Что делать в новой сессии

1. Прочитать `/root/fravart-ru/docs/topics/triangula.md` и `triangula-tech.md`
2. Прочитать этот handoff-документ
3. Проверить `curl https://triangula.app/api/health` (должен 200)
4. Спросить у Сергея: "Продолжаем с email — что именно? User reports / notifications / contact?"

### Если Сергей говорит "go to Stripe"

```bash
cd /opt/triangula-api
# Получить ключи из Dashboard:
# https://dashboard.stripe.com/apikeys
# publishable_key (pk_live_...) — фронт
# secret_key (sk_live_...) — НИКОГДА не в коде, через env

# Тестовые ключи для разработки:
# publishable: pk_test_...
# secret: sk_test_...

# Создать продукты в Stripe:
# - Product: Triangula Reading
# - Price: $14.99 one-time
# - Получить price_id

# Добавить в Worker:
# 1. POST /api/checkout — создаёт Stripe Session
# 2. POST /api/webhook — обрабатывает успешную оплату
# 3. Вернуть полный отчёт только после верификации webhook
```

### Если Сергей говорит "go to LLM"

```bash
# Рекомендую OpenRouter (любая модель, pay-as-you-go, $0.02-0.05 за отчёт)
# Или Anthropic Claude Haiku 4.5 (~$0.02/отчёт, качество)

# Что нужно:
# 1. API ключ OpenRouter / Anthropic
# 2. Заменить buildNarrative() в src/worker.js на вызов LLM
# 3. Кэшировать нарративы в KV (по archetype code, чтобы не платить за одинаковые)
```

### Если Сергей говорит "go to feedback"

Добавить кнопки «Это про меня» / «Не про меня» в result-card → POST /api/feedback → запись в CF Analytics Engine. Когда накопится 50+ фидбеков — пересчёт весов в rules.json.

---

## Связанные документы

- `/root/fravart-ru/docs/topics/triangula.md` — архитектура
- `/root/fravart-ru/docs/topics/triangula-tech.md` — технология/монетизация
- `/root/fravart-ru/docs/topics/face-method-startup-playbook.md` — стратегия запуска (исходный playbook)
- `/tmp/triangula-validation/scripts/extract_pairs.py` — парсер 26 пар Aftershock
- `/root/face_method_kb/face_visual_rules.json` — исходная KB Тополова (RU)
- github.com/SergiusG/triangula — исходный код + docs/TECHNOLOGY.md
- Google Drive: Triangula/Documents/triangula-tech-report.md

---

*Документ создан 2026-07-28 перед handoff в новую сессию.*

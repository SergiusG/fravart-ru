---
title: Triangula — face-archetype reading service
date: 2026-07-28
topic: project
tags: [triangula, cloudflare, ai, face-recognition, mvp]
status: live
domain: triangula.app
repo: github.com/SergiusG/triangula
---

# Triangula

AI face-archetype reading service. Сергей запустил 2026-07-26, в проде с 2026-07-28.

**Live:** https://triangula.app
**Pricing:** $14.99 one-time, strictly entertainment positioning (не B2B/HR, чтобы обойти EU AI Act Art.5(1)(f), BIPA, CUBI, EEOC)

## Что делает продукт

1. Загружаешь фото лица (анфас)
2. Отвечаешь на 12 вопросов (5-point Likert)
3. Получаешь один из 16 психотипов + нарратив + подходящие профессии

**16 психотипов** взяты из метода Тополова (АА-вс Лидер, ВВ-ас Партнёр, СС-ав Аналитик, +13 гибридов).

## Архитектура

**Один Cloudflare Worker раздаёт всё** — статику + API + WASM-файлы MediaPipe. Никакого отдельного Pages, никаких R2, никаких KV/D1.

```
Browser
  ↓ POST { face, answers }
triangula.app/api/reading → Worker
  ↓
scoreArchetype() → cosine similarity в 3D A/B/C
  ↓
{ archetype, scores, confidence, narrative }
```

**Почему в браузере:**
- MediaPipe FaceMesh (468 landmarks) считается на клиенте через WebAssembly
- Фото юзера НИКУДА не уходит с его устройства
- В Worker уходят только агрегированные метрики + ответы на вопросы
- Это снимает GDPR/BIPA/CUBI одним махом

## Stack

| Слой | Технология | Цена |
|---|---|---|
| Frontend | Static HTML/CSS/JS | бесплатно |
| CV | MediaPipe FaceMesh (в браузере) | бесплатно |
| Backend | Cloudflare Worker | бесплатно до 100k req/день |
| Scoring | JS, cosine similarity | бесплатно |
| LLM (будущее) | TBD (Anthropic/OpenRouter) | ~$0.02/отчёт |
| Stripe (будущее) | TBD | 2.9% + $0.30 |

**Стоимость 1 отчёта после LLM:** ~$0.05 → при $14.99 маржа 99.6%.

## Структура репозитория

github.com/SergiusG/triangula (приватный):

```
.github/workflows/deploy.yml  # CI/CD: push to main → auto-deploy
.gitignore                    # исключает wasm-inline.js (8.5 MB)
README.md                     # инструкция deploy
package.json                  # только type:module
wrangler.toml                 # CF Worker config (assets binding)
rules.json                    # 16 архетипов + компас-правила (из /root/face_method_kb/)
scripts/fetch-mediapipe.sh    # regenerates src/wasm-inline.js
src/
├── worker.js                 # routing + scoring engine (~320 строк)
└── wasm-inline.js            # 8.5 MB base64 (НЕ в git, регенерируется скриптом)
public/                       # static assets (HTML/CSS/JS)
├── index.html                # 3 шага: upload → survey → result
├── script.js                 # MediaPipe + 12 вопросов + API-вызов
├── styles.css                # Editorial Depth style (Cormorant + Inter)
└── face_mesh.js              # MediaPipe FaceMesh class (CDN-копия)
```

## Deploy

### Автоматически через GitHub Actions

Push в `main` → CI деплоит на Cloudflare Worker. Нужны секреты в репо:

- `CLOUDFLARE_API_TOKEN` — с правами Pages/Workers Scripts/Routes:Edit
- `CLOUDFLARE_ACCOUNT_ID`
- `CLOUDFLARE_ZONE_ID`

### Вручную

```bash
cd /opt/triangula-api
export CLOUDFLARE_API_TOKEN=...
export CLOUDFLARE_ACCOUNT_ID=...
wrangler deploy
```

## Что было сделано за 2 дня (26-28 июля 2026)

### 26 июля — старт
- Выбран бренд **Triangula** (геометрия лица = треугольники)
- Зарегистрирован домен **triangula.app** через Cloudflare Registrar
- Лендинг на `/root/triangula-landing/`
- Playbook: `/root/fravart-ru/docs/topics/face-method-startup-playbook.md`
- Исследование: конкуренты, юр. риски, культурные коды (3 отчёта субагентов)

### 27 июля — деплой v1
- Лендинг задеплоен через Wrangler Pages (production `fe332fb3`)
- Субагент: парсинг 26 пар (фото, психотип) из Aftershock, прогон через MediaPipe FaceMesh — заблокирован DDoS-Guard

### 28 июля — v0.2.0 в проде
- **Worker** `/opt/triangula-api/` с 3 эндпоинтами + scoring engine (косинусное сходство в 3D-пространстве)
- **Static+API в одном Worker** (Workers Static Assets binding + API routes)
- **MediaPipe WASM** (6.08 MB non-SIMD) inline в Worker как base64 — обходит проблему с CF-кэшем MIME-типов
- **Same-origin API** через route `triangula.app/*` → Worker
- **GitHub repo** создан: github.com/SergiusG/triangula (приватный)
- **CI/CD** настроен: push в main → auto-deploy

## Технические нюансы и питфаллы

### 1. CF-кэш ломает MIME-типы MediaPipe

Workers Static Assets по умолчанию отдаёт `.wasm*` как `text/javascript`, что ломает Emscripten.

**Решение:** отдавать WASM-файлы через сам Worker (in-memory bytes) с правильным MIME. Используется URL `/mpx/...` чтобы избежать кэша от старого пути `/face_mesh_*`.

### 2. SIMD vs non-SIMD WASM

На Samsung S24+ в Chrome иногда зависает компиляция SIMD WASM (баг V8 на мобильных).

**Решение:** выбрана non-SIMD версия — на 1% больше размер, но компилируется за 2-3 сек вместо потенциальных 60+.

### 3. Same-origin vs cross-origin

Раньше Pages раздавал лендинг, а Worker жил на отдельном URL — были CORS-проблемы.

**Решение:** переключили DNS `triangula.app` → `triangula-api.senega.workers.dev` (через CF API), Pages-проект остался жить на `triangula-app.pages.dev` для preview. Домен теперь обрабатывается Worker'ом напрямую.

### 4. Git ignore для WASM

`src/wasm-inline.js` — 8.5 MB base64. Не коммитим в git, регенерируем через `./scripts/fetch-mediapipe.sh` который скачивает с npm @mediapipe/face_mesh.

## Что осталось / TODO

### Ближайшее (v0.3)

- [ ] **Stripe** — нужны ключи для приёма $14.99 (checkout после survey)
- [ ] **LLM-нарратив** — заменить шаблонный текст на генеративный (Anthropic/OpenRouter, ~$0.02/отчёт)
- [ ] **Implicit feedback** — кнопки «Это про меня» / «Не про меня» в отчёте → логирование в KV → пересчёт весов правил по треку 3

### Среднесрочное (v1.0)

- [ ] **Трек 2: validation** на Aftershock — нужны 50+ пар фото+психотип. DDoS-Guard мешает скачивать. Альтернатива: твои 26 пар + ручная разметка новых.
- [ ] **Переписать 16 нарративов** на EN-литературный стиль Editorial Depth
- [ ] **Email-доставка отчёта** (PDF в R2 + Resend/SendGrid)

### Долгосрочное (v2.0)

- [ ] **LLM-нарратив как 3-й канал** (CV + опросник + свободный текст)
- [ ] **A/B-тест 5 вариантов лендинга**
- [ ] **Mobile app** (React Native) — Telegram mini-app

## Связанные документы

- `/root/fravart-ru/docs/topics/face-method-startup-playbook.md` — стратегия запуска
- `/root/face_method_kb/face_visual_rules.json` — исходные правила (только референс)
- `/tmp/triangula-validation/` — трек 2, парсер Aftershock

## YouTube-канал

Бренд-канал для Shorts (создан 2026-08-03 на основном аккаунте Сергея как brand account):

- **Channel ID:** `UCOcTFGjgurI2PN03gqrCkZw`
- **URL:** https://www.youtube.com/channel/UCOcTFGjgurI2PN03gqrCkZw
- Назначение: Shorts 15–60 сек (face reading, архетипы знаменитостей), CTA → triangula.app
- Название канала: **Triangula - face reading** (обновлено 03.08.2026 — SEO-ключ в названии, хорошо для поиска), псевдоним @Triangula_faces
- Оформление: аватарка + баннер в стиле Slate & Coral — `/tmp/triangula-video/yt_avatar_800.png`, `yt_banner_2560x1440.jpg` (генерация wan2.7-image + текст JetBrains Mono)
- Загрузка: вручную с телефона (API-загрузка падает в private — приложение Google не верифицировано)
- Баннер принят YouTube 03.08 (Сергей адаптировал сам; исходник 2560×1440 лежит на Drive: file/14LEElYr0XfIeTH0zX7fahRBuhwvQlhte — Telegram сжимает картинки при пересылке, оригинал отдавать через Drive)

## Контакты / контекст

- Сергей Гусев, senega@me.com
- Cloudflare Account: `Senega@me.com's Account` (`ab7b2c8bdab9cd9f9e0cd6235dfe0659`)
- GH: SergiusG/triangula (private)
- Telegram: @SergiusG (ID 419186486)

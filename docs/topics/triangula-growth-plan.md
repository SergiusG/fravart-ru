---
title: Triangula — пошаговый план продвижения
date: 2026-07-29
tags: [triangula, marketing, growth, launch]
---

## Итоги дня (29 июля) — что работает

**Техника:**
- ✅ Лендинг triangula.app (Slate & Coral дизайн)
- ✅ face-api.js детекция лица (CDN, без inline)
- ✅ DeepSeek V4 Flash — нарратив для чтения
- ✅ 16 архетипов + 3 шкалы A/B/C
- ✅ Compatibility matrix (3 группы совместимости)
- ✅ PDF-генерация (/api/pdf)
- ✅ Приём платежей: Gumroad $14.99
- ✅ License Key верификация (через Gumroad API)
- ✅ Paywall (бесплатно/платно)
- ✅ Сохранение результата в sessionStorage

**Продукт готов к продажам.** Первый тестовый платёж прошёл успешно.

---

## Пошаговый план продвижения

### Фаза 0: Довести до ума (до 31 июля)
- [ ] Обновить CF токен → purge кэша → поправить стили блока "Already purchased?"
- [ ] Купить продукт → получить реальный лицензионный ключ → протестировать цепочку
- [ ] Настроить Gumroad Content: инструкция + ссылка на triangula.app

### Фаза 1: Organic (1-7 августа)
- [ ] **Личный пост** в соцсетях/блоге с результатом своего reading
- [ ] **Product Hunt** — запустить Triangula как бесплатный инструмент
  - Заголовок: "Discover your character archetype from a selfie"
  - Категория: AI / Personality / Entertainment
  - Первый комментарий: от первого лица, про историю проекта
- [ ] **Reddit**: r/faceanalysis, r/mbti, r/personality, r/SideProject
  - Аккаунт: возраст ~2 мес (на 07.08.2026), карма 1 — голый, ссылки сразу НЕ давать
  - План: неделя 1 — прогрев (полезные ответы без ссылок, цель 15-30 кармы), неделя 2 — мягкий вход со ссылками по запросу, неделя 3 — Product Hunt + Pinterest
  - Черновики ответов готовит Hermes, публикует Сергей вручную
  - Пост: "I built an AI that reads 16 archetypes from your face — try it free"
  - Без ссылки в посте, в комментариях
- [ ] **Hacker News** — Show HN
  - Заголовок: "Show HN: Triangula – 16 personality archetypes from a selfie"
  - Если взлетит — основной трафик

### Фаза 2: Viral (7-14 августа)
- [ ] **TikTok/Reels** — формат "What's your archetype?"
  - Идея: показывать 3-секундные результаты знаменитостей
  - Хэштеги: #facialreading #personalitytest #archetype
  - Тренд: 3B+ просмотров в категории
- [ ] **Блогеры/инфлюенсеры** — разослать бесплатные промокоды:
  - Психологи, HR-специалисты, коучи (B2B-партнёрства)
  - Beauty/wellness блогеры (B2C)
  - Telegram-каналы про саморазвитие
- [ ] **Коллаборации**:
  - AstroHour / нумерологи / HR-каналы
  - Предложение: "Triangula reading как бонус к консультации"

### Фаза 3: Traffic (14-31 августа)
- [ ] **SEO-статьи** на triangula.app:
  - /blog/face-reading-archetypes — "What Your Face Says About Your Personality"
  - /blog/career-fit — "Best Careers for Each Archetype"
  - /blog/compatibility — "Who Should You Work With?"
- [ ] **Google Ads** — тест на $50:
  - Ключевые слова: "face personality test", "archetype test", "what does my face say"
  - Бюджет: $5/день на 10 дней
- [ ] **Pinterest** — инфографика 16 архетипов
- [ ] **Quora / Medium** — статьи на тему face reading

### Фаза 4: Monetization (сентябрь)
- [ ] Подписка $9.99/мес (3 чтения + база сравнения)
- [ ] PDF-совместимость для пар ($19.99)
- [ ] B2B: HR teams / подбор персонала (с юр. согласованием)

---

## Каналы трафика по приоритету

| # | Канал | Усилия | Потенциал | Старт |
|---|-------|--------|-----------|-------|
| 1 | **Product Hunt** | 1 день | 500-2000 визитов | Фаза 1 |
| 2 | **Reddit (r/SideProject)** | 2 часа | 200-1000 | Фаза 1 |
| 3 | **Personal post** | 1 час | 50-200 | Фаза 1 |
| 4 | **TikTok/Reels** | ~5 дней | 10K+ (если взлетит) | Фаза 2 |
| 5 | **Hacker News** | 1 день | 1000-5000 | Фаза 1 |
| 6 | **Блогеры** | ~7 дней | зависит от охвата | Фаза 2 |

---

## Метрики успеха (первые 3 месяца)

| Метрика | Цель |
|---------|------|
| Визиты | 1,000 |
| Конверсия в reading | 20% (200 чтений) |
| Конверсия в paid | 5% → 10 paid отчётов |
| GMV | ~$150 |
| CAC | < $5 |

---

## Ссылки
- Сайт: https://triangula.app
- Gumroad: https://senega.gumroad.com/l/triangula
- Репозиторий: github.com/SergiusG/triangula
- Документация API: triangula.app/api/health

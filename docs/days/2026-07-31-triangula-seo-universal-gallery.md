# 2026-07-31 — Triangula: SEO + универсальность метода + галерея

## Что сделано

### 1. SEO лендинга (issue #6 → Done)
- Title: "Face Archetype Test — Discover Your Personality Type | Triangula"
- Meta description с ключевыми словами (face personality test, 16 archetypes, 478 landmarks)
- Open Graph + Twitter Card — превью при шеринге
- Schema.org: Quiz + Product (цена $14.99) — rich snippets
- Canonical URL → triangula.app
- robots.txt + sitemap.xml — созданы
- Issues #1-#5 закрыты (Resend, email capture, send-report, D1, Gumroad webhook)

### 2. Универсальность метода (текст)
Добавлен абзац в «About the Method»:
- 478 landmarks одинаковы для любой нации (European, Asian, African, Latin American)
- Точки не меняются с возрастом: тест работает от 16 лет
- SEO-слой усилен: meta description + Schema.org → "every ethnicity and every age from 16"

### 3. Галерея: +4 неевропейских портрета
| Персонаж | Регион | Архетип | Код |
|---|---|---|---|
| Confucius | Китай | Analyst-Leader | СА-в |
| Simón Bolívar | Венесуэла | Visionary Leader | АВА |
| Mahatma Gandhi | Индия | Doing Partner | ВАС |
| Sun Yat-sen | Китай | Thinking Leader | АСВ |

Все public domain (Wikipedia API), квадрат 640×640, JPG. Итого 13 портретов.
Порядок перемешан: европейцы и неевропейцы чередуются.

### 4. Мобильная оптимизация галереи
- На ≤480px: первые 6 карточек + кнопка «Show all 13 faces ↓»
- Toggle: клик раскрывает все, повторный сворачивает
- На десктопе/планшете — все 13 без кнопки
- Скролл на мобиле: ~6000px → ~2800px

### 5. Бэклог
- Issue #19: SEO-страницы по этносам (/face-reading-asian, /face-reading-african, /face-reading-latin)

## Деплой
- wrangler deploy → ✨ Success, кэш ?v=9
- Проверка: robots.txt, sitemap.xml, портреты (все 200), текст универсальности — в проде
- CDN triangula.app: кэш обновляется сам (2-4 часа)

## Инцидент: engineer-бот
- Причина: ImportError `coalesce_tool_call_id` — процесс от 30 июля держал старый модуль
- Фикс: kill PID + очистка .pyc + systemctl restart hermes-engineer
- Бот восстановлен, Telegram подключён

## Связанные файлы
- `/tmp/triangula/public/index.html`, `styles.css`, `script.js`, `img/*.jpg`
- `/tmp/triangula/public/robots.txt`, `sitemap.xml`

## Портреты галереи + hero (wan2.7-image)

- Все 13 портретов галереи перегенерированы через **wan2.7-image** (Alibaba token-plan, бесплатно по подписке): единый стиль — тёмный фон, Rembrandt-свет, крупный план, фотореализм. 2048→640 JPEG.
- Hero-портрет Jefferson переделан: убраны веснушки («freckled» был в промпте), чистая кожа, сетка пересобрана (92 точки, 149 треугольников).
- Сетка усилена: линии opacity 0.88/1.9px + glow, узлы белые, измерительные линии белые.
- Кэш ?v=12.
- Replicate/Kling — без баланса, MiniMax — нет генерации изображений. wan2.7-image — рабочий путь.

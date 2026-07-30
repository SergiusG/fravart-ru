# Triangula — лендинг: метод + галерея знаменитостей (деплой)

**Дата:** 2026-07-30
**Проект:** Triangula (triangula.app)
**Статус:** ✅ Задеплоено и проверено в проде

## Что сделано

### 1. Предупреждения о фото (секция загрузки, Reading)
По просьбе Сергея добавлены явные требования к фотографии:
- ✓ Full face, looking straight ahead (чистый фас)
- ✓ Neutral expression, even lighting
- ✕ No glasses — they distort the eye area (без очков)
- ✕ Beards can skew the reading — results may be less accurate (борода искажает)

Реализация: блок `.photo-requirements` в `index.html` + стили в `styles.css`.

### 2. Расширен раздел «About the Method» (секция 01)
Добавлены 2 абзаца (английский):
- О многолетних наблюдениях и систематизации в 16 архетипов (3 базовых каста: Leader, Partner, Analyst).
- О двух чтениях (геометрия лица + ответы на вопросы) и честности результата там, где они расходятся.

### 3. Новая секция «Famous faces» (секция 02)
9 исторических персонажей, все портреты **public domain** (скачаны с Wikipedia, приведены к квадрату 640×640, в `public/img/`):

| Персонаж | Код | Архетип |
|---|---|---|
| Julius Caesar | АА-вс | Pure Leader |
| Theodore Roosevelt | АВС | Feeling Leader |
| Nikola Tesla | АС-в | Leader-Analyst |
| Oscar Wilde | ВВ-ас | Pure Partner |
| Mark Twain | В-ас | Doing-thinking Partner |
| Anton Chekhov | ВСА | Thinking Partner |
| Isaac Newton | СС-ав | Pure Analyst |
| Charles Darwin | СВА | Feeling Analyst |
| Abraham Lincoln | авс | Mediator |

Галерея: hover-эффекты (ч/б → цвет, масштабирование), адаптив 3→2→1 колонки.
Дисклеймер: архетипы — наша интерпретация для развлечения, не утверждение о личностях.

### 4. Перенумерация секций
Method 01 → Faces 02 → Archetypes 03 → Reading 04. Пункт «Faces» добавлен в навигацию.

## Деплой
- `wrangler deploy` → ✨ Success, 12 файлов (9 портретов + html/css/js), версия `ce253113`.
- Проверка продакшена: главная 200, секция Famous faces на месте, портреты 200, предупреждение про очки присутствует.
- Worker: https://triangula-api.senega.workers.dev

## Мониторинг
Cron-watchdog `triangula_watchdog.sh` (ежедневно 09:00 UTC, job_id `92305a31a067`):
проверяет главную, CSS/JS, 2 портрета, наличие секций, API на 5xx.
Молчит когда здорово (watchdog-паттерн), алертит при сбое.

## Заметки
- Vision-модель (Gemini) исчерпала кредиты — портреты не проверялись визуально, но это стандартные миниатюры с официальных Wikipedia-страниц.
- Wikimedia давал HTTP 429 при массовом скачивании — лечится задержкой 6с между запросами.
- Только английский язык (по решению Сергея).

## Связанные файлы
- `/tmp/triangula/public/index.html`, `styles.css`, `script.js`, `img/*.jpg`
- `/root/.hermes/scripts/triangula_watchdog.sh`

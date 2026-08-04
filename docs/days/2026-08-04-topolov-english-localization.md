---
date: 2026-08-04
topic: face-method
tags: [topolov, psychotype, i18n, english, pdf-report]
---

# Методика Тополова: полная локализация на английский

По требованию: все отчёты строго на английском, имя Тополова нигде не упоминается, русского не осталось.

## Что переведено

### Контент (`psychotype_content.json`)
- 16 описаний психотипов (intro) — через DeepSeek API
- 64 текста стилей руководства (4 стиля × 16 психотипов) — через DeepSeek API
- Названия: 40 индикаторов, ~23 деструктора, 6 типов личности, 5 типов мышления, 4 стиля — вручную точным словарём
- Названия психотипов: Leader, Thinking-Feeling Leader, Partner, Mediator и т.д.
- **Коды (АА-вс, ВАС…) остались кириллицей** — это системные обозначения, как MBTI-коды, не переводятся

### Код
- `generate_report.py` — весь HTML-шаблон на EN (заголовки, шкалы, футер), docstring
- `scoring.py` — 41 вопрос переведён вручную, reliability labels: low/medium/high
- `web_app.py` — форма на EN («Personality Assessment», 16 archetypes · 41 questions)
- Имя «Тополов» и «Потенциал руководителя» убраны из рабочего кода (остались только во внутренних утилитах: парсеры книги, matches_*.json — не продукт)

### Артефакты
- Русский оригинал контента: `psychotype_content_ru_backup.json`
- Тестовый PDF: `/tmp/topolov-en-test.pdf` — проверен pdftotext: кириллицы нет (кроме кода психотипа), «Тополов» нет

## Найден и исправлен баг
У психотипов **САВ** и **С-ав** было одинаковое имя «Действующий аналитик» → теперь `Doing Analyst` и `Doing-Thinking Analyst` (согласовано с Triangula).

## Грабли перевода через API
- **deepseek-v4-flash** режет длинные ответы (пустой content, ретраи по 12 с, ~1 текст в 3 мин) — не использовать
- **deepseek-chat** — стабилен, пакетами по 3 текста (разделитель `\n---`), ~40 с на батч
- **Alibaba token-plan (qwen3.8-max)** — весь день 429 Too Many Requests, не использовать для массовых переводов
- Скрипт: `translate_kb.py` (идемпотентный, сохраняет после каждого батча)
- Проверка остатков: искать кириллицу по полям, НЕ по первой букве (переводы могут начинаться со скобок/кавычек)

## Проверка результата
```bash
cd /root/face_method_kb
python3 generate_report.py --name "Test Profile" --answers "..." --out /tmp/test.pdf
pdftotext /tmp/test.pdf - | grep -c "[а-яёА-ЯЁ]"   # 1 (код психотипа)
grep -rn "Тополов" generate_report.py scoring.py web_app.py psychotype_content.json  # пусто
```

## Связано
- День: `2026-08-04-topolov-psychotype-report.md` (создание сервиса)
- Стратегия: `topics/face-method-startup-playbook.md`

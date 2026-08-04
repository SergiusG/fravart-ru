---
title: "Face-method / startup playbook — сводка и рекомендации"
date: 2026-07-26
tags: [face-method, topolov, startup, ai-product, strategy, legal]
---

# 2026-07-26 — Face-method полный разбор

## Контекст
Сергей в дороге, инициировал исследование метода Тополова для EN AI-продукта. Параллельно со сбором YT-стратегии (см. 2026-07-26-yt-research.md).

## Что сделано (6 часов фоновой работы)

1. **Скачал и систематизировал 3 книги + Excel-тест** Тополова с Drive (`1TVoDB-yFSGFRED3n8D6pQ1MImh0G0cEE`)
2. **Запустил 3 субагента** — EN-конкуренты / юр. ограничения / азиатский рынок → 145 КБ отчётов
3. **Запустил 2 субагента** — 41 вопрос с шкалами + визуальные правила AI-ready (5 признаков → 13 классов в MediaPipe)
4. **Собрал 17 публикаций Aftershock** автора `topolov` (uid 39085) — цикл «Штаны», бизнес-кейсы, разборы знаменитостей

## Ключевые находки
- **16 психотипов** (уточнили с 13) — 3 доминанты (Лидер/Партнёр/Аналитик) + гибриды
- **Telegram-ботов автора** уже работают: `@LeaderProfile_bot`, `@PersonaPhotoType_bot`, `@SellerProfile_bot`
- **Юридический разворот**: «professional inclinations» под EU AI Act Art.5(1)(f) hard-prohibited; **единственный путь = entertainment-only**
- **Свободная ниша EN**: B2C + «character archetype» + career-adjacent — никто не закрывает
- **Главная тех. находка**: визуальные правила формализованы в MediaPipe-метрики (`/root/face_method_kb/face_visual_rules.json`)

## Итоговый документ
`/root/fravart-ru/docs/topics/face-method-startup-playbook.md` — playbook на 12.9 КБ

## Открытые вопросы для Сергея
1. Название бренда (предлагаю: ArchetypeAI, PersonaReader, FaceMind, Triangula)
2. Подтверждение entertainment-only позиционирования
3. Что делать с автором — связаться, делать независимо, пригласить adviser
4. Приоритет рынка: US / UK / CA / AU
5. Freemium или paid-only на старте

## Файлы на диске
- `/root/face_method_kb/` — все 17 статей, 16 психотипов, 41 вопрос, визуальные правила
- `/root/ai-face-analysis-competitors-report.md` (30 КБ)
- `/root/ai-face-legal-memo/LEGAL_MEMORANDUM.md` (40 КБ)
- `/root/face-reading-asia-cultural-marketing-analysis.md` (75 КБ)

## Что НЕ делал
- Не связывался с Тополовым (жду решения Сергея)
- Не делал ничего на продуктовой стороне (коды, лендинги, AI-пайплайн)
- Не тратил квоты на генерацию (Kling/Suno/ElevenLabs)

## Следующий сеанс
Жду решения по 5 вопросам. После — могу:
- Сделать MVP-лендинг (HTML+CSS)
- Разработать CV-пайплайн (MediaPipe + rule engine)
- Подобрать Telegram-канал для запуска
- Связаться с автором через t.me/topologialichnosti (по запросу)

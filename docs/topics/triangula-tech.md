# Triangula — технология и монетизация

> Краткая выжимка из `/root/triangula-tech-report.md` (полный документ — 13 KB).

## Что такое архетип

**16 психотипов** из метода Тополова (Ridero 2020-2024). Каждый = комбинация трёх доминант по шкалам A (Leader) / B (Partner) / C (Analyst).

```
              A — Leader
             /|\
            / | \
     AB-с / авс \ AS-в
          /   |   \
   B — Partner ——— C — Analyst
```

## Параметры определения

### Геометрия лица (5 метрик, face-api.js в браузере)

| Метрика | Категории |
|---|---|
| `face_shape` | pentagonal / angular / oval / rounded / elongated / narrow |
| `nose_shape` | triangle-down / saddle / potato / thin / minimal / soft |
| `mouth_width` | smallest / small / medium / wide / widest |
| `duchen_smile` | true / false |
| `asymmetry` | true / false |

### Опросник (12 вопросов, 5-point Likert)

По 4 на каждую шкалу A/B/C. Например:
- Q5 (A): "Do you enjoy conflict?"
- Q2 (B): "Are you patient with people?"
- Q1 (C): "How cautious are you?"

## Алгоритм скоринга

```
1. CV → визуальные шкалы A/B/C (0-10)
2. Опросник → шкалы A/B/C (0-10)
3. Смешивание 50/50
4. Косинусное сходство в 3D → ближайший из 16 архетипов
5. Confidence = 1 - cos_distance
```

## Источник правил

База: `/root/face_method_kb/face_visual_rules.json`
- 16 правил (АА-вс, ВВ-ас, СС-ав + 13 гибридов)
- Компас-правила для каждой шкалы (A_dominant, B_dominant, C_dominant)
- `profession_fit` — эмпирические наблюдения Тополова

## Архитектура

```
Browser → face-api.js (TensorFlow.js WebGL) → 5 metrics
         ↓
         POST /api/reading → Cloudflare Worker
                          ↓
                          16 rules × 3 scales → cosine similarity
                          ↓
                          { archetype, scores, confidence, narrative }
```

**Стек:**
- Frontend: статика через Workers Static Assets
- Backend: Cloudflare Worker (тот же что раздаёт лендинг)
- CV: face-api.js в браузере (фото не покидает устройство)
- Bundle: 1.6 MB, cold start < 1 сек

## Монетизация

**Single paid tier: $14.99 за отчёт.**

### Конкуренты

| | Цена | Что даёт |
|---|---|---|
| MeByFace | €9.99/dim | Текст, 4 типа |
| Facework | $19-49 | Career |
| Face Astro | $4.99 | Top-3 careers |
| **Triangula** | **$14.99** | **16 типов + 12 metrics + CV** |

### Финансы

- Маржа: ~$14.20 чистыми (95%) с отчёта
- Цель 3 мес: $750 GMV (50 отчётов)
- Цель 12 мес: $60,000 GMV (4,000 отчётов)
- Break-even: 5 paid отчётов

### Юридическая защита

**Entertainment-only** positioning обходит:
- EU AI Act Art.5(1)(f) — workplace inference ban
- Illinois BIPA — geofence IL
- Texas CUBI — geofence TX
- FTC AI-washing — нет научных претензий
- EEOC Title VII — не для HR

### Roadmap

| Версия | Что | Статус |
|---|---|---|
| Лендинг | hero+сетка, Method, Faces(9), Archetypes(16), Reading | ✅ ГОТОВ, задеплоен |
| Оплата | Gumroad $14.99 (senega.gumroad.com/l/triangula), License Key API | ✅ работает |
| Отчёт | PDF на CF Worker, прямой DeepSeek, только английский | ✅ работает |
| Email | Resend (домен verified) | ✅ работает |
| Дедупликация | серверная через D1 (фронтенд-блок есть) | ⏳ TODO |
| v1.5 | Подписка / рефералка / A/B | после трафика |

### Текущее состояние лендинга (2026-07-31)
- **Hero**: двухколоночный, справа «исследуемый образец» — портрет Jefferson с настоящей landmark-сеткой (mediapipe 478→92 точки, 130 треугольников Делоне), анимация прорисовки + сканлайн + HUD-метки.
- **Мобильное меню**: бутерброд ☰ (≤820px), выпадающая панель.
- Кэш-бастинг `?v=5`. Деплой: `. /root/.hermes/.env.cf && npx wrangler deploy` из `/tmp/triangula`.
- Подробно: `docs/days/2026-07-31-triangula-hero-mesh-mobile-nav.md`.

### Каналы трафика

- TikTok/Reels — виральный формат "What's your archetype?" (3B+ views тренд)
- Reddit (r/faceanalysis, r/kbeauty)
- Product Hunt
- SEO: "face archetype test", "personality by face"
- Партнёрства с beauty/wellness-блогерами

---

*См. также:*
- `/root/triangula-tech-report.md` — полный отчёт (13 KB)
- `/root/fravart-ru/docs/topics/triangula.md` — архитектура и история
- github.com/SergiusG/triangula — исходный код

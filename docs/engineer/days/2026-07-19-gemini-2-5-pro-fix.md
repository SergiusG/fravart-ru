# 2026-07-19 — фикс HTTP 400 на gemini-2.5-pro

## Что случилось
- Sergei жаловался: «из ботов остался ты один», «при запросе к Hermes ошибка 400»
- В Telegram-логе инженерного gateway видны: rate-limit на primary → fallback deepseek → 400 «Unknown name "thinking_config"»
- 400 приходил от **gemini-2.5-pro через google**

## Root cause
Файл `agent/transports/chat_completions.py`, функция `_build_gemini_thinking_config`:

```python
if normalized_model.startswith("gemini-2.5-"):
    return {"includeThoughts": True}
```

Возвращало `thinking_config` для **любой** gemini-2.5-* модели. Но:
- `gemini-2.5-flash` / `flash-lite` — поле принимают ✓
- `gemini-2.5-pro` — поле **НЕ поддерживает** → Google API отвечает 400 «Unknown name "thinking_config": Cannot find field»

Это известный недосмотр в коде (issue #17426 упомянуто, но не закрыто для `gemini-2.5-pro`).

## Фикс
```diff
- if normalized_model.startswith("gemini-2.5-"):
-     return thinking_config
+ if normalized_model.startswith(("gemini-2.5-flash", "gemini-2.5-flash-lite")):
+     return thinking_config
+ if normalized_model.startswith("gemini-2.5"):
+     # Pro и не-flash варианты 2.5 отвергают поле — пропускаем
+     return None
```

## Что проверено
8 unit-кейсов через прямой вызов функции:
- gemini-2.5-pro → None ✓ (раньше падал)
- gemini-2.5-flash → {"includeThoughts": True} ✓
- gemini-2.5-flash-lite → {"includeThoughts": True} ✓
- gemini-3-pro → {"includeThoughts": True, "thinkingLevel": "high"} ✓
- gemini-3-flash → {"includeThoughts": True, "thinkingLevel": "medium"} ✓
- gemma-3-4b → None ✓ (не gemini, поле не нужно)
- deepseek-v4-flash → None ✓

## Что нужно сделать после фикса
Перезапустить gateway из внешнего SSH-шелла (НЕ из этого чата — защита от самоубийства):
```bash
systemctl --user restart hermes-gateway hermes-gateway-engineer
```

## Параллельно
Бот @fin_kosmos_bot (accountant, 8617245690) мёртв с 14.07 — polling conflict в группе. Восстанавливается:
```bash
hermes gateway install --user --profile accountant
systemctl --user enable --now hermes-gateway-accountant
```

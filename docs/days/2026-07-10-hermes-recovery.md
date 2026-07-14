---
date: 2026-07-10
topic: hermes-config
title: "Hermes: восстановление после сбоя, авторизации, model stack"
tags:
  - hermes-config
  - minimax
  - deepseek
  - youtube
  - google-drive
  - engineer-profile
  - cost-control
session_ids:
  - "20260710_131206_9a01c009"
  - "20260710_124745_b1e72e16"
  - "20260710_121907_666625"
artifacts:
  - name: "google_token.json (engineer)"
    path: "/root/.hermes/profiles/engineer/google_token.json"
    type: local
  - name: "process_watchdog.py"
    path: "/root/.hermes/scripts/process_watchdog.py"
    type: local
---

# Что делали

День восстановления и настройки после сбоев. Три сессии: диагностика gateway/Telegram, YouTube-авторизация, контроль расходов MiniMax, синхронизация engineer-профиля.

## Результаты

### Защита от монстр-сессий (20260710_131206)
- Сергей пополнил баланс MiniMax API
- Обсудили механизмы защиты от «монстр-сессий» — сессий, сжигающих бюджет
- Отключён watchdog (`process_watchdog.py`) из cron — по запросу Сергея

### YouTube и Google Drive авторизация (20260710_121907)
- Настроена OAuth-авторизация Google для загрузки видео на YouTube
- Токен сохранён в `/root/.hermes/google_token.json`
- **Engineer-профиль**: скопирован Google-токен из main, получен доступ к Drive
- **Engineer DeepSeek**: обнаружено отсутствие `DEEPSEEK_API_KEY` в `.env` инженера → исправлено (HTTP 200)
- Убран устаревший `fallback_model: mimo-v2.5-pro` из общего конфига
- MiMo (Xiaomi) полностью удалён из конфигурации main-профиля

### Диагностика сбоя gateway (20260710_124745)
- Системный сбой: проверены логи, статус процессов
- Перезапущены все gateway-процессы (main, engineer, accountant)

## Решения / заметки

- **Монстр-сессии**: основной механизм защиты — `max_turns: 40` + `tool_loop_guardrails`
- **Engineer-профиль**: теперь полностью синхронизирован с main (Google Drive, DeepSeek, модели)
- **MiMo/Videobooks**: профиль `videobooks` оставлен с `MiMo-V2.5-Pro` (Xiaomi) и Gemini 2.5 Flash как есть

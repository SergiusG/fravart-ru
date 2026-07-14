---
date: 2026-07-07
topic: hermes-config
title: "Hermes: восстановление Telegram-бота и переключение моделей"
tags:
  - hermes-config
  - telegram
  - gateway
  - deepseek
  - gemini
  - model-switch
session_ids:
  - "20260707_112109_de9e91"
  - "20260707_111406_562c7d"
  - "20260707_105959_5ba0d3"
  - "20260707_105136_643db9"
artifacts:
  - name: "hermes-gateway.service"
    path: "~/.config/systemd/user/hermes-gateway.service"
    type: local
---

# Что делали

День устранения проблем: Telegram-бот не отвечал, gateway был в нестабильном состоянии. Проведена полная диагностика и восстановление.

## Результаты

### Переключение модели: Gemini → DeepSeek V4
- Основная модель изменена с `gemini-3.5-flash` (Google) на `deepseek-v4-pro` (DeepSeek)
- Команда: `hermes config set model.default deepseek-v4-pro && hermes config set model.provider deepseek`
- Причина: Gemini 3.5 Flash медленный и "тупит", DeepSeek V4 Pro быстрее и мощнее

### Восстановление Telegram-бота
- Диагностирована ошибка Telegram-бота: `Sorry, I encountered an unexpected error`
- Проверены все gateway-процессы: main (PID 822037), engineer (PID 822057), accountant (PID 822055)
- Выполнен `hermes gateway restart` — все три профиля поднялись
- Kling MCP сервер в ошибке (401 Unauthorized) — не влияет на работу ботов

### Проверка серверных сервисов
- Диск: 54% использовано (17G свободно на 38G)
- Память: использовано 2.3G из 3.8G (нет swap)
- Все сторонние боты работают: claude-bot, max_admin_bot, max_vpn_bot, vpn_bot
- VPN-сервисы в норме

## Решения / заметки

- **Gateway**: три профиля (main, engineer, accountant) теперь стабильно работают через systemd
- **Модель**: DeepSeek V4 Pro — основной, с fallback на DeepSeek Chat → MiMo → Gemini
- **Kling MCP**: требует переавторизации, но не критичен — оставлен в parked-состоянии
- **SOUL.md**: gateway-сессии не загружают skills автоматически, только SOUL.md

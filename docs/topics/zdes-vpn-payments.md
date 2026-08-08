# ZDES VPN — стек оплат (MAX mini-app + ЮKassa)

Обновлено: 07.08.2026

## Схема

```
Клиент MAX → app.fravart.ru (mini-app) → бэкенд :8080 (miniapp-backend.service)
  → ЮKassa (магазин 1389669) → оплата → webhook payment.succeeded
  → https://vpn.fravart.ru/yukassa → nginx → :8081 (max_vpn_bot.service)
  → продление пользователя maxbot<user_id> в Marzban (+1м/3м/6м)
```

## Компоненты

| Что | Где | Сервис |
|---|---|---|
| Mini-app фронт | /opt/miniapp/ (index.html, max.html) | nginx sites-available/miniapp |
| API-бэкенд | /opt/miniapp/backend/main.py, порт 8080 | miniapp-backend.service (uvicorn) |
| Бот + вебхук оплат | /opt/max_vpn_bot/ (bot.py, payments.py, database.py), порт 8081 | **max_vpn_bot.service** (должен быть в автозагрузке!) |
| Ключи ЮKassa | /opt/max_vpn_bot/config.py — ЕДИНСТВЕННЫЙ источник (shop 1389669) | — |
| БД заказов/подписок | /opt/max_vpn_bot/vpn_bot.db (orders, payments, subscriptions) | общая для бэкенда и бота |
| Marzban-пользователи | формат maxbot<user_id>, подписки в /var/lib/marzban/db.sqlite3 | marzban-marzban-1 |

## Тарифы

1м — 200₽, 3м — 500₽, 6м — 900₽.

## Инцидент 07.08.2026: «ошибка создания платежа»

**Симптом:** клиент 22411367 в mini-app получает ошибку при попытке продлить подписку; в логах `POST /api/max_payment → 503 Payment not configured`.

**Причины (две):**
1. В /opt/miniapp/backend/main.py ключи ЮKassa были ПУСТЫМИ (`YUKASSA_SHOP_ID = ""` — «заполнить»).
2. max_vpn_bot.service был inactive + disabled — некому принимать webhook и продлевать подписки. Подписка клиента истекла 05.08 и не восстановилась.

**Фикс:**
1. main.py: `sys.path.insert(0, "/opt/max_vpn_bot")` + `from config import YUKASSA_SHOP_ID, YUKASSA_SECRET_KEY` (общие ключи с ботом, без дублирования).
2. `systemctl enable --now max_vpn_bot.service` → порт 8081 слушает, webhook /yukassa отвечает 200 локально и через vpn.fravart.ru.
3. Проверка: тестовый платёж создан (200₽, user 6112604), тестовая строка удалена из orders.
4. Реальная оплата клиента 22411367 (3м, 500₽, SberPay, payment 32081e17-000f-5001-8000-1be7cef460dd) → webhook отработал → maxbot22411367 active до 05.11.2026. Цепочка полностью рабочая.

## Известные косяки

- **chat.not.found при уведомлении об оплате**: заказ из mini-app создаётся без chat_id → бот не может послать «оплата прошла» в MAX. На продление НЕ влияет. Фикс на будущее: сохранять chat_id при создании заказа или искать чат по user_id.
- **flow='' у новых пользователей Marzban из UI панели** — см. скилл marzban-vpn-diagnostics (flow-empty-vision-mismatch). К оплате отношения не имеет, но ломает подключение новых клиентов.

## Диагностика «клиент не может продлить»

1. `journalctl -u miniapp-backend --since "1 hour ago"` — смотреть коды ответа /api/max_payment (503 = ключи, 502 = ЮKassa недоступна).
2. `systemctl is-active max_vpn_bot.service` + `ss -tlnp | grep 8081` — жив ли приёмник вебхуков.
3. `sqlite3 /opt/max_vpn_bot/vpn_bot.db "SELECT * FROM orders ORDER BY created_at DESC LIMIT 5"` — статус pending/succeeded.
4. `SELECT username,status,expire FROM users WHERE username='maxbot<ID>'` в /var/lib/marzban/db.sqlite3 — продлилось ли реально.

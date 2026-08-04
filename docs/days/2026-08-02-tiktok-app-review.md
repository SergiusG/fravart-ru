# 2026-08-02 — TikTok Developer: верификация и сабмит на ревью

## Контекст
Настраиваем TikTok Content Posting API для Triangula (автопостинг Shorts с результатами).

## Что сделано

### 1. DNS-верификация домена
- TikTok потребовал верификацию triangula.app
- Добавлена TXT-запись в Cloudflare DNS (зона 38e06bdaec702e8035942026d8a462b3):
  `tiktok-developers-site-verification=dS8iwagI2rJ5uIEOKuxqGSegN3L2VxuJ`
- Проверена через dig — резолвится ✅

### 2. Privacy Policy
- TikTok потребовал Privacy Policy URL
- Создан `/tmp/triangula/public/privacy.html` (тёмная тема, в стиле terms.html)
- Задеплоен через `wrangler deploy`
- URL: `https://triangula.app/privacy` (Cloudflare редиректит .html → без расширения)

### 3. Demo-видео для App Review
- TikTok требует видео с полным end-to-end флоу интеграции
- Сгенерировано 8 мокап-слайдов (PIL, 1920×1080, тёмная тема Triangula):
  1. Титул
  2. Лендинг + кнопка "Login with TikTok"
  3. OAuth-экран авторизации
  4. Загрузка фото
  5. Отчёт совместимости
  6. Превью видео + подтверждение
  7. Успешная публикация
  8. Итог (scopes, privacy)
- Собрано в MP4 (ffmpeg, 32 сек, fade-переходы): `/tmp/tiktok_demo/triangula_tiktok_demo.mp4`

### 4. Сабмит на ревью
- Products: Login Kit + Content Posting API
- Scopes: user.info.basic, video.upload, video.publish
- Описание интеграции заполнено
- **Статус: IN REVIEW** (ожидание 1-3 дня)

## Блокер
- До одобрения ревью авторизация не работает (ошибка client_key при OAuth)
- Ссылка авторизации готова: `python3 /tmp/triangula-video/tiktok_auth.py`

## Следующие шаги
- [ ] Дождаться одобрения App Review (письмо на fravart7@gmail.com)
- [ ] Запустить `tiktok_auth.py` → получить access_token
- [ ] Протестировать `tiktok_post.py` → публикация видео
- [ ] Интегрировать постинг в pipeline Shorts

## Файлы
- `/tmp/triangula/public/privacy.html` — Privacy Policy
- `/tmp/tiktok_demo/` — слайды + видео
- `/tmp/triangula-video/tiktok_auth.py` — OAuth
- `/tmp/triangula-video/tiktok_post.py` — постинг

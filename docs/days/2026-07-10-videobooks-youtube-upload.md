---
date: 2026-07-10
topic: videobooks
title: "MiniMax EN revoice + YouTube загрузка видео"
tags:
  - videobooks
  - minimax
  - tts
  - youtube
  - english
session_ids:
  - "cron_a5a084355e1d_20260710_191607"
artifacts:
  - name: "EN voiceover (MiniMax SergiusVoice01)"
    path: "/root/projects/videobooks/Анига/Старость/audio/voice_aging_en.mp3"
    type: local
---

# Что делали

Cron-задача перегенерации английской озвучки через MiniMax TTS (после восстановления доступа к API).

## Результаты

- Перегенерирована EN-озвучка для видео «Почему ты стареешь» голосом `SergiusVoice01`
- YouTube-загрузка видео настроена через OAuth (см. [2026-07-10 hermes-config](2026-07-10-hermes-recovery.md))
- Финальные видео `final_ru.mp4` и `final_en.mp4` собраны через ffmpeg с hard-burn субтитрами

## Решения / заметки

- MiniMax TTS восстановлен после пополнения баланса API
- EN-голос с русским акцентом — стилистическое решение, принятое осознанно

---
type: fix
project: uSugar
series: usugar
title: Исправление сохранения настроек WebApp
version: 1.6.5
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-24-usugar-v1-6-5-settings-webapp-save.png
image_text: uSugar 1.6.5 настройки WebApp сохраняются через reply кнопку открыть настройки trusted contact и family group доходят до бота
---

# uSugar 1.6.5

Hotfix для сохранения настроек Telegram Mini App.

Что изменилось:

- `/settings`, `Настройки` и `⚙️ Настройки` теперь присылают свежую reply-кнопку `⚙️ Открыть настройки`;
- Settings WebApp запускается через `KeyboardButton(web_app=...)`, чтобы `Telegram.WebApp.sendData()` возвращал данные боту;
- после сохранения страница пишет, что настройки отправлены, а бот должен ответить `Настройки применены и сохранены`;
- trusted contact и family group продолжают сохраняться в protocol JSON с маскировкой ID в чате;
- групповые настройки остаются безопасными: без WebApp-кнопки, только инструкция с локальными идентификаторами группы и ветки.

Food Log, OCR tuning, DeepSeek, БЖУК, A4-дневник, токены и медицинские формулы не менялись.

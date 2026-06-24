---
type: fix
project: uSugar
series: usugar
title: Стабилизация команд и кнопки настроек
version: 1.6.4
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-23-usugar-v1-6-4-command-routing-settings-button.png
image_text: uSugar 1.6.4 стабилизация команд кнопка настройки маршруты меню с emoji работают до fallback
---

# uSugar 1.6.4

Документационно-технический hotfix для стабильной маршрутизации команд.

Что изменилось:

- кнопка `⚙️ Настройки` теперь распознаётся как `/settings`;
- emoji-кнопки главного меню проходят через alias-router до fallback-ответов;
- `📊 Статус`, `📒 Журнал`, `📷 OCR`, `⏰ Напоминания`, `📐 Формула` и `❓ Помощь` покрыты регрессионными тестами;
- зафиксирован аудит runtime и порядка handlers в `BOT_STABILITY_AUDIT_1_6_4.md`;
- личные настройки остаются через Mini App, а группы получают безопасную инструкцию без WebApp-кнопок.

Food Log, OCR tuning, DeepSeek, БЖУК, A4-дневник, токены и медицинские формулы не менялись.

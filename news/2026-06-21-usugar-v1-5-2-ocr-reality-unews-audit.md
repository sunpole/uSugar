---
type: docs
project: uSugar
series: usugar
title: Проверка OCR и публикаций uNews
version: 1.5.2
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-21-usugar-v1-5-2-ocr-reality-unews-audit.png
---

uSugar 1.5.2: документационное обновление и аудит реальности OCR.

Что сделано:

- проверены три публичные папки OCR-примеров;
- добавлен локальный smoke-скрипт без записи в базу;
- добавлена проверка текстов на битую кодировку;
- найден блокер публикаций uNews: GitHub Actions получает Telegram Unauthorized при отправке поста.

Итог честный: старый Libre2 работает частично, новый Libre2 и фото глюкометра требуют дальнейшей доработки и ручной проверки.

OCR по-прежнему не сохраняет сахар без явного подтверждения пользователя.

Релиз добавил `OCR_REALITY_REPORT.md`, `TEXT_INTEGRITY_REPORT.md` и `UNEWS_PUBLISHING_AUDIT.md`. Локальная публикация в Telegram не выполнялась: пост должен уйти через GitHub-first workflow после обновления настройки публикации в uNews.

---
type: feature
project: uSugar
series: usugar
title: OCR для новых источников сахара
version: 1.5.0
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-21-usugar-v1-5-0-ocr-new-sources.png
---

uSugar 1.5.0 получает обновление OCR: старый Libre2 CV путь сохранен, а новые источники стали source-aware и безопаснее для ручной проверки.

Что сделано:
- добавлены source types `libre2_cv_old`, `libre2_narrow_updated`, `glucometer_photo`;
- старый Libre2 CV путь продолжает работать как основной совместимый сценарий;
- добавлена первичная CV-ветка для обновленного узкого Libre screenshot;
- добавлена первичная CV-ветка для фото ручного глюкометра;
- `/ocr` показывает активные источники распознавания;
- `/ocrlog` показывает source, confidence и saved/not saved без Telegram file IDs;
- добавлены синтетические OCR tests, безопасная политика fixtures и публичные sample folders для Libre/glucometer примеров.

Безопасность:
- OCR по-прежнему не сохраняет значение без явного подтверждения пользователя;
- одиночный результат с фото глюкометра считается ручной проверкой, а не уверенным автосохранением;
- примеры в `img/` добавлены как публичные OCR samples с разрешения пользователя;
- uNews cover не содержит реальных скриншотов, приватных данных или значений сахара;
- база данных, медицинская логика, Telegram token и production deployment не менялись.

Проверки:
- py_compile: OK;
- unit tests: OK;
- uNews dry-run/check: OK;
- Telegram smoke: manual/user-assisted, без локального автопостинга в Telegram.

Короткий текст для Telegram:
uSugar 1.5.0: обновление OCR для новых источников. Добавлены source-aware ветки для старого Libre2, узкого Libre screenshot и фото глюкометра. Сохранение результата по-прежнему только после явного подтверждения.

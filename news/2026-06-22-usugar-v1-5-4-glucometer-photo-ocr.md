---
type: feature
project: uSugar
series: usugar
title: Улучшение OCR для фото глюкометра
version: 1.5.4
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-22-usugar-v1-5-4-glucometer-photo-ocr.png
---

# uSugar 1.5.4

Обновление OCR для фото ручного глюкометра.

Что изменилось:
- добавлен fallback-поиск экрана глюкометра по нескольким ROI;
- добавлены contrast, sharpen, upscale и небольшая коррекция наклона;
- все находки глюкометра остаются ручными кандидатами и не сохраняются без подтверждения.

Smoke по `img/simple_gluk`: было `19 no_result`, `8 manual`, `0 accepted`; стало `9 no_result`, `18 manual`, `0 accepted`.

Libre2 old/narrow не ухудшились. Автосохранение OCR не добавлялось.

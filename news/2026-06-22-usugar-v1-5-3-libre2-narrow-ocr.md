---
type: feature
project: uSugar
series: usugar
title: Улучшение OCR для нового Libre2
version: 1.5.3
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-22-usugar-v1-5-3-libre2-narrow-ocr.png
---

# uSugar 1.5.3

Обновление OCR для нового Libre2.

Что изменилось:
- добавлена ветка `libre2_narrow_updated` для landscape-скриншотов `1280x576`;
- OCR смотрит в правую область, где обычно находится текущее значение;
- новые находки остаются ручными кандидатами и не сохраняются без подтверждения.

Smoke по `img/simple_new`: было `14 no_result`, `3 manual`, `1 accepted`; стало `9 no_result`, `8 manual`, `1 accepted`.

Старый Libre2 и глюкометр не ухудшились. Глюкометр остаётся отдельной будущей задачей.

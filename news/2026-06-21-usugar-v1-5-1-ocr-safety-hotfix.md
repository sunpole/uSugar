---
type: bugfix
project: uSugar
series: usugar
title: OCR safety hotfix after live smoke
version: 1.5.1
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-21-usugar-v1-5-1-ocr-safety-hotfix.png
---

uSugar 1.5.1 is an OCR safety patch after live smoke with real user-approved examples.

What changed:

- Old Libre2 CV stays the trusted baseline when experimental OCR branches disagree.
- Updated Libre and glucometer-photo OCR results are treated carefully and remain manual-check candidates when uncertain.
- Libre-specific OCR paths skip camera-photo-like glucometer frames instead of trying to read them as Libre screenshots.
- Ambiguous glucometer digit strings no longer become confident values.
- The manual-check OCR message is readable and tells the user to verify the number on the photo.

Why this matters:

The 1.5.0 release made OCR source-aware and added new source types. Live smoke showed that the next important step was not more confidence, but better refusal to guess. The bot should help, but when recognition is doubtful it must ask the user to check manually.

Safety:

- OCR still never saves a glucose record without explicit confirmation.
- No Telegram tokens, configuration secrets, private chat IDs, temporary tunnel URLs, or database contents are included.
- User-approved sample images are used only as public OCR development examples.
- Test records created during learning smoke were intentionally left in the local database by user request.

Checks:

- py_compile: OK
- unit tests: OK
- uNews dry-run/check: pending in the GitHub-first release flow
- Telegram smoke: user-assisted

Короткий текст для Telegram:
uSugar 1.5.1: патч безопасности OCR после живой проверки. Старый Libre2 снова имеет приоритет при конфликте, фото глюкометра не притворяются Libre-скрином, а сомнительные цифры остаются ручной проверкой.

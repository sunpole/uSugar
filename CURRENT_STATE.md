# uSugar Current State

Date: 2026-06-21
Version: 1.4.3

This file is a short operational snapshot after the 1.4.2 pause. It does not replace `PROJECT_STATUS.md`, `RUNBOOK.md`, or `docs/AI_CONTEXT.md`; it is the quick "where are we now" map.

## What Works Now

- Telegram bot runtime starts locally from `bot.py` through the managed runtime scripts.
- Version is read from `VERSION.json` through `version_info.py` and shown in Telegram replies.
- SQLite stores users, protocol settings, glucose logs, insulin logs, OCR attempts, reminder events, and follow-up reminders.
- Settings WebApp is served from `settings.html` and can prefill saved protocol data through `/settings`.
- `/sugar` records glucose in private chat.
- `/status` shows the latest glucose state for the current user.
- `/food` and smart inputs like `50+40` / `50 40` calculate carbohydrates and dose from the saved protocol.
- `/insulin short 4.5` and `/insulin long 18` record insulin entries.
- `/undo` deletes only the latest selected glucose or insulin entry after confirmation.
- `/log` shows journal data and can export long logs as CSV.
- `/formula` shows the current protocol and formula reference.
- `/ocr` and photo intake are implemented behind `USUGAR_OCR_ENABLED`.
- Libre2 CV OCR was verified locally earlier; Tesseract is optional; EasyOCR is disabled by default on this Windows runtime.
- `/ocrlog` shows recent OCR attempts without exposing Telegram file IDs.
- `/reminders` shows reminder state; background reminders and follow-ups are implemented.
- `/backup` exports a private ZIP without `.env`, token, database file, ngrok URL, or Telegram file IDs.
- `/story` opens the generated project story page.
- Telegram family mode exists: group/supergroup medical entry is blocked by default, while safe commands remain available.
- `/whoami` shows `user_id`, `chat_id`, and `chat_type`.
- `/trustedtest` sends a safe trusted-contact connection test without medical data.

## Commands

Core:

- `/start`
- `/help`
- `/version`
- `/health`

Daily private use:

- `/sugar`
- `/status`
- `/food`
- `/insulin`
- `/undo`
- `/log`

Settings and reference:

- `/settings`
- `/formula`
- `/setname` legacy fallback
- `/whoami`

OCR and reminders:

- `/ocr`
- `/ocrlog`
- `/reminders`

Export, story, family setup:

- `/backup`
- `/story`
- `/trustedtest`

## What Can Be Tested Now

- Runtime startup with `scripts/start_runtime.ps1 -WithBot`.
- Static settings page on port `8001`.
- Version sync in `VERSION.json` and `settings.html`.
- Unit tests in `tests/`.
- Telegram private smoke: `/version`, `/help`, `/health`, `/status`, `/whoami`, `/trustedtest`.
- Manual glucose entry and `/undo` cleanup.
- Smart food calculation with `50+40` and `50 40`.
- OCR disabled flow with `USUGAR_OCR_ENABLED=false`.
- OCR Libre2 flow when `USUGAR_OCR_ENABLED=true` and a safe test screenshot is available.
- Group behavior if a test group exists: `/help`, `/whoami`, and a blocked private-only command such as `/sugar`.

## Partially Working

- Telegram Web automation from Codex is unreliable: Codex can see the Chrome window and logs, but keyboard focus can land on reply buttons instead of the message input.
- Telegram API connectivity depends on local network/VPN state and may temporarily fail with `api.telegram.org` network errors.
- OCR is useful for the current Libre2 screenshot style, but not yet robust for all Libre layouts or blurred glucometer photos.
- Trusted contact alerts can send to a configured chat ID, but full consent, verification, quiet hours, and revocation are not finished.
- Food calculations are shown, but there is no full food journal table yet.
- GitHub contains documentation/history only; local working copy remains the implementation source of truth.

## Not Implemented Yet

- DeepSeek/LLM integration.
- Product/food database.
- PostgreSQL or production deployment.
- Full trusted-contact consent and administration flow.
- Backup restore/import.
- Search and filtering across journals.
- Editing old records beyond the latest `/undo`.
- Full graph/time extraction from Libre screenshots.
- Multiple independent production-grade OCR engines.
- Public production token rotation and public launch checklist.

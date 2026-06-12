# uSugar Project Status

Date: 2026-06-12

## Current State

uSugar is an early-stage Telegram bot project for family diabetes support. The local computer version is currently more advanced than GitHub for code: it contains the active Telegram bot, local OCR helpers, reminder logic, tests, `settings.html`, and a local SQLite database. The GitHub snapshot is still valuable as public documentation and project story, but the local working copy is now the implementation source of truth.

The current working code identifies itself as version `1.2.1` through `VERSION.json` and `version_info.py`. The bot has moved beyond the initial cleanup stage into a stable daily family-use MVP: OCR intake, Libre2 local recognition, glucose feedback, measurement reminders, basal insulin reminder checks, trusted-contact alerts, post-short-insulin follow-up reminders, mobile settings cleanup, project audit documentation, an interactive project-story page, confirmed `/undo`, WebApp settings prefill, the first UX cleanup pass, and the first two safe `bot.py` split phases are now implemented and tested.

## Comparison Summary

- GitHub is documentation-first and cleaner.
- Local is code-first and newer for implementation.
- Local had many manual backup copies and legacy setup scripts; these were moved to `_archive/2026-06-04_initial_cleanup`.
- `LICENSE` was copied from GitHub into the local project.
- The active local runtime files are now the root `bot.py`, `config.py`, `db.py`, `keyboards.py`, `settings.html`, `.env`, `usugar.db`, the helper modules `usugar_logic.py`, `usugar_export.py`, `usugar_ocr.py`, `usugar_vision.py`, `usugar_brain.py`, `usugar_reminders.py`, `usugar_protocol.py`, the handler modules `handlers/system.py`, `handlers/profile.py`, `handlers/info.py`, and the common modules `common/text.py`, `common/fsm.py`.

## Security Notes

An old generated setup script contained a real Telegram bot token. It was archived out of the working root, but the token should be regenerated in BotFather because it has existed in a plain text file.

`.env` and `*.db` remain ignored by `.gitignore` and should not be committed.

## Safe Coexistence With uChurch

uChurch was detected as a sibling project using:

- API server: `localhost:3000`
- Vite client: `localhost:5173`

uSugar now defaults its settings web page to `localhost:8001` through `USUGAR_WEB_PORT` and `WEBAPP_URL`. The new `start_all.bat` does not stop processes by broad window title rules, so it should not interfere with uChurch.

Separate launch scripts are available:

- `start_settings.bat` starts only the local settings page.
- `start_ngrok.bat` starts an ngrok tunnel to the settings page.
- `run_bot.bat` starts only the Telegram bot.
- `start_all.bat` runs health check, starts settings page, then starts the bot.
- `scripts/start_runtime.ps1`, `scripts/status_runtime.ps1`, and `scripts/stop_runtime.ps1` manage runtime processes with local PID files and logs in `.runtime/`.

## First Cleanup Artifacts

- Text snapshot: `docs/history/2026-06-04_initial_state.txt`
- PNG snapshot: `docs/history/2026-06-04_initial_state.png`
- Archive: `_archive/2026-06-04_initial_cleanup`

## Documentation Consolidation

The GitHub documentation snapshot was copied into `docs/github_original/` on 2026-06-04. A root-level `README_GITHUB.md` was also added for easy comparison with the local `README.MD`.

Use `DOCS_INDEX.md` as the local documentation entry point.

## Logic Stabilization

Manual input helpers and food-dose calculation logic were moved into `usugar_logic.py` and covered by unit tests in `tests/test_usugar_logic.py`. OCR aggregation, backup export, version handling, database persistence, command-surface sync, glucose feedback, protocol parsing, reminder state, follow-up reminder storage, settings prefill, undo deletion, and the first profile/info handler split phases now also have tests or command-surface coverage.

`health_check.py` now also checks that `ngrok` is available before a WebApp tunnel test.

The static settings page was smoke-tested locally through `http://127.0.0.1:8001/settings.html`; it returned HTTP 200.

Live Telegram Web testing confirmed earlier versions. Historical screenshots are indexed in `docs/history/SCREENSHOT_STORY.md`; public-history screenshots are redacted where Telegram values should not be shown. Version `1.0.39` added `story.html`, an interactive page generated from that screenshot history. Version `1.1.0` focused on stability rather than large new features; version `1.1.1` kept the same scope and cleaned up the daily Telegram UX. Version `1.2.0` started the safe `bot.py` split by moving only system handlers; version `1.2.1` continues it with profile and info/OCR-status handlers.

## Current Implementation Focus

- Keep OCR fast and safe: Libre2 CV is the current reliable local path; Tesseract is optional and EasyOCR remains disabled by default on this Windows runtime.
- Keep `USUGAR_OCR_ENABLED=false` as a real safety switch: disabled OCR accepts the photo metadata but does not download or run recognition.
- Keep reminders useful but non-spammy: reminder delivery keys prevent duplicate background messages.
- Keep reminder configuration in the WebApp so the user can tune windows, basal reminders, trusted-contact alerts, and short-insulin follow-ups without editing `.env`.
- Keep mistaken-record deletion narrow: `/undo` deletes only the current user's latest sugar or insulin record after confirmation.
- Keep UX surfaces short and distinct: `/health` is technical runtime status, `/status` is the user's latest glucose record, and `/reminders` is schedule/reminder state.
- Use `PROJECT_AUDIT.md` and `DATA_SOURCES.md` to keep debts, placeholders, and external-source decisions visible.
- Use `FUTURE_BACKLOG.md` for deferred work that should not distract from stable 1.1.x maintenance.
- Keep `story.html` generated from `SCREENSHOT_STORY.md` so the public development history grows automatically with new screenshots.
- Continue splitting the large `bot.py` into modules in small phases. After `1.2.1`, system handlers live in `handlers/system.py`, profile handlers in `handlers/profile.py`, and formula/OCR-status handlers in `handlers/info.py`; OCR photo intake/callbacks, reminders, database schema, protocol parsing, food/insulin/undo, and startup remain unchanged.

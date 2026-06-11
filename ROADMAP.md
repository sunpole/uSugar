# uSugar Roadmap

## Phase 0 - Cleanup And Safety

- Keep one active local code version.
- Archive manual copies and generated caches.
- Keep secrets in `.env` only.
- Make uSugar run on a port that does not conflict with uChurch.
- Add project status, runbook, and historical snapshot.

## Phase 1 - Stabilize MVP

- Split the large `bot.py` into feature modules.
- Add tests for dose calculation, phrase parsing, and database reads/writes.
- Make database path and web settings URL fully configurable. (Done.)
- Add a small health/check command for local diagnostics. (Done in `1.0.11` with `/health`.)
- Add confirmed deletion for the last mistaken glucose or insulin entry. (Done in `1.1.0` with `/undo`.)
- Make the core Telegram UX clearer before large refactoring: grouped `/help`, distinct `/health`/`/status`/`/reminders`, safer `/undo`, and shorter OCR prompts. (Done in `1.1.1`.)
- Replace manual batch setup with documented dependency installation.

## Phase 2 - Settings WebApp

- Connect `settings.html` to Telegram WebApp data flow.
- Prefill `settings.html` from the current saved user protocol before editing. (Done in `1.1.0` through a signed-by-context local URL payload and localStorage fallback.)
- Validate user protocol values before saving.
- Add a clear version display and compatibility note.
- Make the settings form compact enough for mobile-first use. (Started in `1.0.38` with compact grid and collapsible advanced sections.)
- Decide whether the static settings page stays standalone or moves to a tiny dedicated web server.

## Phase 3 - OCR And Medical Safety

- Add safe Telegram photo intake before OCR calculations. (Done in `1.0.17` with `/ocr` and photo acknowledgements.)
- Add OCR result aggregation rules before wiring OCR engines. (Done in `1.0.18`.)
- Store OCR attempt metadata and ignored local runtime images for processing. (Started in `1.0.19`; image processing active in later OCR versions.)
- Implement Libre2 screenshot OCR behind an explicit feature flag. (Local Libre2 CV path active.)
- Make `USUGAR_OCR_ENABLED=false` skip heavy OCR completely. (Done in `1.1.0`.)
- Compare OCR outputs and ask for manual confirmation on disagreement. (Done for current aggregator/confirmation flow.)
- Add safety copy: the bot helps calculate, but does not replace medical advice.
- Log confidence, source image metadata, and manual overrides.

## Phase 3.5 - Reminder Brain

- Show current reminder state in Telegram. (Done in `1.0.30`.)
- Send first and second missed-measurement reminders without duplicate spam. (Done in `1.0.31`.)
- Keep command/help/docs synchronized with tests. (Done in `1.0.32`.)
- Add basal insulin reminder checks from the configured protocol time. (Done in `1.0.33`.)
- Move reminder windows, basal toggles, and trusted-person settings into the WebApp. (Started in `1.0.34`; trusted contact ID added in `1.0.35`.)
- Add trusted-person alert flow after long missing measurement windows. (Done in `1.0.35` for Telegram ID delivery.)
- Add one-time follow-up reminders after food bolus/correction. (Started in `1.0.37` for logged short insulin.)

## Phase 4 - Production Shape

- Add deployment docs.
- Add backup/restore docs for SQLite data. (`/backup` export added in `1.0.12`; restore docs still future work.)
- Prepare GitHub repository with code, docs, and clean `.gitignore`.
- Add CI checks for syntax and tests.

## Phase 5 - Knowledge And Data

- Review Tidepool, OpenAPS, DIAX, and Nightscout as reference sources before importing data.
- Add a safe knowledge module for explanations and UX hints.
- Add DeepSeek as a bounded helper layer only after prompt and privacy boundaries are documented.

Deferred work after `1.1.0` is tracked in `FUTURE_BACKLOG.md` so the roadmap stays focused on phase direction rather than every parked idea. Version `1.1.1` did not pull deferred functionality back into scope; it only cleaned the existing UX.

## Phase 6 - Project Story And Presentation

- Build an interactive landing page from `docs/history/SCREENSHOT_STORY.md`. (Started in `1.0.39`.)
- Keep screenshot captions, version filters, and visual history generated from one source file.
- Expose the page through a Telegram command while the project is being shaped.

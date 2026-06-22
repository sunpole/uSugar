# Release Plan

Date: 2026-06-22
Version: 1.5.9

This document turns the open work inventory into a practical release sequence. It is intentionally product-focused: each milestone should stay small enough to verify with local tests, manual Telegram smoke, and a uNews patch note.

## 1.5.0 - OCR New Sources

Goal: make image intake more useful without weakening safety.

Status: implemented as the first safe source-aware OCR step. The old Libre2 CV path remains active, `libre2_narrow_updated` and `glucometer_photo` were added as lightweight CV sources, synthetic tests cover the new branches, and every recognized value still requires explicit confirmation before saving.

Completed work:

- Support the updated narrow Libre screenshot format as an initial synthetic-tested CV path.
- Add a first safe path for manual glucometer photos with stricter manual confirmation.
- Add synthetic OCR fixture policy and tests for both new source types.
- Compare OCR variants/engines with source-aware metadata.
- Keep explicit user confirmation before saving any OCR value.

Exit criteria:

- OCR disabled mode still skips heavy recognition.
- Existing Libre2 CV path still works.
- New source tests pass.
- Telegram photo smoke remains manual/user-assisted with safe test images.

## 1.5.1 - OCR Safety Hotfix

Goal: make the new OCR source work safer after live sample smoke exposed false confidence risks.

Status: implemented as a patch release after `1.5.0`. The old Libre2 CV path keeps priority when experimental sources disagree, Libre-specific OCR branches skip camera-photo-like glucometer frames, ambiguous glucometer digit strings are rejected as low-confidence/no-result, and the manual-check message is readable.

Completed work:

- Preserve old Libre2 CV as the trusted candidate for known Libre screenshots.
- Avoid averaging unrelated OCR candidates into a misleading glucose value.
- Prevent Libre OCR paths from processing camera-photo-like glucometer images.
- Treat ambiguous glucometer digit strings as manual/no-result.
- Keep test records created during live smoke in the local database by user request.

Exit criteria:

- Full unit tests pass.
- Manual Telegram smoke confirms the bot reports `v1.5.1`.
- uNews patch note passes local check but is not locally published.

## 1.5.2 - OCR Reality + uNews Publishing Audit

Goal: stop guessing about OCR and uNews status, then document the real state before continuing feature work.

Status: implemented as an audit release. `OCR_REALITY_REPORT.md` records real smoke results for `img/simple`, `img/simple_new`, and `img/simple_gluk`; `scripts/ocr_smoke.py` provides repeatable local checks without database writes; `TEXT_INTEGRITY_REPORT.md` records the text-integrity pass; `UNEWS_PUBLISHING_AUDIT.md` explains why 1.5.0 and 1.5.1 did not appear in `@uNewsLog`.

Completed work:

- Measure old Libre2, new Libre2, and glucometer sample folders with one local smoke command.
- Confirm old Libre2 is partial, new narrow Libre2 is mostly failed/partial, and glucometer photos are low-confidence/manual.
- Keep OCR non-saving unless the user explicitly confirms in Telegram.
- Confirm 1.5.0/1.5.1 uNews patch notes are valid and pending.
- Identify the uNews GitHub Actions `Unauthorized` failure as the publication blocker.

Exit criteria:

- OCR smoke runs across all three sample folders.
- Text integrity guard reports no current suspicious patterns after targeted fixes.
- uNews dry-run/check passes for the new 1.5.2 patch note.
- No local Telegram publication is performed.

## 1.5.3 - New Libre2 Narrow OCR Tuning

Goal: improve only the updated Libre2 landscape screenshot source without weakening OCR safety.

Status: implemented. The real `img/simple_new` files are 1280x576 landscape screenshots with the current value usually on the right side, not the earlier vertical synthetic narrow UI. Version `1.5.3` adds a right-side ROI and preprocessing variants for `libre2_narrow_updated`.

Completed work:

- Add `narrow_landscape_right_value` as a new Libre2 narrow variant.
- Normalize landscape component strings like `1.4.1` into `14.1` inside the Libre2 ROI.
- Improve `img/simple_new` smoke from 14 no-result frames to 9.
- Keep all new Libre2 candidates manual-confirmation by default.
- Confirm `img/simple` and `img/simple_gluk` counts did not regress.

Exit criteria:

- Full unit tests pass.
- OCR smoke confirms better `img/simple_new` coverage and no old-Libre/glucometer regression.
- uNews patch note passes local check only; real publication is GitHub-first.

## 1.5.4 - Glucometer Photo OCR Tuning

Goal: improve only manual-glucometer photo coverage without weakening safety.

Status: implemented. Version `1.5.4` adds fallback display ROI search, contrast/equalize/sharpen preprocessing, upscaling, and small rotation tolerance for `glucometer_photo`.

Completed work:

- Improve `img/simple_gluk` smoke from 19 no-result frames to 9.
- Keep accepted glucometer results at 0 by design.
- Keep every glucometer candidate manual-confirmation only.
- Confirm `img/simple` and `img/simple_new` did not regress.

Exit criteria:

- Full unit tests pass.
- OCR smoke confirms better `img/simple_gluk` coverage.
- uNews patch note passes local check only; real publication is GitHub-first.

## 1.5.5 - OCR Mode Selector + Smart Text Input

Goal: make daily Telegram use less dependent on buttons without changing dose formulas or OCR save policy.

Status: implemented. Version `1.5.5` adds a per-user OCR mode stored in the existing protocol JSON and a small smart text parser for explicit daily phrases.

Completed work:

- Add OCR modes: `auto`, `libre2_old`, `libre2_new`, and `glucometer`.
- Let users change OCR mode through `/ocr`, typed phrases, and Settings WebApp.
- Prioritize the selected OCR source while keeping other source results as diagnostics/backup.
- Parse explicit daily text such as `8.4 сахар`, `сахр 8.4`, `50 40 еда`, `3 4 5 20 70 80 еда`, `3 укол`, and `укло 3`.
- Ask for clarification when the text is ambiguous or an insulin type is missing.

Exit criteria:

- Full unit tests pass.
- OCR smoke does not write to SQLite.
- uNews patch note passes local check only; real publication is GitHub-first.

## 1.5.6 - Runtime Startup Hotfix

Goal: restore the bot after the 1.5.5 smart-text release exposed a Python runtime compatibility issue.

Status: implemented. Version `1.5.6` replaces the too-new type hint in the insulin text fallback with a runtime-compatible annotation and confirms the managed runtime starts again.

Completed work:

- Fix bot startup on the current Windows Python runtime.
- Keep OCR modes and smart text input behavior intact.
- Publish a small uNews hotfix note through GitHub-first workflow.

Exit criteria:

- Managed runtime starts and logs `uSugarBot v1.5.6`.
- Full unit tests pass.
- No formula, database, OCR algorithm, or token changes.

## 1.5.7 - Settings Confirmation + Compact Menu

Goal: make Settings WebApp and the Telegram main keyboard match smart-input daily use.

Status: implemented. Version `1.5.7` adds an apply-confirmation step in Settings WebApp, shows trusted-contact state after save, and removes the old Sugar/Food/Insulin/Name buttons from the main keyboard surface while keeping commands available.

Completed work:

- Add WebApp confirmation before sending settings to Telegram.
- Show trusted contact `chat_id` and enabled/disabled state in the saved-settings reply.
- Simplify the main keyboard around Settings, Status, Log, OCR, Reminders, Formula, and Help.
- Keep daily data entry available through smart text input and compatibility commands.

Exit criteria:

- Unit tests and text-integrity checks pass.
- Settings WebApp still sends current protocol data.
- Food Log remains a separate planned feature.

## 1.5.8 - Remove Legacy Daily Buttons

Goal: make the main Telegram menu match the smart text input flow.

Status: implemented. Version `1.5.8` confirms that the old Name/Sugar/Food/Insulin daily-entry buttons are not exposed in the main reply keyboard, while compatibility commands remain available.

Completed work:

- Keep the main menu focused on Settings, Status, Log, OCR, Reminders, Formula, and Help.
- Keep `/sugar`, `/food`, `/insulin`, and legacy `/setname` available for compatibility.
- Make `/help` show the preferred daily input examples: `8.4 сахар`, `3 укол`, `50 40 еда`, `новый либре`, `глюкометр`, and `отмена`.
- Keep smart text input as the preferred daily entry path.

Exit criteria:

- Unit tests and text-integrity checks pass.
- Main keyboard contains no Name/Sugar/Food/Insulin legacy daily buttons.
- Telegram smoke confirms the visible keyboard and smart text examples.

## 1.5.9 - Russian Command Aliases + Help Split

Goal: make the bot easier to use after the legacy daily buttons were removed.

Status: implemented. Version `1.5.9` keeps smart text input as the daily path, adds Russian text aliases for navigation commands, makes `/help` short, moves the full technical list to `/commands` and `/devhelp`, and shows the current OCR mode in key status/version surfaces.

Completed work:

- Add Russian aliases: help, commands, settings, status, journal, OCR, OCR log, reminders, formula, version, health, whoami, backup, and story.
- Keep slash commands and legacy compatibility commands available.
- Add OCR mode footer to `/version`, `/help`, `/commands`, `/status`, and `/health`.
- Confirm the `8.4` manual-smoke test glucose row was removed before the patch.

Exit criteria:

- Unit tests and text-integrity checks pass.
- Main keyboard still contains no Name/Sugar/Food/Insulin legacy daily buttons.
- Manual Telegram smoke can verify `/version`, `помощь`, `команды`, `настройки`, `журнал`, `версия`, `OCR`, OCR mode selection, and smart-food cancel flow.

## 1.6.0 - Food Log

Goal: make food calculations recordable and exportable.

Planned work:

- Add a `food_log` data model and migration-safe storage path.
- Save confirmed smart-food calculations as meal records.
- Show food entries in `/log`.
- Include food data in backups and CSV exports.
- Add `/undo` coverage for latest food entry if safe.

Exit criteria:

- Test food entries can be created and removed safely.
- No calculated meal is saved without explicit confirmation.
- Backup does not include secrets or unrelated users.

## 1.6.1 - Smart Daily Dialog

Goal: reduce command burden while keeping medical actions explicit.

Planned work:

- Route ambiguous numeric input through a clearer smart dialog.
- Improve fallback replies for random text.
- Keep buttons and text fallbacks equivalent.
- Improve prompts for glucose, food, and insulin intent.

Exit criteria:

- User can complete common daily flows with fewer commands.
- Wrong-context input is handled safely.
- Telegram smoke covers text-only alternatives.

## 1.6.0 - Trusted Contact MVP

Goal: make family alerts understandable and consent-based.

Planned work:

- Add trusted-contact consent and verification.
- Add revocation/disable flow.
- Add quiet hours.
- Clarify alert escalation rules and message templates.
- Keep `/trustedtest` as a non-medical connection check.

Exit criteria:

- Trusted contact receives alerts only after consent.
- User can disable or change the contact.
- Quiet hours are respected.

## 1.6.1 - A4 Doctor Diary

Goal: produce a practical printable report for doctor visits.

Planned work:

- Add date-range selection for logs.
- Generate a printable A4 diary from glucose, insulin, and food data.
- Include protocol snapshot and clear units.
- Keep private data local unless the user exports it.

Exit criteria:

- A test diary can be generated from local data.
- Output is readable on A4 without manual editing.

## 1.7.0 - Product Database

Goal: make food entry easier with reusable product knowledge.

Planned work:

- Define local product data format.
- Add search or quick lookup for common foods.
- Keep user-editable values and avoid hidden assumptions.
- Keep database local by default.

Exit criteria:

- Product lookup helps food calculation without replacing confirmation.
- User can override product values.

## 1.8.0 - DeepSeek

Goal: add LLM assistance only after safety boundaries are explicit.

Planned work:

- Define allowed and forbidden LLM inputs.
- Add prompt-builder tests.
- Avoid sending secrets, raw database exports, private screenshots, or medical logs without explicit design.
- Start with non-medical explanation/help use cases.

Exit criteria:

- LLM calls are optional, disabled by default, and covered by tests.
- Failure modes are documented before runtime use.

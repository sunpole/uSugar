# uSugar Roadmap

## Phase 0 - Cleanup And Safety

- Keep one active local code version.
- Archive manual copies and generated caches.
- Keep secrets in `.env` only.
- Make uSugar run on a port that does not conflict with uChurch.
- Add project status, runbook, and historical snapshot.

## Phase 1 - Stabilize MVP

- Split the large `bot.py` into feature modules. (Started in `1.2.0` with low-risk system handlers in `handlers/system.py`; continued in `1.2.1` with profile handlers in `handlers/profile.py` and info/OCR-status handlers in `handlers/info.py`; continued in `1.2.2` with basic glucose command handlers, in `1.2.3` with glucose text flow in `handlers/glucose.py`, in `1.2.4` with settings/WebApp handlers in `handlers/settings.py`, in `1.2.5` with log handlers in `handlers/logs.py`, in `1.2.6` with therapy handlers in `handlers/therapy.py`, in `1.2.7` with OCR flow handlers in `handlers/ocr.py`, in `1.2.8` with reminders runtime in `runtime/reminders.py`, and finalized in `1.3.0` by moving startup version sync into `runtime/startup.py` while keeping `bot.py` as the composition root.)
- Audit remaining large files after the `bot.py` split. (Done in `1.3.1` with `LARGE_FILES_AUDIT.md`; no runtime logic changed.)
- Verify the live local runtime with OCR enabled before the next feature/refactor phase. (Done in `1.3.2`; real Telegram Web OCR responses were observed, test records were cleaned, and `.env` remains local-only.)
- Capture product requirements before starting `1.4.0` feature work. (Done in `1.3.3` with `PRODUCT_REQUIREMENTS.md`; no runtime behavior changed.)
- Add text fallbacks for safety flows when Telegram buttons are hidden. (Done in `1.4.1` for `/undo`, OCR confirmation, and smart-food confirmation.)
- Prepare Telegram family mode and bot identity. (Done in `1.4.2`: BotFather guide, group-safe command policy, private-only medical entry guards, `/whoami` IDs, `/trustedtest`, and channel notice.)
- Rebuild the current-state map and release-reporting workflow after the pause. (Done in `1.4.3` with `CURRENT_STATE.md`, `MANUAL_TELEGRAM_TEST_PLAN.md`, and `PATCH_NOTIFICATION_RULES.md`.)
- Map open work and release targets before the next feature phase. (Done in `1.4.4` with `OPEN_WORK.md` and `RELEASE_PLAN.md`.)
- Audit real OCR behavior and uNews publication failure before continuing OCR feature work. (Done in `1.5.2` with `OCR_REALITY_REPORT.md`, `scripts/ocr_smoke.py`, `TEXT_INTEGRITY_REPORT.md`, and `UNEWS_PUBLISHING_AUDIT.md`.)
- Tune the updated Libre2 landscape screenshot path before expanding into other OCR sources. (Done in `1.5.3`; `img/simple_new` no-result frames dropped from 14/18 to 9/18, and new candidates remain manual.)
- Tune manual glucometer photo OCR without increasing trust. (Done in `1.5.4`; `img/simple_gluk` no-result frames dropped from 19/27 to 9/27, accepted stayed 0, and all candidates remain manual.)
- Split remaining large files only as small follow-up releases: start with `settings.html` CSS extraction or `/undo` extraction from `handlers/therapy.py`, not with OCR or database schema.
- Add tests for dose calculation, phrase parsing, and database reads/writes.
- Make database path and web settings URL fully configurable. (Done.)
- Add a small health/check command for local diagnostics. (Done in `1.0.11` with `/health`.)
- Add confirmed deletion for the last mistaken glucose or insulin entry. (Done in `1.1.0` with `/undo`.)
- Make the core Telegram UX clearer before large refactoring: grouped `/help`, distinct `/health`/`/status`/`/reminders`, safer `/undo`, and shorter OCR prompts. (Done in `1.1.1`.)
- Replace manual batch setup with documented dependency installation.

## Phase 2 - Settings WebApp

- Connect `settings.html` to Telegram WebApp data flow.
- Prefill `settings.html` from the current saved user protocol before editing. (Done in `1.1.0` through a signed-by-context local URL payload and localStorage fallback.)
- Move user display-name editing into the Settings WebApp as the preferred path, while keeping `/setname` as a compatibility command until the WebApp flow is complete. (Implemented in `1.4.0`; `/setname` remains legacy/fallback.)
- Re-check name and protocol prefill against the current database before changing the settings UX. (Implemented in `1.4.0` with stronger payload/localStorage/Telegram fallback.)
- Validate user protocol values before saving.
- Add a clear version display and compatibility note.
- Make the settings form compact enough for mobile-first use. (Started in `1.0.38` with compact grid and collapsible advanced sections.)
- Decide whether the static settings page stays standalone or moves to a tiny dedicated web server.

## Phase 3 - OCR And Medical Safety

- Add safe Telegram photo intake before OCR calculations. (Done in `1.0.17` with `/ocr` and photo acknowledgements.)
- Add OCR result aggregation rules before wiring OCR engines. (Done in `1.0.18`.)
- Store OCR attempt metadata and ignored local runtime images for processing. (Started in `1.0.19`; image processing active in later OCR versions.)
- Implement Libre2 screenshot OCR behind an explicit feature flag. (Local Libre2 CV path active; live runtime verified in `1.3.2` with `USUGAR_OCR_ENABLED=true`.)
- Make `USUGAR_OCR_ENABLED=false` skip heavy OCR completely. (Done in `1.1.0`.)
- Compare OCR outputs and ask for manual confirmation on disagreement. (Done for current aggregator/confirmation flow.)
- Add safety copy: the bot helps calculate, but does not replace medical advice.
- Log confidence, source image metadata, and manual overrides.
- Add future OCR source types for updated long/narrow Libre screenshots and blurred manual glucometer photos. (Initial source-aware support added in `1.5.0` with `libre2_narrow_updated` and `glucometer_photo`; `1.5.1` added safety guards after live smoke showed disagreement and false-source risks; `1.5.2` measured the current reality; `1.5.3` tuned real 1280x576 updated Libre2 screenshots; `1.5.4` tuned real glucometer photos while keeping all glucometer candidates manual.)

## Phase 3.1 - Smart Daily Dialog

- Keep the main Telegram keyboard minimal.
- Route ordinary daily input through a smart dialog that understands numbers and asks whether the user means glucose, food, or insulin. (Partially done; carb sums now calculate directly, single numbers still use clarification.)
- Treat `50+40` and `50 40` as carbohydrate totals when food context is clear. (Done in `1.4.0`.)
- Ask for a fresh glucose measurement if the last value is older than 60 minutes before using it for correction. (Done in `1.4.0`; configurable as `glucose_fresh_minutes`.)
- Move rounding and dose-reduction rules into protocol parameters. (Partially done in `1.4.0`; `pen_step` and `dose_reduction_percent` are protocol fields.)
- Keep friendly fallback replies for casual text.
- Add typed confirmation/cancel fallbacks for smart-food calculations when reply buttons are collapsed. (Done in `1.4.1`; food saving stays disabled until a food journal exists.)

## Phase 3.5 - Reminder Brain

- Show current reminder state in Telegram. (Done in `1.0.30`.)
- Send first and second missed-measurement reminders without duplicate spam. (Done in `1.0.31`.)
- Keep command/help/docs synchronized with tests. (Done in `1.0.32`.)
- Add basal insulin reminder checks from the configured protocol time. (Done in `1.0.33`.)
- Move reminder windows, basal toggles, and trusted-person settings into the WebApp. (Started in `1.0.34`; trusted contact ID added in `1.0.35`.)
- Add trusted-person alert flow after long missing measurement windows. (Done in `1.0.35` for Telegram ID delivery.)
- Finish trusted-contact consent, verification, disable/revoke flow, quiet hours, and clearer alert rules. (Captured in `PRODUCT_REQUIREMENTS.md`; future work.)
- Add safe trusted-contact connection testing before full alert UX. (Started in `1.4.2` with `/trustedtest`; no medical data is sent by the test.)
- Add one-time follow-up reminders after food bolus/correction. (Started in `1.0.37` for logged short insulin.)

## Phase 3.6 - Telegram Family Mode

- Keep private chat as the default place for daily medical records. (Done in `1.4.2`.)
- Allow only safe commands in groups/supergroups: `/version`, `/help`, `/health`, `/whoami`, and `/reminders`. (Done in `1.4.2`.)
- Show `user_id`, `chat_id`, and `chat_type` through `/whoami` for family setup. (Done in `1.4.2`.)
- Document BotFather identity, command list, privacy mode, channel positioning, and token-rotation policy. (Done in `1.4.2` with `TELEGRAM_BOT_SETUP.md`.)
- Treat channels as future notification/announcement surfaces, not daily input surfaces. (Started in `1.4.2` with a neutral channel command notice.)
- Later: design explicit consent, quiet hours, revocation, and alert routing before sending real medical notifications to groups.

## Phase 4 - Production Shape

- Add deployment docs.
- Add backup/restore docs for SQLite data. (`/backup` export added in `1.0.12`; restore docs still future work.)
- Prepare GitHub repository with code, docs, and clean `.gitignore`.
- Add CI checks for syntax and tests.

## Phase 5 - Knowledge And Data

- Review Tidepool, OpenAPS, DIAX, and Nightscout as reference sources before importing data.
- Add a safe knowledge module for explanations and UX hints.
- Add DeepSeek as a bounded helper layer only after prompt and privacy boundaries are documented.
- Keep product database work and printable A4 doctor diary deferred until core daily flows are stable.

Deferred work after `1.1.0` is tracked in `FUTURE_BACKLOG.md` so the roadmap stays focused on phase direction rather than every parked idea. Version `1.1.1` did not pull deferred functionality back into scope; it only cleaned the existing UX.

## Phase 6 - Project Story And Presentation

- Build an interactive landing page from `docs/history/SCREENSHOT_STORY.md`. (Started in `1.0.39`.)
- Keep screenshot captions, version filters, and visual history generated from one source file.
- Expose the page through a Telegram command while the project is being shaped.

## Phase 7 - Release Notes And uNews

- Keep a short current-state document for returning to the project after pauses. (Done in `1.4.3`.)
- Use a manual Telegram smoke checklist when browser automation is unreliable. (Done in `1.4.3`.)
- Prepare uSugar patch notes for the uNews workflow in `C:\!CODE_CLUB\new 2026\004_uNews`. (Rule documented in `1.4.3`.)
- Publish every future closed uSugar release through the GitHub-first uNews workflow when the project-local patch note has a safe image and the uNews dry-run/check is green. (Required since `1.4.4`; local commands are for checks/diagnostics, not normal real publication.)
- If uSugar news is pending but not visible in `@uNewsLog`, check `UNEWS_PUBLISHING_AUDIT.md` and the `sunpole/uNews` GitHub Actions publisher first. The 1.5.0/1.5.1 queue was blocked by Telegram `Unauthorized` in the uNews Actions secret, not by uSugar YAML/image content.
- Stop instead of publishing if a patch note has missing images, invalid YAML, secrets/private data, or failed dry-run/check.

# Changelog

Version history was originally kept as comments in `config.py`. It is now preserved here and in `VERSION.json`.

## 1.5.4 - 2026-06-22

- Tuned only the `glucometer_photo` OCR source for real manual-glucometer photos from `img/simple_gluk`.
- Added a fallback display search with multiple safe ROIs, contrast/sharpen preprocess variants, upscale, and small rotation tolerance for portrait camera photos.
- Improved `img/simple_gluk` smoke results from `19 no_result / 8 manual / 0 accepted` to `9 no_result / 18 manual / 0 accepted`.
- Kept all glucometer candidates manual-confirmation by default; no automatic OCR saving was added.
- Confirmed Libre2 smoke did not regress: `img/simple` stayed at `4 accepted / 7 manual / 0 no_result`, and `img/simple_new` stayed within the improved 1.5.3 range.
- No Libre2 algorithm change, food logging, trusted contact work, DeepSeek, Telegram token change, or local Telegram publication was added.

## 1.5.3 - 2026-06-22

- Tuned the `libre2_narrow_updated` OCR source for real landscape Libre2 screenshots from `img/simple_new`.
- Added a right-side landscape ROI detector (`narrow_landscape_right_value`) for 1280x576 screenshots where the current glucose value is shown near the right edge.
- Normalized Libre2 component strings such as `1.4.1` into `14.1` only inside the new landscape ROI.
- Improved `img/simple_new` smoke results from `14 no_result / 3 manual / 1 accepted` to `9 no_result / 8 manual / 1 accepted`.
- Kept new Libre2 results manual by default; OCR still does not save sugar without explicit confirmation.
- Confirmed no regression in `img/simple` and `img/simple_gluk` smoke counts.
- No glucometer OCR tuning, food logging, trusted contact work, DeepSeek, Telegram token change, or local Telegram publication was added.

## 1.5.2 - 2026-06-21

- Added `OCR_REALITY_REPORT.md` with measured OCR results across `img/simple`, `img/simple_new`, and `img/simple_gluk`.
- Added `scripts/ocr_smoke.py`, a local OCR smoke command that prints file/source/candidate/confidence/status without writing to SQLite or creating glucose records.
- Added `scripts/text_integrity_check.py` and `TEXT_INTEGRITY_REPORT.md` to guard current documentation, news notes, settings text, and bot-facing text against damaged encoding.
- Fixed the current OCR manual-check text path in `usugar_vision.py` so users do not see question-mark mojibake when OCR needs manual review.
- Added `UNEWS_PUBLISHING_AUDIT.md`; the missing 1.5.0 and 1.5.1 posts are traced to the `sunpole/uNews` GitHub Actions publisher failing with Telegram `Unauthorized`.
- Added the 1.5.2 uNews patch note for GitHub-first publication checks. No local Telegram publication was performed.
- No database schema migration, dose logic change, Telegram token change, local uNews publish, or new large OCR algorithm was added.

## 1.5.1 - 2026-06-21

- Hardened OCR aggregation after live Telegram smoke with real user-approved sample images showed unsafe disagreement between old Libre, new Libre, and glucometer branches.
- Restored old Libre2 CV priority when new experimental sources disagree, so the bot no longer averages unrelated candidates into a misleading value.
- Added image-shape guards so Libre screenshot OCR branches skip camera-photo-like glucometer frames instead of trying to interpret device photos as Libre screens.
- Tightened the glucometer photo branch so ambiguous long digit strings become low-confidence/no-result candidates rather than confident wrong glucose values.
- Fixed the uncertain OCR message for the "manual check needed" path so users see a readable safety prompt instead of damaged encoding.
- Added regression tests for old-Libre priority, far-source disagreement, camera-photo shape guards, and ambiguous glucometer digit rejection.
- Test glucose records created during live learning smoke were intentionally left in the local database because the user asked not to delete them during OCR development.
- No database schema migration, automatic OCR saving, medical dose logic, Telegram token, production deployment, or uNews local publishing was added.

## 1.5.0 - 2026-06-21

- Added source-aware OCR result metadata with `source_type` values for `libre2_cv_old`, `libre2_narrow_updated`, and `glucometer_photo`.
- Kept the existing Libre2 CV path working while adding lightweight CV recognition branches for updated narrow Libre screenshots and synthetic manual-glucometer photos.
- Updated OCR aggregation safety: a single Libre candidate can be shown as medium confidence, while a single glucometer-photo candidate requires manual confirmation.
- Expanded `/ocr` and `/ocrlog` surfaces so users can see active OCR sources, source type, confidence, saved/not-saved status, and engine notes without exposing Telegram `file_id` values.
- Added synthetic OCR unit-test coverage, `tests/fixtures/ocr/README.md`, and public OCR sample folders under `img/` with user-approved Libre/glucometer examples.
- No database schema migration, medical dose logic, Telegram token, production deployment, or automatic OCR saving was added.

## 1.4.4 - 2026-06-21

- Added `OPEN_WORK.md` with the current unfinished-work inventory grouped by OCR, family mode, daily flow, food, exports, and future production/LLM tasks.
- Added `RELEASE_PLAN.md` with the planned milestone order from `1.5.0` OCR new sources through `1.8.0` DeepSeek.
- Strengthened the uNews workflow: each closed uSugar release must now produce a project-local patch note and publish it to `@uNewsLog` when dry-run/check and safety checks pass.
- Added the `1.4.4` uNews patch note in `news/` with a safe documentation screenshot.
- No medical logic, OCR algorithms, Telegram token, database schema, or runtime behavior was intentionally changed.

## 1.4.3 - 2026-06-21

- Added `CURRENT_STATE.md` as a short current-state map after the 1.4.2 pause.
- Added `MANUAL_TELEGRAM_TEST_PLAN.md` to make Telegram smoke testing explicitly user-assisted when Codex cannot reliably type into Telegram Web.
- Added `PATCH_NOTIFICATION_RULES.md` documenting how uSugar should prepare patch notes for the local uNews workflow in `C:\!CODE_CLUB\new 2026\004_uNews`.
- Confirmed the uNews channel/bot workflow from local docs: `@uNewsLog`, `@uNewsDev_bot`, project-local `news/` patch notes, YAML metadata, and dry-run publication checks.
- No medical logic, Telegram handlers, OCR behavior, database schema, or runtime behavior was intentionally changed.

## 1.4.2 - 2026-06-21

- Added `TELEGRAM_BOT_SETUP.md` with BotFather identity setup, command list guidance, privacy-mode notes, channel positioning, and token-rotation policy.
- Added `common/chat.py` for safe chat type detection and shared private/group/channel policy helpers.
- Kept medical data entry private by default: glucose, food, insulin, OCR, settings, logs, backup, and deletion now explain that they belong in private chat when called from groups.
- Added family group safe command behavior for `/version`, `/help`, `/health`, `/whoami`, and `/reminders`; group `/help` now shows a shorter family-mode command list.
- Improved `/whoami` to show `user_id`, `chat_id`, and `chat_type` for trusted-contact and family-group setup.
- Added `/trustedtest` as a safe trusted-contact delivery check without glucose, insulin, OCR, journal, or other medical data.
- Added a neutral channel command notice so Telegram channels are treated as future announcement/notification surfaces, not the main daily dialog.

## 1.4.1 - 2026-06-13

- Added text fallbacks for `/undo` when Telegram reply buttons are hidden: `удалить последний сахар`, `удалить последний укол`, and `отмена`.
- Kept `/undo` safe: text selection only chooses the record type; deletion still requires a separate explicit confirmation.
- Added text alternatives for OCR confirmation: `сохранить`, `не сохранять`, `отмена`, and `ввести вручную`.
- Added smart-food follow-up text actions after `50+40` / `50 40`: `сохранить еду`, `не сохранять`, and `отмена`.
- Kept food saving disabled because there is no food journal table yet; `сохранить еду` explains that the calculation is not saved.
- Updated `/help` with a short note about using words when buttons are not visible.

## 1.4.0 - 2026-06-13

- Improved Settings WebApp prefill for user name and saved protocol data.
- Added WebApp fields for food-calculation safety: fresh-glucose window and optional dose-reduction percentage.
- Made the Settings WebApp denser for Telegram Mini App use with smaller spacing and compact calculation controls.
- Marked `/setname` as a legacy/fallback path in `/help`; `/settings` is now the preferred place to edit the display name.
- Added smart food calculation for inputs like `50+40` and `50 40`: the bot sums carbs, converts to XE, calculates dose from the current protocol, and does not save a journal entry.
- Changed food correction freshness from the old hidden 15-minute rule to a protocol parameter defaulting to 60 minutes.
- Updated `/food` flow to accept carbohydrate sums and explain when an older glucose value is not used for correction.
- Added tests for smart carb sums, protocol food-safety parameters, WebApp fields, and `/setname` legacy help text.

## 1.3.3 - 2026-06-13

- Added `PRODUCT_REQUIREMENTS.md` to capture product requirements before the next implementation phase.
- Documented requirements for Settings WebApp name editing and prefill, compact Telegram Mini App layout, smart dialog input, food calculation, future OCR source types, trusted contact behavior, and deferred DeepSeek/product database/A4 diary/token work.
- Updated roadmap, backlog, project status, and AI context to point future work at the product requirements document.
- No runtime code, medical logic, database schema, or `.env` changes were made.

## 1.3.2 - 2026-06-12

- Enabled local OCR testing through `.env` with `USUGAR_OCR_ENABLED=true`; `.env` remains uncommitted.
- Fixed the local `.env` encoding issue by saving it as UTF-8 without BOM so `python-dotenv` reads `BOT_TOKEN` correctly.
- Restarted the managed runtime and confirmed the bot process starts as `uSugarBot v1.3.2`.
- Confirmed real Libre2 OCR execution in Telegram Web: the local CV path recognized Libre2 screenshot values and reported `uSugarBot v1.3.2`.
- Cleaned test glucose entries created during the live OCR/Telegram smoke so `usugar.db` returned to its pre-smoke journal counts.
- Ran `py_compile` and the full unit test suite; all 66 tests passed.
- Noted that Telegram Web keyboard/focus automation is unstable on this Windows desktop session and should be treated as partially manual for command-smoke verification.

## 1.3.1 - 2026-06-12

- Added `LARGE_FILES_AUDIT.md` after closing the `bot.py` composition-root refactor.
- Scanned active Python, HTML, and JS files for large-file risks and documented future split points.
- Identified `settings.html`, `usugar_ocr.py`, `db.py`, `usugar_reminders.py`, `handlers/therapy.py`, `story.html`, and generated `docs/history/story_data.js` as the main large-file watchlist.
- Kept runtime, business logic, OCR behavior, reminders, database schema, and Telegram handlers unchanged.

## 1.3.0 - 2026-06-12

- Finalized `bot.py` as the composition root after the 1.2.x handler split.
- Moved the startup-only `update_version_in_html()` helper into `runtime/startup.py`.
- Kept `main()`, polling, retry behavior, handler registration order, OCR, reminders, database schema, and medical logic unchanged.
- Updated `/help` so the visible command surface includes `/start` and `/help`.
- Diagnosed OCR as disabled by configuration/default when `USUGAR_OCR_ENABLED` is not enabled locally.
- Diagnosed GitHub Pages as build/artifact successful but deploy job failed with a generic GitHub Pages deployment error visible in public Actions logs.

## 1.2.8 - 2026-06-12

- Phase 9 `bot.py` refactor: moved reminders runtime into `runtime/reminders.py`.
- Moved `/reminders`, `send_due_reminders()`, `reminder_loop()`, and reminder-only runtime wiring.
- Preserved reminder behavior and user-facing texts except for the module boundary.
- Left OCR, glucose, therapy, logs, settings, database schema, startup, polling, and production deployment unchanged.

## 1.2.7 - 2026-06-12

- Phase 8 `bot.py` refactor: moved OCR handlers into `handlers/ocr.py`.
- Moved photo intake, OCR callbacks, confirmation flow, manual OCR value flow, and OCR-only helper wiring.
- Preserved OCR behavior and user-facing texts except for the module boundary.
- Left reminders loop, database schema, startup, polling, and glucose/settings/logs/therapy logic unchanged.

## 1.2.6 - 2026-06-12

- Phase 7 `bot.py` refactor: moved therapy handlers into `handlers/therapy.py`.
- Moved `/food`, `/insulin`, `/undo`, their FSM handlers, undo confirmation, and the short-insulin follow-up scheduling helper.
- Preserved user-facing behavior and texts except for the module boundary.
- Left OCR, reminders loop, settings/WebApp, logs, glucose handlers, database schema, startup, and polling unchanged.

## 1.2.5 - 2026-06-12

- Phase 6 `bot.py` refactor: moved log handlers into `handlers/logs.py`.
- Moved `/log`, journal FSM handling, and CSV export helper code used only by the journal flow.
- Preserved journal behavior and user-facing texts except for the module boundary.
- Left OCR, reminders, food/insulin/undo, settings/WebApp, database schema, startup, and polling unchanged.

## 1.2.4 - 2026-06-12

- Phase 5 `bot.py` refactor: moved settings/WebApp handlers into `handlers/settings.py`.
- Moved `/settings`, `web_app_data`, `build_settings_url()`, and `build_protocol_summary()`.
- Preserved command behavior and user-facing texts except for the module boundary.
- Left OCR, reminders loop, food/insulin/undo, logs, database schema, startup, and polling unchanged.

## 1.2.3 - 2026-06-12

- Phase 4 `bot.py` refactor: moved glucose text flow into `handlers/glucose.py`.
- Moved `process_sugar`, HI/LOW/help shortcuts, ordinary numeric text intake, multi-number flow, number-context choice, and saved-glucose feedback helper.
- Kept command behavior and user-facing texts unchanged except for the module boundary.
- Left OCR photo intake/callbacks, reminders, settings/WebApp, food/insulin/undo, logs, database schema, startup, and polling unchanged.

## 1.2.2 - 2026-06-12

- Phase 3 `bot.py` refactor: moved basic glucose handlers into `handlers/glucose.py`.
- Preserved command behavior for `/sugar` and `/status`.
- Left ordinary glucose value input, HI/LOW/help shortcuts, multi-number flow, OCR, reminders, settings, food/insulin/undo, database schema, startup, and polling unchanged.
- Kept `build_saved_sugar_text()` in `bot.py` because it is still shared by shortcuts, `/sugar` value processing, and OCR save flows.

## 1.2.1 - 2026-06-12

- Phase 2 `bot.py` refactor: moved profile handlers into `handlers/profile.py`.
- Moved info/OCR status handlers into `handlers/info.py`: `/formula`, `/ocr`, and `/ocrlog`.
- Extracted shared FSM command-state clearing into `common/fsm.py`.
- Preserved command behavior for `/setname`, `/whoami`, `/formula`, `/ocr`, and `/ocrlog`.
- Did not touch OCR photo intake, OCR callbacks, manual OCR flow, reminders, database schema, food/insulin/undo flows, startup, or polling.

## 1.2.0 - 2026-06-12

- Phase 1 `bot.py` refactor: moved low-risk system handlers into `handlers/system.py`.
- Extracted shared `with_version()` and `build_story_url()` into `common/text.py`.
- Preserved command behavior for `/start`, `/help`, `/version`, `/health`, `/story`, `/backup`, and unknown slash commands.
- Kept `bot.py` as the composition root for `Bot`, `Dispatcher`, handler registration, startup, polling, version sync, and reminder loop startup.
- Did not touch OCR, reminders, database schema, protocol parsing, food/insulin/undo flows, or production deployment.

## 1.1.1 - 2026-06-11

- UX cleanup release without new product scope, LLM, PostgreSQL, or architecture changes.
- Reworked `/help` into short task-based groups while keeping the full command surface visible.
- Clarified the difference between `/health`, `/status`, and `/reminders`.
- Made `/undo` safer to read by showing the exact record type, value, time, and deletion consequence before confirmation.
- Shortened OCR low-confidence and no-result messages into a consistent "result/action" structure.
- Simplified user-facing prompts for `/food`, `/insulin`, and `/settings`.

## 1.1.0 - 2026-06-11

- Stabilized the daily family-use surface without adding LLM or production deployment work.
- Added `/undo` for deleting the latest mistaken glucose or insulin record with explicit confirmation.
- Made `USUGAR_OCR_ENABLED=false` stop heavy OCR work and return a clear user message instead of running recognition anyway.
- Updated OCR and reminder user-facing messages so Russian text is consistent and readable.
- `/settings` now shows the current protocol before editing and opens `settings.html` with saved values prefilled.
- Added `FUTURE_BACKLOG.md` to keep deferred OCR, architecture, database, search, backup, trusted-contact, LLM, deployment, and administration work separate from the stable 1.1.0 scope.

## 1.0.39 - 2026-06-11

- Added `story.html`, an interactive project-history landing page served by the local WebApp server.
- Added `scripts/generate_story_data.py` to build `docs/history/story_data.js` from `docs/history/SCREENSHOT_STORY.md`.
- Added Telegram command `/story` to open the project-story page.
- Updated docs and tests for the story landing assets.

## 1.0.38 - 2026-06-06

- Made `settings.html` more mobile-friendly with compact base fields and collapsible advanced sections.
- Added a WebApp layout regression test for compact mobile controls.
- Added `PROJECT_AUDIT.md` to track current capabilities, debts, placeholders, and UX issues.
- Added `DATA_SOURCES.md` to track Tidepool, OpenAPS, DIAX, and Nightscout as candidate reference sources.

## 1.0.37 - 2026-06-06

- Added one-time follow-up reminders after logged short insulin.
- Added `followup_reminders` SQLite storage with duplicate protection and sent-state tracking.
- Added WebApp protocol settings for enabling follow-ups and changing the follow-up delay.
- Added reminder, protocol, and database tests for the follow-up flow.

## 1.0.36 - 2026-06-06

- Added `scripts/create_stable_archive.ps1` for local stable rollback ZIPs.
- Added `STABLE_ARCHIVES.md` with archive and restore instructions.
- Added stable archive documentation to the runbook and docs index.
- Created a local stable archive and git tag for `v1.0.35`.

## 1.0.35 - 2026-06-06

- Added trusted-contact Telegram ID support in WebApp reminder settings.
- Background reminder loop can now send one-time trusted alerts after a long missing-measurement window.
- `/whoami` now shows the Telegram ID that can be used as a trusted contact.
- Added tests for trusted alert keys, text, WebApp fields, and protocol persistence.

## 1.0.34 - 2026-06-05

- Added reminder controls to the Telegram WebApp settings page.
- Moved WebApp protocol parsing into `usugar_protocol.py`.
- Stored reminder settings inside each user's protocol and used them in `/reminders` plus the background reminder loop.
- Added tests for protocol reminder settings and WebApp reminder controls.

## 1.0.33 - 2026-06-05

- Added basal insulin reminder state checks based on the configured protocol time.
- `/reminders` now reports both measurement and basal insulin reminder status.
- Background reminders can now send one-time basal-before and basal-overdue messages.
- Added database and reminder tests for last long-insulin logs and basal reminder timing.

## 1.0.32 - 2026-06-05

- Added main-keyboard buttons for OCR and reminders.
- Synchronized `/start`, `/help`, README, and known-command coverage with the current command surface.
- Added a static command-surface test so command/help/documentation drift is caught by tests.

## 1.0.31 - 2026-06-05

- Added background measurement reminders.
- Added `reminder_events` database records so each reminder stage is sent only once.
- Added `.env` controls for enabling reminders and choosing the check interval.
- Added tests for reminder delivery keys and reminder event persistence.

## 1.0.30 - 2026-06-05

- Added a measurement reminder state engine.
- Added `/reminders` to inspect current reminder status without enabling background notifications yet.
- Reminder logic supports the 08:00-22:00 measurement window, hourly expected checks, 5/10 minute reminder states, and future trusted-person alerts.

## 1.0.29 - 2026-06-05

- Added the first bot brain layer for glucose reactions after saved values.
- In-range values now get supportive feedback.
- Low/high values get protocol-aware follow-up prompts, with high correction calculated from the configured formula.
- Near basal time, the bot reminds the user to check whether long insulin was done.
- Added `BOT_BRAIN.md` for reminder and trusted-person roadmap.

## 1.0.28 - 2026-06-05

- Libre2 CV OCR now extracts trend-arrow direction and a basic graph trend summary.
- OCR replies include available Libre2 metadata such as `arrow=right` and `graph=rising`.
- Small timestamp OCR is explicitly marked as not reliable yet until a dedicated small-text OCR path is added.

## 1.0.27 - 2026-06-05

- OCR confirmation now uses inline buttons under the OCR message instead of replacing the main Telegram keyboard.
- Manual glucose input is offered only when OCR is uncertain, missing, or the user explicitly asks for it.
- Added `.env` settings for optional EasyOCR and OCR engine timeout control.

## 1.0.26 - 2026-06-05

- Added a Telegram polling retry loop so the bot process survives VPN or Telegram API outages.
- Startup network failures now wait and retry instead of terminating the bot process.

## 1.0.25 - 2026-06-05

- Disabled EasyOCR by default after confirming it crashes inside PyTorch on this Windows runtime and slows Telegram photo replies.
- A single confident `libre2_cv` result is now treated as a medium-confidence candidate while still requiring the user to press the save button.
- Telegram OCR replies should now be fast for standard Libre2 screenshots even without Tesseract installed.

## 1.0.24 - 2026-06-05

- Added a Libre2-specific computer vision OCR fallback that reads large glucose digits from the green top panel.
- The Libre2 fallback uses OpenCV connected components and local digit templates, so it works without Tesseract or neural OCR.
- EasyOCR now runs in a subprocess so a PyTorch crash cannot stop the main Telegram bot.
- Confirmed the saved Telegram screenshot with `8.2` is recognized locally as `8.2`.

## 1.0.23 - 2026-06-05

- Added a safe OCR module for Telegram screenshots.
- The first OCR pass supports Tesseract on original, contrast, top-crop, and center-crop image variants.
- OCR details are stored with each photo attempt so disagreements and engine errors are visible later.
- The bot still asks for confirmation before saving a recognized glucose value.

## 1.0.22 - 2026-06-05

- Added OCR photo confirmation workflow.
- Telegram photos are downloaded into ignored `.runtime/ocr_images/` for local processing.
- Caption-derived glucose candidates can be confirmed by the user and saved as sugar records.
- If no reliable value is found, the bot asks for manual glucose input instead of calculating silently.

## 1.0.21 - 2026-06-05

- Added OCR/photo intake metadata to `/backup` exports as `ocr_attempts.csv`.
- Backup export hides Telegram `file_id` values while preserving timestamp, chat type, caption, and status.

## 1.0.20 - 2026-06-05

- Added `/ocrlog` command to inspect recent OCR/photo intake attempts.
- OCR intake log output hides Telegram `file_id` values.
- Updated Telegram smoke test and runbook for OCR intake diagnostics.

## 1.0.19 - 2026-06-05

- Added `ocr_attempts` SQLite table.
- Telegram photo intake now stores OCR attempt metadata without downloading or storing image files locally.
- Added database test coverage for OCR attempt persistence.

## 1.0.18 - 2026-06-05

- Added testable OCR result aggregation rules.
- Close OCR candidates can be accepted, but disagreement or empty recognition requires manual confirmation.
- Documented the OCR safety rule before wiring real recognizer engines.

## 1.0.17 - 2026-06-05

- Added `/ocr` command for current Libre2 screenshot intake status.
- Added safe Telegram photo intake that acknowledges images without extracting glucose or calculating doses yet.
- Added `USUGAR_OCR_ENABLED=false` feature flag and tests for photo-intake rules.
- Updated runbook, roadmap, and Telegram smoke test.

## 1.0.16 - 2026-06-05

- Added a fallback response for unknown Telegram slash commands.
- Unknown commands now clear active dialog state and reply with `/help` guidance plus the running bot version.

## 1.0.15 - 2026-06-05

- Added a visible `uSugarBot v...` footer to key diagnostic command responses.
- Updated `/help`, `/formula`, `/settings`, and `/backup` so Telegram testing shows which bot version answered.

## 1.0.14 - 2026-06-05

- Fixed base commands getting swallowed while a dialog/FSM flow was active.
- Moved `/formula` into the early base-command block so journal prompts cannot intercept it.
- Restored the `/formula` button text and fallback protocol message after a local encoding-safe refactor.

## 1.0.13 - 2026-06-05

- Extracted Telegram `/formula` text generation into `usugar_logic.build_formula_reference`.
- Added tests for formula reference text, protocol values, additions, basal settings, and example calculation.
- Reduced the size of the `/formula` handler in `bot.py`.

## 1.0.12 - 2026-06-05

- Added Telegram `/backup` command for private ZIP export.
- Backup ZIP includes `manifest.json`, `protocol.json`, `glucose_log.csv`, and `insulin_log.csv`.
- Added tests for backup archive contents and filename generation.
- Updated runbook, smoke test, roadmap, and README.

## 1.0.11 - 2026-06-05

- Added Telegram `/health` command with runtime status and no secrets.
- Updated smoke-test and runbook docs for the new command.
- Added screenshot-history rules for safe Telegram captures.

## 1.0.10 - 2026-06-04

- Health check now recognizes the managed settings runtime on port `8001`.
- Added `scripts/sync_version.py`.
- `scripts/bump_version.py` now syncs the visible settings page version and appends new versions to `VERSION.json` history.

## 1.0.9 - 2026-06-04

- Moved current version into `VERSION.json`.
- Added `version_info.py` as the single runtime version reader.
- Added managed runtime scripts, process screenshots, and testable dose logic.
- Updated `settings.html` to show the current project version.

## 1.0.8

- Приоритет FSM.
- Фильтр имени.
- 911/help flow.
- Мульти-числа.
- Модульность.

## 1.0.7

- Глобальный HI/LOW.
- Уточнение чисел.
- Поле имени в веб-форме.
- Улучшенная реакция.

## 1.0.6

- Уколы.
- HI/LOW.
- Умная реакция.

## 1.0.5

- Динамический пример.
- Единые аббревиатуры.
- Русские расшифровки.
- Улучшенная памятка.

## 1.0.4

- Конфигурация по умолчанию.
- Приветствие новичка.
- Анонимность.
- `/whoami`.
- `webapp_url` из `.env`.

## 1.0.3

- Шаг ручки.
- Округление.
- Памятка формул.
- Аббревиатуры.

## 1.0.2

- Раздельные таблицы.
- Журнал замеров.
- CSV-экспорт.
- Дублирование команд.

## 1.0.1

- Первая версия с базой данных SQLite.

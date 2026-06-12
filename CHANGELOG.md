# Changelog

Version history was originally kept as comments in `config.py`. It is now preserved here and in `VERSION.json`.

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

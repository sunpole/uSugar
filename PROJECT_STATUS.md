# uSugar Project Status

Date: 2026-06-23

## Current State

uSugar is an early-stage Telegram bot project for family diabetes support. The local computer version is currently more advanced than GitHub for code: it contains the active Telegram bot, local OCR helpers, reminder logic, tests, `settings.html`, and a local SQLite database. The GitHub snapshot is still valuable as public documentation and project story, but the local working copy is now the implementation source of truth.

The current working code identifies itself as version `1.6.3` through `VERSION.json` and `version_info.py`. The bot has moved beyond the initial cleanup stage into a stable daily family-use MVP: OCR intake, source-aware Libre2/glucometer OCR metadata, glucose feedback, measurement reminders, basal insulin reminder checks, trusted-contact alerts, post-short-insulin follow-up reminders, mobile settings cleanup, project audit documentation, an interactive project-story page, confirmed `/undo`, WebApp settings prefill, the first UX cleanup pass, the full 1.2.x `bot.py` handler/runtime split, the 1.3.0 composition-root cleanup, the 1.3.1 large-file audit, the 1.3.2 local runtime/OCR verification pass, the 1.3.3 product requirements capture, the 1.4.x Settings/WebApp/safety/family-mode releases, the 1.5.x OCR and smart-input releases, the 1.6.0 Family Group Launch + Alias Fix release, the 1.6.1 Group Whoami + Trusted Topic hotfix, the 1.6.2 Trusted Family Thread Mode, and the 1.6.3 Settings WebApp hotfix are now implemented and tested.

## 1.6.3 Settings WebApp Hotfix

- The visible main-menu “Настройки” button no longer embeds a static WebApp URL that can go stale inside Telegram.
- Tapping “Настройки” now routes through the same settings handler as `/settings`, which sends a fresh Mini App button with current saved protocol data.
- The WebApp shows a visible send-status message and warns if opened outside Telegram Mini App, because ordinary browser pages cannot call `Telegram.WebApp.sendData`.
- Trusted-contact and trusted-family-group settings are saved through the existing protocol JSON and confirmed back to the user after the bot receives the payload.
- Confirmation masks private IDs in chat output while preserving full local values for runtime matching.

## 1.6.2 Trusted Family Thread Mode

- A configured Telegram group/topic can now be used as a trusted family thread for daily entries while ordinary groups remain blocked.
- Settings WebApp stores `family_group.enabled`, `chat_id`, `thread_id`, `patient_name`, and `accept_any_member` inside the existing protocol JSON.
- Family-thread records resolve to the configured patient profile instead of the Telegram sender, so family members do not create separate medical diaries by accident.
- Group `/settings` now shows safe setup instructions and current group/thread IDs instead of sending a WebApp button.
- OCR in a trusted family thread still requires explicit confirmation before saving.
- Source metadata for glucose/insulin rows is documented as deferred because adding it cleanly requires a separate schema change.

## Comparison Summary

- GitHub is documentation-first and cleaner.
- Local is code-first and newer for implementation.
- Local had many manual backup copies and legacy setup scripts; these were moved to `_archive/2026-06-04_initial_cleanup`.
- `LICENSE` was copied from GitHub into the local project.
- The active local runtime files are now the root `bot.py`, `config.py`, `db.py`, `keyboards.py`, `settings.html`, `.env`, `usugar.db`, the helper modules `usugar_logic.py`, `usugar_export.py`, `usugar_ocr.py`, `usugar_vision.py`, `usugar_brain.py`, `usugar_reminders.py`, `usugar_protocol.py`, the handler modules `handlers/system.py`, `handlers/profile.py`, `handlers/info.py`, `handlers/glucose.py`, `handlers/settings.py`, `handlers/logs.py`, `handlers/therapy.py`, `handlers/ocr.py`, the runtime modules `runtime/reminders.py`, `runtime/startup.py`, and the common modules `common/text.py`, `common/fsm.py`, `common/chat.py`.

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

Manual input helpers and food-dose calculation logic were moved into `usugar_logic.py` and covered by unit tests in `tests/test_usugar_logic.py`. OCR aggregation, backup export, version handling, database persistence, command-surface sync, glucose feedback, protocol parsing, reminder state, follow-up reminder storage, settings prefill, undo deletion, the system/profile/info/glucose/settings/logs/therapy/OCR/reminders split phases, and the startup version-sync helper now also have tests or command-surface coverage.

`health_check.py` now also checks that `ngrok` is available before a WebApp tunnel test.

The static settings page was smoke-tested locally through `http://127.0.0.1:8001/settings.html`; it returned HTTP 200.

Live Telegram Web testing confirmed earlier versions. Historical screenshots are indexed in `docs/history/SCREENSHOT_STORY.md`; public-history screenshots are redacted where Telegram values should not be shown. Version `1.0.39` added `story.html`, an interactive page generated from that screenshot history. Version `1.1.0` focused on stability rather than large new features; version `1.1.1` kept the same scope and cleaned up the daily Telegram UX. Version `1.2.0` started the safe `bot.py` split by moving only system handlers; version `1.2.1` continued it with profile and info/OCR-status handlers; version `1.2.2` moved the basic `/sugar` starter and `/status` into `handlers/glucose.py`; version `1.2.3` moved the remaining glucose text flow into the same module; version `1.2.4` moved `/settings` and WebApp data handling into `handlers/settings.py`; version `1.2.5` moved `/log` and journal CSV export into `handlers/logs.py`; version `1.2.6` moved `/food`, `/insulin`, `/undo`, and therapy FSM handlers into `handlers/therapy.py`; version `1.2.7` moved OCR photo intake, callbacks, confirmation, and manual value flow into `handlers/ocr.py`; version `1.2.8` moved `/reminders`, due-reminder delivery, and the reminder background loop into `runtime/reminders.py`; version `1.3.0` moved the startup-only settings-page version sync into `runtime/startup.py` and left `bot.py` as the composition root.

## Current Implementation Focus

- Keep OCR fast and safe: Libre2 CV remains the current best local path, but `OCR_REALITY_REPORT.md` shows it is still partial rather than production-grade. Version `1.5.0` added source-aware metadata plus lightweight branches for updated narrow Libre screenshots and manual-glucometer photos. Version `1.5.1` added safety guards after live sample smoke: old Libre2 candidates take priority over noisy experimental branches, camera-photo-like frames no longer run through Libre OCR, and ambiguous glucometer digit strings remain manual/no-result. Version `1.5.2` added a repeatable `scripts/ocr_smoke.py` audit over `img/simple`, `img/simple_new`, and `img/simple_gluk`. Version `1.5.3` tuned `libre2_narrow_updated` for real 1280x576 landscape Libre2 screenshots. Version `1.5.4` tuned only `glucometer_photo`: `img/simple_gluk` improved from 19 no-result frames to 9, while accepted stays 0 and all glucometer candidates remain manual-confirmation only. Version `1.5.5` adds a per-user OCR mode selector (`auto`, old Libre2, new Libre2, glucometer) without changing the manual-confirmation save policy. Tesseract is optional and EasyOCR remains disabled by default on this Windows runtime.
- Keep `USUGAR_OCR_ENABLED=false` as a real safety switch: disabled OCR accepts the photo metadata but does not download or run recognition.
- To enable local Libre2 OCR testing, set `USUGAR_OCR_ENABLED=true` in `.env` and restart the bot runtime; keep `.env` uncommitted. Version `1.3.2` verified this path in the live local runtime and confirmed that `.env` must be UTF-8 without BOM for `python-dotenv` to read `BOT_TOKEN`.
- Keep reminders useful but non-spammy: reminder delivery keys prevent duplicate background messages.
- Keep reminder configuration in the WebApp so the user can tune windows, basal reminders, trusted-contact alerts, and short-insulin follow-ups without editing `.env`.
- Keep mistaken-record deletion narrow: `/undo` deletes only the current user's latest sugar or insulin record after confirmation.
- Keep UX surfaces short and distinct: `/health` is technical runtime status, `/status` is the user's latest glucose record, and `/reminders` is schedule/reminder state.
- Treat `/settings` as the preferred path for editing the display name and protocol; `/setname` remains a legacy fallback.
- Smart text inputs like `8.4 сахар`, `сахр 8.4`, `50 40 еда`, `3 4 5 20 70 80 еда`, `3 укол`, and `укло 3` are routed without relying on Telegram buttons. Ambiguous messages ask whether the user means sugar, food, or insulin.
- Common navigation can now be typed in Russian without slash commands: `помощь`, `команды`, `настройки`, `статус`, `журнал`, `оцр`, `журнал оцр`, `напоминания`, `формула`, `версия`, `здоровье`, `кто я`, `бэкап`, and `история`.
- Version `1.6.0` moved these aliases into an early router so `настройки` and `журнал` are handled before the friendly fallback replies.
- `/ocr` now opens the OCR screen without also changing the OCR mode; only explicit mode words (`авто`, `старый либре`, `новый либре`, `глюкометр`) change the stored mode.
- `/help` is now a short daily guide, while `/commands` and `/devhelp` hold the full technical/legacy command list.
- The current OCR mode is visible next to the bot version in `/version`, `/help`, `/commands`, `/status`, and `/health`.
- Food correction uses `glucose_fresh_minutes` from the protocol, defaulting to 60 minutes; older glucose values trigger a fresh-measurement prompt instead of hidden correction.
- Safety flows now support typed fallbacks when Telegram buttons are hidden: `/undo`, OCR confirmation, and smart-food confirmation can be controlled with short text commands while keeping explicit confirmation before saving or deletion.
- Telegram family mode now keeps medical records private by default. In groups/supergroups, `/start`, `/version`, `/help`, `/health`, `/whoami`, `/reminders`, and `/trustedtest` are considered safe; medical entry commands and OCR explain that they belong in private chat. Channel commands receive a neutral notice because channels are future notification/announcement surfaces rather than the main daily dialog.
- `/whoami` now shows `user_id`, `chat_id`, `chat_type`, and `message_thread_id` when Telegram provides it; `/trustedtest` can test a group connection without sending medical data.
- Use `TELEGRAM_BOT_SETUP.md` when configuring BotFather identity, username, description/about text, avatar, command list, privacy mode, and token-rotation timing.
- Use `CURRENT_STATE.md` for the short operational map after a pause.
- Use `MANUAL_TELEGRAM_TEST_PLAN.md` when a release needs Telegram smoke; Codex should run code/log/test checks itself, then ask the user for a short manual command list instead of fighting Chrome focus.
- Use `OPEN_WORK.md` for the current unfinished-work inventory and `RELEASE_PLAN.md` for the next milestone order.
- Use `PATCH_NOTIFICATION_RULES.md` before preparing a uNews patch note. Since `1.4.4`, every closed uSugar release must create a safe `news/` patch note and pass the uNews dry-run/check; normal real publication belongs to the GitHub-first uNews workflow after docs/news reach the public repository. The active local uNews path is `C:\!CODE_CLUB\new 2026\004_uNews`; the documented Telegram channel is `@uNewsLog` and the publishing bot is `@uNewsDev_bot`.
- Use `UNEWS_PUBLISHING_AUDIT.md` when uSugar news does not appear in `@uNewsLog`. For 1.5.0 and 1.5.1, the patch notes and images were present and valid, but the `sunpole/uNews` GitHub Actions publisher failed with Telegram `Unauthorized`; the GitHub secret for the uNews publishing bot must be updated there, not in uSugar.
- Use `PROJECT_AUDIT.md`, `LARGE_FILES_AUDIT.md`, and `DATA_SOURCES.md` to keep debts, future split points, placeholders, and external-source decisions visible.
- Use `PRODUCT_REQUIREMENTS.md` as the product direction source before starting the next implementation phase.
- Use `FUTURE_BACKLOG.md` for deferred work that should not distract from stable 1.1.x maintenance.
- Keep `story.html` generated from `SCREENSHOT_STORY.md` so the public development history grows automatically with new screenshots.
- Continue keeping `bot.py` as the composition root. After `1.3.0`, system handlers live in `handlers/system.py`, profile handlers in `handlers/profile.py`, formula/OCR-status handlers in `handlers/info.py`, glucose command/text handlers live in `handlers/glucose.py`, settings/WebApp handlers live in `handlers/settings.py`, log handlers live in `handlers/logs.py`, therapy handlers live in `handlers/therapy.py`, OCR photo/callback/manual flows live in `handlers/ocr.py`, reminder command/background runtime lives in `runtime/reminders.py`, and startup version sync lives in `runtime/startup.py`; database schema, medical logic, polling, and production deployment remain unchanged.
- GitHub Pages public Actions logs for run `27433398807` showed build/artifact success and a failed deploy job with the generic message "Deployment failed, try again later"; no local workflow file exists in this repository snapshot, so this is not currently diagnosed as a `story_data.js` or static build error.

## 1.3.2 Runtime Verification Notes

- Managed runtime was restarted with OCR enabled locally and reported `uSugarBot v1.3.2`.
- Telegram Web showed real OCR responses for Libre2 screenshots through the local `libre2_cv_top_panel` path; Tesseract was still unavailable locally, as expected.
- During live OCR smoke, Telegram Web focus/keyboard automation accidentally saved test glucose records. They were removed from `usugar.db`, and the database counts returned to the pre-smoke state.
- Full command smoke should still be treated as partly manual in this desktop session because Telegram Web focus can move to reply-keyboard buttons instead of the message input.

## 1.3.3 Product Requirements Notes

- `PRODUCT_REQUIREMENTS.md` now captures the current product direction without implementing new behavior.
- The next implementation phase should prioritize Settings WebApp name/prefill polish, smart dialog input, safer food calculation context, OCR source expansion planning, and trusted-contact consent/verification design.
- DeepSeek/LLM, product database work, printable A4 doctor diary, and Telegram token rotation remain deferred until later phases.

## 1.4.0 Settings And Food Notes

- Settings WebApp now prefills user name and protocol more robustly, including local fallback name storage and Telegram WebApp user fallback.
- The WebApp includes food-calculation safety parameters: `glucose_fresh_minutes` and `dose_reduction_percent`.
- The direct smart-food path calculates carb sums without writing to `glucose_log`, `insulin_log`, or OCR tables.

## 1.4.1 UX Safety Notes

- `/undo` now tells the user what to type if reply buttons are hidden and accepts text selection for sugar or insulin deletion.
- OCR confirmation accepts typed actions: `сохранить`, `ввести вручную`, `не сохранять`, and `отмена`.
- Smart-food calculations accept typed actions after `50+40` or `50 40`; food saving remains intentionally disabled until a food journal exists.

## 1.4.2 Telegram Family Mode Notes

- `TELEGRAM_BOT_SETUP.md` documents official BotFather setup and explains why the current local token should not be rotated until the final production/public-launch phase.
- `common/chat.py` centralizes chat type detection and private/group/channel safety text.
- Private chat remains the daily medical workflow. Groups are limited to safe coordination commands. Channels are documented as future one-way announcement/notification surfaces.
- `/whoami` now exposes the IDs needed to configure family groups and trusted contacts.

## 1.4.4 Open Work And uNews Notes

- `OPEN_WORK.md` now lists the main unfinished tasks by area, status, priority, and target release.
- `RELEASE_PLAN.md` defines the next planned milestone order: `1.5.0` OCR new sources, `1.5.1` food log, `1.5.2` smart daily dialog, `1.6.0` trusted contact MVP, `1.6.1` A4 doctor diary, `1.7.0` product database, and `1.8.0` DeepSeek.
- uNews patch publication is now a release gate after a closed release, not only a manual optional note.
- The `1.4.4` patch note lives under `news/` and uses a safe documentation screenshot rather than Telegram data.
- `/trustedtest` sends only a connection-check message to the configured trusted contact target.

## 1.5.0 OCR New Sources Notes

- OCR engine results now carry source-aware metadata for `libre2_cv_old`, `libre2_narrow_updated`, and `glucometer_photo`.
- The old Libre2 CV path remains active and is still the preferred local source for known Libre screenshots.
- Updated narrow Libre screenshots and manual-glucometer photos have initial lightweight CV branches covered by synthetic tests, but real-world Telegram smoke should still use explicit manual confirmation.
- `/ocr` reports active OCR sources, and `/ocrlog` reports source, confidence, and saved/not-saved status without exposing Telegram `file_id` values.
- No database schema migration, automatic OCR save, medical dose-logic change, token change, or production deployment was added.

## 1.5.1 OCR Safety Hotfix Notes

- Live Telegram smoke with user-approved samples showed that new experimental OCR branches could disagree with old Libre2 CV or trigger on glucometer photos.
- The hotfix keeps old Libre2 CV as the preferred source when it is present and treats far-away new-source candidates as disagreement instead of averaging them.
- Libre-specific OCR paths now skip camera-photo-like shapes, reducing the risk that a glucometer photo is interpreted as a Libre screenshot.
- Ambiguous glucometer digit strings are rejected as low-confidence/no-result rather than shown as a confident value.
- The manual-check OCR message was made readable for the no-primary-value disagreement path.
- Test glucose records created during this learning smoke remain in the local database by user request; they were not deleted as part of the release.

## 1.4.3 Current State And Patch Notes

- `CURRENT_STATE.md` summarizes what works, what is partial, and what is not implemented.
- `MANUAL_TELEGRAM_TEST_PLAN.md` makes manual Telegram smoke the default when Codex cannot reliably type into Telegram Web.
- `PATCH_NOTIFICATION_RULES.md` connects uSugar release reporting to the existing uNews workflow in `C:\!CODE_CLUB\new 2026\004_uNews`.
- No medical logic, database schema, OCR behavior, or Telegram handler behavior was intentionally changed in this release.

## 1.5.9 Russian Command Aliases + Help Split Notes

- Restarted the local runtime and confirmed the previous bot process was serving `v1.5.8` before the patch.
- Removed one local test glucose row with value `8.4` that was created during manual smoke.
- Added Russian text aliases for common navigation commands so daily use is not dependent on slash commands or reply buttons.
- Split help into short `/help` and full `/commands`/`/devhelp`.
- Added the current OCR mode to key user-visible status/version surfaces.
- No medical formula, OCR algorithm, database schema, food_log, DeepSeek, trusted-contact policy, token, production deployment, or local Telegram publication work was added.

## 1.6.0 Family Group Launch + Alias Fix Notes

- Fixed alias routing order with an early text-alias handler so navigation words no longer fall into random friendly replies.
- Fixed OCR mode UX so `/ocr` and `OCR` show the OCR screen, while only mode words change the stored mode.
- Group `/start` now explains safe family use, group `/help` lists safe commands, `/whoami` includes `message_thread_id`, and `/trustedtest` works in groups without medical data.
- Medical records, OCR saves, logs, settings, and backup remain private-only by default.
- New Libre2 QA is recorded through local OCR smoke; further recognition improvements remain separate OCR patches.
- No Food Log, DeepSeek, BJU/product database, A4 doctor diary, medical formula change, OCR autosave, token change, or local Telegram publication work was added.

## 1.5.8 Remove Legacy Daily Buttons Notes

- The main reply keyboard is navigation-only: Settings, Status, Log, OCR, Reminders, Formula, Help.
- Legacy daily-entry buttons Name, Sugar, Food, and Insulin are not exposed as main menu buttons.
- Compatibility commands `/sugar`, `/food`, `/insulin`, and legacy `/setname` remain available.
- `/help` now shows the preferred smart text examples in one place: `8.4 сахар`, `3 укол`, `50 40 еда`, `новый либре`, `глюкометр`, and `отмена`.
- No food_log table, OCR tuning, medical formula, trusted-contact logic, token, production deployment, or local Telegram publication work was added.

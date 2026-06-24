# uSugar Runbook

## Run Next To uChurch

Use separate ports and separate folders:

- uChurch API: `3000`
- uChurch Vite client: `5173`
- uSugar settings page: `8001`

uSugar is a Telegram bot plus a static `settings.html` page. It does not need the uChurch server.

## Configure

Create or update `.env`:

```env
BOT_TOKEN=your_telegram_token_from_BotFather
USUGAR_WEB_PORT=8001
WEBAPP_URL=http://localhost:8001/settings.html
USUGAR_DB_PATH=usugar.db
USUGAR_OCR_ENABLED=false
```

For public Telegram WebApp access, replace `WEBAPP_URL` with the HTTPS URL provided by a tunnel or deployment. Keep the local port different from uChurch.

## Start

Start both the settings page and the bot:

```bat
start_all.bat
```

Start only the settings page:

```bat
start_settings.bat
```

Start only the Telegram bot:

```bat
run_bot.bat
```

Start ngrok for the settings page:

```bat
start_ngrok.bat
```

Managed runtime option:

```powershell
powershell -ExecutionPolicy Bypass -File scripts\start_runtime.ps1
powershell -ExecutionPolicy Bypass -File scripts\status_runtime.ps1
powershell -ExecutionPolicy Bypass -File scripts\stop_runtime.ps1
```

Start settings, ngrok, and bot together:

```powershell
powershell -ExecutionPolicy Bypass -File scripts\start_runtime.ps1 -WithBot
```

## Project Story Page

The local WebApp server also serves the interactive project history:

```text
http://localhost:8001/story.html
```

Telegram command:

```text
/story
```

The page reads `docs/history/story_data.js`, which is generated from `docs/history/SCREENSHOT_STORY.md`:

```powershell
venv\Scripts\python.exe scripts\generate_story_data.py
```

After adding a new screenshot row to `SCREENSHOT_STORY.md`, rerun the generator or bump the version, because `scripts\sync_version.py` refreshes the story data.

## Verify Without Running The Bot

```bat
venv\Scripts\python.exe -m py_compile bot.py config.py db.py keyboards.py
```

Check whether important ports are occupied:

```bat
netstat -ano | findstr ":3000 :5173 :8001"
```

Run the local health check:

```bat
venv\Scripts\python.exe health_check.py
```

The check does not print the bot token. It only shows whether the token is set.

When the bot is running, Telegram also has a lightweight runtime check:

```text
/health
```

This command shows the bot version, database file name, settings-page configuration status, and a reminder that secrets are hidden. It does not print the Telegram token, `.env` contents, full database path, or current ngrok URL.

Use `/status` for the current user's latest glucose record. Use `/reminders` for measurement, basal, and follow-up reminder state. This separation is intentional since `1.1.1`.

## Telegram Smoke Testing

Codex can run code checks, inspect runtime logs, and read screenshots, but direct Telegram Web typing is unreliable in this Windows desktop session. Use `MANUAL_TELEGRAM_TEST_PLAN.md` for release smoke.

Default private smoke commands:

```text
/version
/start
/help
/health
/status
/whoami
/trustedtest
```

Menu/smart-input smoke after `1.5.8`:

```text
/version
/start
/help
8.4 сахар
отмена
3 укол
отмена
50 40 еда
отмена
```

The visible reply keyboard should show only navigation buttons: Settings, Status, Log, OCR, Reminders, Formula, Help. The old Name/Sugar/Food/Insulin daily-entry buttons should not be visible, although `/sugar`, `/food`, `/insulin`, and `/setname` remain compatibility commands.

Russian aliases/help smoke after `1.5.9`:

```text
/version
помощь
команды
настройки
журнал
версия
OCR
новый либре
/ocr
50 40 еда
отмена
```

Expected behavior: `/help` is short, `команды` shows the full technical/legacy list, OCR mode appears near the version/status footer, and old Name/Sugar/Food/Insulin buttons do not return.

Trusted family thread smoke after `1.6.2`:

```text
Private chat:
/version
/settings
enable family_group
set family_group_chat_id
set family_group_thread_id if the group uses topics
set family_group_patient_name

Configured family thread:
/version
/help
/whoami
настройки
статус
журнал
8.4 сахар
/undo
3 укол
отмена
50 40 еда
отмена
```

Expected behavior: the configured thread accepts family entries for the selected patient profile; another group/topic remains blocked; `/settings` in a group shows safe setup instructions and never sends the WebApp keyboard; OCR still requires confirmation before saving.

Do not publish real group `chat_id`, `message_thread_id`, Telegram user IDs, `.env`, tokens, screenshots with IDs, or medical values in public docs/news/history.

Process:

1. Codex runs `py_compile` and unit tests.
2. Codex starts or checks the runtime.
3. The user sends the smoke commands manually in Telegram.
4. Codex reads the result from Telegram Web, screenshots, and `.runtime/logs`.
5. Codex reports the smoke as confirmed, partially confirmed, or blocked.

Do not spend a long time fighting Chrome focus. If manual smoke is needed, ask for the command list once and wait for the user's confirmation.

## Patch Notes For uNews

uSugar release notes can be prepared for the local uNews workflow.

Current local uNews path:

```text
C:\!CODE_CLUB\new 2026\004_uNews
```

Documented Telegram endpoints:

- channel: `@uNewsLog`
- bot: `@uNewsDev_bot`

Before preparing a public patch note, read `PATCH_NOTIFICATION_RULES.md`. Since `1.4.4`, uNews publication is a release gate after a closed uSugar release: create a project-local patch note in `news/`, include a safe image, run the uNews dry-run/check, and publish documentation/news to GitHub through the docs-only path so GitHub-first auto-discovery can publish it. Local uNews commands are for checks, credential diagnostics, and debugging; normal real Telegram publication should happen through the uNews GitHub Actions workflow after the note lands in the public repository. If the image is missing, YAML is invalid, secrets/private data are detected, or dry-run fails, stop and report instead of publishing.

## Settings WebApp

Use `/settings` in Telegram before changing the protocol. Since `1.1.0`, the bot replies with the current saved protocol and a WebApp URL that includes the current values for prefill. Since `1.1.1`, the message explicitly asks the user to review current values before editing. `settings.html` also keeps the last submitted protocol in local browser storage as a fallback for local testing.

Since `1.6.3`, the main Telegram “Настройки” button is a plain text navigation button. It triggers the same handler as `/settings`, and that handler sends a fresh Telegram Mini App button with the current protocol embedded in the URL. This avoids stale WebApp URLs that Telegram can keep in old reply keyboards.

Since `1.6.4`, menu labels with emoji are normalized before alias lookup. If a visible reply-keyboard button such as `⚙️ Настройки`, `📒 Журнал`, or `📊 Статус` starts falling through to a random friendly reply, check `usugar_aliases.normalize_alias_text()` and `BOT_STABILITY_AUDIT_1_6_4.md` first.

Since `1.6.5`, the saveable Settings WebApp is opened from a fresh reply-keyboard `KeyboardButton(web_app=...)`, not from an inline WebApp button. This matters because `Telegram.WebApp.sendData()` returns settings to the bot through the reply WebApp button flow. An inline WebApp button or direct ngrok/browser URL can open the page, but may not deliver saved settings back to the bot.

Settings smoke:

1. In private chat, send `/settings` or press `Настройки`.
2. Tap the freshly sent reply-keyboard button `⚙️ Открыть настройки`.
3. Confirm the form is prefilled with the current name/protocol.
4. Fill or edit trusted-contact and family-group fields.
5. Tap `Сохранить протокол`.
6. Wait for the bot reply `Настройки применены и сохранены`.
7. Confirm the reply shows trusted-contact state and family-group state.

If the page says it was opened outside Telegram Mini App, reopen it from the button in the `/settings` message. A normal browser page cannot send `Telegram.WebApp.sendData` back to the bot.

If Telegram Web shows the free ngrok warning page, click `Visit Site` once or use Telegram Desktop/mobile for the save smoke. The warning is an ngrok interstitial and does not mean the settings server or SQLite save failed.

## Family Group Thread Lock

Since `1.6.6`, if a group is configured in Settings as a family group, uSugar works only in the configured `message_thread_id`.

- Messages in other topics of the same configured group are intercepted before normal command handlers.
- Ordinary non-command messages outside the allowed topic are ignored silently to avoid spam.
- Explicit slash commands outside the allowed topic get one short safety notice telling the user to move to the configured family topic or change Settings in private chat.
- Use `/whoami` inside the intended topic to confirm `chat_id`, `chat_type`, and `message_thread_id`.

## Undo Mistaken Records

Use `/undo` to delete the latest mistaken entry. The bot shows the last glucose record and the last insulin record for the current Telegram user, asks which one to delete, and requires explicit confirmation before deletion. Since `1.1.1`, the confirmation text repeats the exact type, value, time, and consequence before the user presses delete.

The command only deletes rows that match the current `user_id`; it does not edit older records or delete other users' data.

## Stable Archives

After a stable milestone, create a local rollback archive:

```powershell
powershell -ExecutionPolicy Bypass -File scripts\create_stable_archive.ps1
```

Archives are saved under `_archive/stable_releases/` and are ignored by git. They include tracked code and docs only; secrets, databases, virtual environments, runtime logs, and private archives are excluded. See `STABLE_ARCHIVES.md` for restore instructions.

## Backup From Telegram

Use this in a private chat with the bot:

```text
/backup
```

The bot sends a ZIP file with:

- `manifest.json`
- `protocol.json`
- `glucose_log.csv`
- `insulin_log.csv`
- `ocr_attempts.csv`

The backup does not include the Telegram token, `.env`, ngrok URL, local paths, Telegram `file_id` values, or the full SQLite database file. It exports only the current Telegram user's protocol, log records, and OCR/photo intake metadata.

## OCR / Photo Intake

Current development status:

```text
/ocr
```

The bot can accept a Telegram photo/screenshot and answer with a safe intake message. OCR is controlled by `USUGAR_OCR_ENABLED` and is disabled by default.

Since `1.1.0`, `USUGAR_OCR_ENABLED=false` is a real feature flag: the bot records a lightweight `ocr_disabled` attempt, does not download the image into `.runtime/ocr_images/`, and does not run Tesseract, EasyOCR, or Libre2 CV. The user sees a Russian message explaining that OCR is off and that the glucose value should be entered manually.

When `USUGAR_OCR_ENABLED=true`, the existing OCR behavior is preserved: the bot downloads the image locally for processing, runs the enabled recognition paths, aggregates candidates, and asks for confirmation before saving.

The OCR safety rule is already defined in code: multiple recognizers must produce close glucose candidates before an automatic value can be accepted. If recognizers disagree, or no value is recognized, the bot must ask for manual confirmation instead of saving or calculating silently.

Photo intake stores OCR attempt metadata in SQLite (`telegram_file_id`, chat type, caption, status, timestamp). It does not download or store the image file locally.

Use `/ocrlog` in Telegram to inspect the latest OCR/photo intake metadata for the current user. The log output does not show Telegram `file_id` values.

During the current scaffold stage, Telegram photo files are downloaded into `.runtime/ocr_images/` for local processing and remain ignored by git. If the photo caption contains a glucose-like value, the bot can ask for confirmation and save it as a sugar record only after the user confirms. Without a reliable extracted value, the bot asks for manual input.

Version 1.0.23 adds the first image OCR pass. The bot runs Tesseract on several variants of the same screenshot: original, grayscale/contrast, top crop, and center crop. If Tesseract is not installed as a Windows executable, the bot still starts and records an OCR engine note in the attempt metadata. Install Tesseract separately, then restart the bot so `pytesseract` can find `tesseract.exe`.

Version 1.0.24 adds a local Libre2 computer vision fallback. It detects large dark components on the green Libre2 top panel, compares digit shapes with local Windows font templates, reconstructs decimal values such as `8.2`, and still asks the user to confirm before saving. EasyOCR is isolated in a subprocess because on this Windows runtime PyTorch can crash with exit code `3221225620`; this must not stop the Telegram bot.

Version 1.0.25 keeps EasyOCR disabled by default because the subprocess still takes too long before returning the PyTorch crash code. The fast production path is now `libre2_cv` plus optional Tesseract if `tesseract.exe` is installed.

Version 1.0.26 keeps the Telegram bot process alive across VPN or Telegram API outages. If `api.telegram.org` is temporarily unreachable, the bot logs the network error, waits 10 seconds, and retries polling.

Version 1.0.27 changes OCR confirmation to inline buttons so the main Telegram keyboard stays available after a recognized screenshot. Manual OCR input is shown only for uncertain/no-result cases or when the user explicitly taps the manual button. Heavy OCR engines are controlled by `.env`: keep `USUGAR_OCR_EASYOCR_ENABLED=false` during normal testing and use `USUGAR_OCR_ENGINE_TIMEOUT_SECONDS=8` or lower when debugging slow engines.

Version 1.0.28 adds local Libre2 metadata extraction. The current reliable fields are the main glucose value, the large trend arrow direction, and a basic graph trend computed from the visible chart line. Small text such as the app timestamp and bottom axis labels is not treated as reliable until Tesseract or another small-text OCR path is stable.

Version 1.0.29 adds the first behavior layer after saved glucose values. The bot now responds differently for in-range, low, and high values and can remind near configured basal time. See `BOT_BRAIN.md` for the reminder roadmap.

Version 1.0.30 adds the reminder state engine and `/reminders` status command. This does not send background reminder messages yet; it computes when a measurement is expected, whether the first or second reminder is due, and when a future trusted-person alert would be triggered.

Version 1.0.31 enables background measurement reminders. The bot checks known users every `USUGAR_REMINDER_CHECK_SECONDS` seconds, sends the first and second missed-measurement reminders once per expected measurement, and records delivery in `reminder_events`. Use `USUGAR_REMINDERS_ENABLED=false` in `.env` to disable the background loop during debugging.

Version 1.0.33 adds basal insulin reminder checks. `/reminders` now reports both measurement and basal status. The background loop can send a one-time reminder shortly before the configured long-insulin time and a one-time overdue check after the scheduled time if no `long` insulin log is found.

Version 1.0.34 stores reminder settings inside the user's protocol from the WebApp. The settings page now controls measurement reminders, reminder windows, basal reminder timing, and the trusted-alert threshold. Trusted-person delivery still needs a contact flow before it can notify another chat.

Version 1.0.35 adds trusted-contact delivery by Telegram ID. The trusted person can send `/whoami` to the bot to see their Telegram ID, then that ID can be saved in the user's WebApp reminder settings. When the long missing-measurement threshold is reached, the bot sends one trusted alert and stores the delivery key so it does not repeat.

Version 1.0.37 adds one-time follow-up reminders after logged short insulin. The WebApp reminder section controls whether this is enabled and how many minutes after the short-insulin log the control message is sent. The default is 120 minutes. Follow-ups are stored in `followup_reminders`, marked as sent after delivery, and are separate from hourly measurement reminders.

Version 1.1.0 stabilizes daily-use behavior: OCR disabled mode skips heavy recognition, reminder/OCR user messages are readable in Russian, `/undo` can remove the latest mistaken sugar or insulin entry with confirmation, and `/settings` shows/prefills the current protocol before editing.

Version 1.1.1 is a UX cleanup release: `/help` is grouped by task, `/health`/`/status`/`/reminders` are explained as different screens, `/undo` is harder to misread, and low-confidence OCR replies use shorter result/action wording.

Version 1.5.0 adds source-aware OCR paths without changing the database schema or automatic-save policy. Engine results now identify `libre2_cv_old`, `libre2_narrow_updated`, and `glucometer_photo` inside the existing OCR JSON metadata. `/ocr` lists active sources, and `/ocrlog` shows source, confidence, and saved/not-saved status without showing Telegram `file_id` values. The old Libre2 CV path remains the most reliable local source; the narrow Libre and glucometer-photo branches are initial lightweight CV support and should be treated as manual-confirmation candidates during real-world smoke.

Version 1.5.1 is an OCR safety hotfix after live sample smoke. If old Libre2 CV and experimental branches disagree, do not average unrelated values into a confident answer. Libre-specific OCR branches should skip camera-photo-like glucometer frames, and ambiguous glucometer digit strings should stay manual/no-result. During this learning phase, user-approved test records may remain in the local database if the user explicitly asks not to delete them.

Version 1.5.2 adds a local OCR reality smoke script and audit reports. Use these commands to measure OCR without writing to SQLite or creating glucose records:

```powershell
venv\Scripts\python.exe scripts\ocr_smoke.py --folder img/simple
venv\Scripts\python.exe scripts\ocr_smoke.py --folder img/simple_new
venv\Scripts\python.exe scripts\ocr_smoke.py --folder img/simple_gluk
```

Read `OCR_REALITY_REPORT.md` before claiming that an OCR source works reliably. The current measured state is: old Libre2 is partial, updated narrow Libre2 is mostly failed/partial, and glucometer photos are low-confidence/manual. Read `UNEWS_PUBLISHING_AUDIT.md` if uSugar news is pending but not visible in `@uNewsLog`; 1.5.0 and 1.5.1 were blocked by the uNews GitHub Actions publisher receiving Telegram `Unauthorized`, not by uSugar patch-note content.

Version 1.5.3 tunes only the updated Libre2 landscape path. Real `img/simple_new` samples are `1280x576`; the current value is usually in the right-side ROI. Expected smoke counts after 1.5.3:

- `img/simple_new`: 1 accepted, 8 manual, 9 no_result.
- `img/simple`: 4 accepted, 7 manual, 0 no_result.
- `img/simple_gluk`: 0 accepted, 8 manual, 19 no_result.

New `libre2_narrow_updated` candidates should still be treated as manual confirmation by default. Do not use 1.5.3 as a reason to autosave OCR values.

Version 1.5.4 tunes only the manual-glucometer photo path. Real `img/simple_gluk` photos contain small displays, blur, glare, tilt, and textile/background noise. Expected smoke counts after 1.5.4:

- `img/simple_gluk`: 0 accepted, 18 manual, 9 no_result.
- `img/simple`: 4 accepted, 7 manual, 0 no_result.
- `img/simple_new`: 1 accepted, 9 manual, 8 no_result.

All `glucometer_photo` candidates must stay manual-confirmation only. Do not use 1.5.4 as a reason to autosave OCR values or to treat glucometer photos as production-grade recognition.

Version 1.5.5 adds OCR mode selection and smart text shortcuts. The selected OCR mode is stored in the existing user protocol JSON as `ocr_mode` and can be changed with `/ocr`, Settings WebApp, or typed phrases:

- `авто`
- `старый либре`
- `новый либре`
- `глюкометр`

Daily text shortcuts are intentionally explicit. Examples:

- `8.4 сахар` or `сахр 8.4` records sugar.
- `50 40 еда` calculates food/carbs without saving a food log.
- `3 укол` asks for short/long insulin before saving.
- `укло 3` is treated as an insulin typo and still asks for type.

If a message mixes meanings, the bot should ask whether it is sugar, food, or insulin. OCR still requires explicit confirmation before saving.

Version 1.6.1 fixes the family group `/whoami` hotfix and adds optional trusted topic routing. Smoke checklist:

Private chat:

- `/version`
- `настройки`
- `журнал`
- `OCR`
- `новый либре`
- `/ocr`
- `50 40 еда`
- `отмена`

Family test group:

- `/start`
- `/help`
- `/whoami`
- `/trustedtest`
- `8.4 сахар`
- optional OCR test image with a clear OCR/sugar caption

Expected group behavior:

- `/start` explains that the group is for safe coordination and diagnostics.
- `/whoami` shows `user_id`, `chat_id`, `chat_type`, and `message_thread_id` if Telegram provides it.
- `/trustedtest` replies in the group without medical data.
- Medical text input and OCR photo handling ask the user to continue in private chat and do not save records.
- If a group uses topics, copy `message_thread_id` from `/whoami` into Settings WebApp only when trusted alerts should go to that exact topic.

See `TELEGRAM_FAMILY_GROUP_SETUP.md` for setup steps.

## Important Caution

Do not use archived setup scripts as launch scripts. They are kept only for history. One legacy script contained a real Telegram token, so regenerate the token before publishing or pushing code.

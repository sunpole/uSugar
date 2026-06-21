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
/help
/health
/status
/whoami
/trustedtest
```

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

The main keyboard WebApp button still uses the static `WEBAPP_URL` from `.env`; for the most reliable prefill during testing, open settings through `/settings`.

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

## Important Caution

Do not use archived setup scripts as launch scripts. They are kept only for history. One legacy script contained a real Telegram token, so regenerate the token before publishing or pushing code.

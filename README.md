# uSugar

uSugar is an early-stage Telegram bot for family diabetes support. The project is currently being assembled into a clean local working version before active feature development continues.

Current local code version: `1.5.9`

## What This Project Is Now

The current local version contains:

- Telegram bot entry point: `bot.py`
- configuration and `.env` loading: `config.py`
- SQLite helpers: `db.py`
- Telegram keyboard helpers: `keyboards.py`
- testable calculation and parsing helpers: `usugar_logic.py`
- in-memory ZIP backup builder: `usugar_export.py`
- local OCR helpers for Libre2 screenshots: `usugar_ocr.py`, `usugar_vision.py`
- first behavior layer for glucose feedback: `usugar_brain.py`
- reminder state and background reminder helpers: `usugar_reminders.py`
- protocol parsing and reminder settings helpers: `usugar_protocol.py`
- one-time follow-up reminders after logged short insulin
- `/undo` flow for deleting the last mistaken glucose or insulin entry with confirmation
- static settings page: `settings.html`
- WebApp settings prefill from the current saved protocol
- interactive project story page: `story.html`
- story data generator: `scripts/generate_story_data.py`
- project audit and external data-source notes: `PROJECT_AUDIT.md`, `DATA_SOURCES.md`
- local SQLite database: `usugar.db` (ignored by git)
- local secrets: `.env` (ignored by git)

GitHub currently contains stronger public documentation than code. A snapshot of that GitHub documentation is stored locally in `docs/github_original/`.

## Safe Local Ports

This project is designed to run beside the local `uChurch` project.

Known ports:

- `uChurch` API: `3000`
- `uChurch` Vite client: `5173`
- `uSugar` settings page: `8001`
- `ngrok` local inspection UI: usually `4040`

Do not run uSugar on `3000` or `5173` while uChurch is running.

## Configure

Create or update `.env`:

```env
BOT_TOKEN=your_telegram_token_from_BotFather
USUGAR_WEB_PORT=8001
WEBAPP_URL=http://localhost:8001/settings.html
USUGAR_DB_PATH=usugar.db
USUGAR_OCR_ENABLED=false
USUGAR_OCR_EASYOCR_ENABLED=false
USUGAR_OCR_ENGINE_TIMEOUT_SECONDS=8
USUGAR_REMINDERS_ENABLED=true
USUGAR_REMINDER_CHECK_SECONDS=60
```

Keep real secrets only in `.env`. Do not put real tokens into `.bat`, `.md`, screenshots, or GitHub.

## Start Locally

Start the settings web page and the bot:

```bat
start_all.bat
```

Start only the settings web page:

```bat
start_settings.bat
```

Start only the bot:

```bat
run_bot.bat
```

For Telegram WebApp testing through ngrok:

```bat
start_ngrok.bat
```

Then set `WEBAPP_URL` in `.env` to the HTTPS ngrok URL plus `/settings.html`.

## Verify Without Starting The Bot

```bat
venv\Scripts\python.exe -m py_compile bot.py config.py db.py keyboards.py
```

Check ports:

```bat
netstat -ano | findstr ":3000 :5173 :8001 :4040"
```

Run the project health check:

```bat
venv\Scripts\python.exe health_check.py
```

It checks required files, `.env`, local ports, and whether `ngrok` is available.

Run logic tests:

```bat
venv\Scripts\python.exe -m unittest discover -s tests -v
```

## Daily Telegram Commands

Short daily help is available through `/help` or `помощь`.

Full technical and legacy command list:

```text
/commands
/devhelp
команды
```

Common Russian aliases without slash:

- `настройки` -> `/settings`
- `статус` -> `/status`
- `журнал` -> `/log`
- `оцр`, `распознавание`, `фото сахара` -> `/ocr`
- `журнал оцр` -> `/ocrlog`
- `напоминания` -> `/reminders`
- `формула` -> `/formula`
- `версия` -> `/version`
- `здоровье`, `проверка` -> `/health`
- `кто я` -> `/whoami`
- `резервная копия`, `бэкап` -> `/backup`
- `история` -> `/story`

Compatibility commands remain available: `/sugar`, `/food`, `/insulin`, and legacy `/setname`.

## Documentation

Start here:

- `DOCS_INDEX.md` - documentation map
- `PROJECT_STATUS.md` - current project status
- `RUNBOOK.md` - local run instructions
- `NGROK_AND_UCHURCH.md` - ngrok and coexistence with uChurch
- `SECRETS.md` - handling `.env` and tokens
- `ROADMAP.md` - planned phases
- `GITHUB_PUBLISH_PLAN.md` - safe GitHub publishing plan
- `VERSIONING.md` - version file and bumping rules
- `STABLE_ARCHIVES.md` - local stable archives and restore rules
- `CHANGELOG.md` - preserved version history

Historical and imported docs:

- `docs/github_original/` - GitHub documentation snapshot
- `docs/legacy_local/` - older local notes and planning documents
- `docs/history/` - screenshots and state snapshots
- `docs/history/SCREENSHOT_STORY.md` - screenshot captions and future story/landing-page plan
- `story.html` - interactive visual story generated from the screenshot history

## Current Development Direction

The next development goal is to stabilize a real MVP:

1. Keep daily family-use flows stable.
2. Split the large `bot.py` into modules.
3. Continue improving OCR metadata such as screenshot timestamp and graph context.
4. Add backup restore/import only after the current export flow is stable.
5. Use `FUTURE_BACKLOG.md` for deferred OCR, LLM, deployment, and administration work.

Useful Telegram commands:

- `/sugar` - record a glucose value
- `/status` - show the latest glucose value
- `/food` - calculate a food dose from carbohydrates
- `/insulin short 4.5` or `/insulin long 18` - record an insulin injection
- `/log` - show glucose or insulin history
- `/formula` - current protocol and calculation reference
- `/settings` - show the current protocol and a prefilled WebApp settings URL
- `/undo` - delete the last mistaken glucose or insulin record after confirmation
- `/setname` - set or change the user display name
- `/whoami` - show user_id, chat_id, and chat_type for family setup
- `/health` - runtime status without secrets
- `/backup` - ZIP export with protocol and logs
- `/ocr` - OCR/photo intake status
- `/ocrlog` - recent OCR/photo attempts
- `/reminders` - current measurement reminder state
- `/trustedtest` - safe trusted-contact delivery test without medical data
- `/story` - interactive history of how uSugar is being created
- `/version` - running bot version

In family groups, use only safe commands: `/version`, `/help`, `/health`, `/whoami`, and `/reminders`. Glucose, food, insulin, OCR, settings, logs, backup, and deletion stay in private chat by default.

## Safety Note

This bot can help record values and calculate based on configured formulas, but it must not replace medical judgment or advice from a doctor. Any dose-related feature needs explicit confirmation and careful testing.

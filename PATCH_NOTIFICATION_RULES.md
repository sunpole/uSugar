# Patch Notification Rules

Date: 2026-06-21
Version: 1.4.3

This file records how uSugar should work with the local uNews / patch-note workflow.

## Found Workflow

The active local uNews project is:

```text
C:\!CODE_CLUB\new 2026\004_uNews
```

Important files:

- `C:\!CODE_CLUB\new 2026\004_uNews\README.md`
- `C:\!CODE_CLUB\new 2026\004_uNews\AGENTS.md`
- `C:\!CODE_CLUB\new 2026\004_uNews\scripts\publish-from-projects.js`
- `C:\!CODE_CLUB\new 2026\004_uNews\data\published.json`

Telegram endpoints documented there:

- Channel: `@uNewsLog`
- Bot: `@uNewsDev_bot`

uNews expects every project to keep patch notes in its own `news/` folder. The uNews publisher reads those files and can publish them to Telegram.

## uSugar Rule

Every meaningful uSugar release should have a short patch note suitable for uNews.

The patch note should include:

- uSugar version;
- release title;
- release type, usually `patch`, `docs`, `feature`, `bugfix`, or `release`;
- local commit hash;
- stable archive path;
- test result summary;
- Telegram smoke status;
- GitHub docs-only publication status;
- screenshot path if a safe public screenshot exists;
- short human-readable summary for Telegram;
- clear note that code was not published to GitHub when that is true.

## File Location

Preferred local uSugar location:

```text
news/YYYY-MM-DD-usugar-vX-Y-Z-short-title.md
```

If an image is safe to publish, place it next to the patch note:

```text
news/YYYY-MM-DD-usugar-vX-Y-Z-short-title.png
```

Do not use Cyrillic characters or spaces in patch-note filenames.

## YAML Template

```yaml
---
type: patch
project: uSugar
series: usugar
title: Current state and patch workflow
version: 1.4.3
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-21-usugar-v1-4-3-current-state.png
---
```

If no safe image exists yet, do not claim that the patch note is ready for Telegram publication. Use a TODO line in the patch note instead.

## Body Template

```markdown
uSugar 1.4.3 documents the current project state and defines the release smoke workflow.

What changed:
- current working features were summarized;
- manual Telegram smoke testing rules were documented;
- uNews patch-note integration was connected to `C:\!CODE_CLUB\new 2026\004_uNews`;
- no medical logic or runtime behavior was changed.

Checks:
- py_compile: OK
- unit tests: OK
- Telegram smoke: pending/manual/confirmed

Release data:
- local commit: ...
- stable archive: ...
- GitHub docs-only: ...

Short text for Telegram:
uSugar 1.4.3 фиксирует карту текущего состояния проекта и правила ручной Telegram-проверки после релизов. Кодовая логика не менялась.
```

## uNews Local Check

From `C:\!CODE_CLUB\new 2026\004_uNews`, use dry-run validation before publication:

```powershell
npm run publish:projects:check -- "..\002_usugar\news\YYYY-MM-DD-usugar-vX-Y-Z-short-title.md"
```

Real publication should be a separate explicit user-approved step:

```powershell
npm run publish:projects -- "..\002_usugar\news\YYYY-MM-DD-usugar-vX-Y-Z-short-title.md"
```

## Safety

- Never put `.env`, tokens, database contents, Telegram file IDs, private chat IDs, ngrok URLs, or real medical values into patch notes.
- Prefer public screenshots from `docs/history/` that have already been redacted.
- Do not publish code to GitHub as part of uNews notification.
- Do not send a patch note to Telegram until tests and archive status are known.
- Do not invent release claims. Describe only what was actually changed and checked.

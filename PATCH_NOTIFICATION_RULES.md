# Patch Notification Rules

Date: 2026-06-21
Version: 1.4.4

This file records how uSugar works with the local uNews / patch-note workflow.

## Found Workflow

The active local uNews project is:

```text
C:\!CODE_CLUB\new 2026\004_uNews
```

Important files:

- `C:\!CODE_CLUB\new 2026\004_uNews\README.md`
- `C:\!CODE_CLUB\new 2026\004_uNews\AGENTS.md`
- `C:\!CODE_CLUB\new 2026\004_uNews\package.json`
- `C:\!CODE_CLUB\new 2026\004_uNews\scripts\publish-from-projects.js`
- `C:\!CODE_CLUB\new 2026\004_uNews\data\published.json`

Telegram endpoints documented there:

- Channel: `@uNewsLog`
- Bot: `@uNewsDev_bot`

uNews expects every project to keep patch notes in its own `news/` folder. The local publisher reads those files and can publish them to Telegram.

The GitHub-side uNews workflow also supports automatic discovery. `projects.json` uses `mode: auto-discover-public-repositories`, scans public repositories under `sunpole`, reads their `news/` folders, and publishes new patch notes through `scripts/publish-all-news.js` / `.github/workflows/publish-all-news.yml`. Therefore, every uSugar release note must be valid both for the local one-file publisher and for the repository-level auto-discovery path.

## Required uSugar Release Rule

Every closed uSugar release must have a short patch note suitable for uNews.

After each release is closed, Codex must:

1. Create a project-local patch note under `news/`.
2. Use `project: uSugar`.
3. Use `series: usugar`.
4. Set the exact release `version`.
5. Describe only real completed changes.
6. Exclude secrets, tokens, `.env`, database contents, Telegram file IDs, private chat/user IDs, ngrok URLs, and real medical data.
7. Add a safe screenshot/image.
8. Run the uNews dry-run/check command.
9. Publish documentation/news to GitHub through the docs-only path so the `news/` folder is available to the uNews auto-discovery workflow.
10. If the check is green and safety checks pass, publish to `@uNewsLog` without another confirmation prompt.
11. If YAML is invalid, the image is missing, secrets/private data are found, or dry-run/check fails, do not publish; stop with a report.

## Patch Note Contents

The patch note should include:

- uSugar version;
- release title;
- release type, usually `patch`, `docs`, `feature`, `bugfix`, or `release`;
- test result summary when known;
- Telegram smoke status when known;
- GitHub docs-only publication status when relevant;
- screenshot path if a safe public screenshot exists;
- short human-readable summary for Telegram;
- clear note that code was not published to GitHub when that is true.

## File Location

Preferred local uSugar location:

```text
news/YYYY-MM-DD-usugar-vX-Y-Z-short-title.md
```

Place the image next to the patch note:

```text
news/YYYY-MM-DD-usugar-vX-Y-Z-short-title.png
```

Do not use Cyrillic characters or spaces in patch-note filenames.

## YAML Template

```yaml
---
type: docs
project: uSugar
series: usugar
title: Карта незавершённых задач и uNewsLog workflow
version: 1.4.4
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-21-usugar-v1-4-4-open-work-unews.png
---
```

Current `C:\!CODE_CLUB\new 2026\004_uNews` scripts require referenced image files to exist. There is no working automatic fallback cover in the local script path. If no safe image exists yet, do not publish the patch note.

## Body Template

```markdown
uSugar 1.4.4 documents the open-work inventory and makes uNewsLog patch publication a release gate.

Что сделано:
— незавершённые задачи сгруппированы по продуктовым направлениям;
— релизы распланированы от 1.5.0 до 1.8.0;
— публикация патчноута в uNewsLog стала обязательной после закрытия релиза, если проверки зелёные;
— медицинская логика и OCR-алгоритмы не менялись.

Проверки:
— py_compile: OK
— unit tests: OK
— uNews dry-run: OK
— Telegram smoke: pending/manual/confirmed

Короткий текст для Telegram:
uSugar 1.4.4 фиксирует карту незавершённых задач и вводит обязательную публикацию патчноутов через uNewsLog после закрытия релизов. Медицинская логика не менялась.
```

## uNews Local Check

From `C:\!CODE_CLUB\new 2026\004_uNews`, use dry-run validation before publication:

```powershell
npm run publish:projects:check -- "..\002_usugar\news\YYYY-MM-DD-usugar-vX-Y-Z-short-title.md"
```

The broader GitHub auto-discovery check is:

```powershell
npm run publish:all:check
```

It scans configured public repositories and their `news/` folders. It only sees uSugar news after the docs-only GitHub publication puts the patch note and image into the repository.

Real publication after a closed uSugar release is mandatory when dry-run/check and safety checks are green:

```powershell
npm run publish:projects -- "..\002_usugar\news\YYYY-MM-DD-usugar-vX-Y-Z-short-title.md"
```

## Safety

- Never put `.env`, tokens, database contents, Telegram file IDs, private chat IDs, ngrok URLs, or real medical values into patch notes.
- Prefer newly captured documentation screenshots or public screenshots from `docs/history/` that have already been redacted.
- Do not publish code to GitHub as part of uNews notification.
- Do not send a patch note to Telegram until tests and archive status are known.
- Do not invent release claims. Describe only what was actually changed and checked.
- If the image is missing, YAML is invalid, dry-run/check fails, or safety is uncertain, do not publish; report the blocker.

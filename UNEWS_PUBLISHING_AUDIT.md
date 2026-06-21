# uNews Publishing Audit

Date: 2026-06-21
Release: uSugar 1.5.2

This audit explains why the uSugar 1.5.0 and 1.5.1 patch notes did not appear in `@uNewsLog`.

## Result

The uSugar news files are valid and visible to uNews auto-discovery, but the GitHub Actions publisher in `sunpole/uNews` failed while sending the first pending post to Telegram.

Root cause: the GitHub Actions Telegram bot secret for `sunpole/uNews` is invalid, revoked, missing, or belongs to the wrong bot. The failing Telegram API result is `Unauthorized`.

## Checked Patch Notes

| Version | Markdown | PNG | YAML/link/image | uNews check | GitHub file | Published |
|---|---|---|---|---|---|---|
| 1.5.0 | `news/2026-06-21-usugar-v1-5-0-ocr-new-sources.md` | present | OK | `sendPhoto` | present on `sunpole/uSugar@main` | no |
| 1.5.1 | `news/2026-06-21-usugar-v1-5-1-ocr-safety-hotfix.md` | present | OK | `sendPhoto` | present on `sunpole/uSugar@main` | no |

## uNews Registry

`C:\!CODE_CLUB\new 2026\004_uNews\data\published.json` contains the uSugar 1.4.4 patch note, but does not contain:

- `sunpole/uSugar|main|news/2026-06-21-usugar-v1-5-0-ocr-new-sources.md`
- `sunpole/uSugar|main|news/2026-06-21-usugar-v1-5-1-ocr-safety-hotfix.md`

## Local uNews Check

Local dry-run/check sees both patch notes as pending and valid:

- `sunpole/uSugar|main|news/2026-06-21-usugar-v1-5-0-ocr-new-sources.md via sendPhoto`
- `sunpole/uSugar|main|news/2026-06-21-usugar-v1-5-1-ocr-safety-hotfix.md via sendPhoto`

Both captions include the required repo link and hashtags:

- `#uSugar`
- `#тыСахар`
- `#uNews`
- `#Sunpole`

No local Telegram publication was performed for this audit.

## GitHub Actions Evidence

Repository: `sunpole/uNews`

Failing run:

- Run ID: `27911927742`
- Workflow: `Publish all project news`
- Event: `schedule`
- Created: `2026-06-21T17:24:31Z`
- Status: completed failure

Relevant failed log lines:

```text
New patchnotes found: 1. This run limit: 1.
Publishing: sunpole/uSugar|main|news/2026-06-21-usugar-v1-5-0-ocr-new-sources.md via sendPhoto
Telegram sendPhoto failed: Unauthorized
```

Because the workflow processes pending news in order, the 1.5.0 failure prevented the 1.5.1 patch note from being published.

## What To Fix

Update the GitHub repository secret in `sunpole/uNews`:

- the Telegram bot token secret must be the current valid token for the uNews publishing bot;
- `TELEGRAM_CHANNEL_ID` should still target `@uNewsLog`;
- after updating the secret, rerun the `Publish all project news` workflow with real publishing enabled or wait for the scheduled run.

Do not fix this by local publishing. Local publishing is intentionally not the normal workflow.

## Safety Notes

- No tokens were printed or stored in this report.
- `.env` was not inspected for values and must not be committed.
- The uSugar bot token is unrelated and must not be changed for this issue.
- The uNews patch YAML is not the source of this failure.


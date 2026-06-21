# Manual Telegram Test Plan

Date: 2026-06-21
Version: 1.4.3

Codex can edit code, read logs, run tests, and inspect screenshots. In this Windows desktop session, direct Telegram Web typing from Codex is unreliable because focus can land on reply buttons or browser UI instead of the message input.

This file defines the stable manual smoke process for uSugar releases.

## Rule

Codex should not spend a long time fighting Telegram Web focus.

For each release:

1. Codex makes code and documentation changes.
2. Codex runs `py_compile`.
3. Codex runs unit tests.
4. Codex starts or checks the runtime.
5. Codex gives the user a short list of Telegram commands.
6. The user sends the commands manually in Telegram Web or the Telegram app.
7. Codex reads the result from visible Telegram Web, screenshots, and `.runtime/logs`.
8. Codex reports whether Telegram smoke is confirmed, partially confirmed, or blocked by network/runtime issues.

## Standard Private Smoke

Send these in private chat with `uSugarBot`:

```text
/version
/help
/health
/status
/whoami
/trustedtest
```

Expected:

- `/version` shows the current release version.
- `/help` shows the current command surface.
- `/health` shows runtime, database, settings WebApp, and hidden secrets status.
- `/status` shows latest glucose status or a clear empty-state message.
- `/whoami` shows `user_id`, `chat_id`, and `chat_type`.
- `/trustedtest` either sends a safe test message to the configured trusted contact or explains that no trusted contact is configured.

## Optional Private Smoke

Use only when it is safe:

```text
50+40
отмена
/ocr
/ocrlog
/undo
отмена
```

Expected:

- `50+40` produces a food calculation and does not silently save food.
- `/ocr` reports OCR feature-flag status.
- `/ocrlog` shows recent OCR attempts without Telegram file IDs.
- `/undo` shows the latest deletable records and does not delete without confirmation.

## Optional Group Smoke

Use only in a test family group:

```text
/help
/whoami
/sugar
```

Expected:

- `/help` shows family/group-safe help.
- `/whoami` shows the group `chat_id` and `chat_type`.
- `/sugar` is blocked with a message explaining that medical records belong in private chat.

## Reading Results

Codex should use:

- Telegram Web screenshot;
- `.runtime/logs/bot.err.log`;
- `.runtime/logs/bot.out.log`;
- visible bot replies;
- database checks only when a test record may have been created.

## Safety

- Do not save real medical test values unless the user explicitly confirms.
- If a test record is created, remove it through `/undo` or document why it remains.
- Do not publish `.env`, tokens, ngrok URLs, database files, or Telegram file IDs.

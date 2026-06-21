# Telegram Bot Setup

Date: 2026-06-21
Version: 1.4.2

This guide describes the Telegram-side identity and family-mode setup for uSugar.

## BotFather Settings

Use Telegram `@BotFather` to configure the public bot identity.

Recommended BotFather items:

- `/setname` - visible bot name, for example `uSugar Family`.
- `/setuserpic` - bot avatar/photo. Use a calm family-safe icon; avoid medical documents or private screenshots.
- `/setdescription` - longer description shown on the bot profile page.
- `/setabouttext` - short about text shown in bot previews.
- `/setcommands` - command list shown by Telegram clients.
- `/setprivacy` - group privacy mode.

Suggested command list:

```text
start - start private setup
help - show help
version - show bot version
health - technical health check
whoami - show user_id, chat_id, and chat_type
reminders - show reminder status
settings - private protocol settings
sugar - private glucose entry
food - private food calculation
insulin - private insulin log
undo - private confirmed deletion
ocr - private Libre2 screenshot intake
ocrlog - private OCR log
trustedtest - test trusted-contact delivery
backup - private backup without secrets
```

## Token Policy

Do not rotate the Telegram token during local MVP work.

A new token should be issued only during the final production/public-launch phase, after secrets handling, deployment, backups, and access boundaries are ready.

## Private Chat

Private chat is the main daily-use mode.

Allowed in private chat:

- glucose entry;
- food calculation;
- insulin logging;
- OCR screenshot intake;
- settings WebApp;
- journal and backup;
- trusted-contact test.

## Family Group

Family groups are for safe coordination, not for entering medical records by default.

In group/supergroup chats uSugar allows only safe commands:

- `/version`;
- `/help`;
- `/health`;
- `/whoami`;
- `/reminders`.

If a user tries to enter sugar, food, insulin, OCR, settings, logs, backup, or deletion in a group, the bot should explain that medical records belong in private chat.

## Channel

A Telegram channel is not the main dialog.

Use a channel later only for one-way announcements or carefully designed notification delivery. Do not use a channel as the primary place for daily glucose, food, insulin, OCR, or settings interactions.

## Privacy Mode

For family groups, BotFather privacy mode should be selected deliberately.

Recommended while testing:

- keep privacy mode enabled unless group command behavior specifically needs broader message visibility;
- use explicit slash commands in groups;
- keep medical input in private chat.

If privacy mode is disabled later, the bot must still keep group writes safe and avoid saving medical records from group text.

## Trusted Contact IDs

Use `/whoami` to read:

- `user_id`;
- `chat_id`;
- `chat_type`.

The trusted contact target may be:

- a personal chat ID;
- a family group chat ID.

Use `/trustedtest` from private chat to send a safe test message to the configured trusted contact. The test message must not contain glucose, insulin, OCR, journal, or other medical data.

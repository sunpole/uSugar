# Telegram Family Group Setup

Version: 1.6.0

This guide explains how to add uSugarBot to a family Telegram group safely.

## What The Group Is For

Use a family group for coordination, diagnostics, and future trusted-contact notifications.

Keep daily medical records in the private chat with the bot:

- glucose values;
- food and carbohydrate calculations;
- insulin entries;
- OCR screenshots/photos;
- logs, backup, settings, and deletion.

By default, uSugar does not save medical records from groups.

## Add The Bot

1. Open the family Telegram group.
2. Add `@uSugarBot` as a member.
3. Keep normal member permissions. Admin rights are not required for the current safe-command mode.
4. If the group uses topics, open the topic where the bot should be tested.

## Verify IDs

Send in the group:

```text
/whoami
```

The bot should show:

- `user_id` - the Telegram user who sent the command;
- `chat_id` - the group id; this is the value that can later be used as `trusted_contact_id`;
- `chat_type` - usually `group` or `supergroup`;
- `message_thread_id` - only when Telegram provides a topic/thread id.

Do not post these IDs publicly. They are not passwords, but they are still operational identifiers.

## Safe Group Commands

These commands are safe in a group:

```text
/start
/help
/version
/health
/whoami
/reminders
/trustedtest
```

`/trustedtest` sends no medical data. It only confirms that the bot can answer in the current group and shows the group identifiers needed for setup.

## Private-Only Actions

These should be done in the private chat with the bot:

```text
8.4 сахар
50 40 еда
3 укол
/settings
/log
/ocr
/ocrlog
/undo
/backup
```

If someone writes a medical value in the group, the bot should ask to continue in private chat and must not save the record.

## Trusted Contact Later

The group `chat_id` can be placed into Settings WebApp as `trusted_contact_id`, but full trusted-contact policy is still future work:

- explicit consent;
- disable/revoke flow;
- quiet hours;
- alert escalation rules;
- clear text explaining what will be sent.

Until that is finished, do not treat group alerts as production-ready medical monitoring.

# Telegram Family Group Setup

Version: 1.6.1

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
3. Admin rights are not required for safe-command mode, but they are acceptable during testing.
4. If the group uses topics, open the topic where the bot should be tested.

## Verify IDs

Send in the group:

```text
/whoami
```

The bot should show:

- `user_id` - the Telegram user who sent the command;
- `chat_id` - the group id; this value can later be used as `trusted_contact_id`;
- `chat_type` - usually `group` or `supergroup`;
- `message_thread_id` - only when Telegram provides a topic/thread id.

Do not publish these IDs in docs, screenshots, news cards, or public history. They are not passwords, but they are operational identifiers.

## Configure Trusted Group Target

In Settings WebApp:

1. Put the group `chat_id` into **Telegram chat_id доверенного лица или группы**.
2. If the group uses topics and alerts should go into a specific topic, put `message_thread_id` into **message_thread_id ветки/топика группы**.
3. Leave the thread field empty if alerts may go to the general group chat.

Notes:

- Group `chat_id` can be negative, for example it can start with `-100`.
- `message_thread_id` is optional and only matters for Telegram groups with topics.
- Full trusted-contact consent, quiet hours, revocation, and escalation policy remain future work.

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

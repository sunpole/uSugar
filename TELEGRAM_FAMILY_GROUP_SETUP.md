# Telegram Family Group Setup

Version: 1.6.2

This guide explains how to add uSugarBot to a family Telegram group safely.

## What The Group Is For

Use a family group for coordination, diagnostics, trusted-contact notifications, and, if explicitly enabled, one trusted family thread for daily diary input.

By default, keep daily medical records in the private chat with the bot:

- glucose values;
- food and carbohydrate calculations;
- insulin entries;
- OCR screenshots/photos;
- logs, backup, settings, and deletion.

By default, uSugar does not save medical records from groups. Version `1.6.2` adds an opt-in trusted family thread: only the configured `chat_id` and optional `message_thread_id` may accept family entries.

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

## Configure Trusted Contact Target

In Settings WebApp:

1. Put the group `chat_id` into **Telegram chat_id доверенного лица или группы**.
2. If the group uses topics and alerts should go into a specific topic, put `message_thread_id` into **message_thread_id ветки/топика группы**.
3. Leave the thread field empty if alerts may go to the general group chat.

Notes:

- Group `chat_id` can be negative, for example it can start with `-100`.
- `message_thread_id` is optional and only matters for Telegram groups with topics.
- Full trusted-contact consent, quiet hours, revocation, and escalation policy remain future work.

## Configure Trusted Family Thread

In the private chat with the bot, open `/settings`, press the fresh **Открыть настройки** Mini App button, and fill the **Семейная группа** block. Do not use an old settings WebApp button from Telegram history; Telegram can keep stale URLs in old reply keyboards.

1. Enable **Включить семейную группу**.
2. Put the group `chat_id` into **chat_id группы**.
3. If the group uses topics and only one topic should accept diary entries, put `message_thread_id` into **message_thread_id ветки/топика**.
4. Put the patient/child name into **Имя пациента/ребёнка**.
5. Leave **Принимать записи от любого участника группы** enabled for normal family use.
6. Save the form and wait for the bot confirmation **Настройки применены и сохранены**. The confirmation should mention the trusted-contact and family-group state.

Important distinction:

- `trusted_contact_id` / `trusted_contact_thread_id` define where alerts are sent.
- `family_group.chat_id` / `family_group.thread_id` define where family diary entries are accepted from.

Do not publish real `chat_id`, `message_thread_id`, Telegram user IDs, screenshots with IDs, or medical values in public docs/news/history.

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

These should be done in the private chat with the bot unless the current group/topic was explicitly configured as the trusted family thread:

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

## Trusted Family Thread Actions

In the configured family thread, the bot can accept:

```text
8.4 сахар
50 40 еда
3 укол
/status
/log
/ocr
/ocrlog
/reminders
/formula
```

Records are attached to the configured patient profile, not to the Telegram account of the family member who typed the message.

OCR still asks for confirmation before saving. If a message is sent in another topic or another group, the bot must keep the safe private-chat behavior.

# Bot Stability Audit 1.6.4

Date: 2026-06-23

## Runtime Facts Before Hotfix

- Local code before the hotfix was `uSugarBot v1.6.3`.
- Local `HEAD` before the hotfix was `aa1f840` with tag `stable/v1.6.3`.
- Managed runtime was running:
  - settings PID: `7672`
  - ngrok PID: `3420`
  - bot PID: `8424`
- Settings page was served on local port `8001`.
- Latest runtime WebApp URL was `https://crib-silliness-bronzing.ngrok-free.dev/settings.html`.
- `bot.err.log` was empty.
- `bot.out.log` showed polling started for `@uSugarBot` and `uSugarBot v1.6.3`.

## Root Cause

The visible private-chat menu button `⚙️ Настройки` no longer carried a stale WebApp URL after `1.6.3`, which was correct. However, the text message produced by that button did not resolve as a command alias.

`register_alias_handlers()` was already registered before the glucose text catch-all, so handler order was not the primary problem. The problem was normalization: `usugar_aliases.normalize_alias_text()` lowercased and trimmed text but did not remove emoji/menu icons. As a result:

- `настройки` resolved to `/settings`;
- `Настройки` resolved to `/settings` because dictionary lookup lowercased it;
- `⚙️ Настройки` did not resolve to `/settings`;
- the message then reached the general text/fallback path and could produce an unrelated friendly reply.

## Routing Policy After Hotfix

Private chat settings entry points must all use the same flow:

- `/settings`
- `настройки`
- `Настройки`
- `⚙️ Настройки`

Expected behavior:

1. The bot refreshes the main reply keyboard.
2. The bot sends a fresh inline Mini App button `⚙️ Открыть настройки`.
3. The Mini App receives the current saved protocol in the URL payload.
4. After save, Telegram WebApp data returns to the bot.
5. The bot replies `Настройки применены и сохранены`.

Group/supergroup settings behavior must remain safe:

- no WebApp button in group/supergroup/channel;
- show instruction to open settings in private chat;
- show current `chat_id` and `message_thread_id` only in the live group reply, not in public docs/news/history.

## Command Routing Matrix

| Surface | Expected route |
|---|---|
| `/start` | start/help surface with safe keyboard policy |
| `/version`, `версия` | version |
| `/settings`, `настройки`, `Настройки`, `⚙️ Настройки` | settings flow |
| `/help`, `помощь`, `❓ Помощь` | short help |
| `/commands`, `команды` | full command list |
| `/status`, `статус`, `📊 Статус` | status |
| `/log`, `журнал`, `📒 Журнал` | log period selector |
| `/ocr`, `OCR`, `оцр`, `📷 OCR` | OCR status/mode selector |
| `/reminders`, `напоминания`, `⏰ Напоминания` | reminders status |
| `/formula`, `формула`, `📐 Формула` | formula reference |
| group `8.4 сахар` | blocked unless the group/topic is the configured trusted family thread |

## Regression Guards

- Alias normalization must remove menu emoji before lookup.
- Alias handlers must stay registered before glucose text/fallback handlers.
- The private main menu must not contain legacy daily-entry buttons: `Имя`, `Сахар`, `Еда`, `Укол`.
- Settings WebApp save confirmation must mention trusted-contact and family-group state.

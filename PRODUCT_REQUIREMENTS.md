# Product Requirements

Date: 2026-06-21
Version: 1.6.0

This document captures current product requirements and implementation status. Items marked done are implemented in the local runtime; future items still need separate work.

## Settings WebApp

- The user's display name should be editable through the Telegram WebApp settings form, not primarily through `/setname`.
- `/settings` should open a form prefilled with the current user data from the database.
- The prefill behavior must cover both the user's name and the saved protocol.
- The form should fit comfortably inside Telegram Mini App on a phone.
- The design should stay compact, practical, and easy to scan.

Status in `1.4.0`: implemented for the current WebApp path. `/setname` remains available as a legacy/fallback command.

Status in `1.5.5`: Settings WebApp also includes the preferred OCR mode selector. It stores `ocr_mode` in the existing protocol JSON without a schema migration.

Status in `1.6.3`: the visible “Настройки” menu button no longer stores a stale WebApp URL. It routes through `/settings`, which sends a fresh Telegram Mini App button with the current saved protocol, trusted-contact fields, and family-group fields. The WebApp shows send feedback, and the bot confirms saved trusted/family state after receiving the payload.

## Smart Dialog

- The main Telegram keyboard should stay minimal.
- The primary daily input should happen through a smart dialog rather than many separate buttons.
- The bot should understand numeric input and clarify whether the user means glucose, food, or insulin.
- Friendly fallback replies should remain for casual or random messages.
- The dialog should reduce user effort without hiding safety confirmations.

Status in `1.4.0`: partially implemented. Carb-sum inputs like `50+40` and `50 40` now trigger a direct food calculation without saving a record; single-number clarification remains available.

Status in `1.5.8`: the main reply keyboard is navigation-only and no longer exposes the old Name/Sugar/Food/Insulin daily buttons. Daily entry should use smart text input, `/help`, and compatibility commands instead of reintroducing those buttons.

Status in `1.5.9`: common navigation can be typed in Russian (`помощь`, `команды`, `настройки`, `статус`, `журнал`, `оцр`, `напоминания`, `формула`, `версия`, `здоровье`, `кто я`, `бэкап`, `история`). `/help` is intentionally short for daily use; `/commands` and `/devhelp` hold the full technical and legacy command list. The current OCR mode is visible in the main status/version surfaces.

Status in `1.6.0`: navigation aliases are routed before friendly fallback replies, so `настройки` and `журнал` no longer fall through to random text. `/ocr` is a status screen; only explicit mode words change OCR mode.

Status in `1.4.1`: safety text fallbacks were added for collapsed Telegram buttons. The user can type `сохранить еду`, `не сохранять`, or `отмена` after a smart-food calculation; food saving remains disabled until a real food journal exists.

## Food Calculation

- Inputs such as `50+40` or `50 40` should be interpreted as carbohydrate totals when the context points to food.
- Example: `50+40 = 90 g carbs = 9 XE` when `BU = 10 g/XE`.
- Dose calculation must use the user's saved protocol.
- The latest glucose value may be used only when it is fresh enough.
- If the latest glucose value is older than 60 minutes, the bot should ask for a fresh measurement before using it for correction.
- Rounding rules and any dose-reduction rules should be protocol parameters, not hidden constants.

Status in `1.4.0`: implemented for carb sums and `/food`. Freshness defaults to 60 minutes through `glucose_fresh_minutes`; `dose_reduction_percent` is stored in the protocol and applied to calculated totals.

Status in `1.4.1`: quick food calculations still do not create journal records without a future explicit food-log implementation.

## OCR Sources

- Keep the current Libre2 CV path as the active local OCR source.
- Add a future OCR source type for the updated Libre screenshot format with a long narrow screen and a smaller glucose number.
- Add a future OCR source type for photos of a manual glucometer, often blurred or poorly lit.
- OCR must not save glucose without explicit confirmation.
- When OCR engines strongly disagree, the bot should ask for manual verification.

Status in `1.5.0`: initial source-aware OCR support is implemented without a database migration. The old Libre2 CV path is labeled `libre2_cv_old`; the updated narrow Libre branch is labeled `libre2_narrow_updated`; the manual-glucometer photo branch is labeled `glucometer_photo`. All OCR values still require explicit confirmation before saving, and single glucometer-photo candidates remain manual-check results rather than trusted automatic reads.

The project also now keeps user-approved public OCR sample folders under `img/simple`, `img/simple_new`, and `img/simple_gluk`. These samples are for development and regression checks, not automatic medical decisions.

Status in `1.5.1`: live smoke with those public samples showed that experimental branches can be worse than no recognition when they disagree. The safety requirement is now stricter: old Libre2 CV keeps priority for known Libre screenshots, Libre OCR must not run on camera-photo-like glucometer frames, ambiguous glucometer digit strings should become manual/no-result, and the bot should ask the user to check the number instead of averaging unrelated candidates.

Status in `1.4.1`: OCR confirmation accepts typed actions (`сохранить`, `ввести вручную`, `не сохранять`, `отмена`) in addition to buttons.

## Trusted Contact

- Trusted contact behavior should be completed later, after the core daily flow is stable.
- The trusted-contact flow needs consent from the trusted person.
- It needs verification, disabling/revocation, quiet hours, and clear alert rules.
- Alert logic should be understandable to the family and should avoid noisy or surprising messages.

Status in `1.4.2`: foundation improved. `/whoami` now shows `user_id`, `chat_id`, and `chat_type`; `trusted_contact_id` may point to a private chat or family group; `/trustedtest` sends only a safe connection-check message without medical data. Full consent, verification, revocation, quiet hours, and alert policy remain future work.

Status in `1.6.0`: `/whoami` also shows `message_thread_id` when Telegram provides it, and `/trustedtest` can confirm group connectivity without sending medical data.

Status in `1.6.1`: group `/whoami` no longer sends private WebApp keyboard markup, and `trusted_contact_thread_id` can optionally route trusted-contact messages into a specific group topic. Consent, revocation, quiet hours, and escalation policy remain future work.

Status in `1.6.2`: a separate trusted family thread mode was added. `family_group` settings define where daily family entries may be accepted from, while `trusted_contact_*` still defines where alerts are sent. Ordinary groups remain blocked. Records from the trusted thread map to the configured patient profile instead of the sender. Persisted `saved_by/source` metadata for glucose/insulin rows remains future work because it needs a careful schema change.

## Telegram Family Mode

- Private chat should remain the main place for glucose, food, insulin, OCR, settings, journal, backup, and deletion.
- Family groups should support coordination and diagnostics without saving medical data by default.
- Safe group commands are `/version`, `/help`, `/health`, `/whoami`, and `/reminders`.
- If someone tries to enter sugar, food, insulin, OCR, settings, logs, backup, or deletion in a group, the bot should explain that medical records belong in private chat.
- Channels are not the primary dialog. They may become a future one-way announcement or carefully designed notification surface.
- Official bot identity should be configured through BotFather: name, username, description, about text, avatar/photo, command list, and privacy mode.
- Token rotation should wait until final production/public launch.

Status in `1.4.2`: implemented as the first family-mode foundation with `TELEGRAM_BOT_SETUP.md`, chat type detection, group/private safety guards, `/whoami` IDs, `/trustedtest`, and a neutral channel command notice.

Status in `1.6.0`: group `/start` explains family mode, group `/help` lists safe commands, and group medical entries or OCR saves remain blocked with a private-chat explanation.

Status in `1.6.1`: live group smoke found and fixed the Telegram WebApp-keyboard restriction for `/whoami`; group topic routing is now represented as optional `message_thread_id` configuration.

## Status in 1.6.0

- Russian navigation aliases are handled before random/friendly fallback replies.
- `/ocr` no longer duplicates a mode-selected message when the user only opens OCR status.
- Family group launch is safe for setup and diagnostics, while real medical records stay private.
- Food Log, DeepSeek, product database, A4 doctor diary, full trusted-contact consent, and production token rotation remain future work.

## Future

- DeepSeek/LLM integration stays deferred until privacy boundaries, prompts, and failure modes are documented.
- Product database / food database work stays deferred.
- Printable A4 doctor diary stays deferred.
- A new Telegram token should be issued only in the final phase before production or public launch.

## Release And Patch Reporting

- After each meaningful release, the project must have a short factual summary with version, tests, Telegram smoke status, and GitHub docs-only status when relevant.
- Telegram smoke should be manual/user-assisted when Codex cannot reliably type into Telegram Web.
- uSugar patch notes must be prepared according to `PATCH_NOTIFICATION_RULES.md` and the local uNews workflow in `C:\!CODE_CLUB\new 2026\004_uNews`.
- The patch note and its image should live in the repository-local `news/` folder so the uNews GitHub auto-discovery workflow can find them in any public project repository.
- After a release is closed, publish documentation/news through the docs-only GitHub path so the uNews GitHub-first workflow can publish the patch note when dry-run/check is green, the image exists, and safety checks pass.
- Do not publish if the note includes secrets, private medical values, database contents, Telegram file IDs, temporary ngrok URLs, or private IDs.

Status in `1.4.3`: documented only. No runtime behavior or medical logic changed.

Status in `1.4.4`: strengthened into a required release workflow and paired with `OPEN_WORK.md` plus `RELEASE_PLAN.md`.

Status in `1.5.1`: uSugar continues to create release notes in `news/` and run the uNews check locally. Real publication remains GitHub-first after the note and safe image reach the public repository; local real publishing is not part of the normal workflow.

## Status in 1.5.9

- Russian text aliases are available for common navigation commands.
- `/help` is short and daily-use oriented.
- `/commands` and `/devhelp` contain the full technical and legacy command list.
- OCR mode is shown next to the bot version in key status/version surfaces.

## Status in 1.5.8

- Main menu buttons are limited to Settings, Status, Log, OCR, Reminders, Formula, and Help.
- Legacy daily-entry buttons Name, Sugar, Food, and Insulin should not be re-added to the main keyboard.
- `/sugar`, `/food`, `/insulin`, and legacy `/setname` remain compatibility commands.
- `/help` documents the preferred daily text inputs: `8.4 сахар`, `3 укол`, `50 40 еда`, `новый либре`, `глюкометр`, and `отмена`.

## Status in 1.5.5

- OCR mode selection is implemented through `/ocr`, typed phrases, and Settings WebApp. The selected mode is saved as `ocr_mode` in the existing protocol JSON.
- Supported modes are `auto`, `libre2_old`, `libre2_new`, and `glucometer`.
- Smart text input now handles explicit daily phrases such as `8.4 сахар`, `сахр 8.4`, `50 40 еда`, `3 4 5 20 70 80 еда`, `3 укол`, and `укло 3`.
- Ambiguous phrases ask for clarification instead of saving silently.
- OCR still requires explicit confirmation before saving glucose.
- Food logging, product database, DeepSeek, trusted-contact completion, A4 doctor diary, and production deployment remain future work.

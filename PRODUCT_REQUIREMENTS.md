# Product Requirements

Date: 2026-06-21
Version: 1.4.3

This document captures current product requirements and implementation status. Items marked done are implemented in the local runtime; future items still need separate work.

## Settings WebApp

- The user's display name should be editable through the Telegram WebApp settings form, not primarily through `/setname`.
- `/settings` should open a form prefilled with the current user data from the database.
- The prefill behavior must cover both the user's name and the saved protocol.
- The form should fit comfortably inside Telegram Mini App on a phone.
- The design should stay compact, practical, and easy to scan.

Status in `1.4.0`: implemented for the current WebApp path. `/setname` remains available as a legacy/fallback command.

## Smart Dialog

- The main Telegram keyboard should stay minimal.
- The primary daily input should happen through a smart dialog rather than many separate buttons.
- The bot should understand numeric input and clarify whether the user means glucose, food, or insulin.
- Friendly fallback replies should remain for casual or random messages.
- The dialog should reduce user effort without hiding safety confirmations.

Status in `1.4.0`: partially implemented. Carb-sum inputs like `50+40` and `50 40` now trigger a direct food calculation without saving a record; single-number clarification remains available.

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

Status in `1.4.1`: OCR confirmation accepts typed actions (`сохранить`, `ввести вручную`, `не сохранять`, `отмена`) in addition to buttons.

## Trusted Contact

- Trusted contact behavior should be completed later, after the core daily flow is stable.
- The trusted-contact flow needs consent from the trusted person.
- It needs verification, disabling/revocation, quiet hours, and clear alert rules.
- Alert logic should be understandable to the family and should avoid noisy or surprising messages.

Status in `1.4.2`: foundation improved. `/whoami` now shows `user_id`, `chat_id`, and `chat_type`; `trusted_contact_id` may point to a private chat or family group; `/trustedtest` sends only a safe connection-check message without medical data. Full consent, verification, revocation, quiet hours, and alert policy remain future work.

## Telegram Family Mode

- Private chat should remain the main place for glucose, food, insulin, OCR, settings, journal, backup, and deletion.
- Family groups should support coordination and diagnostics without saving medical data by default.
- Safe group commands are `/version`, `/help`, `/health`, `/whoami`, and `/reminders`.
- If someone tries to enter sugar, food, insulin, OCR, settings, logs, backup, or deletion in a group, the bot should explain that medical records belong in private chat.
- Channels are not the primary dialog. They may become a future one-way announcement or carefully designed notification surface.
- Official bot identity should be configured through BotFather: name, username, description, about text, avatar/photo, command list, and privacy mode.
- Token rotation should wait until final production/public launch.

Status in `1.4.2`: implemented as the first family-mode foundation with `TELEGRAM_BOT_SETUP.md`, chat type detection, group/private safety guards, `/whoami` IDs, `/trustedtest`, and a neutral channel command notice.

## Future

- DeepSeek/LLM integration stays deferred until privacy boundaries, prompts, and failure modes are documented.
- Product database / food database work stays deferred.
- Printable A4 doctor diary stays deferred.
- A new Telegram token should be issued only in the final phase before production or public launch.

## Release And Patch Reporting

- After each meaningful release, the project should have a short factual summary with version, commit, archive, tests, Telegram smoke status, and GitHub docs-only status.
- Telegram smoke should be manual/user-assisted when Codex cannot reliably type into Telegram Web.
- uSugar patch notes should be prepared according to `PATCH_NOTIFICATION_RULES.md` and the local uNews workflow in `C:\!CODE_CLUB\new 2026\004_uNews`.
- Publishing a uNews patch note to `@uNewsLog` is a separate explicit action and must not include secrets, private medical values, database contents, Telegram file IDs, or temporary ngrok URLs.

Status in `1.4.3`: documented only. No runtime behavior or medical logic changed.

# Product Requirements

Date: 2026-06-13
Version: 1.3.3

This document captures current product requirements before the next implementation phase. It is not an implementation plan and does not mean the features below are already complete.

## Settings WebApp

- The user's display name should be editable through the Telegram WebApp settings form, not primarily through `/setname`.
- `/settings` should open a form prefilled with the current user data from the database.
- The prefill behavior must cover both the user's name and the saved protocol.
- The form should fit comfortably inside Telegram Mini App on a phone.
- The design should stay compact, practical, and easy to scan.

## Smart Dialog

- The main Telegram keyboard should stay minimal.
- The primary daily input should happen through a smart dialog rather than many separate buttons.
- The bot should understand numeric input and clarify whether the user means glucose, food, or insulin.
- Friendly fallback replies should remain for casual or random messages.
- The dialog should reduce user effort without hiding safety confirmations.

## Food Calculation

- Inputs such as `50+40` or `50 40` should be interpreted as carbohydrate totals when the context points to food.
- Example: `50+40 = 90 g carbs = 9 XE` when `BU = 10 g/XE`.
- Dose calculation must use the user's saved protocol.
- The latest glucose value may be used only when it is fresh enough.
- If the latest glucose value is older than 60 minutes, the bot should ask for a fresh measurement before using it for correction.
- Rounding rules and any dose-reduction rules should be protocol parameters, not hidden constants.

## OCR Sources

- Keep the current Libre2 CV path as the active local OCR source.
- Add a future OCR source type for the updated Libre screenshot format with a long narrow screen and a smaller glucose number.
- Add a future OCR source type for photos of a manual glucometer, often blurred or poorly lit.
- OCR must not save glucose without explicit confirmation.
- When OCR engines strongly disagree, the bot should ask for manual verification.

## Trusted Contact

- Trusted contact behavior should be completed later, after the core daily flow is stable.
- The trusted-contact flow needs consent from the trusted person.
- It needs verification, disabling/revocation, quiet hours, and clear alert rules.
- Alert logic should be understandable to the family and should avoid noisy or surprising messages.

## Future

- DeepSeek/LLM integration stays deferred until privacy boundaries, prompts, and failure modes are documented.
- Product database / food database work stays deferred.
- Printable A4 doctor diary stays deferred.
- A new Telegram token should be issued only in the final phase before production or public launch.

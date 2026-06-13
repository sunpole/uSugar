# Future Backlog

Deferred tasks after `uSugar 1.1.0`.

This file is a parking place for work that is intentionally outside the stable daily-use scope. Move an item back into `ROADMAP.md` only when it becomes an active milestone.

## OCR

- Add a reliable small-text OCR path for phone time and Libre2 timestamp.
- Add support for the updated Libre screenshot format with a long narrow screen and a smaller glucose number.
- Add support for blurred photos of manual glucometers.
- Compare before-crop and after-crop OCR results and record disagreements.
- Add two additional reliable OCR engines or local fallbacks beyond the current Libre2 CV path.
- Define a confidence policy for automatic save without manual confirmation.
- Keep explicit confirmation required before saving OCR glucose until that confidence policy is implemented and tested.
- Improve graph extraction from Libre2 screenshots into a useful trend summary.
- Add OCR fixtures from redacted sample screenshots.

## Architecture

- Split `bot.py` into command handlers, OCR handlers, reminder handlers, and shared Telegram response helpers.
- Move protocol summary formatting out of `bot.py` into a dedicated presentation module.
- Add a service layer between Telegram handlers and database helpers.
- Introduce structured constants for button labels and callback names.
- Reduce duplicate summary text between `/settings`, WebApp save confirmation, and documentation.

## Database

- Add explicit schema migrations instead of opportunistic column checks.
- Add created/updated metadata for protocol changes.
- Add edit history for deleted or corrected records.
- Add restore/import flow for exported backups.
- Consider separating runtime OCR attempts from long-term user health logs.

## Search

- Add date-range search for glucose and insulin logs.
- Add filters by insulin type, glucose range, and source.
- Add compact Telegram summaries for search results.
- Add CSV export for filtered subsets.

## Backup

- Add a verified restore command or local restore script.
- Add backup integrity checks.
- Add optional encrypted local backups.
- Document a family-safe manual restore procedure.

## Trusted Contact

- Add a consent flow before a trusted contact can receive alerts.
- Add contact verification and revocation.
- Add quiet hours and escalation rules.
- Add separate trusted-contact message templates.
- Make alert logic understandable and non-surprising for family use.

## Settings WebApp

- Move preferred display-name editing into the WebApp instead of relying on `/setname`.
- Verify `/settings` prefill for both user name and protocol values from the database.
- Keep the form compact enough for Telegram Mini App on a phone.
- Preserve `/setname` only as a compatibility/fallback command until WebApp editing is complete.

## Smart Dialog

- Keep the main Telegram keyboard minimal.
- Route ordinary numeric input through a smart clarification flow: glucose, food, or insulin.
- Interpret `50+40` and `50 40` as carbohydrate totals in food context.
- Keep friendly fallback replies for casual or random messages.
- Avoid hiding safety confirmations behind automation.

## Food Calculation

- Use recent glucose only when it is fresh enough for correction.
- Ask for a fresh measurement when the latest glucose is older than 60 minutes.
- Move rounding and dose-reduction rules into protocol settings.
- Keep calculation examples aligned with `BU`, `ICR`, `ISF`, and the saved user protocol.

## LLM

- Keep DeepSeek/LLM disabled until prompt boundaries, privacy rules, and failure modes are documented.
- Add a local knowledge module before any external LLM call.
- Never send `.env`, tokens, raw database exports, or private screenshots to an LLM.
- Add tests for any LLM prompt builder before enabling runtime calls.

## Deployment

- Decide whether deployment is local-only, VPS, or another controlled environment.
- Add production `.env` template without secrets.
- Rotate the Telegram bot token only in the final phase before production or public launch.
- Add startup and monitoring docs.
- Add CI checks for tests and syntax.
- Decide how ngrok or a permanent HTTPS tunnel should be managed.

## Administration

- Add admin-only diagnostics that do not expose secrets.
- Add role checks before any family-wide or trusted-contact action.
- Add a safe way to list known users.
- Add an audit log for admin actions.

## Product Database And Reports

- Defer food/product database work until the smart daily flow is stable.
- Defer printable A4 doctor diary until log structure, filtering, and export requirements are clearer.

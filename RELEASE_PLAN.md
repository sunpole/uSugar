# Release Plan

Date: 2026-06-21
Version: 1.5.1

This document turns the open work inventory into a practical release sequence. It is intentionally product-focused: each milestone should stay small enough to verify with local tests, manual Telegram smoke, and a uNews patch note.

## 1.5.0 - OCR New Sources

Goal: make image intake more useful without weakening safety.

Status: implemented as the first safe source-aware OCR step. The old Libre2 CV path remains active, `libre2_narrow_updated` and `glucometer_photo` were added as lightweight CV sources, synthetic tests cover the new branches, and every recognized value still requires explicit confirmation before saving.

Completed work:

- Support the updated narrow Libre screenshot format as an initial synthetic-tested CV path.
- Add a first safe path for manual glucometer photos with stricter manual confirmation.
- Add synthetic OCR fixture policy and tests for both new source types.
- Compare OCR variants/engines with source-aware metadata.
- Keep explicit user confirmation before saving any OCR value.

Exit criteria:

- OCR disabled mode still skips heavy recognition.
- Existing Libre2 CV path still works.
- New source tests pass.
- Telegram photo smoke remains manual/user-assisted with safe test images.

## 1.5.1 - OCR Safety Hotfix

Goal: make the new OCR source work safer after live sample smoke exposed false confidence risks.

Status: implemented as a patch release after `1.5.0`. The old Libre2 CV path keeps priority when experimental sources disagree, Libre-specific OCR branches skip camera-photo-like glucometer frames, ambiguous glucometer digit strings are rejected as low-confidence/no-result, and the manual-check message is readable.

Completed work:

- Preserve old Libre2 CV as the trusted candidate for known Libre screenshots.
- Avoid averaging unrelated OCR candidates into a misleading glucose value.
- Prevent Libre OCR paths from processing camera-photo-like glucometer images.
- Treat ambiguous glucometer digit strings as manual/no-result.
- Keep test records created during live smoke in the local database by user request.

Exit criteria:

- Full unit tests pass.
- Manual Telegram smoke confirms the bot reports `v1.5.1`.
- uNews patch note passes local check but is not locally published.

## 1.5.2 - Food Log

Goal: make food calculations recordable and exportable.

Planned work:

- Add a `food_log` data model and migration-safe storage path.
- Save confirmed smart-food calculations as meal records.
- Show food entries in `/log`.
- Include food data in backups and CSV exports.
- Add `/undo` coverage for latest food entry if safe.

Exit criteria:

- Test food entries can be created and removed safely.
- No calculated meal is saved without explicit confirmation.
- Backup does not include secrets or unrelated users.

## 1.5.3 - Smart Daily Dialog

Goal: reduce command burden while keeping medical actions explicit.

Planned work:

- Route ambiguous numeric input through a clearer smart dialog.
- Improve fallback replies for random text.
- Keep buttons and text fallbacks equivalent.
- Improve prompts for glucose, food, and insulin intent.

Exit criteria:

- User can complete common daily flows with fewer commands.
- Wrong-context input is handled safely.
- Telegram smoke covers text-only alternatives.

## 1.6.0 - Trusted Contact MVP

Goal: make family alerts understandable and consent-based.

Planned work:

- Add trusted-contact consent and verification.
- Add revocation/disable flow.
- Add quiet hours.
- Clarify alert escalation rules and message templates.
- Keep `/trustedtest` as a non-medical connection check.

Exit criteria:

- Trusted contact receives alerts only after consent.
- User can disable or change the contact.
- Quiet hours are respected.

## 1.6.1 - A4 Doctor Diary

Goal: produce a practical printable report for doctor visits.

Planned work:

- Add date-range selection for logs.
- Generate a printable A4 diary from glucose, insulin, and food data.
- Include protocol snapshot and clear units.
- Keep private data local unless the user exports it.

Exit criteria:

- A test diary can be generated from local data.
- Output is readable on A4 without manual editing.

## 1.7.0 - Product Database

Goal: make food entry easier with reusable product knowledge.

Planned work:

- Define local product data format.
- Add search or quick lookup for common foods.
- Keep user-editable values and avoid hidden assumptions.
- Keep database local by default.

Exit criteria:

- Product lookup helps food calculation without replacing confirmation.
- User can override product values.

## 1.8.0 - DeepSeek

Goal: add LLM assistance only after safety boundaries are explicit.

Planned work:

- Define allowed and forbidden LLM inputs.
- Add prompt-builder tests.
- Avoid sending secrets, raw database exports, private screenshots, or medical logs without explicit design.
- Start with non-medical explanation/help use cases.

Exit criteria:

- LLM calls are optional, disabled by default, and covered by tests.
- Failure modes are documented before runtime use.

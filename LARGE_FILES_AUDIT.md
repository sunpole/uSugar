# LARGE_FILES_AUDIT - uSugar 1.3.1

Date: 2026-06-12

Scope: audit only. No runtime code was refactored for this release.

## Summary

After `bot.py` was closed as a 126-line composition root in `1.3.0`, the next maintainability risk moved to feature files that still combine several responsibilities. This document records the large-file scan and gives a safe future split plan.

## Scan Rules

- Python files over 250 physical lines.
- HTML/JS files over 300 physical lines.
- Documentation counted separately.
- Requested special files included even when below threshold.
- Ignored runtime/archive/venv/cache folders excluded.

## Large Active Files

| File | Lines | Responsibility | Split? | Risk | Future modules |
| --- | ---: | --- | --- | --- | --- |
| `settings.html` | 462 | Static Telegram WebApp settings UI: CSS, markup, prefill, toggles, submit payload, version display. | Yes, later | Medium | `web/settings.css`, `web/settings.js`, optional `web/settings_schema.js`; keep HTML as shell. |
| `usugar_ocr.py` | 456 | OCR engines, image variants, Tesseract, Libre2 CV, EasyOCR subprocess, metadata, engine summary. | Yes, carefully | High | `ocr/variants.py`, `ocr/libre2_cv.py`, `ocr/tesseract_engine.py`, `ocr/easyocr_engine.py`, `ocr/metadata.py`; keep facade first. |
| `db.py` | 386 | SQLite schema bootstrap and async CRUD for users, glucose, insulin, reminders, followups, OCR attempts. | Yes, carefully | High | `storage/schema.py`, `storage/users.py`, `storage/glucose.py`, `storage/insulin.py`, `storage/reminders.py`, `storage/ocr_attempts.py`; keep `db.py` facade. |
| `usugar_reminders.py` | 325 | Reminder settings, measurement state, basal state, follow-up timing, delivery keys, Telegram texts. | Yes, carefully | Medium-high | `reminders/settings.py`, `reminders/measurement.py`, `reminders/basal.py`, `reminders/followup.py`, `reminders/delivery.py`, `reminders/texts.py`. |
| `docs/history/story_data.js` | 311 | Generated story data consumed by `story.html`. | No manual split | Low | Generated output only; regenerate from `SCREENSHOT_STORY.md`. |
| `handlers/therapy.py` | 307 | `/food`, `/insulin`, `/undo`, related FSM handlers, short-insulin follow-up scheduling. | Yes, later | Medium-high | `handlers/food.py`, `handlers/insulin.py`, `handlers/undo.py`, optional `therapy/followups.py`. |
| `story.html` | 303 | Interactive project history landing: CSS, markup, filters, timeline rendering, scroll progress. | Maybe | Low-medium | `story_assets/story.css`, `story_assets/story.js`; keep HTML shell. |

## Requested Files Below Threshold

| File | Lines | Responsibility | Split? | Risk | Recommendation |
| --- | ---: | --- | --- | --- | --- |
| `usugar_vision.py` | 155 | OCR-facing text helpers, caption filtering, candidate extraction, OCR aggregation. | Not now | Medium | Keep as boundary between Telegram handlers and OCR engines; expand tests before confidence-policy changes. |
| `scripts/generate_story_data.py` | 84 | Parses `SCREENSHOT_STORY.md` and writes `story_data.js`. | Not now | Low | Keep as one focused script; add parser tests before supporting new markdown formats. |

## Documentation Counted Separately

| File | Lines | Note |
| --- | ---: | --- |
| `PROJECT_AUDIT.md` | 379 | Source-of-truth project audit snapshot. Keep as document. |
| `BOTPY_REFACTOR_PLAN.md` | 348 | Historical plan for completed `bot.py` split. Keep as reference. |
| `docs/github_original/NOTE_1.0.2.MD` | 317 | Imported legacy design note. Not current architecture. |
| `docs/legacy_local/NOTE_1.0.2.MD` | 317 | Local legacy copy. Not current architecture. |

## File Notes

### `settings.html`

The file is large because it intentionally keeps a standalone WebApp in one file: layout CSS, all protocol fields, Telegram WebApp integration, local prefill, toggles, and submit payload construction. The safest future split is CSS first, then JavaScript, with no field-name changes in the same patch.

Do not split it while changing protocol semantics. Tests should keep checking version display, compact mobile controls, reminder controls, and prefill support.

### `usugar_ocr.py`

This is the highest-risk large file because it mixes image processing, optional external tools, subprocess safety, and OCR confidence inputs. Split only one engine at a time and keep a facade so handlers/tests do not change imports immediately.

First safe split candidate: Tesseract discovery and variant execution. Hardest part to touch: Libre2 CV digit/template internals, because it needs more image fixtures.

### `db.py`

This file is both schema bootstrap and repository layer. Splitting it without a migration/compatibility plan could break existing local `usugar.db` users. Keep public async function names stable until handlers are moved gradually.

First safe step is not moving code; it is expanding tests around all public DB functions used by handlers and writing a migration/facade policy.

### `usugar_reminders.py`

The reminder file is pure and testable, but mistakes can create missed or duplicate reminders. Its natural future split is policy logic versus user-facing texts.

First safe split candidate: text builders into `reminders/texts.py`, after snapshot-like tests confirm current wording.

### `handlers/therapy.py`

This file contains three user flows. It should not be split together with behavior changes. `/undo` is the best first extraction because it is narrow, confirmation-based, and has direct DB operations. `/food` and `/insulin` can follow after smoke checks are scripted.

### `story.html`

The story page is near the HTML/JS threshold but not dangerous to runtime. Keep it unchanged until GitHub Pages deploy behavior is stable. If it grows, split CSS/JS into static assets while keeping the data source unchanged.

### `docs/history/story_data.js`

This file is generated and should not be hand-edited. Size growth is expected because the project story grows with screenshots.

### `usugar_vision.py`

This file is not large and should stay as a compact OCR safety boundary. It holds user-facing OCR messages and aggregation rules, so changing it is more about safety policy than file size.

### `scripts/generate_story_data.py`

This file is small and focused. Do not split it; only add tests if the markdown story format changes.

## Recommended Split Order

1. `settings.html`: extract CSS only.
2. `handlers/therapy.py`: extract `/undo` into `handlers/undo.py`.
3. `usugar_reminders.py`: extract text builders after text tests are stable.
4. `db.py`: add migration/facade policy and broader DB tests before moving code.
5. `usugar_ocr.py`: extract Tesseract runner first; keep Libre2 CV intact.
6. `story.html`: split only after GitHub Pages deploys are stable.

## Files To Avoid Splitting For Now

- `usugar_vision.py`
- `scripts/generate_story_data.py`
- `docs/history/story_data.js`
- `db.py` schema bootstrap
- Libre2 CV internals in `usugar_ocr.py`

## Future Release Rule

Every future split should be a small release:

1. Move one responsibility.
2. Keep existing behavior and public function names stable where possible.
3. Run `py_compile`, all unit tests, and affected Telegram smoke checks.
4. Update `CHANGELOG.md`, `PROJECT_STATUS.md`, `ROADMAP.md`, and `docs/AI_CONTEXT.md`.

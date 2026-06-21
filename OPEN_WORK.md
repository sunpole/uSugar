# Open Work

Date: 2026-06-21
Version: 1.5.2

This document is the working inventory of unfinished uSugar tasks. It complements `ROADMAP.md` and `RELEASE_PLAN.md`: this file tracks what is open, while `RELEASE_PLAN.md` groups the next release milestones.

Status values:

- `done` - implemented and covered by current local runtime/docs.
- `partial` - implemented in a limited form, needs another release before it is complete.
- `not started` - documented requirement or idea, not implemented yet.

## OCR

| Task | Status | Priority | Target release | Notes |
|---|---|---:|---|---|
| Old Libre2 CV path | partial | high | 1.5.x | Exists and remains the best current source, but `OCR_REALITY_REPORT.md` measured only partial stability on `img/simple`; more tuning is needed before it can be called fully reliable. |
| Updated narrow Libre screenshot | partial | high | 1.5.x | Initial lightweight CV source `libre2_narrow_updated` exists and is covered by synthetic tests; `1.5.2` showed real `img/simple_new` samples are still mostly failed/partial. |
| Manual glucometer photo recognition | partial | high | 1.5.x | Initial lightweight CV source `glucometer_photo` exists with blur/low-confidence safety; `1.5.2` showed real `img/simple_gluk` samples remain low-confidence/manual. |
| Multiple OCR engine comparison | partial | high | 1.5.x | Source-aware aggregation exists; 1.5.1 improved disagreement safety, but production-grade multi-engine coverage is not complete. 1.5.2 added repeatable smoke reporting. |
| Before-crop / after-crop comparison | partial | medium | 1.5.0 | Existing variants are logged by source/variant; disagreement reporting still can become clearer. |
| Protection from mistaken OCR save | done | high | 1.4.1 | OCR requires explicit confirmation before saving. |
| Phone time / measurement time extraction | not started | medium | 1.5.x | Needed before using screenshot age in decisions. |
| Libre graph trend extraction | partial | medium | 1.5.0 | Basic trend exists, not a reliable time-series analysis yet. |

## Family Mode

| Task | Status | Priority | Target release | Notes |
|---|---|---:|---|---|
| Private chat daily use | done | high | 1.4.2 | Private chat remains the main medical-entry surface. |
| Group safe-command mode | partial | high | 1.4.2 | Safe commands exist; deeper group smoke still depends on a test group. |
| Channel behavior | partial | medium | 1.6.0 | Channel is documented as announcement-oriented, not a primary dialog. |
| Trusted contact base | partial | high | 1.4.2 | `/whoami` IDs and `/trustedtest` exist. |
| Trusted contact consent | not started | high | 1.6.0 | Must be explicit before alerts are treated as family-ready. |
| Trusted contact revocation | not started | high | 1.6.0 | Needed for safe long-term use. |
| Quiet hours | not started | medium | 1.6.0 | Avoids noisy alerts during sleep or family rest windows. |
| Alert escalation rules | partial | high | 1.6.0 | Technical alert engine exists, but product policy is unfinished. |

## Daily Flow

| Task | Status | Priority | Target release | Notes |
|---|---|---:|---|---|
| Glucose entry | done | high | 1.0.x | Manual glucose input and `/sugar` are implemented. |
| Food calculation | partial | high | 1.4.0 | Carb sums calculate dose, but full food logging is not complete. |
| Insulin entry | done | high | 1.0.x | Short and long insulin logging exists. |
| Smart dialog for numeric input | partial | high | 1.5.3 | Carb sums and multi-number choices exist; broader dialog remains future work. |
| Friendly fallback | partial | medium | 1.5.3 | Basic fallback exists, needs more consistent smart-dialog routing. |
| Text confirmations when buttons are hidden | done | high | 1.4.1 | `/undo`, OCR, and food confirmation fallbacks exist. |
| Old-record editing | not started | medium | 1.5.3 | `/undo` only handles the latest selected record type. |
| Daily UX smoke plan | done | medium | 1.4.3 | Manual Telegram smoke plan is documented. |

## Food

| Task | Status | Priority | Target release | Notes |
|---|---|---:|---|---|
| `food_log` table and journal | not started | high | 1.5.3 | Required before food confirmations can save real meal records. Deferred because 1.5.2 was used for OCR/uNews audit. |
| Carb sum calculation | done | high | 1.4.0 | `50+40` and `50 40` are interpreted as carbohydrate sums. |
| XE conversion | done | high | 1.4.0 | Uses the saved `BU` protocol value. |
| Dose calculation with fresh glucose | done | high | 1.4.0 | Uses recent glucose only inside the freshness window. |
| Product database | not started | medium | 1.7.0 | Deferred until daily food flow is stable. |
| Printed doctor diary A4 | not started | medium | 1.6.1 | Depends on logs, filters, and export shape. |

## Exports

| Task | Status | Priority | Target release | Notes |
|---|---|---:|---|---|
| CSV export | partial | medium | 1.5.3 | `/log` can export long glucose/insulin logs; food export depends on `food_log`. |
| Backup ZIP | done | high | 1.0.x | `/backup` exports safe user data without secrets. |
| Backup restore | not started | high | 1.5.4 | Needed before backup can be considered a complete data lifecycle. |
| Filtered exports | not started | medium | 1.6.1 | Depends on search/filtering. |
| A4 doctor diary | not started | medium | 1.6.1 | Should be printable and understandable to a doctor. |

## Future

| Task | Status | Priority | Target release | Notes |
|---|---|---:|---|---|
| DeepSeek / LLM | not started | low | 1.8.0 | Deferred until safety, privacy, prompts, and tests are ready. |
| Local knowledge base | not started | medium | 1.8.0 | Should come before external LLM calls. |
| Production deployment | not started | high | 1.9.0 | Needs hosting, monitoring, backup, and secrets plan. |
| New Telegram token | not started | high | production phase | Rotate only before public/production launch. |
| GitHub code publication | not started | medium | production phase | Current GitHub flow remains docs/history/news-only. |
| CI checks | not started | medium | 1.9.0 | Should run syntax and tests before production. |

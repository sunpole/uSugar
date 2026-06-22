# Screenshot Story

This file is the narrative index for project-history screenshots. Each image should have a clear role, caption, and place in the future interactive presentation or landing page.

## Naming Rule

Use this pattern:

```text
YYYY-MM-DD_stage_short-description.png
```

Good examples:

- `2026-06-04_initial_state.png`
- `2026-06-04_process_settings_page_screen.png`
- `2026-06-04_process_health_check_ngrok.png`

Do not include secrets, tokens, private Telegram chats, or medical/private personal data in screenshots.

## Telegram Screenshot Rule

For real Windows screenshots, the Telegram window must be visible on screen. If Chrome or Telegram is hidden behind another window, the capture may show the front window instead. Keep private chats collapsed or hidden, then crop the final public image to the bot conversation and remove personal data before committing it.

## Continuation Rule

After each major stage:

1. Make a technical screenshot if possible.
2. Make a product screenshot if possible.
3. If a screenshot is missing, leave `TODO_SCREENSHOT` with a detailed description.
4. Do not block development because a screenshot is missing.

## Current Screenshots

| File | Story Role | What It Shows | Suggested Caption |
| --- | --- | --- | --- |
| `2026-06-04_initial_state.png` | Beginning | First local state snapshot after finding the project folder. | "The project started as a living folder with code, notes, copies, and a local database." |
| `2026-06-04_before_docs_and_secret_cleanup.png` | Before cleanup | State before consolidating GitHub docs and reducing token exposure risk. | "Before documentation consolidation and secret cleanup." |
| `2026-06-04_after_docs_and_secret_cleanup.png` | Documentation consolidation | GitHub docs copied locally and token risk reduced. | "Public docs and local working files begin to meet in one place." |
| `2026-06-04_organized_baseline.png` | Clean baseline | Root cleaned, docs grouped, archive created. | "The first stable local baseline: one project, one direction." |
| `2026-06-04_process_root_tree.png` | Working structure | Current root tree after cleanup and first implementation steps. | "The working project tree after organizing the code and documentation." |
| `2026-06-04_process_docs_tree.png` | Documentation structure | `docs/` folders: GitHub original, legacy local, history. | "Documentation split into source history, legacy notes, and visual story." |
| `2026-06-04_process_git_log.png` | Version control | Local git commits created during cleanup and implementation. | "Local commits become the project memory." |
| `2026-06-04_process_health_check.png` | Safety check | Health check before adding ngrok availability detection. | "The first local safety check for files, ports, and environment." |
| `2026-06-04_process_health_check_ngrok.png` | Runtime readiness | Health check with ngrok detection and port status. | "uSugar is ready for a safe local Telegram/WebApp test beside uChurch." |
| `2026-06-04_process_codex_screen.png` | Work process | Codex working in the project with checklist and changes visible. | "The assistant working inside the project, turning notes into a runnable baseline." |
| `2026-06-04_process_settings_page_screen.png` | First visible UI | Local settings page opened in browser through `localhost:8001`. | "The first local WebApp settings screen running beside uChurch." |
| `2026-06-05_runtime_telegram_bot_version_public.png` | First live Telegram proof | Cropped Telegram Web chat showing uSugarBot responding with version `1.0.10` and help/manual-input flow. | "The bot is alive: uSugarBot answers in Telegram with the new managed version." |
| `2026-06-05_runtime_telegram_formula_reference_public.png` | Formula reference in Telegram | Cropped Telegram Web chat showing the `/formula` reference command and the current protocol example. | "The bot explains how the calculation works before the runtime health check is added." |
| `2026-06-05_runtime_telegram_health_public.png` | Runtime status in Telegram | Cropped Telegram Web chat showing `/health` on uSugarBot `1.0.11` with no secrets exposed. | "The bot can now report its own runtime status safely inside Telegram." |
| `2026-06-05_runtime_telegram_backup_public.png` | Telegram backup export | Cropped Telegram Web chat showing `/backup` sending a ZIP and `/help` listing the new command in version `1.0.12`. | "uSugar gains its first user-controlled export: a private ZIP backup with protocol and logs." |
| `2026-06-05_runtime_telegram_backup_help_showcase_public.png` | Telegram feature showcase | Wider Telegram crop showing the journal, `/health`, `/backup`, and `/help` together. | "A compact view of the live bot feature set after the first export command." |
| `2026-06-05_process_codex_telegram_backup_side_by_side_public.png` | Development process | Side-by-side screenshot with Codex work on the left and Telegram Web testing on the right. | "The project being built and tested in real time: implementation notes beside the running bot." |
| `2026-06-05_process_codex_telegram_ocr_intake_side_by_side_public.png` | First OCR intake step | Side-by-side screenshot showing Codex work and Telegram `/ocr` response on uSugarBot `1.0.17`. | "The OCR roadmap becomes a live Telegram surface: safe photo intake before recognition and dose calculation." |
| `2026-06-05_ocr_pipeline_v1_0_23_running.png` | OCR pipeline implementation | Side-by-side screenshot showing Codex implementing OCR and Telegram holding the Libre2 screenshot test. | "The photo OCR pipeline becomes code: variants, engine status, and Telegram confirmation are now part of the bot." |
| `2026-06-05_bot_brain_v1_0_29_working.png` | First bot brain layer | Codex implementing behavior reactions while Telegram shows fast Libre2 OCR and saved glucose tests. | "The bot begins to respond like an assistant, not just a recorder: OCR, trend metadata, and the first behavior layer come together." |
| `2026-06-05_reminder_engine_v1_0_30_working.png` | Reminder engine checkpoint | Codex recovering after an internet break, with Telegram Web beside the work and uSugarBot running locally. | "The project moves from one-time replies toward a schedule-aware assistant: v1.0.30 introduces the reminder state engine." |
| `2026-06-05_background_reminders_v1_0_31_working.png` | Background reminders enabled | Codex after adding one-shot background reminders, with Telegram Web kept visible but redacted for public history. | "uSugarBot v1.0.31 starts checking the day by itself: first and second missed-measurement reminders can now be sent in the background." |
| `2026-06-05_basal_reminders_v1_0_33_working.png` | Basal reminder layer | Codex after adding long-insulin reminder checks, with Telegram Web visible but redacted for public history. | "uSugarBot v1.0.33 learns the nightly long-insulin checkpoint: reminders now cover both measurements and basal schedule awareness." |
| `2026-06-05_webapp_reminder_settings_section_v1_0_34_working.png` | WebApp reminder settings | Settings page opened locally on port `8001`, showing measurement windows, reminder delays, basal reminder timing, and trusted-alert threshold controls. | "uSugarBot v1.0.34 makes reminders configurable: the assistant's schedule moves from code defaults into the user's protocol." |
| `2026-06-06_trusted_contact_settings_v1_0_35_working.png` | Trusted contact settings | WebApp reminder settings showing the new Telegram ID field for a trusted contact, with no real ID entered. | "uSugarBot v1.0.35 begins the trusted-person path: long missing-measurement alerts can now leave the user's chat." |
| `2026-06-06_followup_settings_v1_0_37_working.png` | Post-bolus follow-up settings | Settings page opened locally on port `8001`, showing the new control reminder after short insulin, enabled by default at 120 minutes. | "uSugarBot v1.0.37 starts remembering actions: after short insulin, the assistant can schedule a one-time control check." |
| `2026-06-06_mobile_settings_compact_v1_0_38_working.png` | Mobile WebApp compacting | Mobile viewport screenshot of the settings page after compact grid layout, collapsible advanced sections, and corrected radio/checkbox sizing. | "uSugarBot v1.0.38 turns the setup form from a long wall of fields into a mobile-first protocol editor." |
| `2026-06-11_story_landing_v1_0_39_working.png` | Interactive project story | Local `story.html` page showing the generated project-history landing, version filters, and screenshot timeline metrics. | "uSugarBot v1.0.39 turns the build process itself into a living presentation generated from the project history file." |
| `2026-06-11_stability_release_v1_1_0_working.png` | Stability release workbench | A real desktop capture with Codex, Telegram Web, and the running project story visible while the `1.1.0` milestone is being documented. The frame shows the release work, the bot conversation, and the story pipeline together instead of inventing a clean promo shot. | "uSugarBot 1.1.0 is captured as a living working session, with the release story already feeding the history pipeline." |
| `2026-06-11_runtime_telegram_version_v1_1_0_public.png` | Live version fix proof | Cropped Telegram Web chat showing the stale `1.0.39` answer, then the corrected `/version` and `/health` responses from the restarted runtime with `1.1.0`. | "The live Telegram bot finally matches the local 1.1.0 release after replacing the stale runtime." |
| `2026-06-12_system_handlers_refactor_v1_2_0_working.png` | Phase 1 bot.py refactor | Cropped Telegram Web chat showing the extracted system handlers still responding in uSugarBot `1.2.0`: `/help`, `/health`, `/story`, and `/backup`. | "uSugarBot 1.2.0 proves the first safe bot.py split in the live chat: system handlers moved out, behavior stayed familiar." |
| `2026-06-12_profile_info_refactor_v1_2_1_working.png` | Phase 2 bot.py refactor | Cropped Telegram Web chat showing uSugarBot `1.2.1` after moving profile and info/OCR-status handlers out of `bot.py`; `/ocrlog`, `/help`, and `/health` still answer normally. | "uSugarBot 1.2.1 keeps the daily chat familiar while profile and information handlers move into their own modules." |
| `2026-06-12_settings_handlers_refactor_v1_2_4_working.png` | Phase 5 bot.py refactor | Real desktop capture with Codex on the left and Telegram Web on the right, showing the 1.2.4 settings/WebApp handler split work and live `/health` plus `/version` answers from uSugarBot `1.2.4`. | "uSugarBot 1.2.4 moves settings into its own handler module while the live Telegram bot keeps answering with the current release version." |
| `2026-06-12_logs_handlers_refactor_v1_2_5_working.png` | Phase 6 bot.py refactor | Real desktop capture with Codex on the left and Telegram Web on the right, showing uSugarBot `1.2.5` after moving `/log` and journal CSV export into `handlers/logs.py`; `/log`, `/help`, `/health`, and `/status` still answer normally. | "uSugarBot 1.2.5 proves the journal split: log handlers move out of bot.py while the live Telegram flow stays familiar." |
| `2026-06-12_therapy_handlers_refactor_v1_2_6_working.png` | Phase 7 bot.py refactor | Real desktop capture with Codex on the left and Telegram Web on the right, showing uSugarBot `1.2.6` after moving `/food`, `/insulin`, `/undo`, food/insulin FSM, undo confirmation, and short-insulin follow-up scheduling into `handlers/therapy.py`; smoke commands are handled without creating new test records. | "uSugarBot 1.2.6 pulls therapy flows out of bot.py while the live chat keeps the food, insulin, undo, health, and status surfaces working." |
| `2026-06-12_ocr_handlers_refactor_v1_2_7_working.png` | Phase 8 bot.py refactor | Real desktop capture with Codex on the left and Telegram Web on the right, showing uSugarBot `1.2.7` after moving photo intake, OCR callbacks, OCR confirmation, and manual OCR value flow into `handlers/ocr.py`; the smoke frame includes OCR log, help, health, and status responses without creating new OCR or glucose records. | "uSugarBot 1.2.7 moves the OCR conversation out of bot.py while the live chat still reports the OCR journal and runtime health at the new version." |
| `2026-06-12_reminders_runtime_refactor_v1_2_8_working.png` | Phase 9 bot.py refactor | Real desktop capture with Codex on the left and Telegram Web on the right, showing uSugarBot `1.2.8` after moving `/reminders`, due-reminder delivery, and the background reminder loop into `runtime/reminders.py`; the smoke frame includes reminders, help, health, and status responses. | "uSugarBot 1.2.8 pulls the reminder runtime out of bot.py, leaving the main file as a clean composition root while Telegram keeps the schedule status visible." |
| `2026-06-12_composition_root_v1_3_0_working.png` | Final composition root cleanup | Real desktop capture with Codex on the left and Telegram Web on the right, showing uSugarBot `1.3.0` after moving startup version sync into `runtime/startup.py`; the fresh `/help` response includes `/start` and `/help` and confirms the current version. | "uSugarBot 1.3.0 closes the bot.py split: the main file now composes the runtime while Telegram shows the full command surface at the new version." |
| `2026-06-21_family_mode_identity_v1_4_2_working.png` | Telegram family mode and identity | Cropped Telegram Web chat showing uSugarBot `1.4.2` after the family-mode release, with live `/story` and `/health` responses. The temporary WebApp URL is redacted for public history. | "uSugarBot 1.4.2 prepares for family use: identity, safe chat boundaries, trusted-contact testing, and public-history hygiene move into the product." |
| `2026-06-21_ocr_safety_hotfix_v1_5_1_working.png` | OCR safety hotfix | Real collage built from user-approved public OCR samples: old Libre2, updated Libre, and glucometer photo. It records the moment live smoke showed false-confidence risk and the project responded with source priority, shape guards, and manual-check defaults. | "uSugarBot 1.5.1 turns a failed OCR smoke into safety logic: old Libre stays trusted, camera photos stop masquerading as Libre screenshots, and uncertain glucometer reads remain manual." |
| `2026-06-21_ocr_reality_unews_audit_v1_5_2_working.png` | OCR reality and uNews audit | Safe public release card with no medical sample values, Telegram IDs, tokens, or private screenshots. It records the 1.5.2 audit outcome: old Libre2 is partial, new Libre2 is mostly failed/partial, glucometer photos are manual/low-confidence, and uNews publication is blocked by GitHub Actions Telegram Unauthorized. | "uSugarBot 1.5.2 stops pretending OCR is solved: the project measures what works, documents what fails, and finds why the release news queue stalled." |
| `2026-06-22_libre2_narrow_ocr_tuning_v1_5_3_working.png` | New Libre2 narrow OCR tuning | Safe public release card showing the before/after smoke counts for `img/simple_new` after adding the landscape right-side ROI. It avoids Telegram IDs, private screenshots, secrets, and autosave claims. | "uSugarBot 1.5.3 teaches the new Libre2 landscape screenshots where to look: no-result frames drop from 14 to 9 while new candidates stay manual by default." |
| `2026-06-22_glucometer_photo_ocr_tuning_v1_5_4_working.png` | Glucometer photo OCR tuning | Safe public release card showing the before/after smoke counts for `img/simple_gluk` after adding display ROI fallback, photo preprocessing, upscaling, and small rotation tolerance. It avoids readable medical-photo crops, Telegram IDs, secrets, and autosave claims. | "uSugarBot 1.5.4 makes real glucometer photos less silent: no-result frames drop from 19 to 9, but every candidate still asks for human confirmation." |

## Future Screenshot Slots

Add new screenshots for these moments:

| Planned File | Moment | Notes |
| --- | --- | --- |
| `TODO_SCREENSHOT_2026-06-05_libre2_cv_8_2` | Libre2 OCR 8.2 milestone | TODO_SCREENSHOT: Telegram Web showing a real Libre2 capture with the bot returning the `8.2` candidate and the manual confirmation flow. Keep the phone screenshot visible, but hide any private chat list or personal notifications. |
| `TODO_SCREENSHOT_2026-06-11_ai_context` | AI_CONTEXT milestone | TODO_SCREENSHOT: editor or browser view where `docs/AI_CONTEXT.md` is open and the headings make the project's memory easy to scan. No secrets, no raw token values. |
| `TODO_SCREENSHOT_2026-06-11_future_backlog` | FUTURE_BACKLOG milestone | TODO_SCREENSHOT: `FUTURE_BACKLOG.md` open in an editor or preview with the category layout visible. Keep the stable release context in frame if possible. |
| `TODO_SCREENSHOT_2026-06-11_project_audit` | PROJECT_AUDIT milestone | TODO_SCREENSHOT: `PROJECT_AUDIT.md` visible in a clean editor or preview view, ideally with the headings and summary sections readable. |
| `TODO_SCREENSHOT_2026-06-11_code_audit` | CODE_AUDIT milestone | TODO_SCREENSHOT: `CODE_AUDIT.md` visible in a clean editor or preview view. Keep the screenshot technical and free of secrets. |
| `2026-06-04_runtime_ngrok_tunnel.png` | ngrok tunnel running | Hide account details if visible. Capture public HTTPS URL only if it is safe to show. |
| `2026-06-04_runtime_telegram_start.png` | Telegram `/start` test | No private chats or personal medical data. |
| `2026-06-04_runtime_telegram_manual_sugar.png` | Manual sugar entry flow | Use safe dummy values. |
| `2026-06-04_runtime_telegram_food_calc.png` | Food dose calculation | Use test protocol and dummy carbs/glucose. |
| `2026-06-04_runtime_webapp_in_telegram.png` | WebApp opened from Telegram | Capture settings form if no secrets/private data are shown. |

> Rows prefixed with `TODO_SCREENSHOT` are intentional placeholders. They document the missing frame now and stay out of the generated story data until a real image exists.

## Future Landing Page Sections

1. Origin: why uSugar exists.
2. First folder: the messy but real starting point.
3. Cleanup: turning copies and notes into a project.
4. Safety: `.env`, no secrets in git, local health checks.
5. Coexistence: running beside uChurch without port conflicts.
6. First implementation: extracting and testing dose logic.
7. First live test: settings page, ngrok, Telegram bot.
8. Roadmap: OCR, better WebApp, tests, deployment.

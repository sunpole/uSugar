# Screenshot Capture Guide

This guide explains how to capture story screenshots for uSugar without blocking development or exposing private data.

## Core rule

After every major milestone:

1. Take a technical screenshot if possible.
2. Take a product screenshot if possible.
3. If no real screenshot exists yet, add `TODO_SCREENSHOT` with a detailed description.
4. Do not stop development because a screenshot is missing.

## What to capture after major stages

- First local setup and cleanup.
- Any new runtime-visible Telegram command.
- OCR milestones and confidence-flow changes.
- Reminder milestones and follow-up behavior.
- Settings screen changes.
- Documentation milestones such as `AI_CONTEXT`, audits, and backlog files.
- Story system changes, but only as supporting evidence, not as the main story.

## What to hide

- `.env` contents.
- Bot tokens, API keys, cookies, and private URLs.
- Database dumps and raw backup archives if they include personal data.
- Private Telegram chats and contact names.
- Phone numbers, user IDs, and medical details that should stay private.
- Temporary debug files, terminal history, and shell commands with secrets.

## How to make public screenshots

- Keep one clear subject per screenshot.
- Crop away the taskbar, private tabs, and unrelated window clutter.
- Prefer readable zoom over full-screen noise.
- Keep the version label visible when it helps the story.
- Use the same general framing across similar milestones so the timeline feels consistent.

## Telegram Web recommendations

- Keep Telegram Web visible when a live Telegram frame is needed.
- Leave the bot chat open and collapse private chat lists.
- Use a public test conversation when possible.
- Keep the message that triggered the bot response visible in the same crop.
- If the screenshot needs a live reply, switch the assistant window to Telegram Web before capturing.

## Codex recommendations

- Show the terminal, editor, or diff area when the implementation itself is the point.
- Keep the command output visible if it explains the milestone.
- Avoid screenshots that expose secrets in environment variables or logs.
- Side-by-side Codex plus Telegram screenshots are useful for "build and verify" moments.

## story.html recommendations

- Use story.html screenshots to show the generated presentation, version filters, and timeline metrics.
- Keep the browser zoom at a readable level.
- Capture the story page after regeneration so the screenshot matches the current story data.
- If the page is the milestone itself, keep the browser clean and avoid overlapping windows.

## Practical checklist

- Is the screenshot real?
- Is the subject obvious in one glance?
- Are secrets hidden?
- Does the caption explain why this frame matters?
- Would this screenshot still make sense to someone who was not present during the session?


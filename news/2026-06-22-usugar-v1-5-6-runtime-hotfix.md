---
type: bugfix
project: uSugar
series: usugar
title: Runtime hotfix for bot startup
version: 1.5.6
repo_url: https://github.com/sunpole/uSugar
image: 2026-06-22-usugar-v1-5-6-runtime-hotfix.png
---

# uSugar 1.5.6

Bugfix release after the OCR mode and smart text input update.

What changed:
- fixed a startup crash in the insulin text fallback on the current Windows Python runtime;
- replaced a too-new type hint with a compatible `Optional[str]`;
- confirmed the bot can start again with the current Windows virtual environment;
- kept OCR, formulas, database schema, tokens, and production settings unchanged.

This patch is intentionally small: it only restores runtime compatibility and keeps the 1.5.5 UX work intact.

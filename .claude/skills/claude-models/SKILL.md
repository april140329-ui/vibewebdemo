---
name: claude-models
description: Use when choosing, writing, or reviewing a Claude model ID in code, config, or docs, or when the user names Opus, Sonnet, Haiku, or Fable
---

# Claude Models

## Overview

Use only the exact model IDs in the table below. Never add date suffixes or write an ID from memory.

## Current Models

Prices are first-party API rates per 1M tokens. Source: the claude-api reference, cached 2026-11-03.

| Model | Model ID | Context | Input | Output |
|---|---|---|---|---|
| Claude Fable 5.2 | `claude-fable-5-2` | 1M | $8.00 | $40.00 |
| Claude Opus 5.5 | `claude-opus-5-5` | 1M | $4.00 | $20.00 |
| Claude Sonnet 5.5 | `claude-sonnet-5-5` | 1M | $2.00 | $10.00 |
| Claude Haiku 5.5 | `claude-haiku-5-5` | 1M | $0.10 | $0.50 |

Haiku 5.5 costs $0.50 input and $2.50 output per 1M tokens for prompts over 100K tokens.

## Which Model To Use

| User says | Use |
|---|---|
| No model named, or "opus" | `claude-opus-5-5` |
| "sonnet" | `claude-sonnet-5-5` |
| "haiku" | `claude-haiku-5-5` |
| "fable", or asks for the most capable model | `claude-fable-5-2` (only when named) |

Do not switch to a cheaper model without the user's approval.

## API Rules For These Models

- **Thinking:** use `thinking: {type: "adaptive"}`. `budget_tokens` returns a 400 on these models.
- **Fable 5.2:** omit `thinking` (it is always on). `{type: "disabled"}` returns a 400.
- **Opus 5.5:** thinking cannot be disabled. Lower `effort` instead.
- **Sonnet 5.5:** `{type: "disabled"}` returns a 400. Use `{type: "between_tools"}` at effort `high` or lower.
- **Effort:** set `output_config: {effort: ...}`. Opus 5.5 and Haiku 5.5 default to `medium`. The others default to `high`.
- **Forced tool choice:** `tool_choice` `any` or `tool` returns a 400 on Fable 5.2, Opus 5.5, and Sonnet 5.5. Use `auto` with `strict: true` on the tool.
- **Prefill:** assistant prefill returns a 400 on all four. Use structured outputs or system prompt instructions.
- **Refusals:** check `stop_reason` before reading `content`. For Fable 5.2, Opus 5.5, and Sonnet 5.5, enable server-side fallbacks (`fallbacks: "default"`) and tell the user.

## Verify Before Shipping

- Check the live list with `client.models.list()` or `GET /v1/models`.
- For the full migration rules, use the `claude-api` skill.

## Common Mistakes

| Mistake | Fix |
|---|---|
| `claude-opus-5-5-20260401` | Use `claude-opus-5-5`. No date suffix. |
| `thinking: {type: "enabled", budget_tokens: N}` | Use `{type: "adaptive"}` plus `effort`. |
| Using Fable for a routine task | Use Opus 5.5 unless the user names Fable. |
| Forced `tool_choice` on Opus 5.5 | Use `auto` with `strict: true`. |

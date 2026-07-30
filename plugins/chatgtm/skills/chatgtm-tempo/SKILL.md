---
name: chatgtm-tempo
description: >
  Produce the daily ChatGTM tempo report — pulled, signalled, blocked, drafted,
  sent, replied — per rep, and optionally post it to team chat. Use when the user
  says "run the tempo report", "daily tempo", "how did outbound go today", or on
  a daily schedule. Stage 12 of the pipeline.
metadata:
  version: "0.3.0"
---

# ChatGTM — Tempo report

Read `chatgtm-core/SKILL.md` and `references/guardrails.md` first. This skill is
read-only: it reads the local working lists, `~~mail` (sent/replied), and `~~crm`
(read-only, for stages/orders). It never writes to the CRM (G2).

## What the report shows

For the period (default: today), per rep:

- **Pulled** — prospects sourced.
- **Signal-ready / Blocked** — with the **block rate** called out.
- **Drafted / Queued** — voice-passed drafts created.
- **Sent** — drafts the rep actually sent (from `~~mail`).
- **Replied** — and reply classification breakdown.
- **Follow-ups advanced** and **orders touched**.
- **Hold-verify** — signal-ready prospects still waiting on a verified email.

## The one interpretive rule

**Zero blocks is a warning, not a win.** If the run shows 0 blocks, flag it
prominently — it means signal research likely stopped short or accepted invented
hooks. Healthy outbound blocks a real fraction of prospects.

## Steps

1. Gather the day's numbers from the working lists, `~~mail` (sent/replied), and
   `~~crm` (stages, orders).
2. Compute the rollups and the block rate.
3. Write a short report file to the outputs folder (feed the same numbers to
   `chatgtm-dashboard` if a board refresh is wanted).
4. If `~~chat` is connected, post it to the team channel — lead with anything
   needing attention (zero-block warnings, hot replies, hold-verify backlog).

## Output

A tight daily rollup: the numbers, the block-rate call-out, top replies needing
action, and the hold-verify backlog. Keep it skimmable.

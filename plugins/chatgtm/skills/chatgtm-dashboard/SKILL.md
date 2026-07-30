---
name: chatgtm-dashboard
description: >
  Regenerate the one-screen ChatGTM status board as a view-only HTML file,
  pulling the latest run data into a single screen. Use when the user says "show
  the dashboard", "regenerate the board", "one-screen view", or "status board".
  Renders on demand from live data — it is a snapshot, not an interactive app.
metadata:
  version: "0.3.0"
---

# ChatGTM — Dashboard (HTML status board)

Read `chatgtm-core/SKILL.md` and `references/guardrails.md` first. This skill is
read-only and renders locally: it reads working lists, `~~mail`, and `~~crm`
(read-only), and writes a single HTML file to the local outputs folder. No CRM
writes, no sends, no external endpoints (G1–G4).

## What this is (and isn't)

The agent is the engine — the dashboard is a **view-only snapshot** regenerated
on demand from the latest run data. It has no live action buttons (all actions
run through the skills so the gates are always enforced). To refresh, run this
skill again.

## What the board shows (one screen)

- **Segment lock** — the leadership-owned segment, geo, titles, and status
  (CONFIRMED / PROPOSED). Show PROPOSED in a warning colour.
- **Funnel** — pulled → signal-ready → blocked → drafted → sent → replied, with
  the **block rate** (zero blocks rendered as a warning) and the hold-verify count.
- **Drafted** — prospects with a queued draft, each with its one-line signal + source.
- **Blocked** — with reasons.
- **Needs attention** — hot replies, hold-verify backlog, any Gate-03 flagged drafts.
- **Sources** — which connected tools are live (`~~crm`, `~~mail`, `~~enrichment`,
  and any optional ones), which are not.
- **Sales process** — the twelve-stage flowchart and four gates from
  `references/sales-process.md`, with a print button for reps.

## Steps

1. Gather current numbers from the working lists, `~~mail` (sent/replied), and
   `~~crm` (stages), plus the lock status from `company-config.md`.
2. Render a single self-contained HTML file (inline CSS, no external calls; the
   only interactive element is a print button). Use a clean neutral theme; if the
   company has brand colours in its config, use those.
3. Save to the outputs folder and present it. Offer to publish it view-only if the
   team has a sharing connector.

## Guardrails

- View-only. No send/write buttons in the HTML.
- Show the honest state: label PROPOSED locks, not-connected tools, hold-verify
  backlog, and zero-block warnings rather than hiding them.

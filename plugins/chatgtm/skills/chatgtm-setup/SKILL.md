---
name: chatgtm-setup
description: >
  First-run onboarding wizard for ChatGTM. Connects the team's own tools (CRM,
  Gmail or Outlook mailbox, and an enrichment provider such as Apollo, Clay, or
  FullEnrich), verifies each, captures the company and its leadership-owned
  segment, writes the company config, and runs a small test pull. Use when the
  user says "set up ChatGTM", "get started", "onboard us", "connect my tools",
  "configure ChatGTM", or whenever another chatgtm-* skill reports that setup is
  incomplete (no company-config.md or no connected tools).
metadata:
  version: "0.3.0"
---

# ChatGTM — Setup (onboarding wizard)

Read `chatgtm-core/SKILL.md` and `references/guardrails.md` first. This skill gets
a new team from zero to a first reviewable draft. Keep the conversation in plain
language — the person running this may not be technical. Be encouraging and concrete.

Welcome them: ChatGTM runs high-volume outbound that still reads as human, on
top of the team's own tools. Nothing is ever sent automatically. Setup takes a
few minutes.

State the guardrails up front, plainly, because they shape how tools are connected:

- It only ever creates **draft** emails in your mailbox. It never sends.
- It has **read-only** access to your CRM — it looks at history, never writes.
- Sent emails reach your CRM through your CRM's own **email integration**, not
  through this tool.
- It shares only your **target filter** (segment, country, job title) with an
  enrichment provider. Your contacts and CRM data are never uploaded anywhere.
- Everything else stays local to your workspace.

## Step 1 — Which tools does the team use

Use AskUserQuestion to capture one tool per category. Ask all three required
categories together; the two optional ones can follow or be skipped.

- **CRM (required)** — "Where do you track contacts and deals?" Options:
  HubSpot, Salesforce, Attio, Pipedrive, Close.
- **Mailbox (required)** — "Where should draft emails be created for review?"
  Options: Gmail, Outlook / Microsoft 365.
- **Enrichment (required)** — "What do you use to find and enrich prospects?"
  Options: Apollo, Clay, FullEnrich, Hunter, ZoomInfo.
- **Team chat (optional)** — Slack, Microsoft Teams, or skip.
- **Market intelligence (optional)** — Crunchbase, PitchBook, LinkedIn, or skip.

Record their answers; these map to `~~crm`, `~~mail`, `~~enrichment`, `~~chat`,
`~~market intel`.

## Step 2 — Connect each tool

For each chosen tool that is not already connected, guide them to connect it in
Cowork:

> Open **Settings → Connectors → Add connector**, search for **<tool>**, and
> sign in when the tool's own login window appears. Come back here when it says
> connected.

Rules that matter, state them plainly:

- The team signs in through each tool's own login window. Never ask for or type
  their password, API key, or token in the chat — the connection is authorised
  in the tool's own window.
- Drafts are created in **their** mailbox and sent by a human. ChatGTM never
  sends mail on its own.
- They can connect more tools later; the three required ones are enough to start.

**Least-privilege tokens — recommend this at connection time (enforces the guardrails structurally):**

- **CRM** — connect with a **read-only** token/scope. ChatGTM only reads history;
  a read-only token makes the "no CRM writes" guardrail impossible to breach even
  by accident. Confirm their CRM's email integration is on, so sent mail syncs to
  the CRM automatically (that is how outbound gets recorded — not via ChatGTM).
- **Mailbox** — connect with **compose/drafts** scope, not a send-only automation
  scope. ChatGTM needs to create drafts and read replies, not to send.
- **Enrichment** — a standard search/enrich token is fine; ChatGTM only ever
  sends it the target filter (segment, country, job title).

If a tool they use is not available as a connector, tell them honestly and offer
the closest connected alternative (for example, if their enrichment tool has no
connector, another connected enrichment provider can do the pull).

## Step 3 — Verify each connection

Once they say a tool is connected, probe it with one cheap call and confirm it
responds:

- `~~crm` — a small **read** (record search). Also confirm the connection is
  read-only where the tool reports scope; if it exposes write tools, note that
  ChatGTM will not use them regardless.
- `~~enrichment` — a tiny people search (1–2 results, no paid reveal), sending
  only the target filter.
- `~~mail` — confirm draft/compose access (do not create a real draft yet).

Report a simple connected / not-connected (and read-only where known) line per
tool. If something fails, walk them back through Step 2 for that tool. Do not
proceed to a real run until the three required categories verify.

## Step 4 — Capture the company and segment

Copy `references/company-config.template.md` to `references/company-config.md`
and fill it in with the team:

- Company name, what they sell, the one-line value.
- The connected tool recorded per category.
- The **SEGMENT LOCK** — segment, geo, job titles, exclusions. Confirm who the
  go-to-market owner is; the lock stays `PROPOSED` until that person confirms it.
- Signal sources to hunt for their segment (be specific: which tender portals,
  which job boards, which funding/policy sources).
- Voice notes — audience, what to lead with, the one-sentence differentiator,
  words to avoid.

## Step 5 — Test pull (proof it works)

With the GTM owner's OK on the lock, run a tiny pull (3–5 prospects) via
`chatgtm-pull`, then `chatgtm-signal` on them, and show the result WITHOUT
drafting. This proves the connectors work and shows how Gate 02 blocks
prospects with no verifiable signal. Then explain the normal flow:

> Say things like "pull 25 <segment> prospects, find signals, draft the emails,
> and show me the dashboard." I create drafts in your mailbox for you to review
> and send. I never send anything myself.

## Output

A short setup summary: tools connected (per category), company and segment
captured, lock status and owner, and the test-pull result. Offer to schedule a
daily tempo report and to do the first full run when they are ready.

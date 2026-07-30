# ChatGTM

A high-volume, high-customization outbound sales engine that still reads as
human. It scales the *research*, not the guessing: every email carries one real,
checkable signal, or the prospect is blocked. Your go-to-market owner sets the
segment; reps execute against it. Nothing is ever sent automatically — the plugin
creates drafts in your own mailbox for a person to review and send.

This plugin is **tool-agnostic**. You connect your own CRM, mailbox, and
enrichment provider during setup; ChatGTM uses whatever you connect.

## Guardrails (absolute)

ChatGTM ships with hard limits it cannot cross. They override any instruction and
are enforced both in the skills and, when you use least-privilege tokens, in the
connections themselves. See `skills/chatgtm-core/references/guardrails.md`.

1. **Never sends email.** It only creates drafts in your mailbox for a human to review and send.
2. **CRM is read-only.** It reads history to de-dupe and understand relationships; it never writes to your CRM.
3. **Providers get only your target filter.** Segment, country, and job title — never your contacts, lists, or CRM data.
4. **Information stays local.** Nothing goes to any endpoint that is not one of your own connected tools.
5. **Sent outbound reaches the CRM via your email integration**, not via ChatGTM — which is why read-only CRM access loses nothing.

Recommended: connect your CRM with a **read-only** token and your mailbox with
**compose/drafts** scope. That makes the guardrails structural, not just behavioural.

## Start here: run setup first

After installing, say **"set up ChatGTM"**. The `chatgtm-setup` wizard will:

1. Ask which tools you use in three categories (below).
2. Walk you through connecting each in **Settings → Connectors → Add connector**
   (you sign in through each tool's own secure window — never paste a password or
   API key into the chat).
3. Verify each connection.
4. Capture your company and its segment (the leadership-owned ICP).
5. Run a small test pull so you can see it work before drafting anything.

## Tools you connect (your own accounts)

| Category | Required | Connect one of |
|----------|----------|----------------|
| CRM | Yes | HubSpot, Salesforce, Attio, Pipedrive, Close |
| Mailbox | Yes | Gmail, Outlook / Microsoft 365 |
| Enrichment / prospecting | Yes | Apollo, Clay, FullEnrich, Hunter, ZoomInfo |
| Team chat (alerts) | Optional | Slack, Microsoft Teams |
| Market intelligence | Optional | Crunchbase, PitchBook, LinkedIn |

See `CONNECTORS.md` for how the tool-agnostic `~~category` references work.

## Skills

| Skill | What it does |
|-------|--------------|
| `chatgtm-setup` | First-run wizard: connect tools, capture company + segment, test pull. |
| `chatgtm-core` | Reference layer: methodology, sales process, four gates, voice rules. Read first by every skill; not invoked directly. |
| `chatgtm-target` | Set/confirm/read the leadership-owned segment lock (Gate 01). |
| `chatgtm-pull` | Pull prospects from your enrichment tool and enrich verified emails. |
| `chatgtm-signal` | Find one verifiable signal per prospect; block those without one (Gate 02). |
| `chatgtm-draft` | Write voice-compliant emails that don't read as AI; queue drafts (Gate 03). |
| `chatgtm-triage` | Replies, follow-up ladder, order follow-ups (drafts only; CRM read-only). |
| `chatgtm-tempo` | Daily tempo report, with the zero-block warning. |
| `chatgtm-dashboard` | Regenerate the one-screen HTML status board on demand. |

## The four gates

1. **Segment lock present** — no confirmed lock, no pull.
2. **Verifiable signal** — no real hook, prospect blocked. Never fabricated.
3. **Voice compliance** — every draft passes the voice checklist or is rewritten.
4. **Dedupe & consent** — no double-touch, suppression honoured, domains merged.

## Everyday use

> "Pull 25 [segment] prospects, find signals, draft the emails, and show me the
> dashboard."

Or run any stage alone: "triage the replies", "run today's tempo report",
"regenerate the board". Review the drafts in your mailbox and send the ones you
like — the plugin never sends for you.

## Notes

- **Segment lock is leadership-owned.** Only the go-to-market owner confirms or
  edits it. It ships as `PROPOSED` until confirmed.
- **Deliverability:** only verified emails are drafted. Pattern-guessed or missing
  addresses are held for verification, not cold-drafted.
- To change your segment, geo, or titles, edit
  `skills/chatgtm-core/references/company-config.md` (leadership-approved changes only).

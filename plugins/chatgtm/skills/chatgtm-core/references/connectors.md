# Connectors & Enrichment Waterfall

ChatGTM fetches everything on demand through connected tools. Nothing is cached
in the plugin. At the start of a run, report which categories are connected so
the honest state of the pipeline is visible (the dashboard "Sources" panel
mirrors this).

## Category map (tool-agnostic)

| Job | Category | Required | Access | Notes |
|-----|----------|----------|--------|-------|
| Prospect search + email enrichment | `~~enrichment` | Yes | filter-only (G3) | Apollo, Clay, FullEnrich, Hunter, ZoomInfo |
| CRM history | `~~crm` | Yes | **read-only (G2)** | HubSpot, Salesforce, Attio, Pipedrive, Close |
| Mail drafts (never auto-send) | `~~mail` | Yes | compose/drafts (G1) | Gmail, Outlook / Microsoft 365 |
| Team alerts | `~~chat` | No | post message | Slack, Microsoft Teams |
| Firmographics / signals | `~~market intel` | No | filter-only (G3) | Crunchbase, PitchBook, LinkedIn |

Access is bounded by the guardrails (`guardrails.md`): the CRM is read-only,
providers receive only the target filter, and the mailbox is used for drafts, not
sends. Prefer least-privilege tokens so this is structural, not just behavioural.

The connected tool for each category is recorded in `company-config.md` during
`chatgtm-setup`. Before a run, probe each with one cheap call and confirm it
responds; report anything not connected rather than silently degrading.

## Enrichment waterfall (cheapest-first, stop when a deliverable email is found)

1. Existing `~~crm` record (read-only — already have the contact? use it, note the source locally).
2. `~~enrichment` person match using only the filter (title + geo + segment).
3. `~~enrichment` email verification for a **verified** work email.
4. If only a pattern-guessed ("extrapolated") or no email resolves, mark
   `hold-verify` and exclude from drafting. Never guess an address pattern for a
   cold send.

Only the target filter goes to the provider (G3). Never upload the company's
contacts, lists, or CRM data to `~~enrichment`.

## Read/write discipline (guardrails)

- `~~crm` is **read-only** (G2). Read it for dedupe and history; never write —
  no records, notes, stages, or fields. De-dupe/merge decisions are tracked in
  the **local** working list, not the CRM.
- Outbound activity reaches the CRM via the mailbox's own email integration (G5),
  not via ChatGTM.
- Suppression and cross-rep contact history live in the local working list.
- Never put personal data in a URL, and never send prospect data to any endpoint
  the team has not connected (G4).

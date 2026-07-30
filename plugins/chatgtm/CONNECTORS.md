# Connectors

## How tool references work

This plugin is tool-agnostic. Skill files refer to tools by **category** using a
`~~` placeholder, not by product name. When your team connects a specific tool
in Cowork, the plugin uses whatever you connected in that category. So
`~~crm` means "whatever CRM you connected" — HubSpot, Salesforce, Attio,
Pipedrive, Close, or another.

The `chatgtm-setup` skill walks you through connecting one tool per category and
records your choices so every other skill knows what to call.

## Categories for this plugin

| Category            | Placeholder     | Required? | Access            | Common options                                  |
| ------------------- | --------------- | --------- | ----------------- | ----------------------------------------------- |
| Enrichment / prospecting | `~~enrichment` | Required | filter-only | Apollo, Clay, FullEnrich, Hunter, ZoomInfo |
| CRM                 | `~~crm`         | Required  | **read-only**     | HubSpot, Salesforce, Attio, Pipedrive, Close    |
| Mailbox             | `~~mail`        | Required  | compose/drafts    | Gmail, Outlook / Microsoft 365                  |
| Team chat (alerts)  | `~~chat`        | Optional  | post message      | Slack, Microsoft Teams                          |
| Market intelligence | `~~market intel`| Optional  | filter-only       | Crunchbase, PitchBook, LinkedIn                 |

## Minimum to run

You need one tool in each of the three **required** categories: an enrichment
provider (to find and enrich prospects), a CRM (to record who was touched), and
a mailbox (where drafts are created for review). Signal research uses built-in
web search, so no separate intelligence tool is required — it just helps.

## What each is used for

- `~~enrichment` — search for people matching your segment and resolve deliverable
  work emails. **Receives only your target filter** (segment, country, job title);
  your contacts and CRM data are never uploaded (guardrail G3).
- `~~crm` — **read-only.** Read existing relationships and history to de-dupe and
  avoid double-touching. ChatGTM never writes to the CRM (G2). Connect it with a
  read-only token.
- `~~mail` — create outreach **drafts** for a human to review and send. The plugin
  never sends mail automatically (G1).
- `~~chat` (optional) — post the daily tempo report and block alerts.
- `~~market intel` (optional) — richer firmographics and signal sources (filter-only, G3).

## Guardrails

This plugin ships with absolute guardrails (see
`skills/chatgtm-core/references/guardrails.md`): never send email (drafts only),
never write to the CRM (read-only), send providers only the target filter, keep
all information local, and rely on your mailbox-to-CRM email integration to record
sent outbound. Provisioning read-only CRM tokens and compose-only mail scope makes
these structural.

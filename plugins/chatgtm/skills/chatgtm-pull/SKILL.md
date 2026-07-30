---
name: chatgtm-pull
description: >
  Pull prospects matching the locked segment from the connected enrichment tool
  and run the email enrichment waterfall. Use when the user says "pull
  prospects", "find [N] [segment] leads", "get me contacts in the segment", or
  "build the list", or after chatgtm-target confirms a lock. Stage 2–3 of the
  pipeline.
metadata:
  version: "0.3.0"
---

# ChatGTM — Pull (source + enrich)

Read `chatgtm-core/SKILL.md`, `references/guardrails.md`, and
`references/connectors.md` first.

## Guardrails that shape this skill

- **G3 — data minimization.** The only thing sent to `~~enrichment` is the target
  filter: segment, country/geo, and job title. Never upload the company's
  contacts, CRM data, lists, names, or emails to a provider. Enrichment uses only
  what the provider returns from that filter, plus identifiers the provider itself
  returned (to page or reveal within that same provider).
- **G2 — CRM read-only.** Reading `~~crm` for dedupe/history is allowed. Writing is not.
- **G4 — local.** The working list is written locally; prospect data is not sent anywhere else.

## Preconditions

- **Gate 01**: the segment lock must be `CONFIRMED`. If it is `PROPOSED`, stop
  and route to `chatgtm-target`.
- Probe `~~enrichment` and `~~crm` once and report connected vs not.

## Steps

1. **Load the lock** from `company-config.md` (segment, geo, titles, exclusions).
2. **Search** `~~enrichment` using the lock's titles + geo + segment. Respect the
   requested batch size; default to a manageable batch (25–40) rather than
   everything at once.
3. **De-dupe on the way in** (Gate 04, first pass): **read** `~~crm` to drop
   anyone already in contact, on the local suppression list, or off-segment per
   the exclusions. Reading only — never write the merge back to the CRM.
4. **Enrich** each prospect through the waterfall (cheapest first): existing
   `~~crm` contact → `~~enrichment` match → `~~enrichment` email verification.
   Keep only **verified** emails. Mark pattern-guessed or missing as
   `hold-verify` and exclude from later stages. Never guess an address pattern.
5. **Firmographics**: attach company context (size, funding, recent activity)
   from `~~enrichment` or `~~market intel` to support signal hunting.
6. **Write the working list** to a structured file in the outputs folder (one row
   per prospect: name, title, company, domain, email, email status, geo,
   firmographics, dedupe status) so `chatgtm-signal` and the dashboard can read it.

## Output

Counts: searched, matched, de-duped out, verified email, hold-verify. Name the
file written. Do NOT draft or write to CRM here. Hand off to `chatgtm-signal`.

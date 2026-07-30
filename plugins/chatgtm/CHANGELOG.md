# Changelog — ChatGTM

All notable changes to the ChatGTM plugin. Bump the `version` in
`.claude-plugin/plugin.json` and the matching entry in the marketplace's
`.claude-plugin/marketplace.json` with every release, then push.

## 0.3.0 — Guardrails
- Added canonical `chatgtm-core/references/guardrails.md` (G1–G5), loaded first by every skill and overriding all other instructions.
- G1 never sends email (drafts only). G2 CRM is read-only. G3 providers receive only the target filter (segment, country, job title). G4 information stays local. G5 outbound reaches the CRM via the mailbox email integration.
- Rewrote `chatgtm-triage` to remove all CRM writes; state now lives in a local working list.
- `chatgtm-pull` enforces data minimization to enrichment providers.
- Setup wizard now recommends least-privilege tokens (read-only CRM, compose/drafts mail) so guardrails are structural.

## 0.2.0 — External, tool-agnostic release
- Made the plugin tool-agnostic via `~~category` placeholders (CRM, mailbox, enrichment, chat, market intel).
- Added `chatgtm-setup` onboarding wizard (connect tools, capture company + segment, test pull).
- Replaced internal company configs with a single self-authored `company-config.md` template.

## 0.1.0 — Initial pipeline
- Core methodology, twelve-stage sales process, four gates, follow-up ladder, voice rules.
- Skills: target, pull, signal, draft, triage, tempo, dashboard.

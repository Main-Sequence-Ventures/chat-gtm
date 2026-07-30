---
name: chatgtm-draft
description: >
  Write voice-compliant, non-AI-sounding outbound emails from each prospect's
  verified signal, then queue them as drafts in the connected mailbox (never
  auto-send). Use when the user says "draft the emails", "write outbound", "draft
  from the signals", or after chatgtm-signal marks prospects signal-ready. Stage
  6–8 of the pipeline and the owner of Gate 03 (voice compliance).
metadata:
  version: "0.3.0"
---

# ChatGTM — Draft (voice-compliant email)

Read `chatgtm-core/SKILL.md`, `references/guardrails.md`, and
`references/voice-rules.md` first, and load the "Voice notes" from `company-config.md`.

## Guardrail that shapes this skill

- **G1 — drafts only.** Create a draft in `~~mail` for each prospect. Never send,
  schedule, or auto-release. Never use a send tool even if the connected mailbox
  exposes one. A human reviews and sends. No CRM writes here either (G2).

## Preconditions

- Only draft for prospects marked `signal-ready` by `chatgtm-signal`, AND with a
  **verified** email. Skip `hold-verify` and blocked prospects.
- Pass the voice rules into every draft **verbatim**.

## Steps

1. **Load** the signal-ready + verified-email prospects and the company voice notes.
2. **Draft each email** from that prospect's specific signal:
   - Body 45–95 words, one attributed fact, exactly one small ask.
   - Rotate the opening archetype across the batch (signal / question /
     one-line result / direct offer) so no two consecutive emails match.
   - Subject 2–6 words referencing the signal.
3. **Gate 03 — run the voice checklist** on every draft. A draft that fails is
   rewritten (max two passes); if it still fails, flag it for human rewrite
   rather than queueing it.
4. **Gate 04 — dedupe & consent** final pass: suppression, cross-rep history,
   merged records. Suppress anything that fails.
5. **Queue as drafts in `~~mail`** — create the draft only; never send. One draft
   per prospect, to the verified email, with subject and body.
6. **Stage the follow-up ladder** (rungs 2–4) as notes on each prospect so
   `chatgtm-triage` can advance non-repliers with a new reason each rung.

## Output

Per prospect: subject, body, archetype used, Gate 03 pass/flag, draft created.
Batch summary: drafts queued, rewritten, flagged, suppressed. Confirm nothing was
sent. Hand off to `chatgtm-triage`.

> Sending is a human action. This skill only creates drafts in the team's own
> mailbox for a person to review and send.

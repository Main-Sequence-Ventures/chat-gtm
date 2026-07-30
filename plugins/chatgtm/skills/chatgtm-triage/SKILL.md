---
name: chatgtm-triage
description: >
  Handle inbound replies, advance the follow-up ladder for non-repliers, and
  manage existing-customer order follow-ups — all as mailbox drafts, reading the
  CRM only for history. Use when the user says "triage replies", "handle the
  inbox", "advance follow-ups", "check on orders", or after drafts have gone out.
  Stage 9–11 of the pipeline.
metadata:
  version: "0.3.0"
---

# ChatGTM — Triage (replies, follow-ups)

Read `chatgtm-core/SKILL.md` and `references/guardrails.md` first.

## Guardrails that shape this skill

- **G2 — CRM read-only.** This skill NEVER writes to the CRM. It may read the CRM
  to see history (prior contact, open threads, existing-customer status), nothing more.
- **G5 — sync via email.** Every reply and follow-up is created as a draft in the
  operator's own mailbox. Once a human sends it, the CRM captures the activity
  through the mailbox's own email integration. ChatGTM does not log it.
- **G1 — drafts only.** Reply and follow-up messages are drafts; a human sends them.

## Three tracks

### 1. Reply triage
Read the reply in `~~mail`, classify it, and act (all outputs are drafts or local notes):
- **Interested** → draft a fast, specific reply; if `~~chat` is connected, alert the owner.
- **Not now / timing** → draft nothing to send now; note the follow-up date in the local working list.
- **Referral** → capture the referred contact in the local working list (re-enters at `chatgtm-signal`); draft a thank-you.
- **Objection** → draft a reply answering the specific objection with a checkable fact.
- **Not interested / unsubscribe** → add to the local suppression list; stop all ladders. (Do not write to CRM.)
- **Order / commercial** → route to the order track below.

Stop every follow-up ladder the instant a prospect replies.

### 2. Follow-up ladder (non-repliers)
Advance each non-replier one rung using the staged notes from `chatgtm-draft`.
Every rung must carry a **new** reason (a second signal, a proof point, a useful
artifact, then a clean break-up). If a rung has no new reason, skip it. Create the
rung as a draft in `~~mail`; never auto-send.

### 3. Existing customers & orders
- **Order follow-up**: draft a check-in confirming receipt / next milestone before
  the customer has to chase. Read the CRM for order history; draft, do not log.
- **Reply-to-existing**: flag product/support replies for the owner; never leave a
  customer reply in the outbound queue.

## State & suppression (local)

Track stage, follow-up dates, suppression, and dedupe in the **local working
list** in the outputs folder — not in the CRM. Read the CRM only to avoid
double-touching someone already in contact (Gate 04).

## Output

A triage summary: replies by class, follow-ups drafted, orders touched,
suppressions added (locally), and any prospect needing a human decision. Confirm
that nothing was sent and nothing was written to the CRM.

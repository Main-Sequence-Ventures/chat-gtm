# The ChatGTM Sales Process

The optimal outbound process, applied to every prospect. Twelve stages, four
hard gates, one follow-up ladder. The `chatgtm-dashboard` skill renders this as
a printable flowchart for reps.

## Twelve stages

1. **Segment lock** — GTM owner sets segment, geo, titles, exclusions. Locked. `[GATE 01]`
2. **Pull** — query `~~enrichment` for people matching the lock. De-dupe on the way in.
3. **Enrich** — resolve a deliverable work email. Verified only; hold guesses.
4. **Signal hunt** — one verifiable, attributable signal per prospect. `[GATE 02]`
5. **Block or pass** — no signal → block with a reason. Signal → pass to draft.
6. **Draft** — write from the signal, in the company voice. `[GATE 03]`
7. **Dedupe & consent** — suppression, cross-rep history, `~~crm` dupes. `[GATE 04]`
8. **Queue** — create the draft in `~~mail` (never auto-send). Rep reviews and sends.
9. **CRM sync** — when the rep sends, the mailbox's email integration records it in `~~crm`. ChatGTM does not write to the CRM (read-only, G2/G5).
10. **Reply triage** — classify replies (interested / not now / referral / objection / order).
11. **Follow-up ladder** — advance non-repliers one rung; stop on any reply.
12. **Tempo report** — daily roll-up of pulled / signalled / blocked / drafted / sent / replied.

## The four gates in flow

```
Segment lock ──[GATE 01: lock present?]── no ─▶ STOP → route to GTM owner
     │ yes
     ▼
  Pull → Enrich (verified email?)
     │
     ▼
 Signal hunt ──[GATE 02: verifiable signal?]── no ─▶ BLOCK (record reason)
     │ yes
     ▼
   Draft ──[GATE 03: voice compliant?]── no ─▶ REWRITE (loop, max 2x)
     │ yes
     ▼
 Dedupe ──[GATE 04: unique + consented?]── no ─▶ SUPPRESS (merge / drop)
     │ yes
     ▼
  Queue draft → (rep sends → email integration syncs to CRM) → Triage → Follow-up ladder → Tempo report
```

> CRM is read-only throughout (G2). The only path data enters the CRM is the
> operator's own mailbox-to-CRM email integration after a human sends (G5).

## Follow-up ladder

Stop the instant a prospect replies. Each rung must carry a **new** reason to
write — never "just bumping this" or "circling back". If a rung has no new
signal, skip it rather than pad it.

| Rung | Timing | What it must be | What it must NOT be |
|------|--------|-----------------|---------------------|
| 1 | Day 0 | The signal email | Generic intro |
| 2 | Day 3–4 | A second, different signal or a specific proof point | A restatement of email 1 |
| 3 | Day 8–10 | A short, useful artifact (relevant case, one-line result) | "Did you see my last email?" |
| 4 | Day 16–18 | A single clean break-up with an easy no | Guilt, urgency, or a fake deadline |

## Existing-customer / order ladder

For current customers (orders, renewals, expansion), triage handles two extra tracks:

- **Order follow-up** — confirm receipt, next milestone, and a proactive check-in
  before the customer has to chase. Log to `~~crm`.
- **Reply-to-existing** — route product/support replies to the owner; never let a
  customer reply sit in the outbound queue.

## Anti-AI-detection principles (why this reads human)

- One checkable fact per email, attributed to where it came from.
- 45–95 words. One small ask. No stacked CTAs.
- Rotating opening archetype so a batch does not look cloned.
- Banned tokens and tells enforced verbatim (see `voice-rules.md`).
- If research cannot find a real hook, the prospect is blocked — not guessed at.

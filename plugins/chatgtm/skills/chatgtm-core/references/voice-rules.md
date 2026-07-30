# Voice Rules (passed verbatim into every draft)

These rules go into the draft prompt word-for-word. Gate 03 checks a draft
against every item before it can be queued. A draft that fails any hard rule is
rewritten (max two passes), never sent as-is.

## Hard bans (any occurrence = fail)

- No em dashes. Use a full stop or a comma.
- No "I hope this email finds you well", "I wanted to reach out", "circling back",
  "just following up", "touching base", "as per", "leverage", "synergy",
  "cutting-edge", "seamless", "revolutionary", "game-changer", "unlock",
  "in today's fast-paced world", "at the end of the day".
- No "I noticed you..." opener unless the thing noticed is the attributed signal.
- No three-item rule-of-three lists ("faster, cheaper, better").
- No emoji. No exclamation marks. No ALL-CAPS words.
- No fake personalisation ("as a fellow [role]", "I've long admired...").
- No stacked CTAs. Exactly one ask.

## Hard requirements (all must be true)

- **One verifiable fact**, attributed to its source in the prospect's world
  (a tender number, a job ad, a funding round, a paper, a policy change). This is
  the signal from Gate 02.
- **Length 45–95 words** in the body, excluding signature.
- **One ask**, small and concrete (a 15-minute call, a yes/no, a pointer to the
  right person). Never "let me know your thoughts".
- **Rotating opening archetype** across a batch — cycle through: the signal
  itself / a specific question / a one-line relevant result / a direct offer.
  No two consecutive emails in a batch may share an archetype.
- **Plain sign-off**: first name, or first name + company. No title stack.
- **Subject line**: lowercase or sentence case, 2–6 words, references the signal,
  no "Quick question" / "Following up".

## Tone

Write like a smart, busy human who did their homework and respects the reader's
time. Short sentences. Concrete nouns. No throat-clearing. It should be
impossible to tell the email came from a batch.

## The checklist (Gate 03 runs this on every draft)

```
[ ] No banned token present
[ ] Exactly one verifiable fact, attributed
[ ] Body 45–95 words
[ ] Exactly one ask
[ ] Opening archetype differs from previous email in batch
[ ] No em dash, emoji, or exclamation mark
[ ] Subject 2–6 words, references signal
```

## Company voice notes

Load the "Voice notes" section of `references/company-config.md` and apply it on
top of these rules — it captures the tone, proof points, and vocabulary specific
to this company and its buyers.

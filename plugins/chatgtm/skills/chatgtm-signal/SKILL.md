---
name: chatgtm-signal
description: >
  Find exactly one verifiable, attributable signal per prospect (tender number,
  job ad, funding round, paper, launch, policy) and block any prospect without
  one. Use when the user says "find signals for the list", "research hooks",
  "signal hunt", or after chatgtm-pull produces a list. Stage 4–5 of the pipeline
  and the owner of Gate 02 — the load-bearing gate.
metadata:
  version: "0.2.0"
---

# ChatGTM — Signal (verifiable hook research)

Read `chatgtm-core/SKILL.md` first. This skill owns **Gate 02**, the single most
important rule in ChatGTM: **no verifiable signal, no email.** Never invent a hook.

## What counts as a signal

A fact that exists in the prospect's world and can be checked by a third party,
with a source you can name:

- A tender / procurement notice with a number.
- A specific job ad the prospect's org posted (role + rough date).
- A funding round, acquisition, or deal (with date).
- A paper, talk, patent, or public post authored by the prospect.
- A product launch, regulatory change, audit finding, or policy that hits them.

The company config's "Signal sources to hunt" lists the best places to look for
this segment. Start there.

## Steps

1. **Load the working list** from `chatgtm-pull`.
2. For each prospect, hunt the config's signal sources using web search (and
   `~~market intel` if connected). Prefer the most recent, most specific,
   most checkable item.
3. **Record the signal**: one sentence, plus its source (URL / tender number /
   date). If you cannot verify it, it does not count.
4. **Gate 02 decision**:
   - Signal found → mark `signal-ready`, attach signal + source.
   - No verifiable signal → mark `BLOCKED` with the reason. Does not proceed.
   - Also confirm the signal matches the prospect's actual org and geo — a signal
     for the wrong entity is not a signal.
5. **Update the working list** in place with signal, source, and status.

## Guardrail on block rate

A batch with **zero blocks is suspicious**, not excellent — flag it. Report the
block rate and call out any prospect where the signal is thin so a human can judge.

## Output

Per-prospect: signal (one line), source, and status (`signal-ready` / `BLOCKED`
+ reason). Summary: N signal-ready, N blocked, block rate. Hand off signal-ready
prospects to `chatgtm-draft`.

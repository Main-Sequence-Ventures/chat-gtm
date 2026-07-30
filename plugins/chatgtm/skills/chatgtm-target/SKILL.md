---
name: chatgtm-target
description: >
  Set, confirm, or read the leadership-owned segment lock (segment, geo, job
  titles, exclusions) before any prospecting. Use when the user says "set our
  segment", "what's our target", "lock the ICP", "confirm the segment", or starts
  a ChatGTM run and no lock is confirmed yet. Stage 1 of the pipeline and the
  owner of Gate 01.
metadata:
  version: "0.2.0"
---

# ChatGTM — Target (segment lock)

Read `chatgtm-core/SKILL.md` first. This skill owns Gate 01: no confirmed
segment lock, no pull. If setup has not been run, route to `chatgtm-setup`.

## Governance rule

The segment lock is **leadership-owned** (founder, head of sales, or CEO). Reps
and this skill may *read* the lock and *propose* changes, but only the go-to-market
owner may *confirm* or *edit* it. Never pull at volume against a lock whose status
is still `PROPOSED`.

## Steps

1. **Read** `references/company-config.md`. If it does not exist, run `chatgtm-setup`.
2. **Show the current lock** (segment, geo, job titles, exclusions, status, owner).
3. **If status is PROPOSED**: present it for confirmation. Confirm only with the
   GTM owner (or explicit confirmation that they approved). On confirm, update
   the config's `SEGMENT LOCK` block: `Status: CONFIRMED — <name>, <date>`.
4. **If someone wants a change**: capture it as a proposal; apply it only if the
   requester is the GTM owner or has their approval. Otherwise route to them.
5. **On confirm**, hand off: the lock is set and `chatgtm-pull` can run.

## Output

A short confirmation block: company, the four lock fields, status, and who owns
and confirmed it.

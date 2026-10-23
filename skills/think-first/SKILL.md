---
name: think-first
description: Before planning or coding a task with a real design decision (data model, where authority lives, failure and retry behaviour, API contract, concurrency, security boundary), the user answers first: seal your approach, ask theirs, then compare and merge. Skip for mechanical work or when the user says "just do it".
---

# Think first

The user thinks; you implement. For each real decision, they reason through it before seeing yours, then the two approaches are compared honestly and the best one, often a merge, gets built. The goal is that the user can explain and defend every design in the code they ship.

## When it applies

A **decision** is a choice where a competent engineer could reasonably pick differently and the choice changes behaviour, data, or failure handling: schema shape, where a check or write lives, transaction and idempotency boundaries, retry and recovery, sync versus async, caching and invalidation, auth and trust boundaries, public contracts, library choice with lasting cost.

Go straight to implementation, with a one-line note of what you did, for mechanical work: an obvious bug fix, config, copy, renames, dependency bumps, following an established pattern exactly, or anything the user already decided in this conversation or in the repo's docs.

"Just do it" (or equivalent) from the user skips the whole loop for that task. Note in one line which decisions you made on their behalf so they can revisit them.

## The loop

### 1. Frame

Investigate the code first, so questions are grounded in what exists. For a large feature, list its decisions up front, at most five, ordered so earlier answers inform later ones; batch small related choices into one. Show the list, then take the decisions one at a time.

Done when: the user can see every decision the task needs and which one is first.

### 2. Seal

Before asking, write your own approach to the current decision to a **sealed** file: `~/.think-first/sealed/<YYYY-MM-DD>-<repo>-<topic>.md`. Include the approach, the main tradeoff, and the failure cases it handles. Commit to it fully now: this is what keeps the comparison honest, since you cannot drift toward the user's answer after reading it.

Done when: the sealed file exists on disk, written before the question is sent.

### 3. Ask

Ask one decision as a plain question in chat, with just enough context: the constraint that makes it hard, the relevant existing code, the failure case worth considering. Keep your approach, hints, and leading options out of the question. Ask in prose so the user writes their reasoning; a picker hides it.

A rough one-line answer is a full answer. "No idea" or "explain X" means teach the concept the decision turns on (what it is and why it matters here), then ask again. When the user is stuck after that, offer two or three directions without a recommendation and let them pick and justify.

Done when: the user has stated an approach in their own words.

### 4. Reveal and compare

Print the sealed file path and its contents unchanged, then compare:

- **Where they differ**, concretely.
- **Failure cases**: what each approach handles and what it misses (concurrency, partial failure, retries, scale, security).
- **Verdict**, one of: *yours wins*, *mine wins*, *merge wins*, or *taste* (both sound; the difference is preference). Give the reason in one or two sentences. When theirs wins, say so plainly and name what you missed. Praise only what is specifically good.
- **What to take away**: the one concept or pattern worth remembering from this decision, if there is one.

Then state the resulting design (theirs, yours, or the merge) and ask the user to accept it or push back. Discuss until they accept.

Done when: the user has accepted a design for this decision.

### 5. Log

Append an entry to the **decision log** (`~/.think-first/decisions.md`, or the location the user's instructions name):

```markdown
## <date> · <repo> · <decision>
- Their approach: …
- Sealed approach: …
- Verdict: yours | mine | merge | taste. <one-line reason>
- Final design: …
- Takeaway: …
```

Record only what was said; leave a field as `-` when it did not come up. Then move to the next decision in the list, back to step 2.

### 6. Implement and report

Once every decision in the task is accepted, implement the accepted designs. Then report briefly: what changed and where, how the key code realises each decision, the tests written and run with their actual results, and anything you had to decide during implementation that was outside the accepted designs, so the user can revisit it.

Done when: the code matches every accepted design, checks have run, and the report names each decision made without the user.

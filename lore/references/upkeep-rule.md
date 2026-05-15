# LORE.md upkeep rule

Paste this section into your project's `CLAUDE.md` or `AGENTS.md` (or wherever your agent reads its instructions). It teaches the agent to keep `LORE.md` alive — the chronicle of *why we are here* — through every change-set landing on `main`.

This rule is the load-bearing piece. The `/lore` skill is the interactive chronicler you call up to read or enrich the chronicle; the rule is what keeps the chronicle fresh in the first place.

---

## LORE.md — the chronicle

`LORE.md` is the canonical source for *why we are here*. It sits alongside the other canonical docs:

- `CHANGELOG.md` — *what shipped, when*
- `CLAUDE.md` (or `AGENTS.md`) — *how it works*
- `TODOS.md` — *what's next*
- **`LORE.md`** — *why we are here*

Each entry is dated and grounded in a real PR, doc, conversation, or memory. Most entries are 4–6 lines; substantive moments earn 150–250 words. Refactors, typos, and dependency bumps usually earn nothing.

### Entry shape

```
### YYYY-MM-DD — Short evocative title

**What happened.** Specific dates, specific people, specific decisions.
**Why it mattered.** What this changed about scope, audience, urgency, or how we saw the problem.
**What it cost / opened.** Tradeoffs treated honestly.
**Reference.** PRs, docs, threads, memory files.
```

### Voice

Chronicler-historian. Direct, factually grounded, evocative without being flowery. Long-form profiles, oral histories, well-crafted biographies are the reference points — narrative anchored to specific people, places, and decisions. Every mood-word ties to a concrete event.

**It's a chronicle of the project, not a personal diary of the user.** Be literary when it serves the moment; don't lean on personal-feeling phrases that crowd out the collective story.

### Era boundaries

A new era begins when the project's center of gravity shifts — the audience changes, the architecture changes, the relationship to leadership changes, the shape of the work changes. Roughly 1–2 per quarter. Default-to-entry when unsure; eras are easier to merge later than to split.

Each era opens with one italicized paragraph that sets the scene, and closes (when it does) with a "What this era taught us" summary line.

---

## When to add an entry — the change-set boundary

Run a LORE check whenever a change-set is about to land on `main`. Three common shapes:

1. **PR-based workflow** → run the LORE check on the branch's diff *before opening the PR*. Bundle the entry into the same merge.
2. **Direct push to main** → run the LORE check on the commit(s) *before `git push`*.
3. **Commit-and-push as one motion (the vibe-coder default)** → run the check *before the commit*, so the entry lands in the same commit as the code change.

Trigger across all three: this change-set is about to be permanent on `main`.

### The check

For each change-set, ask: **does this represent a moment worth chronicling?** A decision, a tradeoff, a conversation, a shift in how we see the problem.

- **If yes:** add a date-headed entry under the current era following the entry shape above.
- **If no:** say "**No LORE entry needed**" by name. Silence is not a pass — explicit absence is the discipline that prevents the file from going stale.

**Granularity calibrates retroactively.** Write entries at the granularity that fits the moment — you can't always tell which moments will become part of a larger arc. The `/lore` skill scans for over-clustered topics when invoked and will offer to roll them up under one umbrella entry, preserving the chronological detail as a compact bullet list. The bar is long-term human readability at the right altitude.

---

## Proactively invoke `/lore` at substantive change-sets

For most change-sets the LORE check is a quiet pass — an entry or "No LORE entry needed" by name. But at **substantive** change-sets the chronicle gets *much* richer if a primary source is consulted while they're in front of you.

Substantive means:

- A minor or major version bump (not a patch).
- A multi-module change.
- A long-running PR finally landing.
- A leadership-conversation moment, a pivot, or a clear shift in direction.

When you detect a substantive change-set, surface ONE structured prompt to the user (AskUserQuestion-style if available, otherwise inline):

> *(specific fact about the change — e.g. "This PR was open 12 days and went through 3 review rounds. It renames the module and closes 4 deferred items.")* The chronicler can ask one quick question to make the LORE entry richer for posterity. Want to take 30 seconds?
>
> [1] Yes — ask me  
> [2] Skip this time

If yes → invoke `/lore` with the change-set context. The chronicler will ask one grounded journalist-question, weave the answer into the entry, show the diff, apply on approval.

If skip → write a baseline entry (or "No LORE entry needed" by name) and continue.

**Do NOT trigger the prompt on patches, single-file fixes, doc-only commits, or anything routine.** If users get prompted too often they will disable the rule — and the rule is the load-bearing piece. Reserve the proactive invocation for moments genuinely worth a journalist's attention.

---

## How the rule and the `/lore` skill work together

| Path | Trigger | Behavior |
|------|---------|----------|
| **Routine change-set** | PR / push / commit on `main` | Run the LORE check. Entry or "No LORE entry needed" by name. No skill invocation. |
| **Substantive change-set** | Minor/major bump, multi-module change, long PR landing | Surface the 30-second prompt. If yes → invoke `/lore` for one grounded question + weave. If skip → baseline entry or explicit pass. |
| **User initiates** | User types `/lore` | Conversational chronicle Q&A. The chronicler reads LORE, summarizes, takes the user's thread, probes gaps as you go. |

The rule fires on every change-set. The skill is invoked occasionally — by the rule at thresholds, or by the user when they want to talk.

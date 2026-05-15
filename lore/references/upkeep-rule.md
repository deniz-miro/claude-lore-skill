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

### Cluster check — auto-consolidation in the same change-set

After deciding "entry needed" (and drafting the entry), run one more pass: **does this entry push a cluster of related entries over the threshold?**

Look across the era the new entry sits in. A cluster = same era + topic kinship (recurring person, recurring PR series, numbered title prefix like "Stas r4", "Stas r5"…, or shared theme phrase) + tight chronology. The threshold is a judgment call:

- **3+ trivial or near-duplicate entries on the same topic** → trip.
- **5+ substantive entries on one arc** → trip.
- **Many entries each capturing a genuinely distinct moment** → don't trip; err toward the auto-consolidation only when bundling clearly improves readability without losing meaningful nuance.

**If tripped: auto-consolidate. No permission prompt.** This is doc-sync, same shape as updating CHANGELOG or marking TODOs done — not a conversation. The PR diff (or the commit, for direct-push) is the review surface. The chronicler:

1. Drafts an umbrella entry under the cluster's date span (e.g. `### 2026-05-04 → 2026-05-12 — Title`), using the standard quartet at campaign altitude.
2. Replaces the original entries with the umbrella + a `**Rounds (chronological detail).**` (or `**Milestones.**`) bullet list — one line per subsumed entry, `YYYY-MM-DD — [original title]: [one-sentence essence]`. **Preserve every original date and essence** in the bullet list.
3. **Names what it did in the doc-sync output**, explicitly:

   > *"LORE.md updated: new entry for v0.2.15, auto-consolidated 5 entries on the Stas campaign into umbrella at Era 6 (originals preserved as bullet list)."*

If the consolidation lands wrong, recovery is the same as for any other doc-sync miss: revert the commit (or follow-up PR) before merge. The original entries' essence is preserved in the bullet list either way.

**Once consolidated, don't re-consolidate.** Future entries on the same topic in the same era append to the umbrella's bullet list and update the prose if the new entries shift the through-line — don't create a sibling umbrella.

**Granularity calibrates retroactively.** Write each entry at the granularity that fits the moment — you can't always tell which moments will become part of a larger arc. The cluster check catches the moment a topic outgrows per-entry detail and rolls it up automatically; the `/lore` skill also offers retroactive consolidation when invoked manually, for any clusters that slipped through. The bar is long-term human readability at the right altitude.

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

**When a substantive change-set also trips the cluster check**, reframe the 30-second prompt around the campaign arc rather than the single new entry. The auto-consolidation runs regardless of whether the user opts into the question — but the user-supplied lesson, if given, lands in the umbrella's "Why it mattered" instead of in a per-PR entry:

> *"This PR makes the 5th entry on [topic] in [Era N] — the chronicler is consolidating these into one umbrella as part of this change-set. Want to take 30s to tell me the through-line lesson across all five? It'll land in the umbrella's 'Why it mattered.'"*
>
> [1] Yes — ask me  
> [2] Skip; distill from existing entries

**Do NOT trigger the prompt on patches, single-file fixes, doc-only commits, or anything routine.** If users get prompted too often they will disable the rule — and the rule is the load-bearing piece. Reserve the proactive invocation for moments genuinely worth a journalist's attention. (The cluster check runs on every change-set regardless of substantive-ness, but it auto-applies silently — no prompt overhead.)

---

## How the rule and the `/lore` skill work together

| Path | Trigger | Behavior |
|------|---------|----------|
| **Routine change-set** | PR / push / commit on `main` | Run the LORE check. Entry or "No LORE entry needed" by name. Then cluster check — if tripped, auto-consolidate silently and name it in the doc-sync output. No skill invocation. |
| **Substantive change-set** | Minor/major bump, multi-module change, long PR landing | Run the LORE check + cluster check (auto-consolidate silently if tripped). Surface the 30-second prompt — reframed around the campaign arc if a consolidation just happened. If yes → invoke `/lore` for one grounded question + weave. If skip → baseline entry or explicit pass. |
| **User initiates** | User types `/lore` | Conversational chronicle Q&A. The chronicler reads LORE, summarizes, takes the user's thread, probes gaps as you go. Also offers consolidation for any clusters that slipped through — diff first, applies on approval. |

The rule fires on every change-set. The cluster check auto-applies silently. The skill is invoked occasionally — by the rule at substantive moments for the 30-second journalist-Q, or by the user when they want to talk.

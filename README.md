# claude-lore-skill

A Claude Code skill + rulebook addition for keeping a `LORE.md` chronicle in your project — the *why we are here* alongside `CHANGELOG.md` (*what shipped*), `CLAUDE.md` (*how it works*), and `TODOS.md` (*what's next*).

The chronicle answers questions other docs can't: *why did we choose this? Who was in the room when this got decided? What did we almost do instead?* Future readers — collaborators, agents, future-you — pick up the project with the texture intact.

## Why this exists

CHANGELOG tells you what shipped. CLAUDE.md tells you how the code works. TODOS tells you what's next. None of them tell you the *why* — the conversations, decisions, tradeoffs, and shifts that produced the current state. After a few months on a project, the why is gone unless someone wrote it down.

`LORE.md` is where the why lives. This repo gives you two things to keep it alive:

1. **A rulebook addition** ([`lore/references/upkeep-rule.md`](lore/references/upkeep-rule.md)) — paste it into your `CLAUDE.md` or `AGENTS.md` and your agent will run a LORE check on every change-set landing on `main`.
2. **A chronicler skill** (`/lore`) — invoked manually when you want to read or enrich the chronicle, or invoked proactively by the rulebook at substantive moments (version bumps, multi-module changes, long-running PRs landing) to ask one grounded journalist-question about what was behind the change.

The two work together: the rule keeps the chronicle fresh; the skill makes it richer.

## Install

### 1. The skill

```bash
git clone https://github.com/deniz-miro/claude-lore-skill.git
cp -r claude-lore-skill/lore ~/.claude/skills/lore
```

The skill is now available as `/lore` in any Claude Code session.

### 2. The rule

Open [`lore/references/upkeep-rule.md`](lore/references/upkeep-rule.md) and paste its contents into your project's `CLAUDE.md` or `AGENTS.md`. From the next change-set forward, your agent will run the LORE check at the right moments. (When you invoke `/lore` on a project that doesn't have a chronicle yet, the skill will offer to wire this rule into your instruction file as part of bootstrap — no manual paste needed.)

## How it works in practice

### Day-to-day — the rule does the work

As you ship code, the agent runs the LORE check on each change-set landing on `main`:

- For routine commits — patches, small fixes, doc-sync — the agent says **"No LORE entry needed"** by name and continues. Silence is not a pass; explicit absence keeps the file from going stale.
- For substantive change-sets — version bumps, multi-module changes, long-running PRs landing — the agent surfaces a 30-second prompt: *"This PR was open 12 days and went through 3 review rounds. The chronicler can ask one quick question to make the LORE entry richer. 30 seconds?"* Skip is always fine.
- If you take the 30 seconds, `/lore` gets invoked with the change-set context, asks one grounded journalist-question, weaves your answer into the entry, shows the diff, applies on approval.

### When clusters accrete — retroactive altitude

The chronicle is meant to stay readable at the right altitude. In the moment of writing, you can't tell which entries will pile up; what feels like a singular moment may turn out to be one of many — a security review campaign that ends up spanning 17 rounds, a design overhaul that takes a weekend and a half, three near-identical doc-sync entries in a week.

The chronicler catches this at two moments, with different postures:

- **At each PR (upkeep flow, silent).** When the agent writes a new LORE entry as part of pre-merge doc-sync, it also runs a cluster check on the era the entry sits in. If the new entry pushes a cluster of related entries past the threshold (3+ trivial or 5+ substantive entries on the same topic), the chronicler **auto-consolidates** the cluster into one umbrella entry in the same change-set — no permission prompt, same shape as updating CHANGELOG or marking TODOs done. The umbrella keeps the standard quartet — *what happened / why it mattered / what it cost or opened / reference* — and preserves every original entry's date and essence as a compact `Rounds (chronological detail)` bullet list, so you don't lose searchability or context. The agent names what was done in the doc-sync output and commit message; the PR diff is the review surface. This is the path that catches bloat the moment it forms.
- **At `/lore` (conversational, asks first).** When you invoke the chronicler manually, it sweeps the whole chronicle for any clusters that slipped through and **offers** consolidation as the optional second beat of the opening. Diff first, applies on approval — you're in chronicle-mode, having a conversation; asking is the natural rhythm.

Once a cluster is consolidated into an umbrella, future entries on the same topic in the same era append to the umbrella's bullet list. No sibling umbrellas; no re-consolidation churn.

### When you want to read the chronicle — type `/lore`

The chronicler reads `LORE.md`, summarizes where the project is (current era, what's recent, what's ahead, what was learned), and asks what you're curious about. Conversation flows from there:

- The chronicler answers from LORE with citations to the date-headed entries.
- Every answer ends with one grounded gap-question — like a reporter who has face time with a primary source and uses it to fill the record.
- When you give new color, the chronicler verifies names and codenames before drafting (transcripts garble proper nouns), shows the diff, and applies on approval.

### When the project doesn't have a `LORE.md` yet

`/lore` offers to retrace from git history, your `docs/` folder, README, CHANGELOG, and whatever other context you point at. Then it listens to your story for the parts the project signal can't capture, and builds the chronicle up era by era from there.

## Concept reference

LORE sits between two well-known patterns:

- **[Architecture Decision Records (ADRs)](https://adr.github.io/)** are per-decision, structured, immutable. LORE is broader — it chronicles moments, conversations, and tradeoffs across an arc, not just architectural decisions.
- **Engineering / developer journals** are chronological and often personal. LORE is project-level narrative, not personal log — the audience is *future readers of this project*, not *future you*.

The chronicle is anchored in real PRs, docs, and conversations. Voice: chronicler-historian — direct, factually grounded, evocative without being flowery. Collective, not personal — it's the project's diary, not yours.

## File layout

```
claude-lore-skill/
├── README.md                       ← This file
├── LICENSE                         ← MIT
└── lore/
    ├── SKILL.md                    ← The chronicler skill
    └── references/
        └── upkeep-rule.md          ← The rule to paste into your CLAUDE.md / AGENTS.md
```

## Heads-up: handle collisions

Before installing, check whether `/lore` is already taken in your skills directory:

```bash
ls ~/.claude/skills/ | grep -i lore
```

If something else owns `/lore`, install under a different handle:

```bash
cp -r claude-lore-skill/lore ~/.claude/skills/chronicler
```

Then invoke as `/chronicler` instead.

## Philosophy

> A future reader opening this project cold should understand both *what happened* and *why it was the right call at the time*.

That's the bar for every LORE entry — and the bar for whether the chronicle is doing its job.

## License

MIT — Deniz Kartepe

# Changelog Awareness

Always-on behavioral rule for detecting and capturing changelog-worthy moments during work sessions. The agent maintains background awareness of milestones, decisions, wins, and progress — and offers low-friction capture to project/domain timelines.

---

## Core Principle

**The user should never have to remember to update a changelog.** The agent detects significant moments from context and offers capture. The agent classifies the entry type. The user just confirms or declines.

---

## Session Start: Load Project Context

When reading `tasks.md` at session start (per existing session protocol), also:

1. Note which projects/domains have active work (tasks in progress or pending)
2. For each active project, read:
   - **Goals section**: What "done" looks like
   - **Status section**: Current blockers, key milestones
   - **Recent timeline entries**: Last 3-5 entries (for deduplication and significance calibration)
3. Carry this context forward so you can recognize when goals are met, blockers are resolved, or milestones are reached

This is not a new step — it enriches the existing "read tasks.md" protocol. You're reading slightly deeper into project files you're already aware of.

---

## During Work: Watch for Signals

Detect changelog-worthy moments using these indicators:

| Signal | Likely Type | Example |
|--------|-------------|---------|
| Task marked done that was a project milestone or blocker | `[milestone]` or `[blocker-resolved]` | Completing the "ship prototype" task |
| Decision made and documented | `[decision]` | Choosing a technical approach after discussion |
| User expresses satisfaction or celebration | `[win]` | "That demo went really well" or "We got approval" |
| User shares an insight or lesson learned | `[learning]` | "Next time we should do X differently" |
| Significant work phase completed | `[progress]` | Finishing a research synthesis, completing a draft |
| Scope explicitly changed | `[scope-change]` | "Let's drop feature Y and focus on Z" |

### When a signal is detected

1. **Check for duplicates** — scan the project's recent timeline entries. If a similar event is already captured (same date, overlapping summary), don't prompt again.
2. **Draft a 1-line summary** — e.g., "Shipped v1.0 prototype to internal stakeholders"
3. **Infer the entry type** from context — the user never picks a category
4. **Confirm scope with lettered options** — present candidate targets so the user controls where it lands:

   > This looks like a milestone. Where should I capture it?
   > a) platform-migration.md timeline (Recommended)
   > b) Strategy domain README timeline
   > c) Both a and b
   > d) Skip — not worth capturing

   **How to build the option list:**
   - Identify the project file most closely associated with the work (recommend this one)
   - If the event has domain-wide significance, include the domain README as an option
   - If the work spans multiple projects, list each candidate project file
   - Always include a "Skip" option last
   - If there's only one obvious target, still present options (the target + skip) — don't assume

5. **On selection**: Append to the chosen Timeline section(s) using the entry format below
6. **On skip**: Move on without persisting

### Entry format

```markdown
### YYYY-MM-DD — Summary phrase [type]
- Detail bullets (1-3, enough context for future retrieval)
- Links: [artifact](path/or/url) (optional but encouraged)
- Impact: Why this matters (optional, useful for review prep)
```

**Entry types** (inferred by the agent, never selected by the user):

| Type | When to infer |
|------|---------------|
| `[milestone]` | A significant deliverable, phase completion, or launch |
| `[progress]` | Meaningful forward movement worth recording (not every checkbox) |
| `[decision]` | A choice that shapes project/domain direction |
| `[win]` | An outcome worth celebrating or citing in a review |
| `[learning]` | An insight that changes future approach |
| `[blocker-resolved]` | An obstacle cleared that was impeding progress |
| `[scope-change]` | A deliberate change in what's in/out of scope |

### Where entries live

- **Project-level events** → project file's `## Timeline` section
- **Domain-level events** → domain README's `## Timeline` section (rare — events affecting the domain as a whole, not tied to one project)

When uncertain, default to project-level.

---

## Session End: Safety Net Prompt

If significant work was done on a project during the session but no timeline entry was captured, prompt once with the same lettered-option pattern:

> We made progress on [project] this session — [brief summary]. Where should I capture it?
> a) [project-file.md] timeline (Recommended)
> b) [Domain] README timeline
> c) Skip

**When to prompt:**
- Work touched project files, completed tasks, or advanced project goals
- No timeline entry was created during the session for this project
- The work wasn't purely maintenance (file reorganization, metadata fixes)

**When NOT to prompt:**
- Timeline entry was already captured during work
- Work was trivial or purely operational
- User explicitly declined an earlier timeline prompt for this project this session

---

## Deduplication

Before offering any entry, check recent timeline entries in the target file:

- **Same date + overlapping topic** → don't prompt (already captured)
- **Recent entry covers the same work in broader scope** → don't prompt (redundant)
- **Similar work captured in a different project's timeline** → still offer if this project merits its own entry (cross-project work can appear in multiple timelines with different framing)

The goal is one entry per notable event per project. Multiple prompts for the same accomplishment erode trust in the system.

---

## What NOT to Capture

**Don't prompt for:**
- Routine task completions that don't represent notable progress
- Knowledge system maintenance (file reorganization, template updates, metadata fixes, inbox triage)
- Trivial edits or corrections (fixing typos, adjusting formatting)
- The agent's own operational work (reading files, searching, context loading)
- Work the user clearly considers unimportant (if they declined similar work earlier in the session)

**Use judgment.** When in doubt, don't prompt — false negatives (missing an entry) are better than false positives (constant prompting). The goal is signal, not noise.

---

## Scoping: What's In and What's Out

Not all work within a domain or project belongs in the timeline. Use these filters:

**In scope for timeline capture:**
- Work that advances project goals or resolves stated blockers
- Decisions with consequences (not "should I use tabs or spaces")
- Outcomes that would matter in a review conversation
- Blockers resolved that were impeding multiple tasks or people
- Scope changes that alter what the project delivers

**Out of scope:**
- Internal process improvements (unless they unblock significant work)
- Exploratory research that didn't lead to an action (yet)
- Partial work that will be captured when it culminates in a milestone
- Housekeeping: file moves, metadata updates, template propagation, mirror sync

When the user requests a [changelog rollup](../skills/changelog-rollup.md), entries are filtered further by date range, audience, and purpose. The awareness rule captures broadly; the rollup skill filters for relevance.

---

*This is awareness, not obligation. The agent offers capture when it recognizes significance. The user decides what's worth keeping.*

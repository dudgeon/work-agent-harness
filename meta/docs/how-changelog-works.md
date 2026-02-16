---
created: 2026-02-15
updated: 2026-02-15
tags: [docs, changelog, feature-guide]
---

# How the Work Changelog Works

A guide to the work changelog feature — how timeline entries get captured, how to scope what's included, and how to request reports.

---

## Overview

The work changelog captures **notable work events** — milestones, decisions, wins, progress, learnings, blockers resolved, scope changes — into Timeline sections within project and domain files. The agent detects these moments during work sessions and offers to record them. You just confirm or decline.

Later, you can ask the agent to roll up these entries into formatted reports for annual reviews, stakeholder updates, retrospectives, or self-reflection.

**Two components:**
1. **Changelog Awareness** (always-on rule) — agent detects and offers to capture timeline entries during work
2. **Changelog Rollup** (invoked skill) — agent aggregates entries into reports on request

---

## How Entries Get Captured

You don't need to do anything special. The agent watches for signals during normal work:

| What happens | Agent infers | Example |
|--------------|-------------|---------|
| You complete a major task or project milestone | `[milestone]` | "Ship v1 prototype" task marked done |
| You make and document a decision | `[decision]` | Chose React over Vue for the frontend |
| You express satisfaction or celebrate an outcome | `[win]` | "The demo went really well" |
| You share a lesson learned | `[learning]` | "Next time we should prototype earlier" |
| Significant work completes on a project | `[progress]` | Finished the research phase |
| A blocker gets resolved | `[blocker-resolved]` | OAuth redirect issue fixed |
| Scope changes deliberately | `[scope-change]` | "Let's drop feature Y and focus on Z" |

When the agent detects a signal, it:
1. Drafts a 1-line summary
2. Asks you briefly: "This looks like a milestone for [project]. Should I add it to the timeline?"
3. On yes, appends the entry to the project or domain file's Timeline section
4. On no, moves on

At session end, if notable work happened but nothing was captured, the agent prompts once as a safety net.

### What doesn't get captured

- Routine task completions (not every checkbox)
- System maintenance (file reorganization, template updates, metadata fixes)
- Trivial edits (typos, formatting)
- The agent's own operational work

The goal is signal, not noise. When in doubt, the agent doesn't prompt.

---

## Where Entries Live

Entries are stored as markdown in `## Timeline` sections:

- **Project-level events** → in the project file (e.g., `domains/strategy/platform-migration.md`)
- **Domain-level events** → in the domain README (e.g., `domains/strategy/README.md`)

Project-level is the default. Domain-level entries are rare — reserved for events that affect the domain as a whole (domain created, cross-project milestones).

### Entry format

```markdown
### 2026-02-15 — Shipped v1.0 prototype [milestone]
- Deployed to internal stakeholders for feedback
- Links: [PR #42](url), [demo recording](url)
- Impact: First external visibility; unblocked user testing phase
```

Each entry has a date, summary, type tag, and 1-3 detail bullets. Links and Impact lines are optional but encouraged — they make reports richer later.

---

## Scoping: What Gets In and What Stays Out

### Inclusion criteria

Timeline entries should represent work that:
- Advances project goals or resolves stated blockers
- Would matter in a review conversation
- Involves decisions with consequences
- Represents a turning point or phase transition

### Exclusion criteria

Skip entries for:
- Internal process improvements (unless they unblock significant work)
- Exploratory research that didn't lead to action (yet)
- Partial work that will be captured when it culminates in a milestone
- Housekeeping: file moves, metadata, template propagation, mirror sync

### Controlling capture density

The agent starts conservative — "when in doubt, don't prompt." If you find it's:
- **Too noisy**: Decline a few prompts. The agent calibrates within a session.
- **Missing things**: Mention it. The agent can prompt more aggressively for certain projects.
- **Just right**: Do nothing. The defaults are designed to err on the side of under-prompting.

---

## Requesting Reports (Changelog Rollup)

Ask the agent to aggregate timeline entries into a report. Natural language works:

- "Prepare my annual review material"
- "Draft a stakeholder update for the platform migration project"
- "What have I accomplished this quarter?"
- "Summarize progress on [domain] since January"
- "Write a weekly writeup for [project]"

### Scoping your request

Control what's included:

| Dimension | How to scope | Default |
|-----------|-------------|---------|
| **Projects/domains** | Name them explicitly, or say "all active" | All active |
| **Date range** | "Last 2 weeks", "Q3", "this year", "since project started" | Broad (YTD or all) |
| **Entry types** | "Only milestones and wins" or "everything" | All types |
| **Audience** | "For my manager", "for stakeholders", "for myself" | Inferred from request |

If your request is ambiguous, the agent asks one clarifying question and defaults broad.

### Report types

| Purpose | Best for | What it emphasizes |
|---------|----------|-------------------|
| **Annual review** | Manager/self-eval | Milestones, wins, decisions showing judgment, learnings showing growth |
| **Stakeholder update** | Team/leadership | What shipped, what changed, what's next |
| **Retrospective** | End-of-project reflection | Chronological narrative, decision points, cause-and-effect |
| **Self-reflection** | Personal review | Where time went, themes, patterns, gaps |

Reports are presented as drafts for your review before finalizing. The agent writes them to `reports/` (cross-domain) or `domains/[domain]/reports/` (domain-scoped), or presents inline for quick summaries.

---

## Platform-Specific Behavior

The changelog feature works across all supported agents. Here's how each platform interacts with it:

### Claude Code

- **Skill discovery**: Claude Code reads `.claude/skills/changelog-rollup.md` to know the rollup skill exists. The frontmatter `description` field triggers skill matching.
- **Rule loading**: Changelog awareness rule is referenced in `rules/communication.md` (session protocol) and loaded when the agent reads session start instructions. CLAUDE.md's `@rules/communication.md` ensures it's always available.
- **Invocation**: Just ask naturally. Claude Code matches your request to the changelog-rollup skill via its description.
- **Entry capture**: Happens automatically during work sessions. The agent prompts inline in the conversation.

### Windsurf

- **Skill discovery**: Windsurf reads `.windsurf/skills/changelog-rollup.md` (thin pointer to the universal version).
- **Rule loading**: `.windsurf/rules/changelog-awareness.md` mirrors the universal rule. Windsurf loads rules marked `trigger: always` automatically.
- **Invocation**: Same natural language triggers. Windsurf matches via the skill's frontmatter description.
- **Entry capture**: Same inline prompting behavior during work sessions.

### Common across platforms

- Timeline entries are stored in the same project/domain files regardless of which agent captured them
- Both agents read the same universal rule and skill files (via their mirrors)
- Reports generated by one agent are readable and extendable by the other
- If you switch agents mid-session, the new agent reads recent timeline entries at session start and won't re-prompt for already-captured work

### Sync considerations

- When modifying the changelog-awareness rule or rollup skill, update the universal version (`rules/`, `skills/`) and verify mirrors are in sync
- Mirror files are thin pointers — they shouldn't contain duplicated content
- See [AGENTS.md](../../AGENTS.md) for the full cross-agent sync protocol

---

## Underlying Files

| File | Role |
|------|------|
| [rules/changelog-awareness.md](../../rules/changelog-awareness.md) | Always-on rule: detection, capture, deduplication |
| [skills/changelog-rollup.md](../../skills/changelog-rollup.md) | Invoked skill: aggregation, synthesis, reporting |
| [templates/project.md](../../templates/project.md) | Project template with Timeline section (v2) |
| [templates/domain.md](../../templates/domain.md) | Domain template with Timeline section (v2) |
| [meta/specs/work-changelog.md](../specs/work-changelog.md) | Original spec (design rationale and open questions) |

---

## Deferred Capabilities

These are planned but not yet implemented:

1. **Retroactive population** — Backfilling timeline entries from git history for existing projects
2. **Proactive periodic reports** — Agent offering weekly/monthly writeups on a schedule
3. **Entry density calibration** — Tuning how aggressively the agent prompts (currently conservative by default)

See [meta/specs/work-changelog.md](../specs/work-changelog.md) § Open Questions for design rationale on each.

---

*The changelog captures work significance, not just status. The agent handles detection and classification. You handle judgment on what's worth keeping.*

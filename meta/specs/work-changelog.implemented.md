---
created: 2026-02-15
updated: 2026-02-16
status: implemented
implemented_date: 2026-02-16
origin: inbox items 2026-02-14T12-32-33, 2026-02-14T12-38-03 (home-brain inbox)
applies_to: [home-brain, work-agent-harness]
tags: [spec, changelog, domain-lifecycle, skills, rules]
---

# Spec: Work Changelog (Implemented)

> **Status: Fully implemented for work-agent-harness as of 2026-02-16.** See implementation notes below for what shipped where. Home-brain implementation is tracked separately.

A behavioral specification for capturing, storing, and reporting on work activity across domains and projects. The changelog tracks **the work itself** — milestones, decisions, wins, progress — not changes to the knowledge system's structure.

---

## Problem

Knowledge workers accumulate accomplishments, decisions, and progress across projects and domains, but this narrative is lost. Task trackers capture status (done/not done) but not significance. When it's time to prepare an annual review, draft a stakeholder update, or reflect on a quarter's work, the information has to be reconstructed from memory, git history, and scattered notes.

**Target use cases:**

1. **Annual/quarterly reviews** — "What did I accomplish this year?" Aggregated across all domains and projects, filtered for milestones and wins.
2. **Stakeholder updates** — "Draft a status report for projects X, Y, Z covering the last 2 weeks." Scoped to specific projects, recent date range, all entry types.
3. **Project retrospectives** — "How did project X unfold?" Full timeline of a single project.
4. **Self-reflection** — "Where has my time gone this quarter?" Cross-domain scan for recent activity.

---

## Design Principles

### 1. Agent-driven, not user-driven

The user should never have to remember to update a changelog, categorize an entry, or choose a type. The agent detects changelog-worthy moments from context — project goals, active blockers, task completions, decisions made — and offers low-friction capture. The agent classifies the entry type based on what it knows about the project's state.

**Good:** "You just resolved the auth blocker that's been holding up the prototype — should I add this to the timeline?"

**Bad:** "Please select an entry type: milestone / progress / decision / win / learning / blocker-resolved"

### 2. Predictable scaffolding

The agent always knows WHERE to read and write timeline entries because the folder structure guarantees it. No searching. No guessing.

- Domain-level events → `domains/[domain]/README.md` → `## Timeline` section
- Project-level events → project file → `## Timeline` section
- Domain rollup = README timeline + all project timelines within that domain

### 3. Flexible scoping

Work doesn't always fit neatly into one domain or one project. Timeline entries can reference other domains/projects via links. Cross-domain initiatives get a project file in their natural home with cross-references. The rollup skill follows links to build initiative-level views.

### 4. Skills and rules over data structures

The value is in the behavioral changes — what the agent does differently — not in the format of entries. The entry format should be minimal, parseable, and human-readable. The intelligence lives in the rules (when to capture) and skills (how to aggregate and report).

### 5. Low friction, high signal

Not every task completion gets a timeline entry. The agent uses judgment: milestones, shipped work, resolved blockers, decisions with consequences, and wins are worth capturing. Routine progress is not, unless it represents meaningful forward movement.

---

## Conventions (Scaffolding)

### Domain README: Timeline section

Every domain README includes a `## Timeline` section as the last major section before the review date. This captures domain-level events — things that matter to the domain as a whole, not to any single project.

```markdown
## Timeline

### 2026-02-15 — Completed stakeholder mapping [milestone]
- Mapped 12 key stakeholders across 3 teams with influence/interest grid
- Links: [stakeholder-map](../../context/stakeholder-map.md)

### 2026-02-10 — Domain created [milestone]
- Bootstrapped from initial project kickoff materials
```

### Project file: Timeline section

Every project file includes a `## Timeline` section (replaces the existing `## Progress Log` or `## Timeline` sections). This captures project-level events — milestones, decisions, progress, wins, blockers resolved.

```markdown
## Timeline

### 2026-02-15 — Shipped v1.0 prototype [milestone]
- Deployed to internal stakeholders for feedback
- Links: [PR #42](url), [demo recording](url)
- Impact: First external visibility; unblocked user testing phase

### 2026-02-13 — Chose React over Vue [decision]
- Team expertise + component library availability
- Links: [decision record](decisions/frontend-framework.md)

### 2026-02-10 — Resolved OAuth redirect blocker [blocker-resolved]
- Root cause: misconfigured staging redirect URIs
- Links: [issue #15](url)
```

### Entry format

Each entry follows this structure:

```markdown
### YYYY-MM-DD — Summary phrase [type]
- Detail bullets (1-3, enough context for future retrieval)
- Links: [artifact](path/or/url) (optional but encouraged)
- Impact: Why this matters (optional, useful for review prep)
```

**Entry types** (inferred by the agent, not selected by the user):

| Type | When the agent infers this |
|------|---------------------------|
| `[milestone]` | A significant deliverable, phase completion, or launch |
| `[progress]` | Meaningful forward movement worth recording (not every checkbox) |
| `[decision]` | A choice that shapes project/domain direction |
| `[win]` | An outcome worth celebrating or citing in a review |
| `[learning]` | An insight that changes future approach |
| `[blocker-resolved]` | An obstacle cleared that was impeding progress |
| `[scope-change]` | A deliberate change in what's in/out of scope |

The agent maintains awareness of this list and maps events to types based on context. Users never interact with this taxonomy directly.

### Cross-domain references

For work that spans domains, the entry in the primary project links to related domains/projects:

```markdown
### 2026-02-15 — Integrated research findings into product roadmap [milestone]
- Synthesized user research from [domain-x](../domain-x/) with technical feasibility from [domain-y](../domain-y/)
- Links: [roadmap update](roadmap.md), [research summary](../domain-x/research/summary.md)
```

The rollup skill follows these links to build cross-domain views.

---

## Rule: Changelog Awareness

**Type:** Always-on behavioral rule (not a skill to invoke)

**Purpose:** The agent maintains background awareness of changelog-worthy moments during all work sessions and offers to capture them with low friction.

### Behavior

#### At session start

When the agent reads `tasks.md` (per existing session protocol), it also:

1. Notes which projects/domains have active work
2. Reads the goals and current blockers from those project files
3. Carries this context forward so it can recognize when goals are met, blockers are resolved, or scope changes

This is not a new step — it's an enrichment of the existing "read tasks.md at session start" behavior. The agent reads slightly deeper (project-level goals/blockers, not just task checkboxes).

#### During work

The agent watches for these signals:

| Signal | Likely type | Example |
|--------|-------------|---------|
| Task marked done that was listed as a project milestone or blocker | `[milestone]` or `[blocker-resolved]` | Completing the "ship prototype" task |
| Decision made and recorded | `[decision]` | Choosing a technical approach after discussion |
| User expresses satisfaction or celebration | `[win]` | "That went really well" or "The demo landed" |
| User shares an insight or lesson | `[learning]` | "Next time we should do X differently" |
| Significant work completed on a project | `[progress]` | Completing a research phase, finishing a draft |
| Scope explicitly changed | `[scope-change]` | "Let's drop feature Y and focus on Z" |

When a signal is detected, the agent:

1. **Drafts a 1-line entry** based on what just happened
2. **Infers the type** from context (the user never picks a type)
3. **Asks a brief confirmation** — e.g., "This looks like a major milestone for [project]. Should I add it to the timeline?"
4. **On confirmation**, appends the entry to the appropriate Timeline section (project file or domain README, based on scope)
5. **On decline**, moves on without persisting

#### At session end

If significant work was done on a project during the session but no timeline entry was captured, the agent prompts once:

> "We made progress on [project] this session — [brief summary]. Worth adding to the timeline?"

This is a safety net, not the primary capture mechanism. Most entries should be captured in-flow.

#### What NOT to capture

- Routine task completions that don't represent notable progress
- Knowledge system maintenance (file reorganization, template updates, metadata fixes)
- Trivial edits or corrections
- The agent's own operational work (reading files, searching, etc.)

Use judgment. When in doubt, don't prompt — false negatives (missing an entry) are better than false positives (constant prompting).

### Context requirements

For changelog awareness to work, the agent needs to know:

1. **Project goals** — what "done" looks like, what the project is trying to achieve
2. **Active blockers** — what's in the way, so resolution can be detected
3. **Recent timeline** — what's already been captured, to avoid duplicates and to calibrate significance

This context comes from reading project files at session start. Projects with well-defined goals and blockers sections will get better changelog behavior. This creates a virtuous cycle: the agent will naturally encourage users to maintain these sections because they improve the changelog experience.

---

## Skill: Changelog Rollup

**Type:** Invoked skill (user requests reporting or aggregation)

**Trigger phrases:**

- "Prepare my annual/quarterly review material"
- "Draft a stakeholder update for [projects/domains]"
- "What have I accomplished [time period]?"
- "Summarize progress on [project/domain/initiative]"
- "Write a weekly/monthly writeup for [domain]"

### Workflow

#### 1. Determine scope

From the user's request, identify:

- **Projects/domains**: Which to include (specific list, or "all active")
- **Date range**: Time window (last week, Q3, YTD, all time)
- **Audience**: Who will read this (self-reflection, manager, stakeholders, team)
- **Focus**: All types, or filtered (e.g., only milestones and wins for annual review)

If the request is ambiguous, ask one clarifying question. Default to broad scope — it's easier to cut than to re-run.

#### 2. Collect timeline entries

Navigate the predictable structure:

- For each domain in scope: read `domains/[domain]/README.md` → Timeline section
- For each project in scope: read project file → Timeline section
- Filter by date range
- Follow cross-domain links to include related entries from adjacent domains/projects

#### 3. Organize and synthesize

Don't just concatenate entries. Organize by:

- **For annual reviews**: Group by domain/project, lead with milestones and wins, include decisions that show judgment, include learnings that show growth
- **For stakeholder updates**: Group by project, lead with what shipped/changed, include blockers and next steps
- **For retrospectives**: Chronological within the project, full narrative arc
- **For self-reflection**: Chronological across domains, identify themes and patterns

#### 4. Draft output

Produce a formatted report appropriate to the audience:

- **Annual review material**: Bullet-pointed accomplishments organized by theme, with specific examples and impact statements. Ready to paste into a review form.
- **Stakeholder update**: Professional summary with project-level sections. 1-2 paragraphs per project. What changed, what's next, what needs attention.
- **Retrospective**: Narrative timeline with key decision points, turning points, and lessons learned.
- **Self-reflection**: Informal summary with honest assessment of where time went and what mattered.

#### 5. Output location

Write the report to the domain's natural location:

- Domain-scoped reports → `domains/[domain]/reports/YYYY-MM-DD-[type].md` (e.g., `2026-02-15-weekly-update.md`)
- Cross-domain reports → `reports/YYYY-MM-DD-[type].md` (root-level reports folder)
- Or present inline if the user just wants a quick summary (don't create a file for everything)

Present the draft to the user for review before finalizing. Reports are creative writing — the user may want to adjust tone, emphasis, or framing.

---

## Updates to Existing Rules and Skills

### Task system rule

Add to the task completion workflow:

> After marking a task as done, assess whether the completion is changelog-worthy (per the Changelog Awareness rule). If so, offer to capture a timeline entry. Not every task completion qualifies — use the project's goals and blockers as context for significance.

### Decision system rule

Add to the decision documentation workflow:

> After recording a decision, offer to add a timeline entry linking to the decision record. Decisions that shape project direction are always changelog-worthy.

### Domain synthesis skill

Add a step to the synthesis workflow:

> **Review Timeline**: Check the domain README's Timeline section and project-level Timeline sections within the domain. Flag:
> - Stale timelines (active projects with no recent entries)
> - Gaps (significant work evident from file changes but no timeline entries)
> - Entries that need enrichment (missing links, vague summaries)

This is a light touch — synthesis reviews the timeline's health, it doesn't generate timeline entries (that's the changelog awareness rule's job).

### Project template

Update to include a structured Timeline section with guidance:

```markdown
## Timeline

<!-- Timeline entries are added by the agent during work sessions.
     Each entry captures a notable event: milestone, decision, progress, win, learning, or blocker resolved.
     The agent infers the type — you don't need to categorize manually. -->

### YYYY-MM-DD — Project created [milestone]

Initial project context — what prompted this and what the starting conditions are.
```

### Domain README template

Add a Timeline section as the last major section:

```markdown
## Timeline

<!-- Domain-level events that matter across the domain, not specific to any one project.
     Project-specific events go in project files. -->
```

---

## Implementation Notes

### For home-brain

1. **Template updates**: Update `templates/project.md` to replace `## Progress Log` with structured `## Timeline` section
2. **Domain READMEs**: Add `## Timeline` section to existing domain READMEs (can be empty initially — entries accumulate naturally)
3. **Skill updates**: Update `.claude/skills/domain-synthesis.md` with the timeline review step
4. **Rule additions**: The changelog awareness behavior integrates into existing session protocol and communication rules
5. **Existing progress logs**: For projects with existing Progress Log entries, migrate content to Timeline format during next touch (not a bulk migration — opportunistic)

### For work-agent-harness — IMPLEMENTED 2026-02-16

All items shipped:

| Spec Item | Shipped To | Notes |
|-----------|-----------|-------|
| Template: project.md Timeline section | `templates/project.md` (v2) | Replaces Progress Log with structured Timeline |
| Template: domain.md Timeline section | `templates/domain.md` (v2) | Domain-level timeline scaffolding |
| Skill: changelog rollup | `skills/changelog-rollup.md` | Full reporting skill with scoping, synthesis, output |
| Skill: domain-source-synthesis timeline review | `skills/domain-source-synthesis.md` | Timeline health check during domain reviews |
| Rule: changelog awareness | `rules/changelog-awareness.md` | Always-loaded; session start/during/end behavior |
| Rule hook: task-system.md | `rules/task-system.md` | Changelog assessment after task completion |
| Rule hook: decision-system.md | `rules/decision-system.md` | Timeline entry offer after decisions |
| Rule hook: communication.md | `rules/communication.md` | Session protocol references changelog-awareness |
| Always-loaded registration | `AGENTS.md`, `CLAUDE.md` | changelog-awareness in always-loaded lists |
| Mirror: .claude/skills/changelog-rollup | `.claude/skills/changelog-rollup.md` | Thin pointer |
| Mirror: .windsurf/skills/changelog-rollup | `.windsurf/skills/changelog-rollup.md` | Thin pointer |
| Mirror: .windsurf/rules/changelog-awareness | `.windsurf/rules/changelog-awareness.md` | Thin pointer |

### Rollout approach

**Phase 1: Scaffolding** — Update templates, add Timeline sections to existing domain READMEs, update skills with timeline review behavior

**Phase 2: Capture behavior** — Add changelog awareness to rules. Start capturing entries during normal work sessions. Validate that detection and classification feel natural.

**Phase 3: Reporting** — Build the changelog rollup skill once there are enough timeline entries to aggregate. This is the payoff — but it needs data first.

---

## Open Questions (for implementation phase)

1. **Report cadence automation**: Should the agent proactively offer periodic reports ("It's Friday — want a weekly writeup for [domain]?"), or only when asked? Start with on-demand and graduate to proactive if the pattern proves useful.

2. **Retroactive population**: For projects with rich git history or completed task archives, should the agent offer to backfill timeline entries? Useful for bootstrapping, but could produce lower-quality entries. Recommend: offer once per project, produce a draft for user review, don't auto-commit.

3. **Entry density calibration**: How many entries per week feels right? Too few = gaps in the narrative. Too many = noise. This will emerge from practice — start with the "when in doubt, don't prompt" default and adjust based on user feedback.

---

*This spec defines behavioral changes — rules and skills — that make work activity visible and reportable. The data structures (Timeline sections, entry format) are minimal scaffolding. The intelligence lives in the agent's awareness of what matters and its ability to synthesize a narrative from accumulated entries.*

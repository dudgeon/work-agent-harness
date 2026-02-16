---
created: 2026-02-16
updated: 2026-02-16
status: draft
origin: inbox items 2026-02-14T12-32-33, 2026-02-14T12-38-03 (home-brain inbox)
applies_to: [home-brain, work-agent-harness]
tags: [spec, domain-lifecycle, strategy, roadmap, changelog, skills]
---

# Spec: Domain Lifecycle

A behavioral specification for the full lifecycle of domains in the harness — from strategic direction through execution to retrospective. Positions existing skills (domain synthesis, domain source synthesis, work changelog) as components of a broader domain strategy layer.

**Backlink**: Originated from [home-brain inbox](https://github.com/dudgeon/home-brain) items on domain changelog and domain lifecycle skills.

---

## Problem

Domain source synthesis currently operates somewhat independently — it has its own lifecycle (capture → read → process → extract), its own infrastructure (sources/, knowledge-base/), and its own health checks. But source synthesis is a **child** of a broader domain strategy: you collect and synthesize sources *in service of* a domain's strategic direction, not as an end in itself.

The harness is opinionated about some domain primitives (synthesis, task management, changelog capture) but lacks an overarching lifecycle that connects them. Without this, domains accumulate content without clear direction, and the agent has no framework for asking "is this domain healthy?" beyond structural metrics.

**What's missing:**

1. **Domain roadmaps** — where is this domain heading, what are the open questions, what's the strategic context for all the work happening within it
2. **Domain changelogs** — what changed in the domain's structure and knowledge (distinct from work activity captured by the work changelog)
3. **Lifecycle framing** — a clear hierarchy: strategy governs execution, execution feeds back into strategy

---

## Design Principles

### 1. Strategy layer governs, doesn't replace

The domain lifecycle adds a strategy envelope around existing skills. It does NOT replace domain synthesis, domain source synthesis, or the work changelog. It connects them with strategic context.

### 2. Roadmaps are living documents, not artifacts

A domain roadmap is a maintained section of the domain README, not a separate deliverable. It evolves as the domain evolves. The agent helps maintain it, not generate it from scratch.

### 3. Lightweight by default, rich when warranted

Not every domain needs a full roadmap, active projects, PRDs, and a changelog. The lifecycle framework defines what's *available* — domains adopt the pieces they need. A personal hobby domain might only have a roadmap; a professional learning domain might use the full stack.

### 4. Source synthesis feeds UP into strategy

The source-to-knowledge pipeline produces insights. Those insights should inform the domain roadmap, surface new questions, and sometimes generate new projects or tasks. This upward flow is currently implicit — the lifecycle makes it explicit.

---

## The Domain Lifecycle

```
    ┌──────────────────────────────────────────┐
    │           DOMAIN STRATEGY LAYER           │
    │                                           │
    │  Roadmap ──── Open Questions              │
    │     │              │                      │
    │     ▼              ▼                      │
    │  Projects ──── Tasks ──── PRDs            │
    │     │              │         │            │
    └─────┼──────────────┼─────────┼────────────┘
          │              │         │
    ┌─────┼──────────────┼─────────┼────────────┐
    │     ▼    EXECUTION LAYER     ▼            │
    │                                           │
    │  Source Processing ──── Knowledge Extract  │
    │  Work Activity ──────── Timeline Entries   │
    │                                           │
    └──────────────┬────────────────────────────┘
                   │
    ┌──────────────┼────────────────────────────┐
    │     ▼    FEEDBACK LAYER                   │
    │                                           │
    │  Domain Synthesis ──── Health Check        │
    │  Changelog ──────────── Audit Trail        │
    │  Insights ───────────── Feed up to roadmap │
    │                                           │
    └───────────────────────────────────────────┘
```

### Strategy Layer (new)

**Roadmap** — Where is this domain heading? What are the goals, open questions, and priorities? Lives in the domain README as a `## Roadmap` section.

**Projects** — Scoped efforts within a domain. Each gets a file (using `templates/project.md`) with goals, tasks, timeline.

**Tasks** — Actionable work items. Tracked in `tasks.md` (authoritative) with mirrors in domain files.

**PRDs** — Structured definitions for non-trivial deliverables. Optional — only for work that warrants upfront specification. <!-- OPEN QUESTION: see below -->

### Execution Layer (existing)

**Source Processing** — [domain-source-synthesis.md](../../skills/domain-source-synthesis.md) handles the capture → read → process → extract pipeline.

**Work Activity** — Normal work sessions produce task completions, decisions, progress. The [work changelog](work-changelog.md) spec captures these as timeline entries.

### Feedback Layer (existing + new)

**Domain Synthesis** — [domain-synthesis.md](../../skills/domain-synthesis.md) reviews domain health (structure, staleness, gaps). Already includes timeline review.

**Domain Changelog** — A structured record of what changed in the domain's knowledge and structure over time. Distinct from work timeline entries (which track activity). <!-- OPEN QUESTION: see below -->

**Insights feeding up** — When source processing reveals new concepts, questions, or gaps, these should flow up to update the roadmap and open questions.

---

## Domain README Enhancements

The domain README becomes the single authoritative view of domain strategy AND state. Current READMEs include Purpose, Structure, and (sometimes) Timeline. The lifecycle adds:

```markdown
## Roadmap

### Current Focus
[What's being actively worked on and why]

### Open Questions
- [Question 1 — what would answering it unlock?]
- [Question 2]

### Goals
- [ ] [Goal 1 — measurable or clearly completable]
- [ ] [Goal 2]

### Deferred / Someday
- [Ideas parked for later consideration]
```

The roadmap section is maintained by the agent during domain synthesis, and updated incrementally when work within the domain shifts priorities.

---

## New Skill: Domain Roadmap Review

**When this applies:**
- User asks "where is [domain] heading?" or "what's the plan for [domain]?"
- During domain synthesis (as a new step before the structural review)
- When a significant milestone is completed and the domain's direction may need reassessment

**Workflow:**

1. **Read the domain README** — current roadmap, open questions, goals
2. **Scan recent activity** — timeline entries, recently processed sources, completed tasks
3. **Identify roadmap drift** — has the work diverged from the stated direction? Are goals stale?
4. **Surface new questions** — do recently extracted knowledge entries raise questions not yet captured?
5. **Propose updates** — present roadmap changes for user approval

This is a strategic complement to domain synthesis (which focuses on structural health).

---

## Domain Changelog Convention

<!-- OPEN QUESTION: Should the domain changelog be a separate section from the Timeline, or should it extend the existing Timeline with a new entry type? -->

The domain changelog captures **structural and knowledge evolution** — not work activity (that's the work changelog's job). Examples:

- New knowledge entries extracted and added to the knowledge base
- Taxonomy updated or restructured
- Domain scope changed (expanded/narrowed)
- New sub-domain created
- Source processing batch completed (X sources → Y entries)
- Domain roadmap updated

**Proposed approach**: Add a `## Changelog` section to domain READMEs (separate from `## Timeline`) that tracks domain-level structural changes. The work timeline tracks *what you did*; the domain changelog tracks *how the domain evolved*.

```markdown
## Changelog

### 2026-02-16 — Processed 5 new sources, extracted 8 knowledge entries
- Sources: [list or link to batch]
- New entries: [list with links]
- Taxonomy: no changes needed

### 2026-02-10 — Restructured knowledge base by lifecycle phase
- Previous: flat organization
- New: organized by 6-phase PM lifecycle taxonomy
- Rationale: entries exceeded 20, flat browsing became unwieldy
```

---

## Integration with Existing Skills

### Domain Synthesis (updated)

Add a new first step before the structural inventory:

> **Step 0: Review Roadmap**
> Before diving into structural health, review the domain's Roadmap section.
> Is the stated direction still current? Do recent activities align with stated goals?
> Flag roadmap staleness or drift for the user.

Add a new final step after suggesting reorganization:

> **Step 7: Feed insights up**
> Based on synthesis findings, propose updates to the domain roadmap:
> - New open questions surfaced by gap analysis
> - Goals that should be marked complete or deprioritized
> - Emerging themes that suggest new strategic directions

### Domain Source Synthesis (updated)

Add a post-processing step:

> **After processing a batch of sources:**
> 1. Update the domain changelog with what was processed and extracted
> 2. Check if newly extracted knowledge entries answer any open questions in the roadmap
> 3. If they do, propose updating the roadmap (mark question as answered, add new follow-up questions)
> 4. If they surface entirely new questions, propose adding them to the roadmap

### Work Changelog (no changes)

The work changelog continues to track work activity as designed. Domain lifecycle doesn't modify it — it adds the strategic context that makes changelog entries more meaningful.

---

## Open Questions for User

### Q1: Domain Roadmap Formality

How formal should domain roadmaps be?

- **a) OKR-style** — Explicit objectives with key results, quarterly cadence
- **b) Goals + Open Questions** (recommended) — Lightweight goals list + evolving questions, no cadence forcing
- **c) Narrative** — Freeform "where this is heading" prose, updated when it feels stale

### Q2: PRDs Within Domains

Should the harness be opinionated about PRDs (structured definitions for what to build/create)?

- **a) Yes — template + skill** — Create `templates/prd.md` and a lightweight skill for when to use them
- **b) Yes — template only** — Provide a template but no prescriptive skill; users decide when to write PRDs
- **c) No — defer** (recommended) — PRDs are too project-specific; let this emerge from practice. The existing project template + task system handles most cases.

### Q3: Domain Changelog vs Timeline

Should the domain changelog be separate from the work timeline?

- **a) Separate sections** (recommended) — `## Timeline` for work activity (per work changelog spec), `## Changelog` for domain evolution (structural/knowledge changes). Clear separation of "what I did" vs "how the domain grew."
- **b) Unified timeline** — Extend the existing Timeline with new entry types (`[knowledge-added]`, `[restructured]`, `[scope-changed]`). Simpler, one place to look.
- **c) Changelog in meta/** — Put the domain changelog in `meta/changelog.md` within the domain, keeping the README focused on current state only.

### Q4: Which Domains Get Full Lifecycle?

How should domains decide which lifecycle pieces to adopt?

- **a) Pattern flag in frontmatter** (recommended) — Like `pattern: domain-source-synthesis`, add `lifecycle: [roadmap, changelog, source-synthesis]` to declare what a domain uses
- **b) Convention by domain type** — Professional/learning domains get full stack; personal/family domains get lightweight
- **c) Everything everywhere** — All domains get all pieces, use what they want

### Q5: Home-Brain Adoption

The inbox note says this applies to both WAH and home-brain. For home-brain, the ai-pm domain already has sophisticated source synthesis. Should the domain lifecycle spec be:

- **a) WAH-first, port later** (recommended) — Design and implement in WAH, then adapt for home-brain once validated
- **b) Simultaneous** — Implement in both repos at the same time
- **c) Home-brain first** — ai-pm is the most mature domain; implement there and generalize

---

## Implementation Approach (Pending Question Resolution)

### Phase 1: Roadmap Convention
- Add `## Roadmap` section to domain README template
- Update domain synthesis skill with roadmap review step (Step 0 + Step 7)
- Add roadmap sections to existing active domains (initially sparse, grow organically)

### Phase 2: Changelog Convention
- Add `## Changelog` section to domain README template (pending Q3 decision)
- Update domain source synthesis to log batch processing to changelog
- Update domain synthesis to review changelog health

### Phase 3: Upward Flow
- Connect source synthesis → roadmap updates (post-processing step)
- Connect domain synthesis → roadmap freshness checks
- The virtuous cycle: sources inform knowledge → knowledge informs strategy → strategy guides what sources to seek

---

*This spec positions domain source synthesis as a child of domain strategy, not a peer. The key behavioral change: the agent should always know not just what a domain contains, but where it's heading — and should use that direction to guide all work within the domain.*

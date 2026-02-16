---
type: template
version: 2
description: Template for tracking projects within a domain. Projects have defined goals, timelines, and completion states.
changelog: |
  v2 (2026-02-15): Enriched Timeline section with type tags, entry format guidance, and agent instructions
  v1 (2026-02-09): Initial template
---

<!-- When creating a file from this template, replace this block with: -->
<!-- ---                                                                 -->
<!-- created: YYYY-MM-DD                                                 -->
<!-- updated: YYYY-MM-DD                                                 -->
<!-- template: templates/project.md                                      -->
<!-- template_version: 1                                                 -->
<!-- tags: [domain, project]                                             -->
<!-- status: planned|active|paused|completed|archived                    -->
<!-- origin: endogenous|imported                                         -->
<!-- ---                                                                 -->

# [Project Name]

> One-sentence description of the project goal.

---

## Goal

What this project aims to achieve and why it matters.

## Status

Current state: **planned** / **active** / **paused** / **completed** / **archived**

Key milestones or blockers.

## Scope

What's in scope:
- Item 1
- Item 2

What's explicitly out of scope:
- Item 1

## Key Decisions

| Date | Decision | Rationale |
|------|----------|-----------|
| YYYY-MM-DD | Decision made | Why this was chosen |

## Tasks

- [ ] Task 1
- [ ] Task 2
- [ ] Task 3

*Cross-reference: These tasks should also appear in [tasks.md](../../tasks.md) for cross-domain visibility.*

## Related

- [Domain README](README.md)
- [Related concept](related-concept.md)
- [Stakeholder context](../../context/stakeholder-map.md)

---

## Timeline

<!-- Timeline entries are added by the agent during work sessions.
     Each entry captures a notable event: milestone, decision, progress, win, learning, blocker resolved, or scope change.
     The agent infers the type based on context — you don't need to categorize manually.

     Format:
     ### YYYY-MM-DD — Summary phrase [type]
     - Detail bullets (1-3, enough context for future retrieval)
     - Links: [artifact](path/or/url) (optional but encouraged)
     - Impact: Why this matters (optional, useful for review prep)

     Types: [milestone], [progress], [decision], [win], [learning], [blocker-resolved], [scope-change]
-->

### YYYY-MM-DD — Project created [milestone]

Initial project context — what prompted this and what the starting conditions are.

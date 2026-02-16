---
created: 2026-02-09
updated: 2026-02-16
tags: [meta, tasks]
status: active
---

# Tasks

Cross-domain task tracker. Check this at session start.

Tasks may also appear in domain-specific files for local context, but this file is the authoritative cross-domain view.

---

## Active

### Harness Build — Skills & Rules Port

Remaining items from home-brain port (interactive triage — discuss before building):

- [ ] rules/mcp-optimization.md — writing for discoverability; relevance TBD @agent
- [ ] newsworthy-events.md — likely skip, too personal/family-specific @agent

### Harness Build — Post-Port

- [ ] Personalize the harness: create initial context files (company-info.md, stakeholder-map.md, etc.) from real content @user
- [ ] Test inbox → triage → domain flow with real inbox items @agent
- [ ] Validate cross-agent sync: test AGENTS.md changes propagate to Windsurf and Claude mirrors @agent
- [ ] Create first domain from actual work content (don't pre-seed) @user

## Backlog

### Shared Team Context Repo Support

Design and build harness support for shared team knowledge repos. The harness is private by default, but workers often operate alongside a shared team repo. The harness needs to: create well-structured team repos, ingest/search them, and contribute back via PRs.

Key decisions captured:

- **Independence**: Team repo is fully self-contained — own templates, rules, agent instructions. Cannot depend on any individual worker harness.
- **Linking**: Harness tracks team repos via a lightweight config/index (e.g., `context/team-repos.md`) pointing to local clones. Guard against drift between local clone and canonical repo.
- **Creation vs interaction**: Harness carries a spec for creating team repos (opinionated at creation). Once created, harness defers to the team repo's own norms/AGENTS.md.
- **Primitives**: Team repo gets independent copies of templates/rules (forked from harness at creation, evolves independently).
- **Shared structure**: Same primitives (domains, concepts, inbox) apply. Team repo includes its own agent instructions for onboarding and operations.
- **Automation TBD**: No automation for who operates the team repo yet. Assume team members accept each other's PRs.

Deliverables:

- [ ] `templates/team-repo/` — scaffold/spec for creating a new shared team context repo @agent
- [ ] `skills/team-repo-create.md` — skill for bootstrapping a new team repo from the spec @agent
- [ ] `skills/team-repo-interact.md` — skill for searching, ingesting, and contributing PRs to a team repo @agent
- [ ] `context/team-repos.md` pattern — lightweight index of team repos the worker interacts with @agent
- [ ] Update AGENTS.md to document team repo integration model @agent

### Work Changelog — Deferred

- [ ] Retroactive timeline population — offer to backfill entries from git history for existing projects (draft for user review, don't auto-commit) @agent
- [ ] Proactive periodic reports — agent offers weekly/monthly writeups on a cadence ("It's Friday — want a weekly writeup?"); start on-demand, graduate to proactive if pattern proves useful @agent
- [ ] Entry density calibration — tune capture aggressiveness based on user feedback; currently defaults to "when in doubt, don't prompt" @user

### Other Backlog

- [ ] Evaluate publishing/syncing pipeline options (GitHub Actions vs manual review)
- [ ] Determine metadata-based vs path-based filtering for shareable content
- [ ] Consider competitive-analysis template (referenced in README but not yet created)
- [ ] Test Windsurf skill auto-invocation with `.windsurf/skills/` files
- [ ] Define bidirectional linking rule for cross-domain content (primary home + source links via relative paths; integration via triage) @agent

## Completed

- [x] Initial bootstrap — AGENTS.md, CLAUDE.md, rules/, skills/, templates/, scaffolding done:2026-02-09
- [x] Skills & rules port (8/11) — README, decision-system, task-system, entity-verification, inbox-triage, template-propagation, stakeholder-intelligence, source-processing done:2026-02-09
- [x] Processed meta/inbox item — 4 backlog items added to roadmap done:2026-02-10
- [x] rules/git-workflow.md — commit discipline and session boundaries folded into communication.md done:2026-02-13
- [x] Work changelog — implemented changelog awareness rule, rollup skill, template scaffolding, and rule hooks done:2026-02-15
- [x] Inbox triage enhancements — CriticMarkup enrichment pass, hub-and-spoke entity timelines, domain emergence and corpus backfill done:2026-02-16

---
created: 2026-02-09
updated: 2026-02-18
tags: [meta, tasks]
status: active
---

# Tasks

Cross-domain task tracker. Check this at session start.

Tasks may also appear in domain-specific files for local context, but this file is the authoritative cross-domain view.

---

## Active

### Harness Build — Post-Port

- [x] Build /onboarding skill — guided conversational flow that asks the real user for requisite info (company, team, stakeholders, domains) and creates context files done:2026-02-16 @agent
- [ ] Test inbox → triage → domain flow with real inbox items @agent
- [ ] Create first domain from actual work content (don't pre-seed) @user

## Backlog

### External Repo Integration (Public + Team)

WAH domains can optionally link to external Git repos where the external repo is the Source of Record for base/shared knowledge and WAH provides a private augmentation layer. Two subtypes: public repos (open/community knowledge bases) and team repos (org-internal shared knowledge).

**Design spec**: [meta/specs/public-repo-design.md](meta/specs/public-repo-design.md)

Key decisions captured:

- **Layered model**: External repo is SOR for shared knowledge; WAH domain is the private overlay (annotations, company context, unpublished drafts)
- **Clone management**: External repos cloned to `.repos/` at WAH root (gitignored)
- **Registry**: `context/external-repos.md` tracks all external repos and their linked domains
- **Independence**: External repo is fully self-contained — own templates, rules, agent instructions
- **Contribution via PRs**: Agent drafts content, creates branch and PR; user reviews for privacy before merging
- **Bidirectional flow**: WAH consumes external repo content AND contributes back; also supports creating new public repos from WAH domain content (externalization)

All design questions resolved (see spec D1-D5): one repo per domain, ask at creation, agent-assisted scrubbing + user review, on-demand clone with active-domain fetch, batch contributions.

Deliverables — Shared primitives (serve both public and team repos):

- [x] Add `.repos/` to WAH `.gitignore` @agent done:2026-02-18
- [x] Create `context/external-repos.md` template @agent done:2026-02-18
- [x] Add `external_repo` fields to domain template frontmatter @agent done:2026-02-18
- [x] Document external repo concept in AGENTS.md @agent done:2026-02-18
- [x] Add external repo phase to onboarding skill @agent done:2026-02-18
- [ ] `skills/external-repo-link.md` — skill for cloning and linking an external repo to a domain @agent
- [ ] `skills/external-repo-contribute.md` — skill for preparing and submitting PRs to external repos @agent

Deliverables — Public repo specific:

- [ ] `skills/external-repo-create.md` — skill for externalizing WAH domain content into a new public repo @agent
- [ ] Privacy review checklist for public contributions @agent

Deliverables — Team repo specific (builds on shared primitives):

- [ ] `templates/team-repo/` — scaffold/spec for creating a new shared team context repo @agent
- [ ] Team-specific contribution defaults (more frequent, less guarded than public) @agent
- [ ] Update AGENTS.md to document team repo integration model @agent

### Work Changelog — Deferred

- [ ] Retroactive timeline population — offer to backfill entries from git history for existing projects (draft for user review, don't auto-commit) @agent
- [ ] Proactive periodic reports — agent offers weekly/monthly writeups on a cadence ("It's Friday — want a weekly writeup?"); start on-demand, graduate to proactive if pattern proves useful @agent
- [ ] Entry density calibration — tune capture aggressiveness based on user feedback; currently defaults to "when in doubt, don't prompt" @user

### Research Review

- [ ] Review CoworkPowers analysis ([meta/research/coworkpowers-analysis.md](meta/research/coworkpowers-analysis.md)) — techniques for adoption and future expansion @user

### Newsworthy Events — Revisit When Integrations Mature

- [ ] Design newsworthy-events skill for corporate context — proactive surfacing of external signals (competitor moves, regulatory changes, industry news) from intranet and other corporate sources. Deferred: existing skills (inbox triage, domain-source-synthesis) cover ad-hoc and systematic cases today; revisit as corporate intranet/API integrations become available. @agent

### Other Backlog

- [ ] Consider competitive-analysis template (referenced in README but not yet created)
- [ ] Test Windsurf skill auto-invocation with `.windsurf/skills/` files
- [ ] Define bidirectional linking rule for cross-domain content — should trigger naturally when first real cross-domain content surfaces; primary home + source links via relative paths, integration via triage @agent

## Completed

- [x] Initial bootstrap — AGENTS.md, CLAUDE.md, rules/, skills/, templates/, scaffolding done:2026-02-09
- [x] Skills & rules port (8/11) — README, decision-system, task-system, entity-verification, inbox-triage, template-propagation, stakeholder-intelligence, source-processing done:2026-02-09
- [x] Processed meta/inbox item — 4 backlog items added to roadmap done:2026-02-10
- [x] rules/git-workflow.md — commit discipline and session boundaries folded into communication.md done:2026-02-13
- [x] Work changelog — implemented changelog awareness rule, rollup skill, template scaffolding, and rule hooks done:2026-02-15
- [x] Inbox triage enhancements — CriticMarkup enrichment pass, hub-and-spoke entity timelines, domain emergence and corpus backfill done:2026-02-16
- [x] Skills & rules port complete (11/11) — mcp-optimization skipped (not relevant), newsworthy-events moved to backlog (revisit when corporate integrations mature) done:2026-02-16
- [x] Cross-agent mirror sync validation — full verification passed, all 23 mirrors correct done:2026-02-16

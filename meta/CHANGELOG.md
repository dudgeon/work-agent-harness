# Changelog

Tracks significant changes to the harness framework itself (not domain content).

---

## 2026-02-09 — Initial Bootstrap

- Created AGENTS.md (primary instruction routing document)
- Created CLAUDE.md (minimal Claude Code wrapper)
- Created rules/: philosophy.md, communication.md, context-management.md
- Created skills/: inbox-triage.md, domain-synthesis.md
- Created skills/README.md with agentskills.io spec alignment
- Created templates/: domain.md, concept.md, project.md
- Created scaffolding: tasks.md, inbox/README.md
- Created .windsurf/ and .claude/ mirror directories with READMEs
- Created meta/ folder, moved STARTER-PROMPT.md there
- Created meta/CHANGELOG.md and meta/ROADMAP.md

## 2026-02-09 — Skills & Rules Port (in progress)

- Created skills/README.md with agentskills.io spec alignment
- Created rules/decision-system.md — conversational-first decision documentation
- Created rules/task-system.md — tasks.md as SoR, domain clustering, @agent/@user ownership, date format, mirror sync
- Created skills/entity-verification.md — utility skill for resolving vague entities, called by other skills
- Enhanced skills/inbox-triage.md — added unstructured input decomposition section, entity-verification integration
- Restructured: dictation-processing moved to backlog as thin voice-specific overlay; core decomposition logic lives in inbox-triage
- Created skills/template-propagation.md — update derived documents when templates change structurally
- Created skills/stakeholder-intelligence.md — proximity-based relationship tracking, guided new-person onboarding, multi-person interaction updates
- Created skills/source-processing.md — generic source capture → knowledge pipeline with topic lifecycle, frontmatter-based topic discovery, initialization workflow

## 2026-02-10 — Inbox Triage & Documentation

- Processed meta/inbox item (dictated backlog ideas) → 4 new backlog items on roadmap: cross-repo inbox bridging, FTUX onboarding, task complexity tiers, inbox triage scoping
- Created meta/inbox/_archive/ structure
- Updated tasks.md with domain clustering, completed items, remaining port triage items

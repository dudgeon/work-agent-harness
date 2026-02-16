# Changelog

Tracks significant changes to the harness framework itself (not domain content). Entries are in **reverse chronological order** (newest first).

---

## 2026-02-16 — Inbox Triage Enhancements: Enrichment, Hub-and-Spoke, Domain Emergence

- **CriticMarkup enrichment pass** — new triage step: before archiving, agent scans raw notes for questions/gaps answerable from existing WAH content and annotates inline using `{>>YYYY-MM-DD @agent: ...<<}` syntax. Boundary: only from WAH content, never web or speculation.
- **Hub-and-spoke entity timeline updates** — meeting note is the authoritative hub in `inbox/_archive/`; person and project entity files get lightweight reverse-chronological timeline entries pointing back to it. Cross-links between spokes only for durable relationships, not per-meeting.
- **Domain emergence and corpus backfill** — triage now detects when routing reveals a missing domain (clustering signal). Flow: propose domain → create with approval → bounded backfill scan of existing notes for connections → execute approved changes.
- Updated processing workflow from 9 to 12 steps integrating all three capabilities
- Updated skills/README.md table description
- Design decision: dedicated meeting-processing skill deferred — generic unstructured input flow covers meeting notes for now
- Refined backlog: narrowed "inbox triage scoping" to recurring pattern recognition (domain emergence handles implicit scope); cross-referenced hub-and-spoke with bidirectional linking rule; added deferred meeting-processing skill

## 2026-02-13 — Domain Source Synthesis Pattern & Template Evolution

- **Named the domain-source-synthesis pattern** — canonical name for the reusable "capture sources → extract atomic knowledge → build knowledge base" architecture that originated in home-brain's ai-pm-craft domain
- Renamed skills/source-processing.md → skills/domain-source-synthesis.md — the skill now documents both the pattern architecture and the operational procedure
- Added domain-level scoping: domains declare `pattern: domain-source-synthesis` in frontmatter and include a Source Synthesis Scope section defining learning goal, in/out scope, quality bar, and classification approach
- Promoted from ai-pm-craft: taxonomy mechanism, featured entry marking, note source handling, workflow decomposition guidance
- Added domain-specific extension mechanism via `meta/processing-guide.md` within domains
- Updated inbox-triage.md with source-synthesis domain detection section
- Restored "Number all items" rule to communication.md (accidentally dropped during formalization from home-brain)
- Updated all agent mirrors (.claude/skills/, .windsurf/skills/)
- Created home-brain backpropagation spec (inbox/home-brain-backpropagation-spec.md) — 8-part change spec for applying WAH improvements back to ancestor repo

## 2026-02-13 — Git Workflow Triage & Retro Process Improvement

- Folded git-workflow guidance into communication.md (no standalone rule): Commit Discipline section (message format, timing, safety), Session Boundaries section (start/end checklists), surgical merge resolution guidance
- Removed standalone rules/git-workflow.md and .windsurf/rules/git-workflow.md mirror
- Updated AGENTS.md and CLAUDE.md to remove git-workflow.md from always-loaded list
- Strengthened retro process: step 2 now requires per-finding evaluation instead of allowing batch "no changes needed" conclusions
- Restored meta/research/cross-repo-agent-analysis.md from divergent local branch
- Added bidirectional linking rule to backlog
- Resolved 1-vs-6 commit divergence with remote using surgical per-file merge strategy

## 2026-02-10 — Inbox Triage & Documentation

- Processed meta/inbox item (dictated backlog ideas) → 4 new backlog items on roadmap: cross-repo inbox bridging, FTUX onboarding, task complexity tiers, inbox triage scoping
- Created meta/inbox/_archive/ structure
- Updated tasks.md with domain clustering, completed items, remaining port triage items

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

# Roadmap

What's planned for the harness framework. Organized by phase.

---

## In Progress — Porting Home-Brain Skills & Rules

Working through home-brain's `.claude/` files one at a time, adapting for corporate/PM context:

- [x] skills/README.md — meta-skill guidance with agentskills.io alignment
- [x] rules/decision-system.md — ADR pattern, decision detection
- [x] rules/task-system.md — task conventions and ownership
- [x] skills/entity-verification.md — utility skill for resolving vague entities
- [x] skills/inbox-triage.md — enhanced with unstructured input decomposition section
- [x] skills/template-propagation.md — update docs when templates change
- [x] skills/stakeholder-intelligence.md — proximity-based relationship tracking with guided onboarding
- [x] skills/source-processing.md → **skills/domain-source-synthesis.md** — named the pattern; promoted taxonomy mechanism, featured marking, note sources, decomposition guidance from ai-pm-craft; added domain-level scoping and extension mechanism
- [x] rules/git-workflow.md — commit discipline and session boundaries folded into communication.md
- [ ] rules/mcp-optimization.md — writing for discoverability (relevance TBD)
- [ ] newsworthy-events.md — likely skip

## In Progress — Template Evolution & Bidirectional Sync

Designing the system for WAH (template) and its instances (home-brain, future brains) to evolve together:

- [x] Named domain-source-synthesis pattern — first pattern extracted from home-brain, generalized, ready for re-instantiation
- [x] Backpropagation spec for home-brain — 8-part change spec in inbox/home-brain-backpropagation-spec.md
- [x] Restored "Number all items" rule (accidentally dropped from home-brain during formalization)
- [ ] Execute home-brain backpropagation spec — apply in a Claude Code session on home-brain repo
- [ ] Validate domain-source-synthesis with a second domain (first work domain to use the pattern)
- [ ] Refactor ai-pm-craft skill to wrap generic skill (after second domain validates)
- [ ] Design template evolution system — lineage tracking, binding classification, adoption workflow (see session analysis for conceptual framework)

## Completed — Inbox Triage Enhancements

Enriched the inbox-triage skill with three new capabilities driven by the meeting-notes use case:

- [x] CriticMarkup enrichment pass — agent annotates raw notes with answers from existing WAH content before archiving (`{>>YYYY-MM-DD @agent: ...<<}`)
- [x] Hub-and-spoke entity timeline updates — meeting note as authoritative hub; person and project entities get lightweight timeline entries pointing back
- [x] Domain emergence and corpus backfill — detect when triage reveals a missing domain; propose creation; run bounded backfill scan for existing connections

Design decision: a dedicated meeting-processing skill was considered and deferred. The generic unstructured input flow in inbox-triage covers meeting notes for now. If meeting-specific parsing (attendees, agenda structure) proves valuable, it can be extracted later.

## Completed — Work Changelog

Implemented from [meta/specs/work-changelog.md](specs/work-changelog.md):

- [x] Template scaffolding — project.md v2 (enriched Timeline), domain.md v2 (added Timeline section)
- [x] Changelog awareness rule — always-on detection of milestones, decisions, wins, etc. with session start/end hooks
- [x] Rule hooks — task-system, decision-system, communication rules updated with changelog integration points
- [x] Domain-synthesis update — timeline health review step added
- [x] Changelog rollup skill — aggregation into reports (annual review, stakeholder update, retrospective, self-reflection)
- [x] Cross-agent mirrors — .claude/skills/, .windsurf/skills/, .windsurf/rules/
- [x] Explainer doc — meta/docs/how-changelog-works.md (platform-specific instructions, scoping guide)

Deferred: retroactive population from git history, proactive periodic reports, entry density calibration.

## Planned — Post-Port

- [ ] Decision templates (decision.md, decision-quick.md)
- [ ] New root README.md — written for new users (setup instructions, core concepts, getting started). Move current README.md to meta/ as the design document.
- [ ] Sample knowledge base — mock up example domains (clearly marked as samples) to show the system in action
- [ ] context/writing-style/ — folder with README.md demonstrating the pattern of gathering artifacts (writing samples, style notes) that get distilled into rules/skills. Flesh out input sample frontmatter, processing pipeline, etc.
- [ ] Test inbox → triage → domain flow with real content
- [ ] Validate cross-agent sync (Windsurf mirror loading behavior)
- [ ] Create first domain from actual work content
- [ ] Evaluate agentskills.io directory-based SKILL.md migration

## Backlog

- [ ] Evaluate GitHub Issues as task SoR — agent-managed issues with domain labels, mirrored in domain files with issue refs. Both Claude Code and Windsurf can use `gh` CLI. Revisit when tasks.md friction becomes real.
- [ ] Publishing/syncing pipeline (GitHub Actions vs manual review)
- [ ] Metadata-based vs path-based filtering for shareable content
- [ ] Competitive analysis template
- [ ] INDEX.md (when there's content to index)
- [ ] skills/dictation-processing.md — lightweight voice-specific overlay (homonyms, speech artifacts, pronoun resolution) on top of inbox-triage + entity-verification
- [ ] Cross-repo inbox bridging — give agents in this repo visibility into home-brain/inbox (e.g., add folder to VSC project scope); detect items relevant to this project; mark routing intentionally (process here, there, or both); replicate pattern for other active project repos
- [ ] FTUX onboarding experience — agent instructions + task list + infra for first-time users of a new harness instance; syllabus of core primitives/concepts to explore; questions agent should ask a new user; checklist tracking when each component has been demonstrated with references to where/how
- [ ] Task complexity tiers — extend tasks.md to support medium-complexity tasks (bigger than a todo, smaller than a multi-month project); think through what additional structure medium tasks need (associated files, subtasks, progress tracking) without over-engineering into full project management
- [ ] Inbox triage scoping — pre-register recurring item patterns (e.g., "weekly 1:1 with Jared" = covers projects X/Y/Z, "monthly leadership update" = parent org summary, covers X teams). Domain emergence detection now handles the "implicit scope" part; this item is narrowed to pattern recognition for recurring inputs.
- [ ] Bidirectional linking rule — define cross-domain linking conventions (primary home + source links via relative paths; integration via triage). Note: hub-and-spoke pattern in inbox-triage establishes the first concrete convention (meeting note as hub, entities as spokes). General rule should build on this.
- [ ] Meeting-processing skill — dedicated skill for structured meeting parsing (attendees, agenda, multi-project decomposition). Deferred: generic unstructured input flow covers this for now. Extract if meeting-specific structure proves valuable after real usage.

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
- [ ] Inbox triage scoping — triage should detect when items carry implicit scope the agent doesn't know and proactively ask for context; support pre-registering recurring item patterns (e.g., "monthly leadership update" = parent org summary, covers X teams, user cares about Y) so the agent learns the pattern once

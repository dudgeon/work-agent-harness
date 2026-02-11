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
- [x] skills/source-processing.md — generic source capture → knowledge pipeline with topic lifecycle
- [ ] rules/git-workflow.md — commit discipline, session reminders (scope TBD)
- [ ] rules/mcp-optimization.md — writing for discoverability (relevance TBD)
- [ ] newsworthy-events.md — likely skip

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

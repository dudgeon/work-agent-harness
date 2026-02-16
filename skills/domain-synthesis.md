---
name: domain-synthesis
description: Synthesize information across a domain. Use when user asks to review a domain, identify patterns, update a domain overview, flag gaps, or suggest reorganization.
---

# Domain Synthesis

Reading all files in a domain, identifying patterns, updating the domain README, flagging gaps or stale content, and suggesting reorganization.

---

## When This Applies

- User asks to "review" or "synthesize" a domain
- A domain has accumulated significant new content since its last review
- During periodic domain health checks
- When preparing a domain overview for sharing or decision-making

---

## The Goal

A domain README that accurately reflects the current state of knowledge, with no orphaned notes, no stale content, and clear structure that aids both human and agent navigation.

---

## Synthesis Workflow

### 1. Inventory the Domain

Read every file in the domain (and subdomain) tree:
- List all files and subfolders
- Note which files have been recently updated vs potentially stale
- Check frontmatter for status, dates, and tags

### 2. Identify Patterns

Look across the domain's content for:
- **Recurring themes** — concepts or topics that appear in multiple notes
- **Emerging sub-domains** — clusters of content that might warrant their own folder
- **Cross-domain connections** — links or references to other domains or context files
- **Decision patterns** — repeated trade-offs or principles the user applies in this area
- **Knowledge gaps** — topics referenced but not yet documented
- **Contradictions** — notes that disagree with each other or with context files

### 3. Update the Domain README

The domain README should reflect current reality. Update it to include:
- **Purpose**: What this domain covers and why it exists
- **Current state**: Summary of what's here now
- **Key concepts**: The most important files/ideas, with links
- **Sub-domains**: If subfolders exist, what they contain
- **Open questions**: What's unresolved or under-explored
- **Related domains**: Links to adjacent domains or context files

Use [templates/domain.md](../templates/domain.md) as the structural guide.

### 4. Flag Issues

Report to the user:
- **Stale content**: Notes not updated in a long time that may be outdated
- **Orphaned notes**: Files with no inbound links from other notes
- **Missing metadata**: Notes lacking proper frontmatter
- **Structural suggestions**: Sub-domains that should be created, split, or merged
- **Content gaps**: Topics the domain should cover but doesn't yet

### 5. Review Timeline Health

Check the domain README's Timeline section and project-level Timeline sections within the domain.

**Flag:**
- **Stale timelines**: Active projects (`status: active`) with no timeline entries in the last 30 days
- **Timeline gaps**: Evidence of significant work (file changes, completed tasks) but no corresponding timeline entries
- **Under-documented entries**: Timeline entries with vague summaries, missing links, or no context bullets
- **Missing timelines**: Domain READMEs or project files with empty or absent Timeline sections

**Don't:**
- Generate timeline entries yourself — that's the [changelog awareness rule's](../rules/changelog-awareness.md) job
- Flag old/completed projects for stale timelines — those are naturally quiet
- Expect timeline entries for routine maintenance (file reorganization, metadata fixes)

This is a health check, not timeline generation. Report what you observe and suggest the user capture missing entries if they remember the context.

### 6. Suggest Reorganization (If Warranted)

Only suggest structural changes when there's clear benefit:
- A subfolder has grown enough to become its own sub-domain
- Multiple notes should be merged into a single concept file
- Content is in the wrong domain and should be moved
- The domain has grown unwieldy and needs splitting

**Always present reorganization proposals to the user for approval.** Don't restructure autonomously.

---

## Output Format

After synthesis, present findings as:

```
## Domain Synthesis: [domain-name]

### Summary
[2-3 sentence overview of the domain's current state]

### Key Findings
- [Pattern or insight 1]
- [Pattern or insight 2]

### Issues Found
- [ ] [Stale/orphaned/missing metadata items]
- [ ] [Timeline health issues]

### Recommended Actions
- [ ] [Specific actionable items]

### README Updated
[Yes/No — what changed]
```

---

## When NOT to Reorganize

- The domain has very few files (under 5) — synthesis is premature
- The user is actively working in the domain — wait for a natural pause
- Changes would break many existing links without clear benefit
- The proposed structure is speculative rather than evidence-based

---

*Domain synthesis is a maintenance activity. It should clarify, not complicate. When in doubt, document what exists rather than proposing what should change.*

---
name: changelog-rollup
description: Aggregate timeline entries into reports for annual reviews, stakeholder updates, retrospectives, or self-reflection. Use when user asks to prepare review material, draft an update, or summarize work across time periods.
---

# Changelog Rollup

Aggregating timeline entries from project and domain files into formatted reports for reviews, stakeholder updates, retrospectives, and reflection.

---

## When This Applies

User requests reporting or aggregation across work timelines:

- "Prepare my annual/quarterly review material"
- "Draft a stakeholder update for [projects/domains]"
- "What have I accomplished [time period]?"
- "Summarize progress on [project/domain/initiative]"
- "Write a weekly/monthly writeup for [domain]"

---

## The Goal

A formatted report that synthesizes timeline entries into a coherent narrative appropriate to the audience (self, manager, stakeholders, team) and purpose (annual review, status update, retrospective, reflection).

---

## Rollup Workflow

### 1. Determine Scope

From the user's request, identify:

- **Projects/domains**: Which to include (specific list, or "all active")
- **Date range**: Time window (last week, Q3, YTD, all time, since project start)
- **Audience**: Who will read this (self-reflection, manager, stakeholders, team)
- **Focus**: All entry types, or filtered (e.g., only milestones and wins for annual review)

**If ambiguous, ask one clarifying question.** Default to broad scope — easier to cut than re-run.

**Scoping examples:**
- "Prepare my annual review" → all active projects/domains, current calendar year, manager audience, milestones + wins + decisions + learnings
- "Draft update for platform migration" → single project, last 2 weeks, stakeholder audience, all types
- "What did I accomplish in Q4?" → all domains, Oct 1 – Dec 31, self-reflection, all types

### 2. Collect Timeline Entries

Navigate the predictable structure:

1. **For each domain in scope:** Read `domains/[domain]/README.md` → `## Timeline` section
2. **For each project in scope:** Read project file → `## Timeline` section
3. **Filter by date range:** Include only entries within the specified time window
4. **Follow cross-domain links:** If an entry references related domains/projects, fetch those entries too for context

**Handle missing data gracefully:**
- Empty Timeline section → note "No timeline entries for [project/domain]"
- No Timeline section at all → skip (don't error)
- Date range yields nothing → report "No timeline entries found for [date range]" and suggest expanding scope

### 3. Organize and Synthesize

Don't just concatenate entries. Apply structure based on purpose:

**For annual reviews:**
- Group by domain/project
- Lead with milestones and wins (accomplishments)
- Include decisions that demonstrate judgment
- Include learnings that show growth
- Omit routine progress unless it builds to a larger story

**For stakeholder updates:**
- Group by project
- Lead with what shipped/changed (milestones, progress)
- Include decisions that affect stakeholders
- Surface blockers-resolved and scope-changes (show responsiveness)
- End sections with "What's next" if `tasks.md` has relevant pending work

**For retrospectives:**
- Chronological within the project (narrative arc)
- Include all entry types to show full evolution
- Highlight decision points, turning points, learnings
- Connect entries to show cause-and-effect

**For self-reflection:**
- Chronological across domains (where did time go?)
- Identify themes and patterns (what types of work dominated?)
- Surface learnings and wins
- Note gaps or imbalances (heavy in one area, neglect in another?)

### 4. Draft Output

Produce a formatted report. Templates by purpose:

**Annual review material:**
```markdown
# [Year] Accomplishments

## [Domain/Project Name]

**Key Milestones:**
- [Milestone entry with date and impact]

**Significant Decisions:**
- [Decision with brief rationale and outcome]

**Learnings:**
- [Learning with context]
```

**Stakeholder update:**
```markdown
# [Project] Status Update — [Date Range]

## [Project Name]

**What Changed:**
[1-2 paragraphs synthesizing recent milestones and progress]

**Key Decisions:**
- [Decision with links]

**What's Next:**
- [Upcoming tasks from tasks.md]
```

**Retrospective:**
```markdown
# [Project] Retrospective

## Timeline
[Chronological narrative woven from timeline entries]

## Key Learnings
[Synthesized insights from [learning] entries and decision outcomes]

## Outcomes
[What was accomplished, what changed, what was left undone]
```

**Self-reflection:**
```markdown
# [Time Period] Reflection

## Where Time Went
[Summary of domains/projects with activity]

## Highlights
- [Win entries and major milestones]

## What I Learned
- [Learning entries]

## Patterns
[Observations about work distribution, themes, gaps]
```

### 5. Output Location

- **Domain-scoped reports** → `domains/[domain]/reports/YYYY-MM-DD-[type].md`
- **Cross-domain reports** → `reports/YYYY-MM-DD-[type].md`
- **Quick summaries** → present inline without creating a file

Present the draft to the user for review before finalizing. Reports are creative writing — the user may want to adjust tone, emphasis, or framing.

---

## Scoping Changelog Requests

Users can control what's included via natural language. The agent interprets scope from the request:

**By domain/project:** "Just the platform migration project" or "everything in the strategy domain"
**By time range:** "Last 2 weeks", "Q3", "since the project started", "this year"
**By entry type:** "Only milestones and wins" (for reviews) or "everything including progress" (for retrospectives)
**By audience:** "For my manager" (formal, accomplishment-focused) or "for myself" (honest, reflective)

When the request is vague, ask one clarifying question. Default broad, cut later.

---

## Watch Out For

- **Synthesis, not concatenation**: A good report tells a story. Don't just list entries.
- **Over-attribution**: Not every entry belongs in every report type. Filter aggressively.
- **Speculation**: Don't infer accomplishments not captured in timeline entries. If there are gaps, note them and suggest retroactive capture.
- **Scope creep**: This skill aggregates existing entries. It does NOT generate new timeline entries, audit for uncaptured work, or backfill from git history.

---

*Changelog rollup transforms accumulated timeline entries into narrative reports. The quality of output depends on the quality of captured entries.*

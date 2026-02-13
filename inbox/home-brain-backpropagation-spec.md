---
created: 2026-02-13
tags: [meta, home-brain, backpropagation, domain-source-synthesis]
status: active
context: Change spec for backpropagating WAH improvements to home-brain. Execute in a Claude Code session with home-brain repo access.
---

# Home-Brain Backpropagation: Domain Source Synthesis & Structural Upgrades

This document specifies changes to home-brain that backpropagate improvements from work-agent-harness (WAH). It's designed to be executed by an agent in a Claude Code session on the home-brain repo.

**Scope**: Domain-source-synthesis pattern + the highest-confidence structural upgrades identified during the WAH/home-brain comparison.

**Safety constraint**: No change should alter existing ai-pm-craft behavior. All changes are additive or refactoring that preserves current functionality.

---

## Part 1: Bring in the Generic Domain-Source-Synthesis Skill

### New file: `.claude/skills/domain-source-synthesis.md`

Copy the full content of WAH's `skills/domain-source-synthesis.md` into home-brain's `.claude/skills/domain-source-synthesis.md`.

Adjustments for home-brain context:
- The file path convention is `.claude/skills/` (not `skills/`)
- No mirror architecture needed (home-brain is Claude-only)
- The skill is self-contained, not a thin wrapper

### Annotate ai-pm-craft's skill as an extension

At the top of `.claude/skills/ai-pm-craft-source-processing.md`, add a note:

```markdown
> **Pattern**: This skill extends [domain-source-synthesis](domain-source-synthesis.md) with domain-specific guidance for the AI PM Craft learning project. The generic skill handles the source lifecycle; this file adds ai-pm-craft's taxonomy, extraction heuristics, and specialized workflows.
```

Do NOT refactor ai-pm-craft's skill to remove duplicate logic yet. That refactoring happens later, after the generic skill has been validated with a second domain.

### Add frontmatter to ai-pm-craft's domain README

In `domains/professional-development/ai-pm-craft/README.md`, add to the frontmatter:

```yaml
pattern: domain-source-synthesis
source_types: [article, video, podcast, book-chapter, organic, note]
```

---

## Part 2: Agentskills.io Frontmatter on All Skills

Add `name:` and `description:` frontmatter to every skill file in `.claude/skills/` that doesn't already have it. Based on the current files:

### `.claude/skills/inbox-triage.md`
```yaml
---
name: inbox-triage
description: Process and route inbox items to appropriate domains. Use when user says "process inbox", "triage inbox", "clear inbox", or after completing tasks to check for accumulated items.
---
```

### `.claude/skills/dictation-processing.md`
```yaml
---
name: dictation-processing
description: Transform unstructured voice dictation into structured knowledge. Use when processing dictated input, voice memos, or stream-of-consciousness text dumps.
---
```

### `.claude/skills/person-intelligence.md`
```yaml
---
name: person-intelligence
description: Handle person notes, multi-person interaction updates, and entity tracking. Use when processing interactions involving multiple people, creating person files, or updating relationship context.
---
```

### `.claude/skills/entity-verification.md`
```yaml
---
name: entity-verification
description: Verify and resolve vague entities before capturing. Use when encountering approximate names (companies, people, products, tools, frameworks) or links that contain canonical names. Research before asking the user.
---
```

### `.claude/skills/template-propagation.md`
```yaml
---
name: template-propagation
description: Update documents when templates change. Use when a template has structural changes, user asks to "propagate template changes", or documents have outdated template_version.
---
```

### `.claude/skills/newsworthy-events.md`
```yaml
---
name: newsworthy-events
description: Link to trusted sources for newsworthy events affecting family. Use when capturing news events, natural disasters, or public incidents relevant to family members or their locations.
---
```

### `.claude/skills/ai-pm-craft-source-processing.md`
```yaml
---
name: ai-pm-craft-source-processing
description: Process sources and extract knowledge for the AI PM Craft learning project. Use when user shares content for ai-pm-craft, asks to process sources, or wants reading queue status for the AI PM learning topic.
---
```

---

## Part 3: Post-Triage Annotation in Inbox Triage

Add this section to `.claude/skills/inbox-triage.md` after the "Processing Inbox Workflow" section (before the final closing line):

```markdown
---

## Post-Triage Item Annotation

When filing an inbox item, annotate it with triage metadata before archiving:

\`\`\`yaml
---
# Original frontmatter preserved above
triage_status: filed|merged|discarded
triage_date: YYYY-MM-DD
triage_action: "Brief description of what was done"
triage_destination: "path/to/destination.md"
---
\`\`\`

This creates an audit trail: what came in, what happened to it, where it went.
```

Also add source-synthesis domain detection, before the "Watch Out For" section:

```markdown
---

## Source-Synthesis Domain Detection

Some inbox items are destined for domains that use the [domain-source-synthesis](domain-source-synthesis.md) pattern. When triaging:

1. **Check for domain/project tags** — if an item is tagged for a specific domain (e.g., `project: ai-pm-craft`), check whether that domain has `pattern: domain-source-synthesis` in its README
2. **Hand off to the pattern** — route to domain-source-synthesis (or a domain-specific extension like ai-pm-craft-source-processing) for capture rather than filing generically
3. **Source vs non-source** — items with a source URL route to the capture workflow; items describing personal experience route to the organic idea workflow
```

Note: This replaces the existing "Project-Tagged Items" section with a more generic mechanism. The existing ai-pm-craft routing still works — the agent checks the domain README, sees `pattern: domain-source-synthesis`, and routes to the ai-pm-craft extension skill.

---

## Part 4: Entity Verification Resolution Order

Replace the "How to Think About It" section in `.claude/skills/entity-verification.md` with the formalized version:

```markdown
## Resolution Order

When resolving an entity, check these in order:

1. **Context files** — `context/*.md` (family context, people files, domain READMEs)
2. **Domain files** — relevant domain notes, project files, concept files
3. **Web research** — search to confirm details, find canonical names, get links
4. **Ask the user** — only after exhausting the above

## How to Think About It

**Links are verification opportunities.** When the user provides a URL, the source likely contains the canonical name. Fetch and verify before capturing.

**Do the research before asking.** Don't immediately ask the user when direct fetch fails. Try multiple approaches:
1. Fetch the URL directly
2. If blocked, try alternative access methods (different URL format, cached version, Chrome MCP)
3. Search for the URL content via web search
4. Search for the vague description with qualifying terms
5. Cross-reference results to confirm identity

**Ask only as a last resort.** After exhausting research options, then ask the user for the actual name.

**Specificity matters for retrieval.** Vague names make notes harder to find later. "The analytics tool" should become "Amplitude" (with link). "Mike's wife" should become "Mike and Sarah Chen."
```

Add after "Watch Out For":

```markdown
## After Verification

When you've resolved an entity:
1. Use the canonical name in the note being created
2. Add a link if one was found (company website, product page, LinkedIn, etc.)
3. If this is a recurring entity, consider whether it belongs in a context file for future resolution
4. Note the verification in your work — "Confirmed: [vague reference] is [canonical name]"
```

---

## Part 5: Person Intelligence Improvements

Add these sections to `.claude/skills/person-intelligence.md`:

### Add "Relationship Proximity" section (after Core Structure):

```markdown
## Relationship Proximity

People in your world exist at different distances and need different levels of tracking:

**Close circle** — family, close friends, people you interact with daily/weekly. Build rich profiles over time.

**Regular contacts** — friends, colleagues, recurring acquaintances. Know their role, context, and how you interact.

**Extended network** — people you encounter occasionally. Lightweight entries — name, context, last interaction, anything notable.

### When Encountering a New Person

Don't assume the user remembers this model. When a new person comes up, ask clarifying questions:

> "I haven't seen [name] before. A few quick questions:"
>
> 1. **How do you know them?** (family friend, colleague, kid's friend's parent, etc.)
> 2. **How often do you interact?** (regularly, occasionally, one-off)
> 3. **Anything I should know?** (context that would help track them)

Based on answers, decide file vs lightweight entry vs child's friends.md entry.
```

### Add "Detecting Relationship Changes" section (before or after Multi-Person Interactions):

```markdown
## Detecting Relationship Changes

Watch for signals that someone's proximity is shifting:
- User starts mentioning someone much more frequently → may need an individual file
- Someone gets a new role or life change that affects the user
- A casual acquaintance becomes important to a project or family activity
- Someone the user knew well drops out of mention → profile may be going stale

When you notice a shift, surface it:

> "[Name] has come up in 3 conversations this week. They're currently just in friends.md — should I create a full person file for them?"
```

### Add "Enrichment Checklist" section:

```markdown
## Enrichment Checklist

Before completing an update to any person file:

1. **Timeline entry** — Is there a date-anchored event to record?
2. **Profile updates** — Did this reveal new evergreen info (role change, new context, communication preference)?
3. **Cross-references** — Does this connect to other people, domains, or context files? Add links.
4. **Context files** — Are there cross-domain facts that belong in `context/` rather than a single person's file?
5. **Kids' cross-posting** — If a child was involved, does their memories.md need a timeline entry too?
```

---

## Part 6: Context Management Improvements

Add to `.claude/context-management.md`:

### Expand the fact detection criteria to include:

```markdown
- **Changes**: New information that updates previous understanding
- **Decisions**: Outcomes of discussions, rationale, who decided, what was considered
```

### Add "Handle Conflicting Information" section:

```markdown
## Handle Conflicting Information

If new information conflicts with existing context:
- **STOP and ask a clarification question** before proceeding
- Example: "The family context says Alex works at Google, but you just mentioned they moved to Stripe. Can you clarify?"
- Never silently overwrite potentially correct information
- Document resolution: `*Corrected: YYYY-MM-DD (was X, now confirmed as Y)*`
```

### Add "Context Gathering — When New Areas Open" section:

```markdown
## Context Gathering — When New Areas Open

When conversation opens a new context area:
1. Recognize the gap (e.g., "competitive landscape mentioned but no details captured")
2. Note follow-up questions that would improve future assistance
3. Reference during idle time or when user asks "what else do you need?"

Indicators that a new area opened:
- User mentions a domain or topic with insufficient current context
- You have questions but don't want to interrupt flow
- You identify gaps that would improve future assistance
```

---

## Part 7: Domain Synthesis Skill (New)

Create `.claude/skills/domain-synthesis.md` — copy from WAH's `skills/domain-synthesis.md`. Add frontmatter:

```yaml
---
name: domain-synthesis
description: Synthesize information across a domain. Use when user asks to review a domain, identify patterns, update a domain overview, flag gaps, or suggest reorganization.
---
```

This is an entirely new capability for home-brain.

---

## Part 8: Communication Rule — Restore "Number All Items"

Verify that `.claude/communication.md` includes this rule under "How to Communicate" → "Be concise but thorough":

```markdown
- **Number all items when presenting multiple things for feedback** — lists of inbox items, proposed actions, options, status updates. This lets the user respond quickly by number (e.g., "1 and 3" or "all except 2") instead of re-describing items.
```

(This rule exists in home-brain already but was accidentally dropped from WAH during formalization. It should also be added to WAH — see separate note below.)

---

## Execution Order

For safety, execute in this order:

1. **Part 2** (frontmatter) — pure metadata addition, zero risk
2. **Part 3** (inbox-triage additions) — additive sections, no existing logic changed
3. **Part 4** (entity-verification) — replaces section with improved version
4. **Part 5** (person-intelligence) — additive sections
5. **Part 6** (context-management) — additive sections
6. **Part 7** (domain-synthesis) — new file
7. **Part 1** (domain-source-synthesis) — new file + annotation of ai-pm-craft
8. **Part 8** (communication) — verification only

Each part can be committed independently. If anything feels wrong during execution, stop and flag.

---

## WAH Note: Backpropagate "Number All Items" Rule

WAH's `rules/communication.md` should also get this rule (it was accidentally dropped during formalization):

```markdown
- **Number all items when presenting multiple things for feedback** — lists of inbox items, proposed actions, options, status updates. This lets the user respond quickly by number instead of re-describing items.
```

Add under "Be concise but thorough" in `rules/communication.md`.

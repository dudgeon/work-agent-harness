---
name: domain-source-synthesis
description: Systematically build a knowledge base from sources within a domain. Use when user shares a link/article/video for a topic, asks to process or extract ideas, wants reading queue status, says "add this to [topic]", or wants to initialize source synthesis for a new domain.
---

# Domain Source Synthesis

A reusable pattern for systematically capturing, processing, and extracting knowledge from sources into a structured knowledge base within a domain. Any domain at any depth in the hierarchy can adopt this pattern independently.

**The pattern in one sentence**: Sources are raw material; knowledge entries are the refined product; lineage connects them.

---

## When This Applies

- User shares a link, article, video, or content to capture for a domain
- User wants to mark a source as read or add reading notes
- User asks to process/extract ideas from a source
- User wants to capture an organic idea (from personal experience, no external source)
- User asks about their reading queue or source status
- User wants to initialize source synthesis for a new domain
- Inbox triage encounters an item tagged for a source-synthesis domain

---

## The Goal

Every source captured should eventually be read, understood, and its discrete ideas extracted into atomic knowledge entries with full lineage. The knowledge base is the product; sources are the raw material.

---

## How Domains Adopt This Pattern

A domain declares source synthesis in its README frontmatter:

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
template: templates/domain.md
template_version: 1
pattern: domain-source-synthesis
source_types: [article, video, podcast, book-chapter, organic, note]
status: active
---
```

The `pattern: domain-source-synthesis` flag is the discovery mechanism. To find all active source-synthesis domains, scan domain READMEs for this flag. The `source_types` field declares what kinds of sources the domain accepts.

---

## Domain Scope Declaration

When a domain adopts this pattern, its README must include a **Source Synthesis Scope** section. This scoping tells the agent what the domain is about so it can make good extraction and classification judgments without needing a domain-specific skill file.

```markdown
## Source Synthesis Scope

**Learning goal**: [What understanding is being built — "How AI changes product management workflows"]

**In scope**: [What kinds of ideas belong here — "Techniques for using AI in product lifecycle phases, frameworks for AI-augmented decision making, adoption strategies"]

**Out of scope**: [What should go elsewhere — "General AI news, pure engineering implementation, company-specific strategy"]

**Quality bar**: [What makes an idea worth extracting — "Must describe a specific technique, framework, or non-obvious insight — not just an observation or opinion"]

**Classification**: [How ideas are organized]
- Taxonomy: [link to meta/taxonomy.md] — OR —
- Emergent: No taxonomy yet. Let structure emerge from content.
```

This scoping replaces the need for a domain-specific skill file in most cases. The generic skill reads the domain's scope declaration and uses it to guide extraction judgment.

---

## Infrastructure

When a domain adopts the pattern, initialize this structure:

```
domains/[path-to-domain]/
├── README.md              # pattern: domain-source-synthesis, scope section, knowledge map, reading queue
├── sources/
│   ├── _index.md          # Source registry with status tracking
│   └── [source files]
├── knowledge-base/
│   └── [knowledge entries, organized by taxonomy or flat]
└── meta/                  # Optional — for domains with classification needs
    └── taxonomy.md        # Domain-specific classification scheme
```

**README.md** gets:
- `pattern: domain-source-synthesis` in frontmatter
- `source_types` listing accepted source types
- Source Synthesis Scope section
- A Knowledge Map section (empty initially, grows as ideas are extracted)
- A Reading Queue section

**sources/_index.md** gets:
- A table tracking all sources: title, author, source type, status, date added, ideas extracted

### When to Initialize

Ask the user these questions when setting up:

> "Setting up source synthesis for [domain]. A few questions:"
>
> 1. **What's the learning goal?** (What understanding are you building?)
> 2. **What kinds of sources do you expect?** (Articles, videos, podcasts, books, organic ideas)
> 3. **Do you have an initial mental model for organizing ideas?** (Optional taxonomy — can add later)

---

## Source Lifecycle

### 1. Capture

A source arrives — user shares a link, forwards content, or mentions something to add.

1. **Determine source metadata**: title, author, source URL, source type, published date
   - Use [entity-verification](entity-verification.md) for vague references
   - For interviews/podcasts: `author` = guest (primary voice), `host` = interviewer
2. **Fetch content**: Retrieve full text/transcript from the source URL. If direct fetch fails, try alternative approaches (cached versions, web search). Flag for manual capture only after exhausting options.
3. **Generate triage summary**: Dense, concise summary for quick queue review. Focus on key claims, frameworks, and what's novel. 1-3 sentences.
4. **Create source file** in `sources/`:
   - Filename: `YYYY-MM-DD-slug.md` (date = discovered date)
   - Set `status: unread`
   - Place raw content under `## Raw Content`
5. **Update `sources/_index.md`**: Add to the index table
6. **Update domain README**: Add to reading queue

**Source file frontmatter:**

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [source, domain-name]
status: unread
source_type: article|video|podcast|newsletter|book-chapter|organic|note
source_url: "https://..."
author: "Primary voice"
host: "Interviewer (if applicable)"
published: YYYY-MM-DD
summary: "Dense triage summary generated at capture."
---
```

**Status lifecycle:** `unread` → `reading` → `read` → `processed`

### 2. Mark as Read

When the user says "I read [source]" or "mark X as read":

1. Update source frontmatter: `status: read`, update date
2. Capture user's summary or reactions if provided
3. Update `sources/_index.md` and domain README
4. Flag for processing: "Ideas can be extracted when you're ready"

### 3. Process — Extract Ideas

This is the judgment-heavy step. Read the domain's **Source Synthesis Scope** before processing — the scope declaration tells you what's in bounds and what quality bar to apply.

1. **Read the source thoroughly** — understand the full argument, not just highlights
2. **Read the domain's taxonomy** (if one exists) for classification guidance
3. **Identify discrete ideas**: Each source typically contains multiple ideas:
   - A named technique or method
   - A framework for thinking about problems
   - An observation, principle, or non-obvious pattern
4. **For each idea, decide**: Is this new to the knowledge base, or a restatement?
   - **New idea** → create knowledge entry in `knowledge-base/`
   - **Existing idea with new perspective** → update existing entry, add source citation
   - **Restatement with nothing new** → just add a citation to the existing entry
5. **Write citation blocks** linking each idea back to the source with a key quote and what this source uniquely adds
6. **Update the source file**: List extracted ideas. Set `status: processed`.
7. **Update `sources/_index.md`** and domain README knowledge map

**Knowledge entry frontmatter:**

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [knowledge, domain-name]
status: draft|solid|canonical
origin: sourced|organic|both
featured: false
---
```

- `status`: Maturity — `draft` (initial extraction) → `solid` (validated by multiple sources or deep personal experience) → `canonical` (well-established)
- `origin`: `sourced` (from external sources), `organic` (from personal experience), `both`
- `featured`: Entries worth championing — candidates for sharing, adoption, or collateral building

### 4. Organic Ideas

When the user describes an idea from their own experience, not from an external source:

1. Check for existing entries that describe the same idea
2. If match found → enrich the existing entry with the organic perspective; update `origin` to `both`
3. If new → create knowledge entry with `origin: organic`, skip the source file step entirely
4. If external sources are later found, add a Sources section and update `origin` to `both`

### 5. Note Sources

Quick notes — ideas, techniques, observations — that are nascent and not yet backed by external sources. These represent kernels of ideas.

1. Create a source file with `source_type: note`. The note text goes in **Summary** and **Notes** sections — no Raw Content section needed. Set `status: unread`.
2. Notes can often be processed immediately since there's nothing to "read" you haven't already captured
3. During processing, extract the idea into a `draft` knowledge entry — **do not elaborate beyond what the note states**
4. A note alone keeps a knowledge entry at `draft`. Promotion to `solid` requires corroboration from an external source or the user fleshing out the idea substantially
5. When a real source corroborates a note, enrich the existing `draft` entry and promote to `solid`

### 6. Featured Entries

When the user marks an entry as important ("feature this", "this one matters", "mark as featured"):

1. Update frontmatter: `featured: true`
2. Update the domain README knowledge map: add a visual marker (e.g., **★**) so featured entries stand out

Featured entries are ideas worth championing — candidates for sharing with others, building collateral around, or driving adoption of.

### 7. Review Reading Queue

When the user asks "what's in my queue?" or "what should I read?":

1. Report status counts: total, unread, reading, read-but-unprocessed, processed
2. List unread sources with their triage summaries
3. Highlight read-but-unprocessed sources (unrealized value)

### 8. Knowledge Gap Analysis

When the user asks "where are the gaps?":

1. Review the knowledge map in the domain README
2. Review the taxonomy (if one exists) for expected coverage areas
3. Assess coverage: which areas are well-sourced vs thin
4. Report gaps and suggest what types of sources might fill them

---

## Taxonomy Mechanism

Some domains benefit from a classification scheme for organizing knowledge entries. This is optional — start without one and add it when the volume of entries makes flat organization unwieldy.

When a domain has a taxonomy (`meta/taxonomy.md`):

1. **Always read the taxonomy before processing sources** — it defines the classification language
2. **Place entries in the most specific applicable category** — use the taxonomy's structure as the organizational guide for `knowledge-base/` subdirectories
3. **Flag entries that don't fit** — if an idea doesn't map to the taxonomy, that's a signal the taxonomy may need updating
4. **Taxonomy evolves** — as the knowledge base grows, the taxonomy may need new categories or restructuring. Propose changes, don't silently reorganize.

When a domain doesn't have a taxonomy:

- Organize `knowledge-base/` flat or with lightweight groupings
- When entries reach ~15-20, consider whether a taxonomy would help navigation
- Let the taxonomy emerge from the content, don't impose one preemptively

---

## How to Think About It

**Atomicity matters — err on the side of more entries, not fewer.** One idea per knowledge entry. If a source describes a three-step workflow, each step is likely its own entry with its own rationale and applicability. The test: does this idea have its own *why*, its own situations where it applies or doesn't? If yes, it's a separate entry.

**Decompose workflows into their constituent techniques.** A workflow like "Research → Draft → Review" contains discrete techniques at each step. Each may have different value and different contexts where it applies. Extract them separately. Cross-reference via Related sections.

**Name entries by what they teach, not where they came from.** "Sarah's three-file system" is a source reference. "Structured PRD Generation with AI" is a teachable idea. Entries should be recognizable and useful even to someone who hasn't read the source.

**Capture the unique mechanism, not just the outcome.** "Use AI for strategy" is too vague. "Templatize strategy inputs so AI can generate first-draft options for human evaluation" captures the actual insight.

**Lineage is the whole point.** The knowledge entry is the synthesis, but its value comes from tracing back to specific quotes and demonstrations in specific sources. Don't skimp on citations.

**Don't duplicate, enrich.** When a new source discusses an idea already captured, resist creating a new entry. Add the new source's perspective to the existing entry. Entries get richer over time.

**Status reflects reality.** If you extracted ideas but didn't create proper knowledge entries, the source isn't "processed" — it's "read." Be honest about status.

**The knowledge map is the consumption layer.** Individual entries are powerful but hard to browse. The domain README's knowledge map is what makes the knowledge base navigable. Every new or moved entry must be reflected there.

---

## Watch Out For

**Overly broad entries.** "Best Practices for X" is too broad. "Pre-mortem for Feature Launches" is right-sized. If you can't explain the idea in 2 sentences, it's probably multiple ideas.

**Under-extracting from rich sources.** A source with three workflows likely contains 5-8 discrete ideas. Look for: the overarching insight *and* each individual technique *and* any sub-techniques that are independently valuable.

**Missing lineage.** Every claim in a sourced knowledge entry should trace to a source. For organic entries, an Origin section explains where the idea comes from in practice.

**Stale reading queue.** Sources sitting at "unread" for weeks are a signal — surface them proactively.

**Knowledge map drift.** After processing sources, always check that the domain README reflects the current state.

**Premature taxonomy.** Don't create deeply nested classification schemes until there are enough entries to warrant it. Let structure emerge.

**Scope creep.** Check the domain's scope declaration before extracting. An idea that falls outside the stated scope should go to a different domain or be flagged for the user to decide.

---

## Domain-Specific Extensions

For domains with specialized extraction needs that go beyond the scope declaration (e.g., domain-specific "screen for X" guidance, custom attribution rules, specialized workflows), create a `meta/processing-guide.md` within the domain. The generic skill reads this guide alongside the scope declaration when processing sources for that domain.

This keeps domain-specific judgment co-located with the domain rather than encoded in a domain-specific skill file. The generic skill handles the lifecycle; the domain's metadata handles the judgment context.

---

## Interaction with Other Skills

- **[Inbox triage](inbox-triage.md)** routes items tagged for a source-synthesis domain to this skill
- **[Entity verification](entity-verification.md)** resolves vague source references (author names, publication titles)
- **[Domain synthesis](domain-synthesis.md)** can review a source-synthesis domain's knowledge base for periodic health checks

---

*Domain source synthesis is a commitment. Each domain has a lifecycle — identify, initialize, capture, process, synthesize, mature. The skill handles all stages, but the user drives the pace.*

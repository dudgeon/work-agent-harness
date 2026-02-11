---
name: source-processing
description: Capture, read, and extract knowledge from external sources for a learning topic. Use when user shares a link/article/video for a topic, asks to process or extract ideas, wants reading queue status, or says "add this to [topic]". Also use when initializing a new learning topic.
---

# Source Processing

Handling the lifecycle of external source material — from capture through reading to knowledge extraction — for any domain that opts into systematic source processing.

---

## When This Applies

- User shares a link, article, video, or content to capture for a learning topic
- User wants to mark a source as read or add reading notes
- User asks to process/extract ideas from a source
- User wants to capture an organic idea (from personal experience, no external source)
- User asks about their reading queue or source status
- User wants to initialize a new learning topic
- Inbox triage encounters an item tagged for a source-processing topic

---

## Topic Discovery

Topics that participate in source processing are identified by a **`source_processing: true` flag** in their domain README frontmatter:

```yaml
---
source_processing: true
source_types: [article, video, podcast, book-chapter, organic, note]
---
```

To find all active topics, scan domain READMEs for this flag. Any domain at any depth in the hierarchy can opt in independently.

---

## Topic Lifecycle

### 1. Identify

The user recognizes a topic worth systematic learning. This is different from "I have some notes about X" — it's a deliberate commitment to build a knowledge base from sources.

**Signals:**
- "I want to start systematically learning about X"
- "I'm collecting resources on X"
- "Let's set up a reading pipeline for X"

### 2. Initialize

When the user wants to create a new learning topic, ask:

> "Setting up source processing for [topic]. A few questions:"
>
> 1. **Where should this live?** (Propose a path in the domain hierarchy)
> 2. **What kinds of sources do you expect?** (Articles, videos, podcasts, books, organic ideas)
> 3. **Do you have an initial mental model for organizing ideas?** (Optional taxonomy — can add later)

Then create the infrastructure:

```
domains/[path-to-topic]/
├── README.md              # With source_processing: true, knowledge map, reading queue
├── sources/
│   ├── _index.md          # Source registry with status tracking
│   └── [source files land here]
├── knowledge-base/
│   └── [extracted ideas land here]
└── meta/                  # Optional, for mature topics
    └── taxonomy.md        # Topic-specific classification scheme
```

**README.md** gets:
- `source_processing: true` in frontmatter
- `source_types` listing accepted source types
- A Knowledge Map section (empty initially, grows as ideas are extracted)
- A Reading Queue section

**sources/_index.md** gets:
- A table tracking all sources with: title, author, source type, status, date added, ideas extracted

### 3. Capture

A source arrives — user shares a link, forwards content, or mentions something to add.

**Capture workflow:**

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
6. **Update topic README**: Add to reading queue

**Source file frontmatter:**

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [source, topic-name]
status: unread
source_type: article|video|podcast|newsletter|book-chapter|organic|note
source_url: "https://..."
author: "Primary voice"
host: "Interviewer (if applicable)"
published: YYYY-MM-DD
summary: "Dense triage summary generated at ingestion."
---
```

**Status lifecycle:** `unread` → `reading` → `read` → `processed`

### 4. Mark as Read

When the user says "I read [source]" or "mark X as read":

1. Update source frontmatter: `status: read`, update date
2. Capture user's summary or reactions if provided
3. Update `sources/_index.md` and topic README
4. Flag for processing: "Ideas can be extracted when you're ready"

### 5. Process — Extract Ideas

This is the judgment-heavy step. When the user asks to process a source:

1. **Read the source thoroughly** — understand the full argument, not just highlights
2. **Identify discrete ideas**: Each source typically contains multiple ideas:
   - A named technique or method
   - A framework for thinking about problems
   - An observation, principle, or non-obvious pattern
3. **For each idea, decide**: Is this new to the knowledge base, or a restatement?
   - **New idea** → create knowledge entry in `knowledge-base/`
   - **Existing idea with new perspective** → update existing entry, add source citation
   - **Restatement with nothing new** → just add a citation to the existing entry
4. **Write citation blocks** linking each idea back to the source with a key quote and what this source uniquely adds
5. **Update the source file**: List extracted ideas. Set `status: processed`.
6. **Update `sources/_index.md`** and topic README knowledge map

**Knowledge entry frontmatter:**

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [knowledge, topic-name]
status: draft|solid|canonical
origin: sourced|organic|both
---
```

- `status`: Maturity — `draft` (initial extraction) → `solid` (validated by multiple sources) → `canonical` (well-established)
- `origin`: `sourced` (from external sources), `organic` (from personal experience), `both`

### 6. Organic Ideas

When the user describes an idea from their own experience, not from an external source:

1. Check for existing entries that describe the same idea
2. If new: create knowledge entry with `origin: organic`, skip the source file step entirely
3. If external sources are later found, add a Sources section and update `origin` to `both`

### 7. Review Reading Queue

When the user asks "what's in my queue?" or "what should I read?":

1. Report status counts: total, unread, reading, read-but-unprocessed, processed
2. List unread sources with their triage summaries
3. Highlight read-but-unprocessed sources (unrealized value)

### 8. Knowledge Gap Analysis

When the user asks "where are the gaps?":

1. Review the knowledge map in the topic README
2. Assess coverage: which areas are well-sourced vs thin
3. Report gaps and suggest what types of sources might fill them

---

## How to Think About It

**Atomicity matters.** One idea per knowledge entry. If a source describes a three-step workflow, each step is likely its own entry.

**Name entries by what they teach, not where they came from.** "Sarah's three-file system" is a source reference. "Structured PRD Generation with AI" is a teachable idea.

**Capture the unique mechanism, not just the outcome.** "Use AI to help with strategy" is too vague. "Templatize strategy inputs so AI can generate first-draft options for human evaluation" captures the actual insight.

**Lineage is the whole point.** The knowledge entry is the synthesis, but its value comes from tracing back to specific quotes and sources.

**Don't duplicate, enrich.** When a new source discusses an idea already captured, add the new perspective to the existing entry rather than creating a duplicate.

---

## Watch Out For

**Overly broad entries.** "Product Management Best Practices" is too broad. "Pre-mortem for Feature Launches" is right-sized.

**Under-extracting from rich sources.** A source with three workflows likely contains 5-8 discrete ideas.

**Missing lineage.** Every claim in a sourced knowledge entry should trace to a source.

**Stale reading queue.** Sources sitting at "unread" for weeks are a signal — either process more or reconsider what's being captured.

**Premature taxonomy.** Don't create deeply nested classification schemes until there are enough entries to warrant it. Let structure emerge.

**Knowledge map drift.** After processing sources, always check that the topic README reflects the current state.

---

## Interaction with Other Skills

- **Inbox triage** routes items tagged for a source-processing topic to this skill
- **Entity verification** resolves vague source references (author names, publication titles)
- **Domain synthesis** can be applied to a topic's knowledge base for periodic review

---

*Source processing is a commitment. Each topic has a lifecycle — identify, initialize, capture, process, synthesize, mature. The skill handles all stages, but the user drives the pace.*

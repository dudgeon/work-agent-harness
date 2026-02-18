---
name: inbox-triage
description: Process and route inbox items to appropriate domains. Use when user says "process inbox", "triage inbox", "clear inbox", or after completing tasks to check for accumulated items.
---

# Inbox Triage

Processing accumulated items in `inbox/` — deciding what to keep, where it goes, and what to discard.

---

## When This Applies

- **After completing any task or interaction** (see [rules/communication.md](../rules/communication.md) — "After Completing Any Task")
- User asks to "process inbox"
- Inbox has accumulated items that need routing
- During periodic review

---

## The Goal

Inbox zero, with everything either:
- Filed in the right domain with proper metadata
- Merged into existing notes
- Discarded as no longer relevant

---

## How to Think About It

**Inbox is temporary storage, not a domain.** If something has been in inbox for weeks and still doesn't have a home, question whether it's worth keeping.

**Read before routing.** Understand what the note actually is before deciding where it goes. The filename might be misleading.

**Check for existing notes.** Before creating a new note in a domain, search for related content. Often the right action is to merge or append, not create.

**Add metadata as you file.** Every note leaving inbox should have proper frontmatter (created, updated, tags, status). This is the tax for leaving inbox.

**Some things should be discarded.** Not everything captured deserves permanent storage. If a note is:
- Outdated and no longer relevant
- Already captured elsewhere
- Too vague to be useful
- A fleeting thought that didn't develop

...then delete it. The system shouldn't accumulate cruft.

---

## Triage Questions

For each inbox item, ask:
1. Is this still relevant? (If no → delete)
2. Does a note for this already exist? (If yes → merge)
3. Which domain owns this? (File there)
4. What metadata does it need? (Add frontmatter)
5. What should it link to? (Add links)

---

## Handling Unstructured Input

Some inbox items are well-formed notes. Others are raw — pasted meeting notes, email forwards, brain dumps, Slack threads, or voice dictations. Unstructured input needs decomposition before triage.

**Before triaging unstructured input:**

1. **Read the whole thing first.** Don't process linearly. Unstructured input often circles back, clarifies, or contradicts earlier statements.
2. **Identify discrete items.** A single dump may contain multiple facts, tasks, decisions, and entity references. Separate them mentally before routing.
3. **Assess what's worth keeping.** Not everything in a brain dump is knowledge. Skip thinking-out-loud that resolved itself, trivial logistics, filler, and repetition.
4. **Resolve entities.** Use [skills/entity-verification.md](entity-verification.md) to resolve vague references (names, companies, tools) against existing files and web sources before storing.
5. **Then triage each discrete item** through the normal workflow below.

One unstructured input often produces multiple outputs across different domains. That's expected — route each piece where it belongs.

---

## Domain & Entity Detection

Every inbox item is a signal. Before routing, ask:

1. **New entity?** Does this introduce a person, project, organization, recurring event, or topic not yet in the system?
   - Person → create note in the relevant domain (use [templates/concept.md](../templates/concept.md) or a people-specific structure)
   - Project → create from [templates/project.md](../templates/project.md) in the relevant domain
   - Organization → create a note in the relevant domain
   - New knowledge area → consider whether a new domain folder is warranted (rare — discuss with user)

2. **Existing entity update?** Does this add information to something already tracked?
   - Update the existing note (add timeline entry, merge details, update status)
   - Update context files if cross-domain facts are involved (`context/*.md`)
   - Cross-reference related entity files
   - When an interaction involves multiple stakeholders, update **all** relevant files — not just the primary one

3. **Connections?** What existing notes should link to/from this content?

---

## Post-Triage Item Annotation

When filing an inbox item, annotate it with triage metadata before archiving:

```yaml
---
# Original frontmatter preserved above
triage_status: filed|merged|discarded
triage_date: YYYY-MM-DD
triage_action: "Brief description of what was done"
triage_destination: "path/to/destination.md"
---
```

This creates an audit trail: what came in, what happened to it, where it went.

---

## Source-Synthesis Domain Detection

Some inbox items are destined for domains that use the [domain-source-synthesis](domain-source-synthesis.md) pattern. When triaging:

1. **Check for domain tags** — if an item's frontmatter or content indicates a specific domain, check whether that domain has `pattern: domain-source-synthesis` in its README
2. **Invoke capture** — call domain-source-synthesis's capture step to create a source file in the destination domain. This replaces "file generically," but triage retains control — you are not handing off, you are delegating one step.
3. **Source vs non-source** — items with a source URL route to the capture workflow; items describing personal experience route to the organic idea workflow
4. **Archive responsibility stays with triage** — after capture, continue to step 12 and archive the original inbox item. Domain-source-synthesis only captures; it does not archive. The inbox item must not remain after triage completes.

### Multi-Domain Routing from a Single Inbox Note

A single inbox item may contain substantive content for more than one source-synthesis domain. When this happens:

- **Create one scoped source file per domain** — don't copy the full note. Each source file contains only the content relevant to that domain. The archived original is the full record; source files are purpose-built extracts.
- **Identify domains by content, not just mention** — a note mentioning a stakeholder and a project both have content for both domains only if there's substantive material for each. Passing references don't warrant a source file.
- **Act when routing is clear; propose when it's ambiguous** — if the content obviously belongs to two domains, create both source files without asking. If you're uncertain which domains apply (a sprawling brain dump, multiple overlapping topics), list your proposed routing and let the user confirm before creating source files.
- **One archive, multiple source files** — the inbox item archives once (in the primary domain per the archiving rules). The other domains get source files only — no second archive copy.

### Capture vs. Processing Invocation

**Capture is automatic** — triage invokes it as part of routing. The user doesn't need to ask.

**Processing (idea extraction) is always manual** — the user drives it: "process my queue", "extract ideas from the stakeholder-x meeting note", etc. Triage never triggers extraction. This keeps judgment-heavy work user-paced and avoids premature knowledge entries from notes that haven't been reviewed.

---

## Watch Out For

**The "just in case" trap.** Keeping vague notes because "maybe someday" leads to bloat. Be willing to discard.

**Creating orphans.** Notes filed without links are hard to rediscover. Connect to related content.

**Domain ambiguity.** If you genuinely can't decide where something belongs, it might be:
- Actually two separate notes
- Missing context (ask user)
- A sign the domain structure needs adjustment

---

## Enrichment Pass — CriticMarkup Annotations

Before decomposing and routing, scan the raw note for opportunities to enrich it from **existing WAH content**. This turns the archived note into a self-contained record — what the user said *plus* what the system already knew.

### When to Annotate

- The note contains a question that existing domain files, context files, or decision records can answer
- A vague reference ("the vendor", "that decision") can be resolved to a specific entity with a link
- A stated fact conflicts with or confirms something already captured — worth noting the connection

### When NOT to Annotate

- The question requires web research or speculation — leave it unanswered (or flag as a task)
- The annotation would just restate what the note already says
- The note is already well-structured and complete

### CriticMarkup Convention

Use CriticMarkup comment syntax, dated and attributed:

```
{>>YYYY-MM-DD @agent: Annotation text. See [entity](relative/path/to/file.md)<<}
```

Place annotations **inline, immediately after** the relevant text. Do not move or modify the original text.

**Example:**

```markdown
jared mentioned they're re-evaluating the vendor -- what was the original
rationale? {>>2026-02-16 @agent: Selected 2025-09 for SOC2 compliance and
pricing. See [acme-corp](../domains/procurement/acme-corp.md#2025-09-15)<<}
```

### Enrichment Boundary

Only annotate from **existing WAH content** — domain files, context files, decision records, stakeholder notes. Never from web searches, speculation, or general knowledge. If the system can't answer it, leave it alone or create a task to research it.

---

## Entity Timeline Updates — Hub and Spoke

When a note references people or projects, update their entity files using a **hub-and-spoke** pattern:

- **Hub**: The archived meeting note in the destination domain's `_archive/` — this is the authoritative record
- **Spokes**: Person entities and project notes each get a lightweight timeline entry pointing back to the hub

### Person Entity Updates

For each person mentioned, add a reverse-chronological timeline entry:

```markdown
### YYYY-MM-DD — Brief description of interaction

One to three sentence summary of what was discussed relevant to this person.
Not a transcript — a useful index entry.

Source: [meeting note](relative/path/to/domain/_archive/YYYY-Www/note.md)
```

### Project Updates

For each project discussed, add a similar timeline entry:

```markdown
### YYYY-MM-DD — Brief topic

One-liner on what was discussed or decided. Detail lives in the meeting note.

Source: [meeting note](relative/path/to/domain/_archive/YYYY-Www/note.md)
```

### Cross-Links Between Spokes

Don't explicitly link person ↔ project for every meeting. The meeting note is the natural join point. Only add direct person ↔ project links when capturing a **durable relationship** (e.g., "Jared is the tech lead on Project Alpha") — that's an entity fact, not a meeting artifact.

---

## Domain Emergence and Corpus Backfill

During triage, you may notice items clustering around a topic that doesn't have a domain. This is a **domain emergence signal**.

### Detection

- Multiple inbox items routing to the same parent domain but sharing a distinct sub-theme
- A single item introducing a clearly distinct knowledge area with no existing home
- Repeated entity references that don't fit neatly into current domain structure

### Response

1. **Propose the new domain** — explain the clustering pattern, suggest a name and location. Number the evidence. Get user approval before creating.
2. **Create the domain** — README.md, any initial notes from the triggering items.
3. **Bounded backfill scan** — search existing notes for mentions of the new domain's key concepts. Present findings as a numbered list:
   - Notes that should **backlink** to the new domain (add a link)
   - Notes that might **belong** in the new domain (propose relocation)
   - Context files that should **reference** the new domain
4. **Execute approved changes** — only after user confirms which items to act on.

Backfill runs **once at domain creation**, not continuously. If the user wants a deeper pass later, they ask for it.

---

## Processing Inbox Workflow

When processing inbox (whether prompted or as post-task check):

1. List items in `inbox/` (excluding `_archive/` and `README.md`)
2. Read each note
3. **Enrichment pass**: annotate answerable questions and resolvable references with CriticMarkup (see above)
4. Detect new or existing entities (see above)
5. **Update entity timelines**: add hub-and-spoke entries for people and projects (see above)
6. Apply triage questions
7. Add proper metadata
8. Create links to related notes
9. Move to appropriate domain folder (or merge into existing note)
10. **Check for domain emergence**: if routing reveals clustering, propose new domain + backfill (see above)
11. Annotate the original with triage metadata
12. Archive processed item to destination domain's `_archive/YYYY-Www/`

---

## Archiving

Processed inbox items archive **in the destination domain**, not in a monolithic `inbox/_archive/`. This keeps source materials near the knowledge they produced.

### Where the archive goes

- **Filed or merged items** → `domains/<target-domain>/_archive/YYYY-Www/` — the original note is preserved with triage annotations added
- **Multi-domain items** (a single inbox item that decomposes across multiple domains) → archive in the **primary domain** (whichever received the most substantial content); other domains get a cross-link in their timeline entries pointing to the archived original
- **Discarded items** → delete outright. If it wasn't worth keeping, it's not worth archiving.

### Why domain-local

- **Locality**: source materials live near the knowledge they inform
- **Hub-and-spoke links stay in-domain**: entity timeline entries link to an archive within the same domain tree, not back to a disconnected `inbox/` folder
- **Domain self-containment**: aligns with fractal hierarchy — each domain carries its own audit trail
- **Domain synthesis**: can audit what raw inputs contributed to a domain's knowledge

---

*The inbox should trend toward empty. If it's consistently growing, that's a signal to either process more frequently or rethink what's being captured.*

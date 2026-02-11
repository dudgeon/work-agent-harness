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

## Watch Out For

**The "just in case" trap.** Keeping vague notes because "maybe someday" leads to bloat. Be willing to discard.

**Creating orphans.** Notes filed without links are hard to rediscover. Connect to related content.

**Domain ambiguity.** If you genuinely can't decide where something belongs, it might be:
- Actually two separate notes
- Missing context (ask user)
- A sign the domain structure needs adjustment

---

## Processing Inbox Workflow

When processing inbox (whether prompted or as post-task check):

1. List items in `inbox/` (excluding `_archive/` and `README.md`)
2. Read each note
3. Detect new or existing entities (see above)
4. Apply triage questions
5. Add proper metadata
6. Create links to related notes
7. Move to appropriate domain folder (or merge into existing note)
8. Annotate the original with triage metadata
9. Archive processed item to `inbox/_archive/YYYY-Www/`

---

## Archiving

Processed inbox items move to `inbox/_archive/YYYY-Www/` (ISO week format). The original note is preserved with triage annotations added, creating a record of what was processed and where it went.

---

*The inbox should trend toward empty. If it's consistently growing, that's a signal to either process more frequently or rethink what's being captured.*

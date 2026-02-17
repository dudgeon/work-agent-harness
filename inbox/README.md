---
created: 2026-02-09
updated: 2026-02-09
tags: [meta, inbox]
status: active
---

# Inbox

Temporary storage for items that need processing. **Not a domain, not a dumping ground.**

---

## How the Inbox Works

Items arrive here for triage and routing. They should not stay long.

### What Goes Here

- Forwarded content the user wants captured
- Quick notes or thoughts that need proper filing
- External content to evaluate and route
- Items where the correct domain isn't immediately obvious

### What Does NOT Go Here

- Content with a clear domain — file it directly
- Things the agent should "just figure out" where to put — do the work to determine the domain
- Permanent reference material — use `context/` or the appropriate domain

### Processing

Items are processed using the [inbox-triage skill](../skills/inbox-triage.md). Processing happens:
- After completing any significant task (agents check automatically)
- When the user asks ("process inbox", "triage inbox", "clear inbox")
- During periodic reviews

### Archiving

Processed items archive **in the destination domain's `_archive/YYYY-Www/`** (ISO week format) with triage annotations preserved. This keeps source materials near the knowledge they produced and supports domain-local audit trails. Discarded items are simply deleted — no archive for things not worth keeping.

---

## Current Inbox

_Empty — no items to process._

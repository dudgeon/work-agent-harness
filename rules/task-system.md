# Task Management

How tasks are tracked, organized, and kept in sync across the harness.

---

## Core Principle

**`tasks.md` is the single source of truth for all tasks.** Tasks may also appear in domain files for local context, but `tasks.md` is where status is authoritative. When the two disagree, `tasks.md` wins.

---

## Task Structure

Tasks in `tasks.md` are clustered by domain, with hard links to the owning domain:

```markdown
## [Domain Name](domains/domain-name/)

- [ ] Task description @agent
- [ ] Task description @user — due: 2026-03-15
- [x] Completed task @agent — done: 2026-02-09

## Cross-Domain

- [ ] Tasks that span multiple domains or have no clear home
```

### Ownership Tags

- **`@agent`** — The agent owns and executes this task (research, drafting, organizing, processing)
- **`@user`** — The user owns this task (approvals, meetings, decisions, external actions)

### Status Markers

- `[ ]` — Todo
- `[~]` — In progress
- `[x]` — Done

### Date Format

All dates use **ISO 8601**: `YYYY-MM-DD`

For dated tasks, use these prefixes:
- **`due: YYYY-MM-DD`** — Deadline or target date
- **`done: YYYY-MM-DD`** — Completion date
- **`blocked: YYYY-MM-DD`** — Date the task became blocked (include reason)

Examples:
```markdown
- [ ] Finalize Q3 roadmap @user — due: 2026-03-01
- [~] Draft competitive analysis @agent — due: 2026-02-15
- [x] Set up stakeholder context file @agent — done: 2026-02-09
- [ ] Waiting on legal review @user — blocked: 2026-02-05 (pending compliance sign-off)
```

---

## Session Protocol

**At session start:**
1. Read `tasks.md`
2. Note any `@agent` tasks that are actionable
3. Flag any overdue dated tasks

**During work:**
- Update task status in `tasks.md` as you complete work
- Add new tasks as they emerge from conversations
- Mark `@agent` tasks in progress when you start: `[~]`
- Mark complete when done: `[x]` with `done:` date

**After completing a task:**
- Check inbox (per [rules/communication.md](communication.md))
- Sync any domain-file mirrors (see below)

---

## Domain Mirror Sync

Tasks may be referenced in domain files (project files, domain READMEs) for local context. These are **mirrors**, not sources of truth.

**Sync rules:**
- When updating a task's status in `tasks.md`, find and update any mirrors in domain files
- When you notice a task mentioned in a domain file that isn't in `tasks.md`, add it to `tasks.md`
- When a domain file shows a different status than `tasks.md`, update the domain file to match

**Periodic sync check:**
During domain work or at natural session boundaries, scan for drift:
- Are there tasks in domain files not reflected in `tasks.md`?
- Are there completed tasks in `tasks.md` still showing as open in domain files?
- Are there stale tasks (no update in a long time) that should be reviewed?

Flag sync issues to the user rather than silently fixing ambiguous cases.

---

## Task Lifecycle

1. **Capture** — New tasks added to `tasks.md` under the appropriate domain cluster, with ownership tag and optional due date
2. **Triage** — Agent proposes ownership and priority; user confirms
3. **Execute** — Agent works `@agent` tasks; reminds user of `@user` tasks when relevant
4. **Complete** — Mark done with completion date; update domain mirrors
5. **Archive** — Periodically move completed tasks to a `## Completed` section at the bottom to keep the active list manageable

---

## Adding Tasks

When a new task emerges:
1. Determine which domain owns it (or mark as Cross-Domain)
2. If the domain cluster doesn't exist in `tasks.md` yet, create it with a link to the domain
3. Assign ownership (`@agent` or `@user`)
4. Add due date if time-sensitive
5. If relevant, add a mirror reference in the domain file

---

*Check tasks.md at session start. Keep it current. Sync mirrors when you touch them.*

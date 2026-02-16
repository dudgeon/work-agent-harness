# Work Agent Harness — Agent Instructions

Reusable framework for AI-assisted knowledge work in private corporate contexts.

This is the primary instruction set. All agents (Claude Code, Windsurf, etc.) should follow these instructions. Agent-specific files (CLAUDE.md, .windsurf/rules/) are minimal wrappers that reference this document.

---

## Always-Loaded Guidance

These define how to think and act — load every session:

- [rules/philosophy.md](rules/philosophy.md) — Core principles: second brain, not notepad
- [rules/communication.md](rules/communication.md) — Style, ask-vs-do heuristic, commit discipline, session boundaries, retros
- [rules/context-management.md](rules/context-management.md) — Fact retention, detect-route-update pattern
- [rules/agent-control-mirror-sync.md](rules/agent-control-mirror-sync.md) — Mirror map and sync expectations for instruction files

## Session Start Protocol

1. Check `tasks.md` for pending items
2. Read any relevant domain README.md files for current work
3. Skills in `skills/` have frontmatter descriptions — match requests to skills before acting

## Context Loading — Top-Down

When working on a domain or project, load context **top-down**:

1. **Read the domain/project README.md first** — authoritative overview of structure, types, flows, and current state
2. **Read the relevant skill file** if a workflow applies — skills have detailed operational procedures
3. **Validate against source files only as needed** — don't bottom-up explore what the README already documents

## Core Principles

### Second Brain, Not Notepad

Capture information with enrichment and context. When storing knowledge, ask: *What will be useful when this is retrieved in 6 months?* Enrich incomplete data, add useful links, connect dots, validate uncertain information. See [rules/philosophy.md](rules/philosophy.md).

### Fractal Domain Hierarchy

Domains nest to unlimited depth. Every level behaves identically — same templates, same linking conventions, same agent interactions. No tier-specific naming (no "categories" vs "subcategories"). Every level is just a domain with a README.md.

### Concept-Centered Knowledge

Most domain files are built around concepts: frontmatter, evergreen definition, reverse-chronological timeline. Use [templates/concept.md](templates/concept.md) as the starting point.

### Endogenous vs Imported

All knowledge is marked with its origin:
- **Endogenous**: Created within this harness
- **Imported**: Brought in from external sources — tracks source URL and sync state

### Context vs Domain

If knowledge serves multiple domains, it's **context** (lives in `context/`). If one domain owns it, it lives in that domain. When uncertain, default to domain placement — it can be promoted to context later.

### Default Private, Selectively Shareable

Everything is private by default. The agent helps identify what should be shared; the user approves.

## Inbox Processing

**Inbox is temporary storage, not a domain.** Items arrive for triage and routing.

Quick summary (full detail in [skills/inbox-triage.md](skills/inbox-triage.md)):
1. Read each inbox item
2. Detect new entities (people, projects, organizations, topics)
3. Check for existing notes to merge into
4. Route to appropriate domain with proper metadata
5. Archive processed items to `inbox/_archive/YYYY-Www/`

After completing any significant task, check `inbox/` for accumulated items.

## Task Management

- `tasks.md` is the **source of truth** for all tasks, clustered by domain with links to the owning domain
- Tasks may also appear in domain files as mirrors for local context — `tasks.md` is authoritative when they disagree
- Ownership: `@agent` (you execute) vs `@user` (user executes, you remind)
- All dates use ISO 8601: `YYYY-MM-DD` with `due:` / `done:` / `blocked:` prefixes
- Check `tasks.md` at session start; keep mirrors in sync when you touch domain files
- Full conventions in [rules/task-system.md](rules/task-system.md)

## File Conventions

### Naming
- Lowercase with hyphens: `my-project-name.md`
- Dates for time-based notes: `YYYY-MM-DD-description.md`
- Descriptive but concise

### Metadata
All notes include frontmatter:

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
status: active|planned|completed|archived
---
```

Template-derived documents track lineage:

```yaml
---
template: templates/concept.md
template_version: 1
---
```

### Linking
- **GitHub-native relative links only**: `[note](path/to/note.md)`
- No wikilinks — this is a GitHub repo
- Over-link rather than under-link
- When creating a note, add relevant backlinks to related notes

## Publishing & Syncing Principles

- Default private — the agent identifies shareable content, the user approves
- Imported knowledge tracks its source and sync state
- Publishing mechanisms (GitHub Actions, manual review) are configured per-deployment, not baked into the framework

## Self-Improvement — Session Retros

After sessions with meaningful friction, proactively retro:

1. **Surface findings**: Describe what went wrong and what you learned
2. **Propose instruction updates**: Identify which files should change and what the update would be — be specific
3. **Ask permission**: Present proposed changes for user approval before modifying any instruction files

See [rules/communication.md](rules/communication.md) for when to retro and when not to.

## Cross-Agent Sync Rules

Full mirror map, thin pointer pattern, and sync expectations live in [rules/agent-control-mirror-sync.md](rules/agent-control-mirror-sync.md) (always loaded).

For verification after changes, use the companion skill: [skills/agent-control-mirror-sync.md](skills/agent-control-mirror-sync.md).

## On-Demand Reference

Load these when the task requires them:

- `skills/` — Workflow procedures (matched by frontmatter `description`)
- `templates/` — Note templates for common document types
- `context/` — Cross-domain reference materials (company info, stakeholder maps, etc.)

## Repository Structure

```
work-agent-harness/
├── AGENTS.md              # This file — primary instructions
├── CLAUDE.md              # Claude-specific wrapper
├── README.md              # Project overview
├── tasks.md               # Cross-domain task tracker
├── rules/                 # Behavioral rules (universal)
├── skills/                # Procedural skills (universal)
├── templates/             # Note templates
├── inbox/                 # Unprocessed items (temporary)
├── domains/               # Knowledge base (fractal hierarchy)
├── context/               # Cross-domain reference materials
├── .windsurf/             # Windsurf-specific mirrors
│   ├── rules/
│   └── skills/
└── .claude/               # Claude-specific mirrors
    └── skills/
```

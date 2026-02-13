# Work Agent Harness — Claude Code

All instructions live in AGENTS.md. This file exists solely for Claude Code compatibility.

## Always-Loaded Files

Load these every session:

- @AGENTS.md
- @rules/philosophy.md
- @rules/communication.md
- @rules/context-management.md

## Session Start

1. Check `tasks.md` for pending items
2. Match requests to skills in `skills/` (check frontmatter descriptions)
3. Load domain README.md files top-down as needed

## Skills

Claude-specific skill mirrors live in `.claude/skills/`. These reference the universal versions in `skills/` — do not duplicate content.

## Sync Responsibility

If you modify any rule or skill, update both the universal version (`rules/`, `skills/`) and all agent-specific analogs (`.windsurf/rules/`, `.windsurf/skills/`, `.claude/skills/`). If you cannot update all mirrors, flag the desync for the user.

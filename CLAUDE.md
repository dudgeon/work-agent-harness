# Work Agent Harness — Claude Code

All instructions live in AGENTS.md. This file exists solely for Claude Code compatibility.

## Always-Loaded Files

Load these every session:

- @AGENTS.md
- @rules/philosophy.md
- @rules/communication.md
- @rules/context-management.md
- @rules/agent-control-mirror-sync.md
- @rules/changelog-awareness.md

## Session Start

1. Check `tasks.md` for pending items
2. Match requests to skills in `skills/` (check frontmatter descriptions)
3. Load domain README.md files top-down as needed

## Skills

Claude-specific skill mirrors live in `.claude/skills/`. These reference the universal versions in `skills/` — do not duplicate content.

## Sync Responsibility

See [rules/agent-control-mirror-sync.md](rules/agent-control-mirror-sync.md) (always loaded) for the full mirror map and sync expectations.

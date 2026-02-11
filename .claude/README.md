# Claude Code Configuration (Mirrors)

This directory contains Claude Code-specific files that reference universal counterparts at the repo root.

## Why This Exists

Claude Code looks for configuration in `.claude/`. Rather than duplicating content from the universal `rules/` and `skills/` directories, files here point to the canonical versions.

## Structure

- **`skills/`** — Skill mirrors that reference `skills/` at repo root. See [skills/README.md](skills/README.md).
- **`CLAUDE.md`** (at repo root, not here) — Minimal wrapper that references [AGENTS.md](../AGENTS.md) and lists always-loaded files via `@` syntax.

## How It Differs From Windsurf

Claude Code's primary instruction loading happens through `CLAUDE.md` → `@` file references. The `.claude/skills/` mirrors are a secondary discovery mechanism. Windsurf relies more heavily on its `.windsurf/rules/` and `.windsurf/skills/` directories for auto-loading.

## Sync Rule

[AGENTS.md](../AGENTS.md) is the single source of truth. If you modify any rule or skill:
1. Edit the universal version (`rules/` or `skills/`)
2. Verify mirrors here still point correctly
3. Flag any desync you can't resolve

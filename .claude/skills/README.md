# Claude Skills (Mirrors)

These files are **thin wrappers** that reference the universal skills in [`skills/`](../../skills/).

## Why This Exists

Claude Code looks for skills in `.claude/skills/`. Rather than duplicating content, each file here points to its universal counterpart. This keeps a single source of truth while maintaining agent compatibility.

## How It Works

- **Universal versions** live in `skills/` at repo root — that's where edits happen
- **These mirrors** exist so Claude Code discovers them (matched by frontmatter `description`)
- Each mirror preserves the same `name` and `description` frontmatter as the universal version

## Sync Rule

If you modify a skill:
1. Make the change in `skills/` (the universal version)
2. Verify the mirror here still points correctly
3. Keep frontmatter `name` and `description` in sync — Claude uses these for skill matching

Note: Claude Code also loads skills via `@` references in [CLAUDE.md](../../CLAUDE.md). The mirrors here serve as a fallback discovery mechanism.

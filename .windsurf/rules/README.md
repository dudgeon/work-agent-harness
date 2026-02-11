# Windsurf Rules (Mirrors)

These files are **thin wrappers** that reference the universal rules in [`rules/`](../../rules/).

## Why This Exists

Windsurf expects rules in `.windsurf/rules/`. Rather than duplicating content, each file here points to its universal counterpart. This keeps a single source of truth while maintaining agent compatibility.

## How It Works

- **Universal versions** live in `rules/` at repo root — that's where edits happen
- **These mirrors** exist so Windsurf discovers and loads them
- Each mirror has Windsurf-specific frontmatter (`trigger: always`) plus a pointer to the universal file

## Sync Rule

If you modify a rule:
1. Make the change in `rules/` (the universal version)
2. Verify the mirror here still points correctly
3. If the mirror needs Windsurf-specific adaptation, note the divergence and flag it

If Windsurf does not follow file references reliably, these mirrors may need to inline the full content. Test this early and update [AGENTS.md](../../AGENTS.md) with findings.

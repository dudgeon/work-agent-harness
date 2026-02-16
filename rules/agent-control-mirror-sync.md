# Agent Control & Mirror Sync

Behavioral rule for keeping instruction files synchronized across all agent surfaces. Always loaded.

---

## When This Applies

Any time you create, modify, rename, or delete files in these locations:

- `rules/` — behavioral rules (universal)
- `skills/` — procedural skills (universal)
- `AGENTS.md` — primary instruction set
- `CLAUDE.md` — Claude Code wrapper
- `.claude/skills/` — Claude skill mirrors
- `.windsurf/rules/` — Windsurf rule mirrors
- `.windsurf/skills/` — Windsurf skill mirrors

---

## The Mirror Map

Universal sources and their required mirrors:

| Universal Source | Claude Mirror | Windsurf Mirror |
|---|---|---|
| `rules/*.md` | `@` reference in CLAUDE.md (if always-loaded) | `.windsurf/rules/*.md` (thin pointer) |
| `skills/*.md` | `.claude/skills/*.md` (thin pointer) | `.windsurf/skills/*.md` (thin pointer) |
| `AGENTS.md` | Referenced by CLAUDE.md | Referenced by `.windsurf/rules/` READMEs |

**No `.claude/rules/` directory exists.** Claude Code loads rules via `@` references in CLAUDE.md. Only always-loaded rules need `@` references; on-demand rules are discovered through AGENTS.md.

---

## Thin Pointer Pattern

Mirror files are **thin pointers** — they contain only:

1. Agent-specific frontmatter (`name`/`description` for skills, `trigger: always` for Windsurf rules)
2. A relative link to the universal source
3. A "do not duplicate content here" warning

Mirror files must **never** contain duplicated procedure or rule content.

---

## Sync Expectations

### On every instruction file change

1. **Edit the universal source first** — `rules/`, `skills/`, or `AGENTS.md`
2. **Update all mirrors** — create, update, or delete corresponding mirrors
3. **Keep frontmatter in sync** — especially `description` fields in skill mirrors (agents use these for matching)
4. **If you cannot update all mirrors**, explicitly flag the desync for the user

### When creating a new file

- New skill → create thin pointers in `.claude/skills/` and `.windsurf/skills/`
- New rule → create thin pointer in `.windsurf/rules/`; add `@` reference in CLAUDE.md if always-loaded
- New always-loaded rule → also add to AGENTS.md "Always-Loaded Guidance" section
- Update `skills/README.md` table if adding a skill

### When renaming or deleting

- Remove/rename the old mirrors
- Update any references in AGENTS.md, CLAUDE.md
- Check `skills/README.md` table

---

## Verification

After making changes, use the [agent-control-mirror-sync skill](../skills/agent-control-mirror-sync.md) for a full verification checklist.

---

*This rule is the behavioral expectation. For the procedural verification checklist, see the companion skill.*

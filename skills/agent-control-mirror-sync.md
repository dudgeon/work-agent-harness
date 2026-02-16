---
name: agent-control-mirror-sync
description: Verify and repair sync state across all agent mirror files. Use after creating, modifying, renaming, or deleting any instruction file (rules, skills, AGENTS.md, CLAUDE.md).
---

# Agent Control & Mirror Sync — Verification Skill

Procedural checklist for verifying that all instruction files are correctly synchronized across agent surfaces. Run this after making changes to any rule, skill, or control file.

---

## When to Use

- After creating a new skill or rule
- After modifying an existing skill or rule (especially frontmatter)
- After renaming or deleting an instruction file
- After a session where multiple instruction files changed
- When you suspect mirrors may be out of sync (e.g., after resolving merge conflicts)

---

## Verification Checklist

### Step 1: Inventory Universal Sources

List current files in:
- `rules/*.md`
- `skills/*.md` (excluding README.md)

### Step 2: Check Skill Mirrors

For each file in `skills/` (excluding README.md):
- [ ] `.claude/skills/{filename}` exists and is a thin pointer
- [ ] `.windsurf/skills/{filename}` exists and is a thin pointer
- [ ] Frontmatter `name` and `description` match across all three locations

For each file in `.claude/skills/` and `.windsurf/skills/`:
- [ ] A corresponding universal source exists in `skills/` (no orphaned mirrors)

### Step 3: Check Rule Mirrors

For each file in `rules/`:
- [ ] `.windsurf/rules/{filename}` exists and is a thin pointer
- [ ] If the rule is always-loaded: `CLAUDE.md` has an `@rules/{filename}` reference
- [ ] If the rule is always-loaded: `AGENTS.md` lists it under "Always-Loaded Guidance"

For each file in `.windsurf/rules/` (excluding README.md):
- [ ] A corresponding universal source exists in `rules/` (no orphaned mirrors)

### Step 4: Check Control Files

- [ ] `CLAUDE.md` "Always-Loaded Files" matches `AGENTS.md` "Always-Loaded Guidance" (same set of rules)
- [ ] `AGENTS.md` "Cross-Agent Sync Rules" references [rules/agent-control-mirror-sync.md](../rules/agent-control-mirror-sync.md)
- [ ] `skills/README.md` "Current Skills" table includes all skills in `skills/`

### Step 5: Report

If everything is in sync: confirm briefly.

If desyncs found:
1. List each desync with the specific file(s) involved
2. Propose the fix (create mirror, update frontmatter, remove orphan, etc.)
3. Execute fixes after user confirmation (or immediately if the fix is clearly mechanical)

---

## Thin Pointer Templates

### Skill mirror (Claude)

```yaml
---
name: {skill-name}
description: {copy from universal version}
---
```

```markdown
# {Skill Title} (Claude Mirror)

This skill references the universal version. See [skills/{filename}](../../skills/{filename}) for the full procedure.

**Do not duplicate content here.** If this file and the universal version diverge, update the universal version and re-sync.
```

### Skill mirror (Windsurf)

```yaml
---
name: {skill-name}
description: {copy from universal version}
---
```

```markdown
# {Skill Title} (Windsurf Mirror)

This skill references the universal version. See [skills/{filename}](../../skills/{filename}) for the full procedure.

**Do not duplicate content here.** If this file and the universal version diverge, update the universal version and re-sync.
```

### Rule mirror (Windsurf)

```yaml
---
trigger: always
---
```

```markdown
# {Rule Title}

This rule references the universal version. See [rules/{filename}](../../rules/{filename}) for the full content.

**Do not duplicate content here.** If this file and the universal version diverge, update the universal version and re-sync.
```

---

*Run this skill proactively after instruction file changes. The goal is zero silent desyncs.*

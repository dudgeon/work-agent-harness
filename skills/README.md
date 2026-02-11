# Skills

Skills encode **judgment patterns**, not mechanical checklists. A good skill captures:
- **When** to apply it (triggers and recognition)
- **Why** certain approaches work better (reasoning)
- **How** to think through edge cases (judgment)
- **What** good output looks like (examples)

Skills are for workflows that benefit from consistent judgment, not for simple procedures that don't need guidance.

---

## Format & Standards

This harness follows the [Agent Skills](https://agentskills.io) open specification, adopted by Claude Code, Windsurf, Cursor, VS Code Copilot, Gemini CLI, and others.

### Current approach: flat files

Skills here are flat markdown files with YAML frontmatter:

```yaml
---
name: skill-name
description: What this skill does and when to use it.
---
```

The `name` and `description` fields are required per the spec. The `description` is critical — agents use it to match user intent to the right skill.

### The full spec: directory-based

The [agentskills.io specification](https://agentskills.io/specification) defines the full format as a directory containing a `SKILL.md` file with optional subdirectories:

```
skill-name/
├── SKILL.md          # Required: frontmatter + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: supporting documentation
└── assets/           # Optional: templates, resources
```

This supports **progressive disclosure**: agents load only `name` and `description` at startup (~100 tokens), the full `SKILL.md` body on activation (<5000 tokens recommended), and referenced files only when needed.

### When to migrate

Flat files work fine when skills are self-contained markdown under 500 lines. Consider migrating a skill to directory format when it:
- Needs bundled scripts or executable code
- Has reference material that shouldn't always load into context
- Grows beyond ~500 lines
- Needs assets (templates, schemas, data files)

### Additional frontmatter fields

Beyond the required `name` and `description`, the spec supports:

| Field | Purpose |
|-------|---------|
| `license` | License name or reference |
| `compatibility` | Environment requirements (tools, network access) |
| `metadata` | Arbitrary key-value pairs (author, version, etc.) |
| `allowed-tools` | Pre-approved tools the skill may use (experimental) |

---

## Current Skills

| Skill | Purpose | Type |
|-------|---------|------|
| [inbox-triage.md](inbox-triage.md) | Route inbox items to appropriate domains, decompose unstructured input | Workflow |
| [domain-synthesis.md](domain-synthesis.md) | Synthesize information across a domain, update READMEs, flag gaps | Workflow |
| [entity-verification.md](entity-verification.md) | Resolve vague entities against existing files and web sources | Utility |
| [template-propagation.md](template-propagation.md) | Update derived documents when templates change structurally | Maintenance |
| [stakeholder-intelligence.md](stakeholder-intelligence.md) | Track stakeholder profiles, multi-person interactions, relationship proximity | Workflow |
| [source-processing.md](source-processing.md) | Capture, read, and extract knowledge from sources for learning topics | Workflow |

---

## Skill Structure

Skills should read like guidance from an experienced colleague, not a software spec:

```markdown
# [Skill Name]

## When This Applies
[Recognition patterns — how to know this skill is relevant]

## The Goal
[What good looks like, in plain language]

## How to Think About It
[Judgment guidance, reasoning, principles]

## Watch Out For
[Common mistakes, edge cases, things that seem right but aren't]
```

Additional sections (workflows, examples, output format) as warranted. Keep the body focused — move detailed reference material to separate files if using directory format.

---

## When to Create a New Skill

A workflow deserves a skill when:
1. It requires **judgment calls** that benefit from documented reasoning
2. It has **edge cases** that are easy to forget
3. It spans **multiple steps** that should happen together
4. It would benefit from **examples** of good vs poor execution

Don't create skills for:
- Simple, obvious procedures
- One-off tasks
- Things that vary too much to standardize

---

## Skill-Worthy Pattern Detection

Watch for workflows that repeat across sessions. After encountering a pattern 2-3 times, consider whether it deserves a skill. Signs:

- You keep making the same judgment calls
- Edge cases keep surprising you
- The user corrects the same mistake twice
- A multi-step process has a specific order that matters

---

## Don't Over-Skill

Avoid creating skills for:
- One-off requests
- Simple single-step tasks
- Things that vary significantly each time
- Workflows still being refined (let them stabilize first)
- Tasks better handled by natural conversation

---

## Cross-Agent Compatibility

Skills in this directory are the **universal versions**. Agent-specific mirrors exist in:
- `.claude/skills/` — Claude Code discovery
- `.windsurf/skills/` — Windsurf discovery

Mirrors reference the universal files here. See those directories' READMEs for the mirroring mechanism.

When adding a new skill, create corresponding mirrors in both agent directories.

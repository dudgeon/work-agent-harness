# work-agent-harness

**A reusable framework for AI-assisted knowledge work in private corporate contexts**

## Purpose

This repository provides a structured harness for product managers (and similar knowledge workers) to collaborate with AI coding agents (Claude Code, Windsurf Cascade, etc.) on non-SWE tasks in a private, corporate-appropriate environment.

Unlike public knowledge bases or personal life management systems, this harness is designed for:
- **Corporate constraints**: Limited third-party services, no cloud dependencies beyond GitHub
- **Professional context**: Work projects, product strategy, competitive intelligence, stakeholder management
- **Agent portability**: Works across Claude Code, Windsurf, and other emerging tools
- **Self-improvement**: Built-in reflection and recursive enhancement of agent instructions

## Conceptual Foundation

This harness is inspired by [home-brain](https://github.com/dudgeon/home-brain) (private; Claude Code can access directly) and [home-brain-config](https://github.com/dudgeon/home-brain-config) (public; accessible to others but lacks full context), a personal knowledge management system that demonstrates:
- **Inbox → triage → domain** flow for processing information
- **Skills-based workflows** that codify repeatable procedures
- **Philosophy-driven behavior** that shapes how agents think, not just what they do
- **Self-reflection patterns** where agents propose improvements to their own instructions

Key differences from home-brain:
- No Cloudflare/MCP infrastructure (pure GitHub + local)
- Windsurf-first design (though Claude Code compatible)
- Work-focused domains (not family/personal)
- More hierarchical domain support (unlimited nesting)

## Agent Compatibility Strategy

This harness follows emerging consensus standards for cross-agent compatibility:

### File-Based Instructions

- **`AGENTS.md`**: Primary instruction set (universal standard)
- **`CLAUDE.md`**: References AGENTS.md, adds Claude-specific context. There should be minimal, ideally zero Claude-specific content; where there is, there should be adapted analogs in AGENTS.md, and rules to keep them in sync no matter which agent is making changes.
- **`.windsurf/rules/*.md`**: Windsurf rules reference AGENTS.md patterns
- **`.windsurf/skills/*.md`**: Procedural workflows (follows [agentskills.io](https://agentskills.io) spec)

### Windsurf-Specific Components

Windsurf's analog to Claude skills consists of three components. A mechanism is needed to keep Windsurf rules, skills, and workflows in sync with their Claude/Claude Code analogs:

1. **Skills** (`.windsurf/skills/`) - Procedural workflows with supporting files
   - Auto-invoked based on description match
   - Manually invoked via `@skill-name`
   - Can reference supporting files in same directory

2. **Rules** (`.windsurf/rules/`) - Behavioral constraints and conventions
   - Serves as "constitutional framework" for AI behavior
   - Prevents "AI drift" across sessions
   - Global rules (all projects) vs workspace rules (project-specific)

3. **Workflows** - Reusable automation via slash commands
   - Markdown-based
   - Complementary to skills

## Repository Structure

```
work-agent-harness/
├── AGENTS.md                  # Primary agent instructions
├── CLAUDE.md                  # Claude-specific references
├── README.md                  # This file
├── INDEX.md                   # Knowledge base map
├── tasks.md                   # Cross-domain task tracker
│
├── .windsurf/
│   ├── rules/                 # Behavioral rules
│   │   ├── philosophy.md      # Core principles
│   │   ├── communication.md   # Style and interaction patterns
│   │   └── context-management.md
│   ├── skills/                # Procedural workflows
│   │   ├── inbox-triage.md
│   │   ├── domain-synthesis.md
│   │   └── [other skills]
│   └── workflows/             # Slash-command automations
│
├── .claude/                   # Claude Code compatibility
│   ├── skills/                # Mirror of Windsurf skills
│   └── [other Claude-specific config]
│
├── inbox/                     # Unprocessed items
│   ├── README.md              # Inbox behavior guide
│   └── _archive/              # Processed items by week
│       └── YYYY-Www/
│
├── domains/                   # Knowledge base (fractal hierarchy)
│   ├── [domain-name]/
│   │   ├── README.md          # Domain overview
│   │   ├── [sub-domain]/
│   │   │   ├── README.md
│   │   │   └── [content].md
│   │   └── [content].md
│   └── [other domains]
│
├── context/                   # Cross-domain reference materials
│   ├── company-info.md        # Generic company context
│   ├── writing-examples/      # Samples for style extraction
│   └── stakeholder-map.md     # Relationship context
│
└── templates/                 # Note templates
    ├── domain.md
    ├── project.md
    ├── concept.md
    └── competitive-analysis.md
```

## Core Features

### 1. Inbox System

Primary means of adding knowledge to the repo. Items arrive here for triage and routing to appropriate domains.

**Not a dumping ground** — the inbox is temporary storage for items that need processing, not a permanent filing location.

**Archiving**: Once triaged, the inbox item should be updated with status frontmatter, a summary of triage actions taken, and the preserved original source from the point of inbox submission. Items that relate to a single domain should be archived within that domain.

**Automation**: Some items can be processed fairly automatically; many will require user oversight.

**Home-brain reference**: See [inbox-triage skill](https://github.com/dudgeon/home-brain/blob/main/.claude/skills/inbox-triage.md) for the proven pattern.

### 2. Knowledge Base / Fractal Domain Hierarchy

Knowledge base divided by domain, with unlimited nesting depth supported.

**"Fractal" principle**: A domain's depth in the file structure should not change how it is referenced, the templates it uses, or how agents interact with it. Domains and sub-domains should not "bottom out" — every level behaves identically.

**Design principle**: Domain ontology evolves over time. The structure should accommodate:
- Initial uncertainty (few top-level domains)
- Organic growth (sub-domains emerge naturally)
- Rearchitecture (domains split/merge without breaking)

**File naming**: No tier-specific naming conventions (no "categories" vs "subcategories"). Every level is just a domain with a README.md.

**Link format**: GitHub-native relative links (no wikilinks) — e.g., `[note](../other-domain/note.md)`

**Concept files**: Within domains, most (but not all) files should be built around concepts. Concept files have: frontmatter; evergreen content that defines the idea, its aspects, and relationships; and a timeline capturing major changes and updates (ideally in reverse chronological order, unless this ordering impacts agent understandability).

**Knowledge origins**: Some knowledge will be endogenous to the harness (created here), and other knowledge will be imported from external shared sources. Both need explicit handling.

### 3. Context Layer

Non-knowledge-base materials that assist agents:
- Company information (not domain-specific)
- Writing examples (for style rule extraction)
- Stakeholder maps
- Reference materials

**Context vs domains**: There is tension between what lives in the knowledge base/domain hierarchy vs what is context. In general, context is cross-domain and broadly applicable. As with knowledge, some context will be endogenous to the harness, and other context will be imported from external shared sources; sync mechanisms, whether manual or automatic, must be determined.

### 4. Skills & Rules

Codified agent behaviors:
- **Skills**: "How to do X" (procedural)
- **Rules**: "How to think about Y" (behavioral)

**Self-reflection built-in**: Agents are instructed to propose updates to skills/rules after encountering friction.

**Home-brain reference**: Study the [philosophy.md](https://github.com/dudgeon/home-brain/blob/main/.claude/philosophy.md) and [communication.md](https://github.com/dudgeon/home-brain/blob/main/.claude/communication.md) patterns for proven self-improvement loops. These should be seeded with evergreen, broadly applicable content starters that are not deeply personal, idiosyncratic, or proprietary.

### 5. Task Management

Single source of truth for work across all domains.

**Not per-domain**: Tasks should be discoverable without knowing which domain they belong to.

Tasks can and will often be embedded within domain files for local context, but `tasks.md` is the authoritative cross-domain view.

### 6. Publishing / Syncing Pipeline

Mechanism to filter and publish subsets of content, and to import and make available external sources. Also to push changes back to the origin of those sources (likely subject to PR review).

The work-agent harness is default private and will contain a mix of public vs private information. What is appropriate to share will be user-specific and part of harness personalization is setting up these heuristics.

Often, the power of an agent is in identifying those items that should be shared with others who do not have access to the harness, so mechanisms must be built for this.

**Questions to resolve**:
- GitHub Actions for filtering/sync?
- Manual review vs automatic?
- Metadata-based filtering vs path-based?

## Development Principles

### Expect Rearchitecture

Requirements will change significantly as patterns emerge. Structure work to accommodate:
- Large-scale refactoring
- Breaking changes to folder structure
- Evolving agent instruction patterns

### Agent-Friendly Context

Every file should help agents understand:
- **Why it exists** (frontmatter, header comments)
- **What it relates to** (links to related files)
- **How to maintain it** (update patterns)

### Recursive Self-Improvement

Agents should:
1. Notice friction or inefficiency
2. Propose specific instruction updates
3. Get user approval before changing behavior
4. Learn from mistakes without being told

**Home-brain reference**: The [session retros pattern](https://github.com/dudgeon/home-brain/blob/main/.claude/communication.md#session-retros) is the key mechanism.

## Getting Started

### For Claude Code

1. Load this README and AGENTS.md
2. Review skills in `.claude/skills/`
3. Check `tasks.md` for current work
4. Load domain README.md files as needed (top-down context)

### For Windsurf

1. Load this README and AGENTS.md
2. Skills in `.windsurf/skills/` auto-load based on description match
3. Rules in `.windsurf/rules/` are always active
4. Use `@skill-name` to manually invoke skills

## Research Notes

### Windsurf Capabilities (February 2025)

- **Skills**: Procedural workflows in `.windsurf/skills/*.md` with YAML frontmatter
- **Rules**: Behavioral guidance in `.windsurf/rules/*.md`
- **Workflows**: Slash-command automation
- **AGENTS.md**: Directory-scoped instructions
- **Memories**: Windsurf auto-generates memories from interactions + user-defined rules

**Key finding**: Windsurf and Claude Code have strong alignment on skills format (both follow agentskills.io spec).

## Open Questions

1. **Publishing**: What's the right approach for filtering/syncing to public repos?
2. **Task management**: Simple markdown file or something more structured?
3. **Domain initialization**: Should we seed with a starter set of domains or let them emerge?
4. **GitHub Actions**: Which workflows would add genuine value vs overhead?

## References

- **Home-brain**: [github.com/dudgeon/home-brain](https://github.com/dudgeon/home-brain) — Personal knowledge management patterns
  - [CLAUDE.md](https://github.com/dudgeon/home-brain/blob/main/CLAUDE.md) — Agent instructions
  - [Philosophy](https://github.com/dudgeon/home-brain/blob/main/.claude/philosophy.md) — Core principles
  - [Communication](https://github.com/dudgeon/home-brain/blob/main/.claude/communication.md) — Interaction patterns
  - [Inbox triage skill](https://github.com/dudgeon/home-brain/blob/main/.claude/skills/inbox-triage.md) — Proven workflow
- **Agent Skills**: [agentskills.io](https://agentskills.io) — Standard format
- **Windsurf docs**: [docs.windsurf.com](https://docs.windsurf.com) — Official documentation

---

## Status: Bootstrap Phase

This is the starting point. The harness will evolve significantly as we:
1. Establish initial domain structure
2. Test inbox → triage → domain flows
3. Develop core skills/rules
4. Discover what works (and what doesn't)

**Next steps**: Begin with basic structure, minimal skills, and iterate based on real usage patterns. Let the system teach us what it needs to be.

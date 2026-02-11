# Starter Prompt: work-agent-harness Bootstrap

Give this to Claude Code before running `/init` in the repo.

---

## The Prompt

I'm bootstrapping a new project called `work-agent-harness` — a reusable framework for AI-assisted knowledge work in private corporate contexts, designed for product managers. The README.md is already in the repo and describes the full vision.

**Read README.md first**, then come back to these instructions.

### Your Primary Reference: home-brain

I have an existing personal knowledge management system at `github.com/dudgeon/home-brain` (private repo, you have access). This is the proven system that work-agent-harness is adapted from. Before building anything, study these files:

1. **`CLAUDE.md`** — How agent instructions are structured as a routing document (~70 lines pointing to specialized files)
2. **`.claude/philosophy.md`** — "Second brain, not notepad" principles. The core philosophy section and best practices are directly reusable; personal domain stuff is not.
3. **`.claude/communication.md`** — Communication style, when to ask vs do, session retros pattern, "exhaust your tools before asking for help." Almost all of this is reusable.
4. **`.claude/context-management.md`** — Fact retention and context routing logic. The detection/routing/update pattern is reusable; the specific context files (family, travel, etc.) are not.
5. **`.claude/skills/inbox-triage.md`** — The full inbox processing workflow including entity detection, triage questions, and archiving. Core pattern is directly reusable.
6. **`.claude/conventions.md`** — File naming, metadata, and linking standards (if it exists).

**What to extract from home-brain**: Patterns, structure, and broadly applicable principles. **What to leave behind**: Personal/family content, domain-specific details (workshop, etsy, travel, food), Cloudflare/MCP infrastructure, wikilink conventions (we use GitHub-native relative links).

### Design Decisions Already Made

These are settled — don't revisit them:

- **Fractal domains**: Unlimited nesting depth, no tier-specific naming. Every level behaves identically.
- **GitHub-native links**: Relative `.md` links only, no wikilinks.
- **Hybrid structure**: File-system primary, tags secondary.
- **Concept files as primary knowledge unit**: frontmatter → evergreen definition → reverse-chronological timeline.
- **Inbox → triage → domain flow**: Borrowed from home-brain.
- **Skills for self-improvement**: Agents propose updates to their own instructions after friction (session retros pattern).
- **AGENTS.md as universal source of truth**: CLAUDE.md is a minimal wrapper. Windsurf files reference universal files.
- **Cross-agent sync rule**: When any agent modifies a skill or rule, it must update both the universal version and all agent-specific analogs, or flag the desync.
- **Context = cross-domain**: If knowledge serves multiple domains, it's context. If one domain owns it, it lives there.
- **Default private, selectively shareable**: The agent helps identify what should be shared; the user approves.
- **Endogenous vs imported**: All knowledge is marked with its origin. Imported knowledge tracks its source and sync state.
- **Start simple**: Don't create empty structure. Build from actual content.

### What to Build (in order)

**Phase 1: Agent Instructions**

1. **`AGENTS.md`** — Primary instruction set. Structure it as home-brain's CLAUDE.md is structured: a routing document that points to specialized files. Include:
   - Session start protocol
   - Always-loaded file references (philosophy, communication, context-management)
   - Top-down context loading pattern
   - Core principles (second brain not notepad, fractal hierarchy, concept-centered knowledge, endogenous vs imported, context vs domain)
   - Inbox processing summary (detail in skill)
   - Task management approach
   - File conventions (naming, metadata, linking)
   - Publishing/syncing principles
   - Self-improvement / session retros
   - Cross-agent sync rules

2. **`CLAUDE.md`** — Minimal wrapper. Should be ~20-30 lines. References AGENTS.md, lists always-loaded files with `@` syntax, notes sync responsibility.

**Phase 2: Rules (behavioral, universal)**

Place these in `rules/` at repo root. These are the universal versions; `.windsurf/rules/` and `.claude/` versions reference them.

3. **`rules/philosophy.md`** — Adapt home-brain's `.claude/philosophy.md`. Keep: core philosophy, best practices (do/don't), system maintenance guidance. Genericize: replace personal domains with corporate PM context. Seed with evergreen content that isn't personal, idiosyncratic, or proprietary.

4. **`rules/communication.md`** — Adapt home-brain's `.claude/communication.md`. Keep: concise-but-thorough style, ask-vs-do heuristic, exhaust-your-tools principle, progress communication patterns, session retros. Genericize: remove home-brain-specific triggers ("capture this" → inbox skill handles this).

5. **`rules/context-management.md`** — Adapt home-brain's `.claude/context-management.md`. Keep: detect-route-update pattern, conflicting information handling. Genericize: replace personal context files with corporate equivalents (company info, stakeholder map, etc.). The "always retain important facts" principle is key.

**Phase 3: Skills (procedural, universal)**

Place in `skills/` at repo root.

6. **`skills/inbox-triage.md`** — Adapt home-brain's `.claude/skills/inbox-triage.md`. Keep: triage questions, entity detection, "inbox is temporary" principle, processing workflow, archiving pattern. Add: post-triage item annotation (status frontmatter, triage summary, preserved original source). Remove: project-specific routing (ai-pm-craft tags).

7. **`skills/domain-synthesis.md`** — New. Skill for synthesizing information across a domain: reading all files, identifying patterns, updating the domain README, flagging gaps or stale content, suggesting reorganization.

**Phase 4: Templates**

8. **`templates/domain.md`** — Template for domain README.md files.
9. **`templates/concept.md`** — Template for concept files (frontmatter, evergreen content, timeline).
10. **`templates/project.md`** — Template for project tracking within a domain.

**Phase 5: Scaffolding**

11. **`tasks.md`** — Initialize with the remaining build-out tasks.
12. **`inbox/README.md`** — Inbox behavior guide.
13. **`.windsurf/rules/`** and **`.windsurf/skills/`** — Windsurf-format mirrors of the universal files.
14. **`.claude/skills/`** — Claude-format mirrors of the universal skills.

### What NOT to Build Yet

- Don't create any domain folders or seed domains — let them emerge from actual use
- Don't create INDEX.md until there's content to index
- Don't build GitHub Actions or publishing pipelines
- Don't create context files (company-info.md, stakeholder-map.md) — those are personalization, not framework

### Quality Bar

For each file you create, check:
- Would this be useful to a PM who cloned this repo and knew nothing about home-brain?
- Is this free of personal, idiosyncratic, or proprietary content?
- Does this reference universal files rather than duplicating content?
- Does the frontmatter explain why this file exists and how to use it?

After building Phase 1-4, do a session retro: what friction did you hit, what would you change about the instructions you just wrote?

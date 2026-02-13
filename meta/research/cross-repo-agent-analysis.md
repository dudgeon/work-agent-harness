# Cross-Repository Agent Instructions Analysis

**Date**: 2026-02-13
**Scope**: All 10 repositories in `/Users/geoffreydudgeon/VSC Projects/`
**Purpose**: Identify organically helpful patterns, bootstrap-worthy instructions, antipatterns, and quality improvement opportunities across the full project portfolio.

---

## Table of Contents

1. [Per-Repo Profiles](#per-repo-profiles)
2. [Cross-Repo Meta-Analysis](#cross-repo-meta-analysis)
3. [Bootstrap Recommendations](#bootstrap-recommendations)
4. [Antipatterns & Quality Improvements](#antipatterns--quality-improvements)
5. [Action Items](#action-items)

---

## Per-Repo Profiles

### Maturity Spectrum

| Repo | Agent Files | Skills | Rules | Templates | Slash Cmds | Multi-Agent | Maturity |
|------|-----------|--------|-------|-----------|------------|-------------|----------|
| **work-agent-harness** | AGENTS.md + CLAUDE.md | 6 universal + mirrors | 5 universal + mirrors | 3 | 0 | Claude + Windsurf | Framework |
| **home-brain** | CLAUDE.md + .claude/ rules | 8 (in .claude/skills/) | 9 (in .claude/) | 9 | 0 | Claude + Windsurf | Mature |
| **git-brain** | CLAUDE.md (764 lines) | 0 | 0 | 0 | 0 | Claude only | Monolithic |
| **markdown-feedback** | CLAUDE.md (215 lines) | 0 | 0 | 0 | 1 (/release) | Claude only | Moderate |
| **scrape-to-markdown** | CLAUDE.md (~400 lines) | 0 | 0 | 0 | 0 | Claude only | Moderate |
| **modernist-recipe-card** | CLAUDE.md + skill file | 1 (root-level) | 0 | 1 (HTML) | 0 | Claude only | Moderate |
| **agents-for-everyone** | CLAUDE.md | 0 | 0 | 0 | 0 | Claude only | Basic |
| **dovetails** | CLAUDE.md (60 lines) | 0 | 0 | 0 | 0 | Claude only | Basic |
| **usher-extension-demo** | CLAUDE.md | 0 | 0 | 0 | 0 | Claude only | Basic |
| **tab-switcher** | None | 0 | 0 | 0 | 0 | None | None |

---

### 1. work-agent-harness

**Type**: Reusable framework for AI-assisted knowledge work
**Agent infrastructure**: Most structured. Universal source of truth in AGENTS.md + rules/ + skills/ + templates/, with thin mirrors in .claude/ and .windsurf/.

**Classified content**:

| Category | Files | Purpose |
|----------|-------|---------|
| **Entry point** | AGENTS.md, CLAUDE.md | Session start protocol, context loading order, core principles |
| **Behavioral rules** | philosophy.md, communication.md, context-management.md, decision-system.md, task-system.md | How to think, communicate, retain facts, document decisions, track tasks |
| **Workflow skills** | inbox-triage, domain-synthesis, stakeholder-intelligence, source-processing | Multi-step judgment-oriented procedures |
| **Utility skills** | entity-verification, template-propagation | Supporting procedures called by other skills |
| **Templates** | concept.md, domain.md, project.md | Structural starting points for new files |
| **Mirrors** | .claude/skills/ (7), .windsurf/rules/ (6), .windsurf/skills/ (7) | Agent-specific discovery wrappers |

**Strengths**: Clean separation of concerns. Cross-agent sync pattern. Progressive context loading. Skill frontmatter for discovery.
**Gaps**: No slash commands. No release workflow. No testing/build instructions (not applicable — it's a knowledge framework, not code).

---

### 2. home-brain

**Type**: Personal second-brain system (private, the source repo for work-agent-harness)
**Agent infrastructure**: Most mature by volume. Rules and skills live in .claude/ directly (predates the universal-source-of-truth pattern).

**Classified content**:

| Category | Files | Purpose |
|----------|-------|---------|
| **Entry point** | CLAUDE.md | Always-loaded guidance references, session protocol |
| **Behavioral rules** | philosophy, communication, context-management, conventions, decision-system, task-system, git-workflow, mcp-optimization | Full behavioral suite |
| **Workflow skills** | inbox-triage, dictation-processing, person-intelligence, ai-pm-craft-source-processing, newsworthy-events | Domain-specific workflows |
| **Utility skills** | entity-verification, template-propagation | Supporting procedures |
| **Templates** | person, project, decision, decision-quick, meeting-note, quick-note, source, knowledge-entry | 9 templates covering all knowledge types |
| **Windsurf mirrors** | .windsurf/rules/, .windsurf/skills/ | Thin wrappers |

**Key differences from work-agent-harness**:
- Has `person-intelligence.md` (not ported — personal/family context)
- Has `dictation-processing.md` (folded into inbox-triage in harness)
- Has `newsworthy-events.md` (too personal, on the "maybe skip" list)
- Has `ai-pm-craft-source-processing.md` (domain-specific, shows skill specialization pattern)
- Has `git-workflow.md` and `mcp-optimization.md` (remaining items to triage for harness port)
- Templates are richer: 9 vs 3 (person, meeting-note, decision variants, source, knowledge-entry not yet ported)
- Rules include `conventions.md` (file naming, metadata standards — folded into AGENTS.md in harness)

**What developed organically here**: Person intelligence with multi-person interaction handling. Dictation processing as a voice-input overlay. The enrichment checklist pattern (person files). Source processing with reading lifecycle states (unread → reading → read → processed). Knowledge entry taxonomy with origin tracking (sourced/organic/both).

---

### 3. git-brain

**Type**: Cloudflare Workers MCP server (BrainStem product)
**Agent infrastructure**: Single massive CLAUDE.md (764 lines) — no separation.

**Classified content (all in one file)**:

| Theme | Lines | Purpose |
|-------|-------|---------|
| Architecture docs | ~200 | MCP tools, OAuth, D1 schema, R2 storage, AI Search |
| Development commands | ~30 | npm scripts, deploy, test procedures |
| Testing requirements | ~40 | Mandatory post-change verification steps |
| Defects overcome | ~60 | 8 documented bugs with fixes (institutional memory) |
| API reference | ~100 | Endpoints, schemas, data flow |
| Customer lifecycle | ~80 | Onboarding, sync, deletion procedures |
| Version history | ~80 | v4.4 through v5.2 changelog embedded in CLAUDE.md |
| Known limitations | ~30 | Current technical constraints |

**What developed organically here**:
- **"Defects Overcome" section** — A pattern of documenting bugs and their fixes inline as institutional memory. Extremely valuable for complex systems.
- **Mandatory test checklist** — "CRITICAL: After making any changes... Claude MUST: 1. Run npm test 2. Run npm run typecheck 3. Run npm run deploy 4. Run node test-user-mcp.mjs" — This emerged from real deployment failures.
- **"Do NOT rely on the user to test"** — An explicit directive born from sessions where the agent assumed the user would verify.
- **Version history in CLAUDE.md** — Tracks what shipped and when, useful for agent context.
- **MCP integration instructions** — curl-based MCP protocol examples for pushing notes to brain from other projects.

---

### 4. markdown-feedback

**Type**: Client-side track-changes editor (React/TipTap/ProseMirror)
**Agent infrastructure**: Comprehensive CLAUDE.md + /release slash command

**Classified content**:

| Theme | Purpose |
|-------|---------|
| Architecture deep-dive | Intercept model explanation, key file map, import design decisions |
| Testing guidance | Browser testing requirements, parser testing discipline, round-trip testing |
| Behavioral rules | No destructive scaffolding, preserve spec documents, verify URLs, keep about panel current |
| Environment setup | NVM version requirement with exact command |
| Release procedure | Full /release slash command (changelog, tag, build, publish) |

**What developed organically here**:
- **"No Destructive Scaffolding" rule** — Never run scaffold commands that overwrite files. Born from a near-miss where `npm create` would have destroyed project files.
- **"/release slash command"** — A complete, multi-step release procedure as a `.claude/commands/release.md` file. Discovers changes, asks user for version, creates changelog, builds, tags, pushes, creates GitHub release with asset.
- **"Verify All URLs" rule** — Every URL written must be verified before committing. Born from incorrect GitHub URLs being committed.
- **Parser testing discipline** — "Always test with real-world input, not just idealized samples." Documents specific bugs that were caught on first real-world use.
- **Environment bootstrapping** — Exact NVM command prefix because Node 18 is too old for Vite 7.

---

### 5. scrape-to-markdown

**Type**: Chrome extension + Tampermonkey userscript
**Agent infrastructure**: Comprehensive CLAUDE.md

**Classified content**:

| Theme | Purpose |
|-------|---------|
| Architecture | Platform abstraction layer, 4-component extension, message protocol |
| Multi-agent coordination | Rules for concurrent agents on same branch |
| Spec-before-code rule | Always write spec and update roadmap before touching source |
| Research discipline | Don't spawn research tasks to validate decisions already made |
| Documentation maintenance | Explicit triggers for when to update which docs |
| Dual-platform rule | Shared code must pass both extension and userscript typechecks |
| Debugging guide | 6 principles for debugging Chrome extensions |

**What developed organically here**:
- **Multi-agent coordination rules** — "Multiple Claude Code agents may work on this repo concurrently on the same branch." Includes high-contention vs low-contention file classification, conflict minimization strategies.
- **Spec-before-code rule** — "Always write the spec and update the roadmap before touching source files — even for changes that feel small." Prevents design drift.
- **Research discipline** — "When prior conversation context or existing specs already contain enough information to act, proceed. Don't spawn research tasks to 'validate' what you already know." Anti-spiraling directive.
- **Documentation maintenance triggers** — Explicit matrix of "when X changes, update Y docs."
- **High/low contention file classification** — Identifies which files are safe to heavily modify vs which need minimal touches.

---

### 6. modernist-recipe-card

**Type**: Recipe formatting tool (static frontend + Cloudflare Worker)
**Agent infrastructure**: CLAUDE.md + standalone skill file + HTML template

**Classified content**:

| Theme | Purpose |
|-------|---------|
| Architecture | Two-part system (frontend + worker), no build step |
| Skill definition | `recipe-formatting-skill.md` — complete formatting spec (typography, layout, two formats) |
| Template reference | `recipe-formatting-template.html` — live CSS/HTML reference with 2 examples |
| MCP integration | curl-based Brain MCP protocol for pushing progress notes |

**What developed organically here**:
- **Standalone skill file at repo root** — The recipe-formatting-skill.md lives at the root, not in a skills/ directory. It's a complete, self-contained spec that the worker's system prompt embeds. Shows how skills can be the entire domain logic for a focused project.
- **HTML reference template** — A live, openable HTML file that serves as both documentation and test fixture.
- **MCP integration pattern** — Same curl-based pattern as git-brain for pushing notes to BrainStem.

---

### 7. agents-for-everyone

**Type**: Creative writing project (YA story about AI progress)
**Agent infrastructure**: CLAUDE.md only

**Classified content**:

| Theme | Purpose |
|-------|---------|
| Project overview | 5-layer stack (timeline → primitives → domains → curriculum → story) |
| Working conventions | Build bottom-up, externalize everything, check roadmap, ask don't assume |
| Research vs collaboration | Timeline research is independent; upper layers require collaboration |

**What developed organically here**:
- **Layered creative project structure** — Build bottom-up like software: lower layers must be solid before designing upper layers. Data → Abstraction → Application → Presentation.
- **"Externalize everything"** — All plans, research, decisions go in files, never rely on conversation context.
- **Research mode distinction** — "Timeline research is independent work. Primitives, domains, curriculum, and story require collaboration."

---

### 8. dovetails

**Type**: React/TypeScript SPA for CNC dovetail layouts
**Agent infrastructure**: Basic CLAUDE.md (60 lines)

**Classified content**: Architecture overview, commands, key patterns (Zod schemas as type source of truth, reducers with exhaustive switch, all measurements in millimeters internally).

**Nothing notably novel** — standard project documentation.

---

### 9. usher-tab-manager-extension-demo

**Type**: Chrome extension for tab management
**Agent infrastructure**: Basic CLAUDE.md + spec document

**Classified content**: Architecture overview, message protocol, key design decisions.

**Nothing notably novel** — standard project documentation.

---

### 10. tab-switcher

**Type**: HTML prototypes for conversation manager UI
**Agent infrastructure**: None whatsoever.

No CLAUDE.md, no .claude/ directory, no skills, no rules. Pure prototype files.

---

## Cross-Repo Meta-Analysis

### Pattern 1: The CLAUDE.md Maturity Ladder

Every repo (except tab-switcher) has a CLAUDE.md. They naturally evolve through maturity levels:

| Level | Pattern | Examples |
|-------|---------|----------|
| **L0: None** | No agent instructions | tab-switcher |
| **L1: Overview** | Project description + commands + architecture | dovetails, usher |
| **L2: Guided** | L1 + behavioral rules + testing guidance + gotchas | markdown-feedback, scrape-to-markdown, modernist-recipe-card |
| **L3: Comprehensive** | L2 + domain logic + institutional memory + release workflow | git-brain, agents-for-everyone |
| **L4: Framework** | Separated rules, skills, templates with cross-agent sync | work-agent-harness, home-brain |

**Insight**: Most projects naturally want to be at L2. The jump from L1 to L2 happens when the agent makes its first costly mistake (scaffolding destruction, bad URL, failed deployment). L3 emerges when the project is complex enough that conversation context alone can't carry institutional knowledge. L4 is only justified for knowledge-work systems, not application codebases.

### Pattern 2: Organically Emerged Rules

These rules appeared independently across multiple repos, suggesting they're genuinely useful:

| Rule | Repos Where It Appeared | Bootstrap Priority |
|------|------------------------|-------------------|
| **No destructive scaffolding** | markdown-feedback | High — prevents real data loss |
| **Verify URLs before committing** | markdown-feedback | High — prevents broken links |
| **Mandatory post-change test checklist** | git-brain, scrape-to-markdown | High — prevents broken deploys |
| **Spec before code** | scrape-to-markdown | Medium — prevents design drift |
| **Don't rely on user to test** | git-brain | Medium — ownership clarity |
| **Research discipline (no re-validation spiraling)** | scrape-to-markdown | Medium — prevents wasted cycles |
| **Environment bootstrapping command** | markdown-feedback, scrape-to-markdown | High — eliminates version mismatch errors |
| **Documentation maintenance triggers** | scrape-to-markdown | Medium — prevents doc drift |
| **Externalize everything** | agents-for-everyone, work-agent-harness | Medium — knowledge persistence |

### Pattern 3: Organically Emerged Skills

| Skill Pattern | Repos Where It Appeared | Bootstrap Priority |
|---------------|------------------------|-------------------|
| **Release procedure** | markdown-feedback (/release cmd) | High — every project needs releases |
| **MCP brain integration** | modernist-recipe-card, git-brain | Low — specific to BrainStem users |
| **Recipe/domain formatting spec** | modernist-recipe-card (skill file) | Niche — shows skill-as-domain-logic pattern |
| **Defects overcome / institutional memory** | git-brain | Medium — valuable for complex systems |
| **Multi-agent coordination** | scrape-to-markdown | Low — rare to have concurrent agents |

### Pattern 4: What CLAUDE.md Sections Prove Universally Useful

Analyzing which sections appear across the most repos and seem genuinely helpful:

| Section | Frequency | Value |
|---------|-----------|-------|
| **Project Overview** | 9/9 repos with CLAUDE.md | Essential — always needed |
| **Commands** | 7/9 | Essential — dev/build/test/deploy |
| **Architecture** | 7/9 | Essential — saves massive context-building time |
| **Key Patterns/Conventions** | 6/9 | High — prevents anti-pattern introductions |
| **Gotchas / Known Issues** | 4/9 | High — prevents repeated mistakes |
| **Testing Guidance** | 4/9 | High — quality gate |
| **Deployment** | 4/9 | Medium — needed for deployed projects |
| **File Organization** | 3/9 | Medium — helps for larger projects |
| **Rules/Behavioral Directives** | 3/9 | Medium — emerges from painful experience |
| **Version History / Status** | 2/9 | Low — useful for long-lived complex projects |

### Pattern 5: The MCP Integration Pattern

Three repos (git-brain, modernist-recipe-card, markdown-feedback) have Brain MCP integration in their .claude/settings.local.json or .env files. This creates a cross-repo knowledge flow:
- Projects push progress notes to the brain
- The brain MCP server makes these searchable across all contexts
- This is the "cross-repo inbox bridging" concept from the ROADMAP

### Pattern 6: Settings Permissions Patterns

Common permission patterns across .claude/settings.local.json files:

| Permission Category | Repos | Notes |
|--------------------|-------|-------|
| Git operations | 8/9 | git add, commit, push, checkout, etc. |
| Build commands | 6/9 | npm/yarn build, test, lint |
| Brain MCP tools | 4/9 | search_brain, brain_inbox_save, etc. |
| WebFetch domains | 4/9 | Whitelisted domains for web research |
| Deploy commands | 2/9 | wrangler deploy, gh release |

---

## Bootstrap Recommendations

### Tier 1: Every New Project (L2 Starter Kit)

These should be bootstrapped into every new project from day one:

**1. CLAUDE.md template** with these sections:
```
# CLAUDE.md
## Project Overview — what this is, one paragraph
## Commands — dev, build, test, lint, deploy
## Architecture — components, data flow, key files
## Key Patterns — conventions the agent must follow
## Gotchas — known issues, version requirements, env setup
## Testing — how to verify changes work
## Deployment — how to ship
## Rules — behavioral directives (added as they emerge)
```

**2. Standard rules to embed** (inline in CLAUDE.md for L2 projects):
- **No destructive scaffolding** — Never run scaffold commands that overwrite existing files
- **Verify URLs** — Every URL written into code/docs must be verified before committing
- **Environment bootstrapping** — Exact command to set up the right runtime version
- **Post-change checklist** — Mandatory verification steps after any change (build, test, deploy, verify)

**3. .claude/settings.local.json** with baseline permissions:
```json
{
  "permissions": {
    "allow": [
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git log:*)"
    ]
  }
}
```
(Expanded per-project for build tools, deploy commands, etc.)

### Tier 2: Projects with Release Cycles

**4. /release slash command** — Adapted from markdown-feedback's pattern:
- Discover unreleased changes since last tag
- Summarize and categorize (features/fixes/infrastructure)
- Ask user for version number
- Create/update CHANGELOG.md
- Build artifacts
- Tag, push, create GitHub release

This could be a universal `.claude/commands/release.md` template.

### Tier 3: Complex or Long-Lived Projects

**5. Defects Overcome section** — From git-brain. When the agent encounters and fixes a non-obvious bug, document it inline. Format:
```markdown
## Defects Overcome
1. **[Short name]**: [Description of the bug and fix]
```

**6. Documentation maintenance triggers** — From scrape-to-markdown. Explicit rules about when to update which docs:
```markdown
## Documentation Maintenance
- New feature → update README + CHANGELOG
- Architecture change → update CLAUDE.md Architecture section
- Version bump → update CHANGELOG + package.json
```

**7. Spec-before-code rule** — From scrape-to-markdown. For projects with multiple features in flight.

### Tier 4: Knowledge Work Systems Only

**8. Full harness framework** — AGENTS.md + rules/ + skills/ + templates/ + mirrors. Only justified for systems where the primary work product is structured knowledge, not application code.

---

## Antipatterns & Quality Improvements

### Antipattern 1: The Context Dumping Ground (git-brain)

**Problem**: git-brain's CLAUDE.md is 764 lines — a single file containing architecture docs, API reference, database schemas, customer lifecycle, defect history, version history, OAuth flows, and testing procedures. It's effective but unsustainable:
- Every session loads all 764 lines into context
- No progressive disclosure — agent gets schema details when it only needs to fix a typo
- Harder to maintain — changes to one section risk breaking another

**Recommendation**: Extract into focused files:
```
CLAUDE.md            — overview, commands, architecture summary, rules (~150 lines)
docs/schema.md       — database schemas (reference, loaded on demand)
docs/api-reference.md — endpoints and data flows
docs/defects.md      — defects overcome (institutional memory)
CHANGELOG.md         — version history (already standard)
```

Keep CLAUDE.md as the entry point that references these, not the container for everything.

**Severity**: Medium. It works today but will hit scaling limits as the project grows.

### Antipattern 2: Duplicated MCP Integration Instructions

**Problem**: Both modernist-recipe-card and git-brain contain curl-based MCP protocol instructions for pushing notes to BrainStem. These are copy-pasted and will drift.

**Recommendation**: This is a candidate for a shared skill or template. Options:
- A `brain-integration.md` snippet that projects can reference
- Or (better) rely on the MCP tools being available natively in Claude Code settings, making the curl instructions unnecessary

**Severity**: Low. Only two repos, and the curl approach may be replaced by native MCP tool access.

### Antipattern 3: No Agent Instructions at All (tab-switcher)

**Problem**: tab-switcher has zero agent configuration. Even a minimal CLAUDE.md would help any future agent session.

**Recommendation**: Add a basic L1 CLAUDE.md:
```markdown
# CLAUDE.md
## Project Overview
HTML prototypes for a conversation manager UI concept.
## Files
- conversation-manager-ui.html — original prototype
- tab-switcher-v3.html / v4.html — iterated versions
- tab-switcher-session.md — design session transcript
## No build system. Open HTML files directly in a browser.
```

**Severity**: Low. This is a dormant prototype repo.

### Antipattern 4: Version History in CLAUDE.md (git-brain)

**Problem**: git-brain embeds its full version history (v4.4 through v5.2) inside CLAUDE.md. This should live in CHANGELOG.md.

**Recommendation**: Extract to CHANGELOG.md, keep only "Current Status" in CLAUDE.md.

**Severity**: Low. Cosmetic, but contributes to the context dump problem.

### Antipattern 5: Inconsistent Skill File Placement

**Problem**: modernist-recipe-card has `recipe-formatting-skill.md` at the repo root. home-brain has skills in `.claude/skills/`. work-agent-harness has them in `skills/`. There's no consistent convention for where a project-specific skill lives.

**Recommendation**: Establish convention:
- **Framework projects**: `skills/` directory (universal) + agent mirrors
- **Application projects**: `docs/skills/` or just the repo root if there's only one
- The key is that CLAUDE.md should reference the skill file location explicitly

**Severity**: Low. Each project is internally consistent; it's only cross-repo that it's inconsistent.

### Antipattern 6: Missing Slash Commands for Common Workflows

**Problem**: Only markdown-feedback has a /release slash command. Every project that deploys would benefit from one.

**Recommendation**: Create a reusable `/release` command template that can be adapted per project. Core structure:
1. Discover unreleased changes
2. Categorize (features/fixes/infra)
3. Ask user for version
4. Update changelog
5. Build + tag + push + create release

**Severity**: Medium. Missing a significant productivity tool.

### Quality Improvement 1: Standardize CLAUDE.md Structure

Create a CLAUDE.md template (for the harness's `templates/` directory) that codifies the L2 structure:

```markdown
## Project Overview
## Commands
## Architecture
## Key Patterns
## Gotchas
## Testing
## Deployment
## Rules
```

This template should be the starting point for any new project.

### Quality Improvement 2: Extract Reusable Rules

Several rules from individual repos deserve to be extracted into the harness as optional rule snippets that new projects can adopt:

| Rule | Source | Reusability |
|------|--------|-------------|
| No destructive scaffolding | markdown-feedback | Universal for any project using npm/yarn |
| Verify URLs | markdown-feedback | Universal |
| Post-change verification | git-brain | Universal for deployed projects |
| Spec-before-code | scrape-to-markdown | Good for multi-feature projects |
| Research discipline | scrape-to-markdown | Good for complex projects |

### Quality Improvement 3: Port Remaining Templates from home-brain

home-brain has 9 templates; work-agent-harness has 3. The missing ones:

| Template | Port Priority | Notes |
|----------|---------------|-------|
| person.md | Low for work context | Would need corporate adaptation (stakeholder profile) |
| meeting-note.md | High | Universal for corporate PM work |
| decision.md / decision-quick.md | Already ported as rule | decision-system.md covers this |
| source.md | Already ported | source-processing skill handles this |
| knowledge-entry.md | Medium | Relevant if source-processing is used |
| quick-note.md | Low | inbox-triage handles this flow |

**Meeting-note.md** is the main gap — meeting notes are a primary PM artifact.

---

## Action Items

### Immediate (This Session)

1. **Create a CLAUDE.md template** in `templates/` for bootstrapping new projects — *based on the L2 pattern identified above*
2. **Create a /release command template** in `templates/` — *adapted from markdown-feedback's pattern*

### Near-Term (Next Few Sessions)

3. **Port meeting-note template** from home-brain to work-agent-harness — *high value for PM context*
4. **Add "common rules" snippets** to harness — *no-destructive-scaffold, verify-URLs, post-change-checklist as optional includes*
5. **Propose git-brain CLAUDE.md refactor** to the user — *extract schema, API reference, defect history into separate docs*
6. **Add minimal CLAUDE.md to tab-switcher** — *low effort, establishes baseline*

### Backlog

7. **Create a universal /release slash command** that can be dropped into any project
8. **Investigate cross-repo CLAUDE.md linting** — a tool that checks CLAUDE.md files against the L2 template for completeness
9. **Port knowledge-entry template** if source-processing is adopted in work context
10. **Resolve remaining harness port items** — git-workflow (fold useful bits into communication.md), mcp-optimization (evaluate relevance), newsworthy-events (skip)

---

## Appendix: Raw File Counts

| Repo | CLAUDE.md Lines | Skills | Rules | Templates | .claude/ Files | .windsurf/ Files | Total Agent Files |
|------|----------------|--------|-------|-----------|---------------|-----------------|-------------------|
| work-agent-harness | 27 (+169 AGENTS.md) | 6+README | 5 | 3 | 8 | 13 | ~35 |
| home-brain | ~100 | 8 | 9 | 9 | ~20 | ~15 | ~45 |
| git-brain | 764 | 0 | 0 | 0 | 2 | 0 | 3 |
| markdown-feedback | 215 | 0 | 0 | 0 | 3 | 0 | 4 |
| scrape-to-markdown | ~400 | 0 | 0 | 0 | 1 | 0 | 2 |
| modernist-recipe-card | ~100 | 1 | 0 | 1 | 1 | 0 | 4 |
| agents-for-everyone | ~80 | 0 | 0 | 0 | 0 | 0 | 1 |
| dovetails | 60 | 0 | 0 | 0 | 1 | 0 | 2 |
| usher-extension-demo | ~60 | 0 | 0 | 0 | 1 | 0 | 2 |
| tab-switcher | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

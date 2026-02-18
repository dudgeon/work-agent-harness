---
created: 2026-02-18
updated: 2026-02-18
status: decided
tags: [spec, public-repo, external-repo, sync, publishing]
---

# Spec: Public Repo Externalization

Design for WAH domains to externalize shared/public knowledge to public GitHub repos, where the public repo becomes the Source of Record (SOR) for base knowledge and the WAH instance provides a private augmentation layer.

---

## Problem

WAH is private by default — all knowledge lives in a single private repo. But not all knowledge is private:

- **Industry knowledge** about a technology, framework, or discipline is shareable
- **Curated knowledge bases** are more valuable when maintained publicly (community contributions, credibility, portfolio value)
- **Team-relevant knowledge** may belong in a shared repo rather than one person's harness

Currently, WAH has no mechanism to:
1. Point a domain at an external public repo as the SOR for base knowledge
2. Layer private annotations/context on top of public content
3. Contribute WAH-authored content back to public repos
4. Keep local clones in sync with remote changes
5. Create new public repos from WAH domain content (externalization)

The "Shared Team Context Repo Support" backlog item covers the team repo case. This spec covers the **public repo** case specifically, while designing primitives that serve both.

---

## Core Concept: External Repos

An **external repo** is any Git repository that WAH interacts with but does not own. Two subtypes:

| | Public Repo | Team Repo |
|---|---|---|
| **Audience** | Open/community | Org-internal team |
| **WAH's role** | Consumer + occasional contributor | Peer contributor |
| **SOR relationship** | Public repo is SOR; WAH augments | Team repo is SOR for shared; WAH owns private |
| **Contribution frequency** | Occasional (PRs for general knowledge) | Regular (PRs for team knowledge) |
| **Content sensitivity** | High (must never leak private info) | Medium (org-internal, but not personal) |
| **Creation** | WAH may scaffold a new public repo | WAH may scaffold a new team repo |

Both share the same infrastructure: clone management, domain linking, PR workflows, sync. This spec focuses on public repos but notes where team repos diverge.

---

## Design

### 1. Clone Management

External repos are cloned to `.repos/` at the WAH root:

```
work-agent-harness/
├── .repos/                          # Cloned external repos (gitignored by WAH)
│   ├── pm-knowledge/                # Clone of github.com/user/pm-knowledge
│   ├── cloud-infra-playbook/        # Clone of github.com/org/cloud-infra-playbook
│   └── ...
├── .gitignore                       # Includes .repos/
├── domains/
│   └── product-management/          # WAH domain that augments pm-knowledge
└── ...
```

**Why `.repos/` at WAH root (not sibling, not inside domain):**
- Everything under one project tree — agent can access by path without workspace gymnastics
- Gitignored by WAH — public repo content doesn't pollute WAH's git history
- Each clone is a full git repo — can branch, commit, push, create PRs independently
- No submodule complexity

**Registry file**: `context/external-repos.md` tracks all external repos:

```markdown
# External Repos

Registry of external repositories this harness interacts with.

## Active

| Repo | Type | Local Path | Remote URL | Linked Domain(s) |
|------|------|-----------|------------|-------------------|
| pm-knowledge | public | .repos/pm-knowledge | github.com/user/pm-knowledge | [domains/product-management/](../domains/product-management/) |
| cloud-infra-playbook | public | .repos/cloud-infra-playbook | github.com/org/cloud-infra-playbook | [domains/cloud-infra/](../domains/cloud-infra/) |
```

### 2. Domain ↔ External Repo Linking

A WAH domain declares its external repo relationship in frontmatter:

```yaml
---
created: 2026-02-18
updated: 2026-02-18
template: templates/domain.md
template_version: 2
status: active
external_repo: https://github.com/user/pm-knowledge
external_repo_path: .repos/pm-knowledge
external_repo_type: public          # public | team
---
```

The domain README includes a section describing the relationship:

```markdown
## Public Base

This domain augments the [PM Knowledge](https://github.com/user/pm-knowledge) public repo, which is the source of record for general product management knowledge.

- **Public repo** (SOR for base knowledge): `.repos/pm-knowledge/`
- **This domain** (private overlay): company-specific PM context, internal annotations, unpublished drafts

### What goes where

| Content type | Destination | Example |
|---|---|---|
| Generally applicable technique/framework | Public repo (via PR) | "Pre-mortem for Feature Launches" |
| Company-specific application of a technique | This domain (`_private/`) | "How we run pre-mortems at Acme Corp" |
| Personal reading notes on public content | This domain (`annotations/`) | "Notes on technique X for Q3 planning" |
| Drafts not yet ready for public | This domain (`_staging/`) | WIP knowledge entry |
```

### 3. WAH Domain Structure (Private Overlay)

When a domain augments an external repo, the WAH domain is a **thin overlay** — not a mirror of the public repo's structure:

```
domains/product-management/
├── README.md                    # Links to public repo, declares overlay scope
├── _private/                    # Company/personal content (never leaves WAH)
│   ├── our-pm-process.md        # How we do PM at our company
│   ├── team-preferences.md      # Tool/approach preferences
│   └── stakeholder-context.md   # Internal dynamics relevant to PM work
├── annotations/                 # Private notes on public repo content
│   ├── technique-x-at-acme.md   # How technique X applies to our context
│   └── reading-notes-2026-02.md # Personal notes on recent public repo additions
├── _staging/                    # Content being prepared for public contribution
│   └── new-framework-draft.md   # Will become a PR to the public repo
└── [standard domain content]    # Tasks, timeline, etc.
```

**Key directories:**

- `_private/` — Content that is inherently private (company context, personal notes). Never candidates for externalization.
- `annotations/` — Private commentary on public repo content. References public repo files by path. May occasionally surface insights worth contributing back.
- `_staging/` — Content being prepared for contribution to the public repo. Once the PR is merged, the file can be removed from staging (the public repo is now the SOR).

The public repo at `.repos/pm-knowledge/` has its own full structure — its own AGENTS.md, templates, domains, etc. WAH doesn't replicate or mirror it; it just reads and references it.

### 4. Agent Behavior

#### At session start:

1. Read `context/external-repos.md` to know which external repos exist
2. For repos linked to domains with **active tasks** in `tasks.md`:
   - Check if `.repos/<name>` clone exists; if not, clone it
   - `git fetch origin` (don't auto-pull — user may have local changes)
   - Check for upstream changes: `git log HEAD..origin/main --oneline`
   - If changes exist, note them: "The PM Knowledge repo has 3 new commits since last fetch."
3. For repos linked to dormant domains (no active tasks): skip — fetch on demand when the user first works on that domain
4. When starting work on a linked domain, load the external repo's README/structure as additional domain context

#### When creating content in this domain:

The agent should actively classify new content:

1. **Is this generally applicable?** → Suggest contributing to public repo
2. **Is this company/context-specific?** → Route to `_private/`
3. **Is this a reaction to public content?** → Route to `annotations/`
4. **Not sure yet?** → Route to `_staging/` for the user to decide later

This classification should be lightweight — a brief question when the destination isn't obvious, not a gate on every write.

#### When processing sources for this domain:

If the domain uses domain-source-synthesis AND has an external repo:

- **Sources** are captured in the WAH domain (private — your reading queue is yours)
- **Knowledge entries** are classified during extraction:
  - Generally applicable → `_staging/` for PR contribution
  - Context-specific → `_private/`
  - Enriches existing public entry → annotation or PR to update the public entry

### 5. Contributing Back (Push Workflow)

Contributions accumulate in `_staging/` until a batch is ready. The agent offers to submit when staging reaches a meaningful threshold or when the user asks.

**Batch submission workflow:**

1. **Agent prepares the batch** in the public repo's format
   - Reads the public repo's templates/conventions
   - Adapts each WAH draft to match
   - **Privacy scrub** (per D3): Scans each file against WAH context files for private patterns — company names, stakeholder names, internal URLs, project codenames, org-specific terminology. Flags or removes matches. Presents a scrub report to the user showing what was found and removed.
2. **User reviews the scrubbed batch** — confirms content is safe for public
3. **Agent creates a branch** in the public repo clone:
   ```bash
   cd .repos/pm-knowledge
   git checkout -b contribute/batch-YYYY-MM-DD
   ```
4. **Agent writes the content** to the appropriate locations in the public repo
5. **Agent pushes and creates PR**:
   ```bash
   git push -u origin contribute/batch-YYYY-MM-DD
   gh pr create --title "Add: [summary of batch]" --body "..."
   ```
6. **User merges** (or requests changes)
7. **After merge**: Remove or archive the `_staging/` files in WAH; the public repo is now the SOR for this content

**Privacy safeguard**: Two layers — agent-assisted scrubbing (automated pattern detection) AND user review (human judgment). The agent should never auto-push to public repos. Every contribution requires explicit user review of the scrubbed content before the PR is submitted.

### 6. Staying Current (Pull Workflow)

1. **At session start**: `git fetch` in all active clones (per session start protocol)
2. **On demand**: User asks to update, or agent notices stale clone
3. **Change detection**: After pulling, scan for changes that affect WAH annotations
   - Public entry was updated → flag WAH annotations that reference it
   - Public entry was moved/renamed → update WAH references
   - New public entries appeared → note them for the user

This is lightweight — no automated reconciliation. Just awareness.

### 7. Creating a New Public Repo (Externalization)

Sometimes the flow goes the other direction: WAH has domain content that should become a public repo.

**Trigger**: User says "I want to make this domain public" or "Let's create a public repo for this."

**Workflow:**

1. **Audit domain content** — classify each file:
   - Public-ready (general knowledge, no private info) → seed the public repo
   - Private (company context, personal notes) → stays in WAH overlay
   - Needs scrubbing (general idea but has private references) → clean and include
2. **Scaffold the public repo** — the harness carries an opinionated structure:
   - Own `AGENTS.md` (so agents can work in it independently)
   - Own `README.md` (explains the repo's purpose and structure)
   - Domain structure matching the WAH domain's public content
   - Templates if the domain uses domain-source-synthesis
3. **Create the GitHub repo**: `gh repo create <name> --public`
4. **Push seed content** to the new public repo
5. **Reconfigure WAH domain** as a private overlay:
   - Add `external_repo` frontmatter
   - Move private content to `_private/`
   - Update `context/external-repos.md`
   - Clone the new repo back to `.repos/`

This is a significant operation — the skill should walk through it step by step with user approval at each stage.

---

## IDE Setup (VSCode)

### Option A: Single Root (Recommended for simplicity)

Open WAH as the workspace root. `.repos/` contents are visible in the file tree:

```
work-agent-harness/          # Workspace root
├── .repos/
│   ├── pm-knowledge/         # Visible, searchable
│   └── cloud-infra/          # Visible, searchable
├── domains/
└── ...
```

**Pros**: Simple, one workspace, everything searchable. Agent (Claude Code or Windsurf) can access everything by path.

**Cons**: `.repos/` clutter in file tree. Can be mitigated with VSCode's `files.exclude` for repos not actively being worked on.

### Option B: Multi-Root Workspace

Create a `.code-workspace` file:

```json
{
  "folders": [
    { "path": ".", "name": "WAH" },
    { "path": ".repos/pm-knowledge", "name": "PM Knowledge (public)" }
  ]
}
```

**Pros**: Clean separation. Each root has its own git context. Agent can work in either root naturally.

**Cons**: More setup. Some agents handle multi-root better than others.

### Recommendation

Start with **Option A**. It's zero-config and works with both Claude Code and Windsurf. Move to Option B if file tree clutter becomes a problem.

### Agent Workspace Awareness

For either option, the agent needs to know:
- Which repos are external (check `context/external-repos.md`)
- Which domain links to which external repo (check domain frontmatter)
- The working directory context when making git operations (WAH root vs `.repos/<name>`)

The `context/external-repos.md` registry serves as the lookup table. Agent reads it at session start when working on linked domains.

---

## Relationship to Existing Patterns

### Origin tracking (endogenous/imported)

The existing `origin: endogenous | imported` pattern maps directly:
- Public repo content is `imported` when referenced by WAH
- WAH-authored content that gets contributed to a public repo transitions from `endogenous` to having a public SOR
- The `source_url` field can point to the public repo file's URL

### Domain-source-synthesis

Works naturally:
- **Sources** are captured privately in WAH (your reading queue)
- **Knowledge entries** get classified: generally applicable → public repo; context-specific → WAH private
- The public repo may have its own source-synthesis setup — WAH doesn't interfere with it

### Team repo backlog item

Shares infrastructure:
- Clone management (`.repos/`, `context/external-repos.md`)
- PR contribution workflow
- Sync/pull mechanisms
- Scaffolding for new repos

Differs in:
- Team repos may use `external_repo_type: team` for different contribution defaults
- Team repo contributions may be more frequent and less guarded
- Team repos carry shared team context; public repos carry general knowledge

The team repo backlog item's deliverables can build on this spec's primitives:
- `context/external-repos.md` pattern → serves both public and team repos
- Clone management in `.repos/` → shared infrastructure
- PR contribution skill → parameterized for public vs team contexts

---

## Decisions

### D1: One External Repo Per Domain

Each domain links to at most one external repo. No nested repos. If a domain spans multiple external repos, that's a signal to split the domain. This keeps content routing unambiguous — the agent always knows which external repo a domain's public content targets.

### D2: Ask at Domain Creation

When linking a domain to an external repo, ask the user what structure the external repo should have (if creating) or adapt to its existing structure (if linking to an existing repo). No blanket enforcement — each external repo's structure is decided case by case at the point of domain linkage.

### D3: Agent-Assisted Privacy Scrubbing + User Review

When contributing to a public repo, the agent actively scrubs content: scans for patterns that look private (company names from context files, stakeholder names, internal URLs, project codenames) and flags or removes them. The scrubbed content is then presented to the user for final review before the PR is submitted. Both layers — agent scrubbing AND user review — are required for public repos.

### D4: On-Demand Clone, Session-Start Fetch for Active Domains

Clone repos on demand when the user first works on a linked domain. At session start, fetch (not pull) only clones linked to domains with active tasks in `tasks.md` — skip dormant domains to keep session start fast. If upstream changes are detected, note them ("PM Knowledge repo has 3 new commits since last fetch") but don't auto-pull. Pull explicitly when the user asks or when starting work on that domain.

### D5: Batch Contributions

Accumulate contributions in `_staging/` and submit them as batched PRs rather than one PR per idea. This reduces PR noise on the public repo and lets the user review a coherent set of additions at once. The agent offers to submit a batch when staging reaches a meaningful threshold or when the user asks.

---

## Implementation Phases

### Phase 0: Primitives (can do now)

- Add `.repos/` to WAH `.gitignore`
- Create `context/external-repos.md` template
- Add `external_repo` fields to domain template frontmatter
- Document the concept in AGENTS.md

### Phase 1: Read-Only Integration

- Skill for cloning and linking an external repo to a domain
- Agent reads external repo content as domain context
- `annotations/` directory for private notes on public content
- Session start protocol extension for external repo awareness

### Phase 2: Contribution Workflow

- `_staging/` workflow for preparing public contributions
- PR creation skill (branch, adapt to public repo format, push, `gh pr create`)
- Privacy review checkpoint
- Post-merge cleanup

### Phase 3: Externalization

- Skill for creating a new public repo from WAH domain content
- Content audit and classification
- Scaffold and seed workflow
- Domain reconfiguration to overlay mode

### Phase 4: Team Repo Extension

- Adapt primitives for team repo case
- Different contribution defaults (more frequent, less guarded)
- Team repo scaffolding spec

---

## Concrete Example

**Scenario**: User maintains a "Product Management" domain in WAH. They want to create a public PM knowledge base and keep their company-specific context private.

**Step 1: Externalize**
```
# Before: everything in WAH
domains/product-management/
├── README.md
├── techniques/
│   ├── pre-mortem.md           # Generally applicable
│   ├── stakeholder-mapping.md  # Generally applicable
│   └── acme-launch-process.md  # Company-specific
├── sources/
└── knowledge-base/
```

**Step 2: Create public repo**
```bash
gh repo create pm-knowledge --public
# Agent seeds with public-ready content (pre-mortem.md, stakeholder-mapping.md)
# Public repo gets its own AGENTS.md, README.md, structure
```

**Step 3: Reconfigure WAH domain**
```
# After: WAH is private overlay
domains/product-management/
├── README.md                    # Updated: links to .repos/pm-knowledge
├── _private/
│   └── acme-launch-process.md   # Stayed private
├── annotations/
│   └── pre-mortem-at-acme.md    # How the public technique applies here
├── _staging/                    # Empty for now
├── sources/                     # Still private (reading queue is yours)
└── knowledge-base/              # May be empty if all entries went public

.repos/pm-knowledge/             # Clone of the new public repo
├── AGENTS.md
├── README.md
├── techniques/
│   ├── pre-mortem.md
│   └── stakeholder-mapping.md
└── ...
```

**Step 4: Ongoing**
- User reads a new article, extracts a generally applicable technique → agent drafts it, creates PR to pm-knowledge
- User has a company-specific insight about a public technique → goes to `annotations/`
- User's team has a unique process → goes to `_private/`
- Public repo gets community contributions → agent pulls, flags new content for the user

---

*This spec establishes the primitive of "external repos that WAH interacts with" and focuses on the public repo case. The same primitives serve team repos with different defaults. The key principle: public repos are the SOR for shared knowledge; WAH is the private intelligence layer on top.*

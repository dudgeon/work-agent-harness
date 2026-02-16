# Progressive Disclosure & Context Graphs for Knowledge Repos

Research findings for home-brain and work-agent-harness retrieval architecture.

---

## Part 1: Progressive Disclosure for Agent Context

### What It Is

Progressive disclosure, borrowed from HCI/UX design, is an information architecture pattern where complexity is revealed in layers rather than all at once. Applied to LLM context management, it means: instead of stuffing everything into the context window upfront, you expose lightweight metadata first and let the agent (or the retrieval system) decide what to fetch in full.

The core insight is that LLMs suffer from **context rot** — as token count increases, the model's ability to accurately recall information from that context decreases. Anthropic's own context engineering guidance frames it this way: good context engineering means finding the **smallest possible set of high-signal tokens** that maximize the likelihood of a desired outcome.

### The Emerging Consensus: 3-Layer Retrieval

Multiple independent implementations have converged on a similar layered pattern:

**Layer 1 — Index/Map (~50-100 tokens per item)**
Lightweight metadata: titles, dates, types, token cost estimates. The agent scans this to understand *what exists* without paying the cost of *reading it all*. Think table of contents, not the book.

**Layer 2 — Summaries/Context (~200-500 tokens per item)**
2-3 sentence summaries, chronological context around interesting items, or section headers. Enough to assess relevance with confidence before committing to full retrieval.

**Layer 3 — Full Content (~500-1,000+ tokens per item)**
Complete documents, detailed records, source files. Retrieved only for items the agent has determined are relevant through layers 1-2.

This pattern achieves roughly **10x token savings** compared to naive "dump everything" approaches, according to Claude-Mem's production measurements. The key insight: filtering before fetching is dramatically more efficient than fetching then ignoring.

### Reference Implementations

**Claude Code (Anthropic, production)**
The hybrid model: CLAUDE.md files are naively loaded upfront as always-available context, while `glob` and `grep` provide just-in-time filesystem exploration. Skills use a lightweight form of progressive disclosure — the system prompt includes only a brief YAML description of each skill (~100 tokens), and the agent reads the full SKILL.md only when it determines one is relevant. This is the pattern your system prompt in this very conversation follows.

**Claude-Mem (open source, Claude Code plugin)**
The most explicit implementation of 3-layer progressive disclosure for coding agent memory. Captures tool usage as typed "observations" (gotcha, decision, trade-off, how-it-works, etc.), stores them in SQLite + Chroma vector DB, and exposes 5 MCP tools following the layer pattern:
- `search` → compact index with IDs and token estimates
- `timeline` → chronological context around specific observations
- `get_observations` → full details for selected IDs only
- `save_memory` → manual knowledge capture
- `search_similar` → semantic search

Each observation carries a type emoji and token cost estimate in the index view, so the agent can make cost/relevance tradeoffs. This is the closest reference implementation to what your home-brain could adopt.

**Klavis Strata (commercial, MCP-based)**
Progressive disclosure applied to *tool discovery* rather than knowledge. Instead of exposing hundreds of MCP tools at once (which causes "choice paralysis" and hallucination), Strata guides the agent through: Intent → Server Categories → Category Actions → Action Details → Execute. Each step only reveals what's needed for the next decision. Benchmark results show +15% higher accuracy versus loading all tools upfront. The pattern is interesting for work-agent-harness since it directly addresses MCP tool overload.

**Letta Context Repositories (product, Feb 2026)**
The most directly relevant implementation to your systems. Letta rebuilt their coding agent memory as a git-backed filesystem — agents clone their "context repository" locally and use standard terminal tools to read, write, and reorganize memory. Key innovations beyond the 3-layer pattern:
- **Filetree-as-layer-1**: The directory structure is always in the system prompt, so folder names and hierarchy *are* the index. No separate index tool needed — `ls` and `find` are the discovery layer.
- **Frontmatter-as-layer-2**: Every file has YAML frontmatter with a description, mirroring Anthropic's SKILL.md pattern. The agent reads descriptions to assess relevance before opening full files.
- **Agent-managed disclosure**: The agent itself can reorganize files, update frontmatter, and move files in/out of a `system/` directory (always-loaded context) as it learns what matters. Progressive disclosure isn't static architecture — it's a living system the agent maintains.
- **Git as temporal layer**: Every memory change is a git commit with an informative message, providing free "when and why did this change" tracking without needing graph database infrastructure.
- **Memory defragmentation**: A background subagent periodically reorganizes knowledge — splitting bloated files, merging duplicates, restructuring into clean hierarchies. This addresses the staleness problem that plagues manually-maintained indexes.
- **Concurrent writes via git worktrees**: Multiple subagents can process memory simultaneously in isolated worktrees, merging through standard git conflict resolution.

Critically, Letta's approach works with **filesystem primitives only** — no embedding search or MCP required. The agent uses `cat`, `grep`, `find`, and scripts it writes itself. This makes the pattern portable across any coding agent environment.

Source: https://www.letta.com/blog/context-repositories (Feb 12, 2026)

**Inferable.ai (production blog post)**
Documented a counterintuitive finding: giving LLMs more context often makes them *worse* at instruction-following tasks. Their solution mirrors the pattern — expose search tools instead of data, let the model construct queries and fetch what it needs per-step. They describe it as "progressive context enrichment" and note it's analogous to how humans retrieve information rather than holding everything in working memory.

**Will Larson / Lethain (blog series, Dec 2025)**
Documented applying progressive disclosure to large file handling in internal agents. Created a `large_files` skill that teaches the agent when to `extract_file`, `peek_file`, or `load_file` based on file type and available context space. The key insight: progressive disclosure is a *skill the agent learns*, not just an infrastructure pattern.

### How Your Implementation Compares

Your current home-brain has some progressive disclosure characteristics but doesn't fully implement the pattern:

**What you're doing well:**
- CLAUDE.md provides a lightweight entry point with pointers to deeper files
- Domain READMEs serve as layer-1 indexes for their areas
- The "context loading: top-down" instruction (read README first, then skills, then source files) *is* progressive disclosure, hand-coded into your workflow
- `mcp-optimization.md` shows awareness that front-loading important concepts and writing for discoverability matters for retrieval
- BrainStem MCP's `search_brain` + `get_document` + `list_folders` are the building blocks for layered retrieval

**Gaps relative to state of the art:**
1. **No token cost awareness.** Claude-Mem exposes token estimates in search results so the agent can make informed budget decisions. Your search results don't signal document size or retrieval cost.
2. **No typed/categorized observations.** Your context files mix facts, preferences, decisions, and questions. Claude-Mem's observation types (gotcha, decision, trade-off, discovery) enable the agent to prioritize by type, not just relevance.
3. **No intermediate summary layer.** You have index (list_folders, README) and full content (get_document), but nothing in between — no way to get a 2-sentence summary of a document before committing to reading it. Your frontmatter has `tags` and `status`, but not a `summary` field.
4. **README staleness risk.** Your progressive disclosure relies heavily on READMEs being up-to-date, but they require manual maintenance. Claude-Mem and Graphiti both auto-generate their index layers.
5. **No search result ranking.** `search_brain` returns semantic matches, but doesn't expose confidence scores, recency, or let the agent compare multiple results before deep-diving.

### Recommendations for Your Systems

The two systems operate under fundamentally different constraints:

- **home-brain** can use brainstem (MCP with semantic search, `search_brain`, `get_document`, `list_folders`) as a bolt-on retrieval layer. This is a luxury — it means Claude.ai, Claude mobile, and any MCP-capable client can do semantic search over your knowledge base.
- **work-agent-harness** must work with **filesystem primitives only** — `cat`, `grep`, `glob`, `find`, `head`, `ls`, and whatever the agent writes itself. No MCP servers, no embedding search, no external services. This is the common denominator across Claude Code, Windsurf, Cursor, Codex, and whatever comes next. Enterprise software policies may preclude external services for a long time. The pattern must be model-agnostic and agent-ecosystem-agnostic.

This constraint actually clarifies the design: **the filesystem *is* the retrieval system.** Directory structure, file naming, frontmatter, and README files aren't just organizational conventions — they're the only discovery and navigation tools the agent has. Every structural decision is a retrieval decision.

#### Shared conventions (both systems)

1. **Add `summary` to frontmatter.** Every note gets a 1-2 sentence summary in its YAML frontmatter. In home-brain, brainstem can surface this in search results. In work-agent-harness, an agent can `grep` or `head -20` to read frontmatter without loading the full file. Example:
   ```yaml
   summary: "Decision to use Cloudflare R2 + AI Search for brainstem, with rationale and alternatives considered."
   type: decision
   ```

2. **Categorize entries by type in frontmatter.** Borrow from Claude-Mem's taxonomy: decision, preference, fact, relationship, question, discovery. This enables `grep -r "type: decision"` across the repo — structured queries using only filesystem tools.

3. **Filetree-as-index (the Letta pattern).** Treat directory structure and file names as layer-1. An agent running `find . -name "*.md" | head -50` or `ls -la domains/` gets the navigational index for free. This means file naming and folder organization must be optimized for scannability, not just human aesthetics. Descriptive file names matter more than short ones.

4. **READMEs as layer-2 summaries.** Each domain/project README should list its contents with one-line descriptions — essentially a human-and-agent-readable manifest. This is the intermediate layer between "I can see the folder exists" and "I've read the whole document."

5. **`system/` or equivalent always-loaded directory.** Following Letta's pattern, designate a small set of files that should always be in the agent's context at session start. In home-brain, this maps to your `.claude/` directory (philosophy.md, communication.md, context-management.md). In work-agent-harness, this becomes a prescribed convention: the repo's top-level CLAUDE.md (or AGENTS.md) plus a `system/` or `.context/` directory with always-loaded guidance.

#### home-brain specific

6. **Enhance brainstem's search results.** Add document size/token estimates and frontmatter summaries to `search_brain` results. Consider a `search_index` tool that returns only metadata (titles, paths, summaries, types, sizes) — not content chunks — for cost-aware retrieval.

7. **Auto-summarization on ingest.** When notes arrive in inbox or get processed via Claude Code, generate and store the frontmatter summary automatically. This keeps the summary layer fresh without manual effort.

8. **Explore agent-managed defragmentation.** Following Letta's pattern, periodically have Claude Code review and reorganize domains — splitting bloated files, updating stale READMEs, consolidating duplicates. Git history preserves everything, so reorganization is safe.

#### work-agent-harness specific (filesystem-only patterns)

9. **Prescribe a manifest convention.** Since there's no semantic search, the repo needs a machine-scannable manifest. Options:
   - A top-level `MANIFEST.md` that lists every knowledge file with its type, summary, and path — regenerated automatically on commit via a git hook or CI step
   - Or: rely on frontmatter + a simple script (included in the repo) that extracts frontmatter from all `.md` files and generates a scannable index. The agent can run this script itself.

10. **Design for `grep`-ability.** Content should be written so that likely search terms appear naturally in text (your mcp-optimization.md guidance applies here too — "use synonyms," "front-load important concepts"). But also: use consistent structured markers that `grep` can find. For example, decisions always start with `## Decision:`, context entries always have `type:` in frontmatter. The agent doesn't need semantic search if the filesystem is grep-friendly.

11. **Include retrieval scripts in the repo.** Ship small utility scripts as part of the harness itself:
    - `scripts/search.sh "query"` — grep across all markdown files with context lines
    - `scripts/manifest.sh` — regenerate the manifest from frontmatter
    - `scripts/recent.sh [n]` — show the n most recently modified knowledge files
    These give any agent immediate retrieval capability without depending on specific tooling. The agent can also discover and use these scripts, or write its own.

12. **Document the "graduation path."** work-agent-harness should acknowledge that filesystem-only retrieval is the baseline tier, and that organizations can layer on:
    - **Tier 1 (baseline):** Filesystem primitives, grep, find, manifest scripts
    - **Tier 2 (enhanced):** MCP server with semantic search (brainstem-like)
    - **Tier 3 (advanced):** Knowledge graph with entity extraction (Graphiti-like)
    
    The filesystem conventions (frontmatter, READMEs, directory structure) are designed to work at tier 1 but become *better* with tiers 2-3. Nothing in the base pattern precludes future enhancement.

---

## Part 2: Context Graphs

### What People Mean When They Tweet About This

The term "context graph" is used loosely and covers a spectrum from lightweight to heavyweight. Here's the landscape:

**At the lightweight end: Relationship-aware knowledge organization**
This is what most individuals and small teams actually mean — your knowledge isn't a flat collection of documents but a web of connected entities (people, projects, decisions, tools, concepts) where the relationships carry meaning. "Geoff serves on the Open Works board" is more useful than having separate notes about Geoff and Open Works that don't explicitly link. This is essentially what you're already doing with cross-references, context files, and bidirectional links — it's just not formalized as a graph data structure.

**In the middle: Graph-augmented retrieval (GraphRAG)**
This approach structures knowledge as entity-relationship triples stored in a graph database (typically Neo4j), combined with vector embeddings for semantic search. Microsoft Research popularized "GraphRAG" as a pattern where entities are extracted, relationships mapped, and community summaries pre-computed. The graph enables "multi-hop reasoning" — the agent can traverse from a customer complaint → to a product → to a supplier → to a known defect, connecting dots that flat retrieval would miss.

**At the heavyweight end: Temporal knowledge graphs / enterprise context graphs**
This is where Zep/Graphiti and enterprise platforms like Glean, Stardog, and Neo4j position themselves. These systems track not just entities and relationships but *when* relationships were true (bi-temporal modeling), automatically invalidate outdated facts, extract entities from conversations, and maintain provenance chains. Glean has recently added a "personal graph" layer that captures individual work patterns. These are sophisticated infrastructure that require graph databases, LLM pipelines for extraction, and ongoing operational investment.

### The Key Distinction: Static Graphs vs. Temporal Graphs

The most important architectural choice in context graphs is whether relationships are treated as permanent or temporal:

- **Static graph**: "Geoff works at [company]" — a fact stored until manually updated
- **Temporal graph**: "Geoff works at [company] (valid_from: 2020-03, valid_to: null)" — a fact with provenance that can be automatically invalidated when new information arrives

Zep/Graphiti is the state-of-the-art open-source implementation of temporal knowledge graphs. Their paper (arxiv 2501.13956) demonstrates 94.8% accuracy on deep memory retrieval benchmarks. The key innovation: every edge in the graph carries `valid_at` and `invalid_at` timestamps, and the system automatically detects when new information conflicts with existing facts, using temporal metadata to update or invalidate (but not discard) outdated information.

### Reference Implementations

**Graphiti / Zep (open source + commercial)**
The most mature implementation. Requires Neo4j (or FalkorDB/Kuzu) + an OpenAI-compatible LLM. Provides an MCP server for integration with Claude, Cursor, etc. Supports custom entity types via Pydantic models. Achieves <300ms P95 retrieval through hybrid search (semantic + BM25 + graph traversal). The MCP server runs locally — data doesn't leave your machine. *This is the closest thing to a reference implementation you can actually study and run.*

GitHub: `getzep/graphiti`
Paper: arxiv.org/abs/2501.13956

**Microsoft GraphRAG (open source)**
Entity-centric knowledge graphs built by extracting entities/relationships and grouping them into thematic "communities" with LLM-generated summaries. Heavier on pre-computation (LLM calls to generate community summaries), slower to update, but strong for static document collections. Less suited to dynamic personal knowledge bases.

**Glean's Enterprise Knowledge Graph + Personal Graph**
Not open source, but architecturally interesting. Their "personal graph" captures individual work patterns across tools — not just what you saved, but what you're working on, who you collaborate with, and how your activities connect. This goes beyond document-level knowledge management into behavioral/activity modeling. Relevant conceptually for how you might eventually model your professional context in work-agent-harness.

### How This Relates to Your Systems

Your home-brain is effectively a **proto-graph implemented in markdown**. You have:
- Entities: people (domains/people/), projects, places, equipment
- Relationships: cross-references, bidirectional links, context files that connect entities
- Some temporal awareness: `created`, `updated` dates; "Corrected: was X, now confirmed as Y" patterns in context-management.md

What you *don't* have:
- **Explicit entity-relationship triples.** Your relationships are embedded in prose and links, not queryable as structured data. You can't ask "what entities are connected to Open Works?" without reading through documents.
- **Automatic entity extraction.** When you capture a note mentioning a person, project, and decision, those connections aren't automatically mapped — they depend on your manual cross-referencing discipline.
- **Multi-hop traversal.** An agent can't follow a chain like "Geoff → Open Works → Finance Committee → budget decisions → relevant precedent for current question" without manually reading through each document in the chain.
- **Conflict detection.** If a new note contradicts an old fact, there's no automatic detection — your context-management.md prescribes manual conflict resolution, but the system doesn't surface conflicts proactively.

### The Honest Assessment: Do You Need a Context Graph?

**Probably not yet, at least not a formal graph database.**

Here's why: Your home-brain has ~15 domains, perhaps a few hundred notes, and primarily one user (you, plus agents acting on your behalf). The overhead of running Neo4j, building entity extraction pipelines, and maintaining a graph database would likely exceed the retrieval benefits at your current scale.

Context graphs shine when:
- You have thousands+ of entities with complex interrelationships
- Multiple agents or users need to reason across the same knowledge
- Temporal accuracy is critical (what was true *at a specific point in time*)
- Multi-hop reasoning is a common query pattern (connecting 3+ entities in a chain)
- Data arrives continuously from multiple sources and must be reconciled

Your system's main retrieval challenge right now is more likely **progressive disclosure** (getting the right content to the agent efficiently) than **graph reasoning** (traversing complex entity relationships).

**However**, there are lightweight graph-adjacent patterns worth adopting:

### Recommendations

**For both systems — lightweight graph patterns in markdown:**

1. **Structured entity references in frontmatter.** Add a `related_entities` field to notes:
   ```yaml
   related_entities:
     - type: person
       name: "[friend]"
       path: domains/people/friend.md
     - type: project
       name: "brainstem"
       path: domains/projects/brainstem/README.md
   ```
   This creates a queryable relationship layer without a graph database. An agent can `grep -r "related_entities" --include="*.md"` to build a relationship map from frontmatter alone.

2. **Relationship-typed links.** Instead of generic `[note](path)` links, tag relationship types in frontmatter: "decided-by", "blocks", "relates-to", "supersedes". This gives future retrieval more signal about *why* things are connected — and it's grep-friendly.

3. **Git history as temporal graph.** Following the Letta insight: you already have git. `git log --all --oneline -- domains/projects/brainstem/` gives you the temporal history of an entity. `git log --diff-filter=M --name-only` shows what changed and when. This is a zero-infrastructure temporal layer that works with any agent that has shell access.

**For home-brain specifically (can use brainstem):**

4. **Graphiti MCP server as an experiment.** When you're ready to explore real graph capabilities, Graphiti's MCP server runs locally with Neo4j. You could point it at conversation history to auto-generate an entity graph, while keeping home-brain as the curated layer. This is a tier-3 graduation, not a prerequisite.

5. **Auto-extraction pipeline.** When processing inbox items via Claude Code, extract entity-relationship triples and store as structured frontmatter. This is the bridge between markdown and graph queryability.

**For work-agent-harness specifically (filesystem only):**

6. **An entity index as a generated file.** A script (shipped with the harness) that walks all markdown files, extracts `related_entities` from frontmatter, and generates a flat `ENTITY_INDEX.md` listing all entities, their types, and which files reference them. This gives any agent a "phone book" of the knowledge base via a single `cat` command. Regenerated automatically, not manually maintained.

7. **Document the graduation path.** The filesystem-only baseline supports entity references in frontmatter and grep-based relationship queries. Organizations with more infrastructure can layer on embedding search (tier 2) or a graph database (tier 3). The frontmatter conventions are designed to be useful at tier 1 and *better* at higher tiers — structured data that can be ingested by Neo4j later without reformatting.

---

## Part 3: How These Two Concepts Connect — And the Filesystem Constraint

Progressive disclosure and context graphs are complementary, not competing:

- **Progressive disclosure** solves the *delivery* problem: how to get the right amount of context to the agent at the right time, respecting token budgets.
- **Context graphs** solve the *navigation* problem: how to find relevant information when the answer requires connecting multiple entities and relationships.

In a mature system, they work together:
1. Agent receives a question
2. **Graph traversal** identifies the relevant entities and relationships (navigating the knowledge structure)
3. **Progressive disclosure** delivers that information in layers (index → summary → detail), respecting the agent's attention budget

### The Filesystem-Only Insight

The Letta Context Repositories work validates a crucial architectural principle: **well-structured markdown in a git repo, navigated with filesystem primitives, is a legitimate retrieval system** — not just a stopgap until you get "real" infrastructure. The filesystem *is* the database. Directory hierarchy is the index. Frontmatter is the metadata layer. Git history is the temporal layer. `grep` is the query engine.

This has implications for both your systems:

**For home-brain:** Brainstem's semantic search is a genuine enhancement layer, but the underlying repo should work well *without* it. If brainstem went down, an agent with filesystem access to the repo should still be able to navigate, discover, and retrieve effectively. This means the filesystem-level conventions (directory structure, frontmatter, READMEs) should be treated as the primary retrieval architecture, with brainstem as acceleration — not a crutch.

**For work-agent-harness:** This is liberating. You don't need to wait for IT to approve an MCP server or embedding database. The pattern works today, in any agent that can read files. The graduation path to richer retrieval (embedding search, graph databases) is available when the organization is ready, but the baseline is fully functional with zero infrastructure beyond a git repo.

### The Priority Stack

For your current stage:
1. **Progressive disclosure via filesystem conventions** — highest leverage, immediately actionable, works everywhere
2. **Structured frontmatter for proto-graph capabilities** — low effort, creates queryable relationships without infrastructure
3. **Agent-managed maintenance** (Letta defragmentation pattern) — medium effort, solves the staleness problem
4. **Brainstem/MCP enhancement for home-brain** — already exists, worth optimizing
5. **Formal context graph** — future consideration when scale demands it

---

## Sources & Further Reading

- Anthropic, "Effective Context Engineering for AI Agents" (Sep 2025) — the foundational framing
  https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Claude-Mem docs on progressive disclosure — the clearest implementation spec
  https://docs.claude-mem.ai/progressive-disclosure
- Inferable.ai, "Progressive Context Enrichment for LLMs" — production findings on attention degradation
  https://www.inferable.ai/blog/posts/llm-progressive-context-encrichment
- Will Larson, "Building an internal agent: Progressive disclosure and handling large files" (Dec 2025)
  https://lethain.com/agents-large-files/
- MCPJam, "Progressive Disclosure Might Replace MCP" — tradeoff analysis, Strata hybrid pattern
  https://www.mcpjam.com/blog/claude-agent-skills
- Letta, "Introducing Context Repositories: Git-based Memory for Coding Agents" (Feb 2026)
  — filesystem-only progressive disclosure, agent-managed memory, git-as-temporal-layer
  https://www.letta.com/blog/context-repositories
- Prakash Kukanoor, "Progressive Disclosure for Knowledge Discovery in Agentic Workflows" (Medium, Dec 2025)
  — detailed MCP tool design for layered document retrieval
- Zep/Graphiti paper: "A Temporal Knowledge Graph Architecture for Agent Memory" (arxiv 2501.13956)
  https://arxiv.org/abs/2501.13956
- Graphiti GitHub — the reference open-source temporal knowledge graph
  https://github.com/getzep/graphiti
- Adnan Masood, "Context Graphs: A Practical Guide" (Medium, Jan 2026)
  — architectural patterns for governed context graphs
- Glean, "How Knowledge Graphs Work" — enterprise knowledge graph + personal graph concepts
  https://www.glean.com/blog/knowledge-graph-agentic-engine

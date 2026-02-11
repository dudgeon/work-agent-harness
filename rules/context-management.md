# Fact Retention & Context Management

Logic for detecting, routing, and storing important facts. This is working logic — preserve it exactly.

---

## Core Principle

**ALWAYS retain important facts mentioned in conversation and store them in appropriate context locations.**

When the user mentions important information (names, dates, preferences, relationships, decisions, etc.), assess importance and retention needs.

---

## Retention Process

### 1. Detect Important Facts

When user mentions information, assess if it's important using these criteria:
- **Identity information**: Names, roles, relationships, team structures
- **Dates & milestones**: Deadlines, launch dates, review cycles, key events
- **Preferences & patterns**: Working style, communication preferences, decision-making patterns
- **Stakeholder details**: Who cares about what, reporting lines, influence dynamics
- **Goals & priorities**: Stated objectives, OKRs, strategic direction
- **Constraints**: Budget limits, headcount, technical limitations, compliance requirements
- **Relationships**: Connections between people, projects, teams, and concepts
- **Changes**: New information that updates previous understanding
- **Decisions**: Outcomes of discussions, rationale, who decided, what was considered

### 2. Determine Retention Location

Store facts in the appropriate location:
- **Domain-specific facts** → relevant domain file or domain README
- **Cross-domain facts** → `context/` files (company info, stakeholder map, etc.)
- **Task-related facts** → `tasks.md` or relevant project file
- **Process/pattern facts** → relevant rule or skill file

Context files in `context/` are created as needed during deployment personalization. Common examples:
- `context/company-info.md` — Company structure, strategy, key facts
- `context/stakeholder-map.md` — People, roles, relationships, preferences
- `context/team.md` — Direct team details, working patterns

**Don't create context files preemptively.** Let them emerge from actual content.

### 3. Update Context Immediately

- Don't wait to batch updates — update context files as facts emerge
- Mark updates with date: `*Updated: YYYY-MM-DD (brief description of change)*`
- If creating new sections, maintain consistent format
- Cross-reference between files when appropriate

### 4. Handle Conflicting Information

If new information conflicts with existing context:
- **STOP and ask a clarification question** before proceeding
- Example: "The stakeholder map says Alex leads Platform, but you just mentioned they moved to Product. Can you clarify?"
- Never silently overwrite potentially correct information
- Document resolution: `*Corrected: YYYY-MM-DD (was X, now confirmed as Y)*`

---

## Examples of Important Facts to Retain

**DO Retain:**
- "Our Q3 priority is platform migration" → company context or relevant domain
- "Sarah prefers async updates over meetings" → stakeholder map
- "The board reviews strategy quarterly in March, June, Sept, Dec" → company context
- "We can't use vendor X due to compliance" → relevant domain or company context
- "The team decided to use approach Y because Z" → decision record in relevant domain

**DON'T Retain:**
- Trivial ephemeral details ("I'm in a meeting right now")
- One-time logistical minutiae ("the doc is on page 3")
- General conversation filler
- Obvious facts already well-documented

---

## Context Enrichment During Conversations

When user naturally mentions information that fills gaps:
1. Recognize it relates to an existing context area
2. Update the appropriate context file
3. Add new follow-up questions that emerge from the new information

---

## Context Gathering — When New Areas Open

When conversation opens a new context area:
1. Recognize the gap (e.g., "competitive landscape mentioned but no details captured")
2. Note follow-up questions that would improve future assistance
3. Reference during idle time or when user asks "what else do you need?"

Indicators that a new area opened:
- User mentions a domain or topic with insufficient current context
- You have questions but don't want to interrupt flow
- You identify gaps that would improve future assistance

---

*When uncertain whether something is worth retaining, err on the side of capturing it. It's easier to prune than to reconstruct.*

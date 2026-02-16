# Decision Documentation

How to detect, discuss, and document significant decisions.

---

## Core Principle

**Decisions should be documented as they're made, not after the fact.**

When significant decisions are being made, the agent should proactively identify them and offer to document them. But documentation comes *after* the conversation settles on an approach — not before.

---

## What Qualifies as a Decision?

A decision is worth documenting if it meets **2 or more** of these criteria:

1. **Affects structure** — Changes how things are organized or architected
2. **Has alternatives** — Multiple viable approaches were considered
3. **Future impact** — Will affect future work or constrain future options
4. **Non-obvious** — The reasoning isn't immediately apparent
5. **Precedent-setting** — Could apply to similar situations later
6. **Reversibility cost** — Would take significant effort to undo
7. **Learning value** — Could benefit future review or other team members

---

## Decision Levels

**Major** (must document):
- Architecture changes, major tool/vendor adoption
- Strategic direction shifts, prioritization framework changes
- Cross-domain decisions affecting multiple work areas

**Medium** (should document):
- Process definitions, conventions, workflow designs
- Project approach choices, resource allocation
- Significant trade-offs with lasting consequences

**Minor** (optional):
- Use a lightweight format if beneficial
- Otherwise skip — avoid decision fatigue

---

## Where Decisions Live

- **Domain-specific decisions** → within the owning domain (e.g., `domains/platform-strategy/decisions/`)
- **Harness/framework decisions** → `meta/decisions/` (architectural choices about the harness itself)
- **Cross-domain decisions** → in the most relevant domain, with links from related domains

---

## The Conversational-First Pattern

**Work it out in conversation, then document the thinking after.**

1. Discuss options and trade-offs conversationally first
2. Make the decision through natural dialogue
3. THEN create documentation once the decision is made

**Don't:**
- Create big decision briefs upfront that need approval
- Present formal decision documents before discussing
- Over-formalize the exploration phase

**Do:**
- Discuss options naturally in conversation
- Once decided, THEN document comprehensively
- Make documentation thorough but only AFTER decision is made

---

## Agent Detection Process

**During conversations:**

1. **Listen for decision indicators:**
   - "Should we..." or "How should we..."
   - Multiple options being discussed
   - Trade-offs being evaluated
   - Rejecting one approach for another

2. **Recognize the decision point:**
   - Pause and assess: does it meet 2+ of the 7 criteria?
   - Determine level: Major, Medium, or Minor

3. **Surface the decision conversationally:**
   - For **Major**: "This seems like an important decision. Let's talk through the options."
   - For **Medium**: "I see a few different ways to approach this..."
   - For **Minor**: Don't interrupt flow

4. **Document AFTER decision is made:**
   - Once decided: "That makes sense. Let me document this decision."
   - Include enough context for future review
   - Document comprehensively, but only after the conversation settles

5. **Offer timeline entry:**
   - After documenting the decision, offer to add a timeline entry linking to the decision record
   - Decisions that shape project direction are always changelog-worthy (per [changelog-awareness.md](changelog-awareness.md))
   - Brief prompt: "Should I add this decision to [project]'s timeline?"

---

## Decision Document Structure

```markdown
# Decision: [Title]

**Date**: YYYY-MM-DD
**Status**: Proposed | Decided | Implemented | Validated | Superseded
**Domain**: [which domain this belongs to]

## Context

What prompted this decision. What problem or question we faced.

## Options Considered

### Option A: [Name]
- Pros: ...
- Cons: ...

### Option B: [Name]
- Pros: ...
- Cons: ...

## Decision

What we decided and why.

## Consequences

What this enables, what it constrains, what to watch for.
```

For minor decisions, a lightweight version:

```markdown
# Decision: [Title]

**Date**: YYYY-MM-DD | **Status**: Decided

**Context**: [One sentence]
**Decision**: [One sentence]
**Why**: [One sentence]
```

---

## Examples

**DO Document:**
- "Should we build this integration in-house or buy a vendor solution?"
- "How should we structure the product org for the new initiative?"
- "Should we migrate to the new platform now or after Q3 launch?"
- "What prioritization framework should we use for the roadmap?"

**DON'T Document:**
- "Should this note go in strategy or product-ops?" (too small)
- "Should we use bullets or numbers here?" (trivial)
- One-off logistical choices unlikely to recur

---

## Keeping Decisions Visible

- Link decision documents in relevant domain READMEs
- Reference decisions in related notes: "See decision: [title](path/to/decision.md)"
- When a decision is proven wrong, document why and what replaced it (set status to Superseded)

---

*Decisions are living documents. Update status as you learn. The goal is a record of reasoning, not a bureaucratic artifact.*

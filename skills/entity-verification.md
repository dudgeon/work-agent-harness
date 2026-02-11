---
name: entity-verification
description: Verify and resolve vague entities before capturing. Use when encountering approximate names (companies, people, products, tools, frameworks) or links that contain canonical names. Research before asking the user.
---

# Entity Verification

Resolving vague or approximate references to their canonical form before storing them in the knowledge base. This is a **utility skill** — called by other skills (inbox triage, stakeholder intelligence, source processing) whenever they encounter entities that need resolution.

---

## When This Applies

- Any skill encounters a vague entity (company name, person, product, tool, framework)
- User provides a link that likely contains the canonical name
- User uses shorthand or approximate names for things that have official names
- Processing unstructured input with names that need disambiguation
- Updating a stakeholder or context file with a new entity reference

---

## The Goal

Act like a chief of staff: research and verify facts from primary or secondary sources, but never assume or hallucinate. When the user provides a link or reference, do the legwork to find the actual name. If verification fails, ask — don't guess.

---

## Resolution Order

When resolving an entity, check these in order:

1. **Context files** — `context/*.md` (company info, stakeholder map, team details)
2. **Domain files** — relevant domain notes, project files, concept files
3. **Web research** — search to confirm details, find canonical names, get links
4. **Ask the user** — only after exhausting the above

---

## How to Think About It

**Links are verification opportunities.** When the user provides a URL, the source likely contains the canonical name. Fetch and verify before capturing.

**Do the research before asking.** Don't immediately ask the user when direct fetch fails. Try multiple approaches:
1. Fetch the URL directly
2. If blocked, try alternative access methods (different URL format, cached version)
3. Search for the URL content via web search
4. Search for the vague description with qualifying terms
5. Cross-reference results to confirm identity

**Ask only as a last resort.** After exhausting research options, then ask the user for the actual name.

**Specificity matters for retrieval.** Vague names make notes harder to find later. "The analytics tool" should become "Amplitude" (with link). "Sarah's manager" should become "Sarah Chen's manager, David Park (VP Engineering)."

---

## What to Verify

Prioritize verification for:
- **People**: Full names, titles, roles, organizations
- **Companies/organizations**: Official name, not shorthand
- **Products/tools**: Canonical product name, not colloquial reference
- **Frameworks/methodologies**: Correct name and attribution
- **Events/conferences**: Official name, dates, location

Don't over-verify:
- Common, unambiguous references ("Google", "Slack")
- Internal shorthand the user has established and consistently uses
- Entities already verified and stored in context files

---

## Watch Out For

**Hallucinating "better" names.** Never invent a more specific or formal-sounding name. An unverified upgrade is worse than a vague original. If you can't verify, store what you have and flag it.

**Capturing before verifying.** Take the extra step to check the source, especially for people's full names and titles, company/organization official names, product names (especially similar-sounding ones), and framework attributions.

**Giving up too early.** Web searches, cached pages, and indirect lookups often work when direct access fails. Exhaust your tools.

**Assuming entity identity.** "Jordan" could be multiple people. When a name is ambiguous against existing entities, ask for disambiguation rather than guessing.

---

## After Verification

When you've resolved an entity:
1. Use the canonical name in the note being created
2. Add a link if one was found (company website, product page, LinkedIn, etc.)
3. If this is a recurring entity, consider whether it belongs in a context file for future resolution
4. Note the verification in your work — "Confirmed: [vague reference] is [canonical name]"

---

*This skill is called by other skills, not typically invoked directly. If you're doing entity resolution as part of inbox triage, stakeholder updates, or source processing, you're already using this skill.*

---
name: stakeholder-intelligence
description: Handle stakeholder profiles, multi-person interaction updates, and relationship tracking. Use when processing interactions involving multiple people, updating stakeholder profiles, onboarding new contacts, or managing the stakeholder map.
---

# Stakeholder Intelligence

Tracking the people in the user's professional world — who they are, how they relate, what they care about, and how interactions update the picture over time.

---

## When This Applies

- Processing input that mentions people (meeting notes, conversations, email context)
- User introduces a new person the system hasn't seen before
- An interaction involves multiple people and each profile needs updating
- User asks to review or update stakeholder context
- Inbox triage encounters person-related content

---

## The Goal

A stakeholder picture that helps the agent be useful: knowing who people are, what they care about, how decisions flow through them, and how to communicate effectively about or with them. Not a CRM — a working model of the user's professional relationships.

---

## Relationship Proximity

People in the user's professional world exist at different distances and need different levels of tracking. Rather than rigid tiers, think of proximity as a spectrum:

**Close collaborators** — people the user works with frequently, knows well, and needs deep context on. Daily or weekly interaction. The agent should build rich profiles over time.

**Regular contacts** — people the user interacts with regularly but less closely. The agent needs to know their role, what they own, and how they prefer to receive information.

**Extended network** — people the user encounters occasionally or has heard about. Lightweight entries — role, org, last interaction, anything notable.

### When Encountering a New Person

**Don't assume the user remembers this model.** When a new person comes up, ask clarifying questions using plain language:

> "I haven't seen [name] before. A few quick questions so I know how to track them:"
>
> 1. **How often do you work with them?** (daily/weekly, regularly but less often, occasionally/rarely)
> 2. **What's their role?** (title, team, what they own)
> 3. **Anything I should know about working with them?** (communication preferences, decision authority, what they care about)

Based on the answers, decide:
- **Close collaborator** → create an individual profile file in the relevant domain
- **Regular contact** → create an individual profile, or add a substantial entry in the stakeholder map — use judgment based on how much context exists
- **Extended network** → add to the stakeholder map with a lightweight entry

If in doubt, **propose a choice** rather than deciding silently:

> "Should I create a dedicated profile for [name], or just add them to the stakeholder map for now?"

---

## Where People Live

### The Stakeholder Map (`context/stakeholder-map.md`)

The cross-domain index of professional relationships. Everyone should be discoverable here, even if they also have individual files. The stakeholder map contains:
- Organizational structure (teams, reporting lines)
- Lightweight entries for extended network contacts
- Links to individual profile files for people with deeper tracking

### Individual Profile Files

For close collaborators and key regular contacts. Live in the relevant domain (e.g., a product partner lives in the product domain, an engineering lead in the platform domain). Structure:

```markdown
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [stakeholder, domain]
status: active
---

# [Full Name]

> [Title] — [Team/Org]

## Profile

- **Role**: What they do, what they own
- **Reports to**: Their manager (if known)
- **Decision authority**: What they can approve or block
- **Communication style**: How they prefer to receive information (async/sync, detail level, format)
- **What they care about**: Their priorities, what gets their attention, what frustrates them
- **Working relationship**: How you interact, shared context, any dynamics to be aware of

## Timeline

### YYYY-MM-DD — [Interaction or Update]

What happened, what was discussed, any commitments or follow-ups.
```

---

## Multi-Person Interactions

When processing input that involves multiple people (meeting notes, group discussions, Slack threads):

1. **Identify all people mentioned** who are tracked in the system
2. **Update each person's profile** with relevant information from the interaction — what they said, positions they took, commitments they made
3. **Customize per person** — the same meeting produces different updates for different profiles. The CEO's profile gets "expressed concern about timeline" while the eng lead's gets "committed to reducing scope by 2 sprints."
4. **Create profiles** for anyone mentioned who isn't tracked yet (use the new-person questions above)
5. **Update the stakeholder map** if relationships, roles, or org structure changed

---

## Enrichment Checklist

When adding information to a stakeholder profile, check:

1. **Timeline entry** — Is there a date-anchored event to record?
2. **Profile updates** — Did this reveal new evergreen info (role change, new responsibility, communication preference)?
3. **Cross-references** — Does this connect to other people, projects, or domains? Add links.
4. **Stakeholder map** — Does the map need updating (new person, role change, org restructure)?
5. **Context files** — Are there cross-domain facts that belong in `context/` rather than a single profile?

---

## Detecting Relationship Changes

Watch for signals that someone's proximity is shifting:
- User starts mentioning someone much more frequently → may need an individual profile
- Someone gets a new role or responsibility that affects the user's work
- A close collaborator leaves or moves teams → update profile status, note in timeline
- Someone the user barely knew becomes important to a project → upgrade tracking

When you notice a shift, surface it:

> "[Name] has come up in 3 conversations this week. They're currently just in the stakeholder map — should I create a full profile for them?"

---

## Watch Out For

**File sprawl.** Not everyone needs an individual file. Use the stakeholder map for lightweight tracking and only create files when there's enough context to justify it.

**Stale profiles.** People change roles, leave, move teams. When you notice outdated information, flag it for update.

**Assuming org knowledge.** Don't assume the user knows everyone's reporting line or decision authority. When context is missing, ask — it's useful information to capture.

**One-sided updates.** When a meeting involves 4 people, all 4 profiles need updating, not just the "main" person. This is the most common mistake.

**Sensitivity.** Stakeholder profiles may contain candid observations about communication style or dynamics. This is valuable working context, but keep the tone professional and factual. Avoid characterizations that would be embarrassing if read by the subject.

---

*Stakeholder intelligence is a living picture, not a database. It should get richer over time through natural interactions, not bulk data entry.*

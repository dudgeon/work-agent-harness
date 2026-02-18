---
name: onboarding
description: Guided harness personalization for new users. Walk through company, team, stakeholders, work domains, inbox workflow, and external repo linking to create context files. Explains key concepts along the way. Re-runnable — detects existing context and extends it.
---

# Onboarding

Guided setup that personalizes the harness for a real user. Conversational, progressive, and educational — the user doesn't need to understand the system's ontology upfront. The skill explains concepts as they become relevant.

---

## When This Applies

- First-time harness setup (no context files exist yet)
- User wants to add or update context about themselves, company, team, or domains
- User invokes `/onboarding`

---

## The Goal

A personalized harness with enough context that the agent can be genuinely helpful from the next session forward. The user should finish onboarding understanding:

- What domains are and how their knowledge will be organized
- What lives inside a domain (projects, concepts, timelines)
- How the inbox works as a capture-and-route mechanism
- Who the key people are in their professional world
- How the agent will use all of this to assist them

---

## Before You Start

Check for existing context files:

```
context/company-info.md
context/stakeholder-map.md
context/team.md
context/external-repos.md
domains/*/README.md
```

**If files exist (rerun):**
- Summarize what's already captured: "Here's what I know so far — [brief summary]. We can update any of this or add new information."
- Offer to run specific phases rather than the full flow
- When asking questions, pre-fill with existing data so the user can confirm or correct

**If no files exist (first run):**
- Start from Phase 1 and work through sequentially

---

## Phases

Run phases in order on first setup. On reruns, let the user pick which phases to revisit.

### Phase 1: About You

**What we're building:** `context/team.md` — your role, team, and responsibilities.

**Questions to ask (conversationally, not as a form):**

1. What's your role? What does your title actually mean in practice?
2. What team are you on? Who do you report to?
3. What are you responsible for day-to-day? What does a typical week look like?
4. What are your current top priorities (this quarter or this month)?

**What to capture:**
- Role and title
- Team name and structure
- Manager name (add to stakeholder map in Phase 3)
- Key responsibilities
- Current priorities and goals

**Create** `context/team.md` with the information gathered. Use standard frontmatter (`created`, `updated`, `tags`, `status`).

---

### Phase 2: Your Company

**What we're building:** `context/company-info.md` — enough company context that the agent understands the environment you work in.

**Questions to ask:**

1. What company do you work for? What does it do?
2. How is the company structured? (divisions, business units, anything that helps the agent understand org context)
3. Are there key company-wide priorities or initiatives that affect your work?
4. Any important constraints the agent should know about? (compliance requirements, tooling restrictions, etc.)

**What to capture:**
- Company name, industry, size (approximate)
- Organizational structure (enough to understand reporting lines and decision flow)
- Strategic priorities relevant to the user's work
- Constraints and guardrails

**Create** `context/company-info.md`.

---

### Phase 3: Key People

**What we're building:** `context/stakeholder-map.md` — the people in your professional world.

**How to introduce this:**

> "Now let's talk about the people you work with most. I'll track key details about them — their roles, what they care about, how you interact — so I can help you prepare for meetings, draft communications, and keep stakeholder context fresh."

**Questions to ask:**

1. Who are the 3-5 people you interact with most frequently?
2. For each: What's their role? What do they care about? How do you typically interact? (meetings, Slack, email?)
3. Anyone else the agent should know about — skip-level leaders, key cross-functional partners, important external contacts?

**Processing:**
- Use the [stakeholder-intelligence](stakeholder-intelligence.md) skill's proximity model to place each person on the spectrum
- Create `context/stakeholder-map.md` with initial profiles
- Don't over-ask — 3-5 people is enough to start. More will emerge naturally from inbox triage

---

### Phase 4: Your Work Domains

This is the most conceptually new phase. Take time to explain.

**How to introduce domains:**

> "Now let's set up your knowledge areas — we call these **domains**. A domain is just an area of your work that has its own context, projects, and evolving knowledge. Think of it like a folder for everything related to one area of responsibility.
>
> For example, if you're a PM working on a platform migration while also managing a team and contributing to company strategy, you might have domains like:
> - `platform-migration/` — the project, its decisions, technical context, stakeholders
> - `team-management/` — your team's goals, 1:1 notes, hiring plans
> - `strategy/` — company strategy work, competitive landscape, planning docs
>
> Domains can nest — if platform migration has a frontend and backend workstream, those could be sub-domains inside it. Every level works the same way."

**Then explain what lives inside a domain:**

> "Inside each domain, you'll build up:
> - A **README** — the overview of what the domain is about, its current state, and key links
> - **Projects** — specific initiatives with goals, scope, tasks, and timelines (like a project brief that stays alive)
> - **Concepts** — reference notes about important topics, people-in-context, tools, or ideas (like a mini wiki entry)
> - A **Timeline** — significant events get logged automatically as you work, so you have a record for reviews and retrospectives
>
> You don't need to create all of this now. We'll set up the folders and READMEs, and the rest fills in as you work."

**Questions to ask:**

1. What are the main areas of your work right now? (Don't worry about naming conventions — just describe them.)
2. For each: What's the current focus? Any active projects or deadlines?
3. Are any of these areas related to each other? (Helps us think about nesting vs. cross-linking.)

**What to create:**
- For each domain the user identifies: create `domains/{domain-name}/README.md` using [templates/domain.md](../templates/domain.md)
- Fill in the Purpose and Current State sections from the conversation
- If the user mentions specific projects, note them in the README's Current State but don't create project files yet — those should emerge from real work
- Add domain tasks to `tasks.md` if the user mentions actionable items

---

### Phase 5: The Inbox and How Information Flows

**How to introduce the inbox:**

> "Last thing — let me explain how new information flows into your system, because this is where the day-to-day value comes from.
>
> You have an **inbox** — it's just a folder where you can dump anything: meeting notes, quick thoughts, voice memos, ideas, things you want to remember. You can drop files in directly, dictate into it, or just tell me something and I'll capture it.
>
> The inbox is **temporary** — nothing lives there permanently. When we process it, I'll:
> 1. Read each item and figure out what it's about
> 2. Identify any people, projects, or topics mentioned
> 3. Route the information to the right domain (or suggest creating a new one)
> 4. Enrich it — add context, links, and connections you might not think to add yourself
>
> Think of it like having an assistant who reads your messy notes and files everything in the right place, with useful annotations. You capture fast and loose; the system organizes."

**Questions to ask:**

1. How do you typically capture information today? (Notes app, email to self, voice memos, docs?)
2. Is there a regular cadence when new information piles up? (After meetings? End of day?)
3. Anything you'd like the agent to watch for specifically when processing your inbox?

**What to capture:**
- Add any preferences to `context/team.md` under a "Working Patterns" section
- If the user mentions a specific tool or capture method, note it so the agent can adapt

---

### Phase 6: External Repos (Optional)

Only offer this phase if the user created domains in Phase 4. Skip if no domains were created or if the user is clearly done.

**How to introduce external repos:**

> "One more thing — some of your domains might overlap with knowledge that could live in a shared or public repo. For example, if you have a domain about a technology or discipline, the general knowledge could live in a public GitHub repo (useful for your portfolio, community contributions, or team sharing), while your company-specific context stays private here.
>
> The harness supports this with **external repos** — you link a domain to a public or team Git repo, and the harness becomes a private overlay on top of it. The external repo is the source of record for shared knowledge; your harness adds company context, personal annotations, and unpublished drafts.
>
> You can also start a domain fully private and fork the public content out later when it's mature enough."

**Questions to ask:**

1. Do any of the domains we just set up have knowledge that could be shared publicly or with your team?
2. Are there existing public or team repos you already use as knowledge references? (GitHub repos, team wikis that could become repos, etc.)
3. For any domain you'd want to link: should we connect to an existing repo, or would you create one later?

**What to do based on answers:**

- **"Yes, link to existing repo"** → Clone the repo to `.repos/`, add `external_repo` frontmatter to the domain README, create `context/external-repos.md` from [templates/external-repos.md](../templates/external-repos.md), set up overlay directories (`_private/`, `annotations/`, `_staging/`)
- **"Yes, but I'll create the repo later"** → Note the intent in the domain README's Open Questions section. Add a task to `tasks.md`: "Create public repo for [domain] and configure external repo link"
- **"No, everything stays private"** → That's fine. Move on. This can always be revisited — domains can start private and externalize later.

**Don't over-sell.** External repos add workflow complexity. If the user doesn't have an immediate use case, don't push it. The option will be there when they need it.

---

## After Onboarding

### Clean up the first-run directive

If this is the first onboarding run, remove the onboarding block from `AGENTS.md`. The block is delimited by comment markers:

```
<!-- ONBOARDING: Remove this block after completing first onboarding run -->
> **First-time setup**: ...
<!-- /ONBOARDING -->
```

Remove everything from `<!-- ONBOARDING:` through `<!-- /ONBOARDING -->` inclusive (including any blank lines within). This is a mechanical cleanup — no user confirmation needed.

### Summarize and offer next steps

> "Here's what we set up:
> - **Context files**: [list files created]
> - **Domains**: [list domains created]
> - **Stakeholders**: [count] people captured
> - **External repos**: [list any linked repos, or "none yet — you can link repos to domains anytime"]
>
> From here, the system will grow as you work. Drop things in the inbox, and I'll route them. Mention new people, and I'll add them. Start working in a domain, and the timeline will build itself.
>
> Want to try dropping something in the inbox now to see the flow in action?"

---

## Tone and Approach

- **Conversational, not interrogative.** This isn't a form — it's a discussion. React to answers, ask follow-ups, connect dots.
- **Explain concepts when they first become relevant**, not all upfront. Don't front-load a system overview.
- **Use the user's language.** If they say "workstream" instead of "domain," use their term in conversation (but use the system term in file names).
- **Don't over-collect.** 80% of context will emerge from real work. Onboarding captures enough to be useful, not everything.
- **Be honest about what's minimal.** "We can start with just 2-3 domains — more will emerge as you work" is better than trying to map everything on day one.

---

## Watch Out For

- **Over-engineering domains on day one.** Users will try to create a perfect taxonomy. Resist — start simple, let structure emerge.
- **Asking too many questions per phase.** If the user gives short answers, that's fine. Move on and let context accumulate naturally.
- **Jargon without explanation.** Never say "frontmatter," "fractal hierarchy," or "concept file" without explaining what it means in plain terms.
- **Creating empty domains.** Every domain created during onboarding should have at least a Purpose and Current State in its README. If the user can't describe it, it's not ready to be a domain yet.
- **Forgetting to cross-link.** When creating stakeholder-map entries, link to relevant domains. When creating domain READMEs, link to relevant stakeholders.

---

*Onboarding captures the starting context. The real value builds over time through daily use.*

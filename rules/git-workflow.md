# Git Workflow — Caddy Mode

The agent acts as a git caddy: handling version control proactively, explaining in plain language, and preventing the user from losing work or getting confused. Most users will not know git terminology or when to take git actions — the agent absorbs this complexity entirely.

---

## Core Posture

**The user should never need to think about git.** Treat version control the way autosave treats file saving — it should just happen at the right moments, with clear explanation when the user needs to understand what's going on.

- **Never use git jargon without a plain-language gloss.** Say "I'll save a snapshot of your work" not "I'll commit." If you must use a term like "branch" or "merge," follow it immediately with what it means in context.
- **Handle git operations yourself.** Don't ask the user to run git commands. Don't suggest they open a terminal.
- **Explain *why*, not *how*.** The user doesn't need to know the command — they need to know why you're doing it and what it means for their work.

---

## When to Save Snapshots (Commits)

Commit at natural checkpoints — the goal is frequent enough to be safe, infrequent enough to be meaningful:

- **After completing a distinct piece of work** — a new note, a reorganized domain, a batch of inbox items processed
- **Before doing anything risky or hard to undo** — major reorganizations, bulk renames, structural changes
- **At the end of a session** — never leave unsaved work behind
- **When the user asks to "save" or "checkpoint"** — treat these as commit requests

**Commit messages should read like a changelog entry**, not a git log. Write for someone skimming history months later:
- Good: "Add competitive landscape domain with initial company profiles"
- Good: "Process inbox: route 4 items to strategy domain, archive 2"
- Bad: "update files"
- Bad: "WIP"

---

## Branch Awareness

**Proactively detect and prevent branch confusion:**

- At session start, note which branch you're on. If it's not what you'd expect for the current work, flag it plainly: *"We're currently on a side branch called X — do you want to switch back to the main line before we start?"*
- If the user's request would make more sense on a different branch, say so before acting.
- Never silently switch branches. Always explain: *"I'm going to create a separate workspace for this experiment so it doesn't affect your main notes."*

**Keep it simple by default.** Most knowledge work should happen on `main`. Only suggest branches when there's a clear reason (experimental changes, parallel workstreams, review workflows).

---

## Syncing with Remote (Fetch/Push/Pull)

**The agent handles remote sync and translates what's happening:**

- **Before starting work**, check if there are remote changes and pull them in. If there's a conflict, explain it plainly: *"Someone (or another session) changed the same file you're working on. Here's what's different — which version do you want to keep?"*
- **After meaningful work**, offer to push: *"Your changes are saved locally. Want me to sync them to GitHub so they're backed up and visible to others?"*
- **Never push without asking.** Pushing is visible to others — always confirm.
- **If a push fails**, diagnose and explain. Don't retry blindly. Common causes: someone else pushed changes (need to pull first), permissions, network issues.

---

## When Things Go Wrong

Git problems are inevitable. The agent's job is to **diagnose, explain plainly, and fix** — never to panic or dump error messages on the user.

**Common situations and how to handle them:**

- **Merge conflict**: *"Two versions of [file] have different changes. Here's what each version says — which do you want to keep, or should I combine them?"* Resolve it yourself when the right answer is obvious; ask when it's ambiguous.
- **Detached HEAD / wrong branch**: Detect this silently and fix it. Explain after: *"You were in a weird state — I've moved you back to your main workspace. No work was lost."*
- **Uncommitted changes blocking an operation**: Stash or commit them, explain what you did: *"I saved your in-progress work as a snapshot before switching, so nothing is lost."*
- **Accidentally deleted or overwritten file**: Check if git can recover it. Explain: *"I was able to restore [file] from the last saved snapshot."*

**Never run destructive git commands** (force push, hard reset, clean) without explicit user approval and a plain explanation of what will be lost.

---

## What to Say (and Not Say)

| Instead of... | Say... |
|---|---|
| "I'll commit these changes" | "I'll save a snapshot of this work" |
| "Let me push to origin" | "I'll sync your changes to GitHub so they're backed up" |
| "There's a merge conflict" | "Two versions of this file have different changes — let me show you" |
| "You're on a detached HEAD" | "You're in a temporary state — let me get you back to your main workspace" |
| "I'll stash your changes" | "I'll set aside your in-progress work temporarily so we can do X, then bring it back" |
| "Let me rebase" | "I'll update your work to include the latest changes from the main line" |
| "The remote is ahead" | "There are newer changes on GitHub — I'll pull those in first" |

Use git terms only when the user has demonstrated they understand them, or when you need to be precise for a technical reason (and gloss it).

---

## Session Boundaries

**Session start:**
1. Check which branch you're on — flag if unexpected
2. Check for uncommitted work from a previous session — commit or flag it
3. Fetch remote changes — pull if clean, flag if conflicts

**Session end:**
1. Commit any uncommitted work with a clear summary message
2. Offer to push if there are unpushed snapshots
3. If leaving work in progress, note it: *"You have unsaved in-progress work on [topic] — I've saved a snapshot so you can pick up where you left off."*

---

*The measure of good git caddying is that the user never loses work and never feels confused about the state of their files. Git should be invisible infrastructure, not a source of anxiety.*

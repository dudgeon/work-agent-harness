---
type: template
version: 3
description: Template for domain README.md files. Every domain (and sub-domain) should have a README following this structure.
changelog: |
  v3 (2026-02-18): Added optional External Repo section and overlay structure for domains linked to public/team repos
  v2 (2026-02-15): Added Timeline section for domain-level events
  v1 (2026-02-09): Initial template
---

<!--
  Frontmatter for a domain README.md:

  Required:
    created: YYYY-MM-DD
    updated: YYYY-MM-DD
    template: templates/domain.md
    template_version: 3
    status: active

  Optional (when domain links to an external repo):
    external_repo: https://github.com/org/repo-name
    external_repo_path: .repos/repo-name
    external_repo_type: public|team
-->

# [Domain Name]

> Brief description of what this domain covers and why it exists.

---

## Purpose

What this domain is about. What kinds of knowledge live here. How it relates to other domains.

## Current State

Summary of what's here now — key files, active projects, recent developments.

## Key Concepts

Links to the most important files in this domain:

- [concept-name](concept-name.md) — brief description
- [another-concept](another-concept.md) — brief description

## Sub-Domains

If this domain has subfolders:

- [sub-domain-name/](sub-domain-name/) — what it covers

## Open Questions

Unresolved topics, areas needing exploration, decisions pending.

## Related

- [Related domain](../related-domain/) — how they connect
- [Context file](../../context/relevant-context.md) — cross-domain context

---

<!-- EXTERNAL REPO SECTION — Include this section only when the domain links to an external repo
     (external_repo is set in frontmatter). Remove this comment block and uncomment the section below.

## External Repo

This domain augments the [Repo Name](https://github.com/org/repo-name) [public|team] repo, which is the source of record for shared knowledge.

- **External repo** (SOR for shared knowledge): `.repos/repo-name/`
- **This domain** (private overlay): company-specific context, annotations, unpublished drafts

### What goes where

| Content type | Destination | Example |
|---|---|---|
| Generally applicable knowledge | External repo (via PR) | Reusable technique or framework |
| Company/context-specific knowledge | `_private/` | How we apply technique X at our company |
| Private notes on external content | `annotations/` | Reading notes, internal commentary |
| Drafts being prepared for contribution | `_staging/` | Content awaiting batch PR |

### Domain structure

    _private/       — Company/personal content (never leaves WAH)
    annotations/    — Private notes on external repo content
    _staging/       — Content being prepared for contribution back

/EXTERNAL REPO SECTION -->

---

## Timeline

<!-- Domain-level events that matter across the domain, not specific to any one project.
     Project-specific events go in project files.

     Format:
     ### YYYY-MM-DD — Summary phrase [type]
     - Detail bullets
     - Links: [related files or contexts](path)

     Types: [milestone], [progress], [decision], [win], [learning], [blocker-resolved], [scope-change]
-->

---

*Last reviewed: YYYY-MM-DD*

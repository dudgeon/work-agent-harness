---
type: template
version: 1
description: Template for context/external-repos.md — registry of external Git repos this harness interacts with.
changelog: |
  v1 (2026-02-18): Initial template
---

<!--
  This file lives at context/external-repos.md and tracks all external
  repos the harness interacts with (public knowledge repos, team repos).

  The agent reads this at session start to know which clones exist and
  which domains they link to.
-->

# External Repos

External Git repositories this harness interacts with. Each is cloned to `.repos/` and linked to a WAH domain that provides the private overlay.

For full design details, see [meta/specs/public-repo-design.md](../meta/specs/public-repo-design.md).

---

## Active

| Repo | Type | Local Path | Remote URL | Linked Domain | Last Fetched |
|------|------|-----------|------------|---------------|--------------|
<!-- | repo-name | public | .repos/repo-name | github.com/org/repo-name | [domain](../domains/domain-name/) | YYYY-MM-DD | -->

## Setup

To add a new external repo:

1. Clone it: `git clone <url> .repos/<name>`
2. Create or update the linked WAH domain — add `external_repo`, `external_repo_path`, `external_repo_type` to domain README frontmatter
3. Add a row to the Active table above
4. Set up the overlay structure in the WAH domain: `_private/`, `annotations/`, `_staging/`

To contribute back:

1. Draft content in the domain's `_staging/` directory
2. When ready, the agent will scrub for private content, prepare a batch, and create a PR on the external repo
3. Review the PR for privacy before merging

---

*This file is the agent's lookup table for external repos. Keep it current when adding or removing repos.*

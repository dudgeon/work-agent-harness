---
name: template-propagation
description: Update documents when templates change. Use when a template has structural changes, user asks to "propagate template changes", or documents have outdated template_version.
---

# Template Propagation

Updating derived documents when a template's structure changes, while preserving their content.

---

## When This Applies

- A template in `templates/` has been updated with structural changes
- User asks to "propagate template changes" or "update docs from template"
- You notice documents with outdated `template_version` during other work

---

## The Goal

Systematically update documents derived from a template to match the new structure, while preserving all existing content. No content loss, no silent changes.

---

## How It Works

Templates track their version:

```yaml
# In the template file
---
type: template
version: 3
changelog: |
  v3 (2026-02-15): Added Related section
  v2 (2026-02-10): Split Overview into Key Aspects and Relationships
  v1 (2026-02-09): Initial template
---
```

Derived documents track which template and version they were created from:

```yaml
# In a document created from the template
---
template: templates/concept.md
template_version: 2
---
```

When the template version is higher than the document's `template_version`, the document is a candidate for propagation.

---

## How to Think About It

**Template changes come in different flavors:**

1. **Additive** (new sections): Add the new sections to derived docs. Safest — no content disruption.
2. **Structural** (reorganization): Move content from old structure to new locations. Requires reading existing content to map correctly.
3. **Clarifying** (better prompts/labels): Update labels but keep content. Usually straightforward.
4. **Subtractive** (removed sections): Decide whether to delete or archive removed content. **Always ask the user** before removing content from derived docs.

**Not all docs need the same treatment.** Some documents may have intentionally diverged from the template. Reference documents (`type: reference`) are always exempt.

---

## The Process

### 1. Identify Affected Documents

Search for all documents with:
- `template:` matching the changed template path
- `template_version:` lower than the template's current `version`

### 2. Understand the Changes

Read the template's `changelog`. For each version increment between the document's version and the current version, categorize changes as additive, structural, clarifying, or subtractive.

### 3. Plan the Update

For each affected document, determine what needs to be:
- **Added**: New sections from the template
- **Moved**: Content relocated to match new structure
- **Updated in place**: Labels, headings, or formatting changes
- **Reviewed by user**: Subtractive changes or ambiguous structural moves

### 4. Apply Updates

For each document:
1. Read the current content fully
2. Apply structural changes, preserving all existing content
3. Update `template_version` to the current version
4. Update the `updated` date
5. Do NOT delete any user content without explicit approval

### 5. Report Results

Summarize:
- How many documents were updated
- What structural changes were applied
- Any documents needing user review (subtractive changes, ambiguous content mapping)
- Any documents skipped (reference docs, intentionally diverged)

---

## Watch Out For

**Content loss.** Never delete user content without asking. When in doubt, keep it and flag for review.

**Over-automation.** Check whether content intentionally diverged from the template. A document that's evolved its own structure may not benefit from being forced back into template shape.

**Version gaps.** A document at version 1 being updated to version 4 may need incremental changes applied in order, not just a jump to the latest structure.

**Reference documents.** Documents with `type: reference` in frontmatter should always be skipped — they don't follow template structure.

**Scope creep.** Template propagation updates structure, not content. Don't rewrite the user's text to match template example language.

---

*Template propagation is maintenance, not creation. It should preserve and restructure, never overwrite or remove.*

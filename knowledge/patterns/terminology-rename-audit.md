---
created: 2026-01-19
last_verified: 2026-01-19
---

# Pattern: Terminology Rename Audit

## When to Apply

- Renaming a framework, methodology, or concept across a codebase
- Rebranding internal terminology while preserving external attribution
- Auditing completeness after a find-and-replace rename
- Extracting or forking a methodology with new naming
- Any rename that must distinguish internal usage from credited sources

## Why It Matters

Terminology renames are deceptively simple. A naive find-and-replace creates two failure modes:

1. **Incomplete internal references** - Old terminology persists in headers, folder names, or explanatory text, creating confusion about canonical naming
2. **Destroyed attribution** - External credits get renamed, erasing intellectual lineage and potentially violating attribution norms

Proper audit methodology distinguishes what SHOULD change (internal usage) from what MUST NOT change (external credits).

## The Pattern

### Classification Framework

Before any rename, classify all occurrences into categories:

| Category | Description | Action |
|----------|-------------|--------|
| **Internal Reference** | Your usage of the concept | Rename |
| **External Credit** | Attribution to original source | Preserve |
| **Quoted Content** | Direct quotes from sources | Preserve |
| **Explanatory Context** | Describing what you built upon | Rename usage, preserve source names |

### Multi-Pass Search Strategy

Execute searches in parallel to find all occurrences:

**Pass 1: Exact Term Match**
```
grep -ri "old term" --include="*.md" --include="*.swift"
```

**Pass 2: Case Variations**
- Title Case: "Old Term"
- lowercase: "old term"
- UPPERCASE: "OLD TERM"
- kebab-case: "old-term"
- snake_case: "old_term"

**Pass 3: Partial Matches**
- Root word only: "compound" (catches "compounding", "compounds")
- Without spaces: "OldTerm" (camelCase)

**Pass 4: Structural Elements**
- Folder names
- File names
- YAML frontmatter
- Markdown headers
- Link text and URLs

### Audit Checklist

For each occurrence found, answer:

- [ ] Is this an internal reference or external credit?
- [ ] If internal: Does renaming maintain meaning?
- [ ] If external: Is the source properly attributed elsewhere?
- [ ] For headers/titles: Does the section describe YOUR work or THEIR methodology?
- [ ] For links: Is this linking to YOUR docs or THEIR source?

### Credit Preservation Pattern

When renaming derived work, add explicit attribution:

```markdown
# [New Framework Name]

> **Credits:** [New Name] is inspired by [Original Work](link)
> by [Original Author], combined with [Other Sources] and extended with
> [your novel contributions].
```

This pattern:
1. Names your framework prominently
2. Credits sources explicitly with links
3. Distinguishes your extensions from the originals

## Example

From the Flywheel Framework rename (commit `e979272`):

**Renamed (internal references):**
- Folder: `Compound Engineering/` -> `Flywheel Framework/`
- Headers: `# Compound Engineering Methodology` -> `# Flywheel Framework Methodology`
- CLAUDE.md section: `## Compound Engineering Workflow` -> `## Flywheel Framework Workflow`
- File titles: `# Swift/macOS Compound Engineering Framework` -> `# Swift/macOS Flywheel Framework`

**Preserved (external credits):**
- Source link: `[Compound Engineering](https://every.to/...)`
- Attribution text: "**Compound Engineering** (from Every.to)"
- Credits block explaining what was combined

**Added (explicit attribution):**
```markdown
> **Credits:** Flywheel Framework is inspired by [Compound Engineering](...)
> (Every.to) and [Advanced Context Engineering](...) (HumanLayer),
> combined and extended with self-auditing rules and stack-agnostic design.
```

## Anti-Pattern

**Blind find-and-replace:**
```bash
sed -i 's/Compound Engineering/Flywheel Framework/g' **/*.md
```

**Problems:**
- Destroys attribution credits
- Renames quoted content
- May create grammatically incorrect text ("Flywheel Framework compounds over time")
- No audit trail of what was intentionally preserved

**Missing attribution after rename:**
Renaming without adding a credits section leaves no indication of intellectual lineage.

## Validation Checklist

After completing a terminology rename:

- [ ] All folder names use new terminology
- [ ] All file headers use new terminology
- [ ] Project instructions (CLAUDE.md) updated
- [ ] Search for old term returns only external credits/quotes
- [ ] Credits block exists with links to original sources
- [ ] Novel contributions are explicitly documented
- [ ] Commit message explains what changed vs. preserved

## Review Checklist for Future Renames

Add to review agent for documentation changes:

```markdown
## Terminology Rename Audit

When a terminology change is detected:
- [ ] Run multi-pass search for all variations
- [ ] Classify each occurrence (internal vs. external)
- [ ] Verify attribution preserved for external sources
- [ ] Check that credits block exists with source links
- [ ] Confirm commit message explains classification decisions
```

## References

- Commit: `e979272` (Rename Compound Engineering to Flywheel Framework)
- Learning: `2026-01-19-flywheel-framework-export.md`
- Pattern: [framework-extraction.md](./framework-extraction.md) (related: extracting frameworks with attribution)

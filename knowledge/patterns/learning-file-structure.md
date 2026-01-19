---
created: 2026-01-17
last_verified: 2026-01-17
---

# Pattern: Learning File Structure

## When to Apply

- Creating new learning files in `.claude/knowledge/learnings/`
- Auditing existing learnings for completeness
- Capturing fixes, patterns, or discoveries from a session
- Deciding what level of detail to include in a learning

## Why It Matters

Learning files capture institutional knowledge that prevents repeating mistakes. Without consistent structure:
- Root causes are unclear (just "what" not "why")
- Solutions can't be reproduced (missing context)
- Patterns aren't generalizable (too specific to one case)
- Files aren't discoverable (poor searchability)

## The Pattern

### Required Sections (8 Core)

| Section | Purpose |
|---------|---------|
| **Problem** | One paragraph explaining what went wrong or was needed |
| **Context** | What part of codebase, what constraints existed |
| **Root Cause** | Why it happened (use "Five Whys" depth) |
| **Solution** | What approach was taken and why (include code) |
| **Pattern Identified** | Reusable insight for future work |
| **Review Checklist Items** | Concrete actionable checkboxes |
| **Code Reference** | Specific files, lines, commits |
| **Related Learnings** | Links to related patterns/learnings |

### Optional Sections (Valuable When Applicable)

- **Testing Notes** - Reproduction steps
- **Future Considerations** - Known follow-up opportunities
- **What We Learned** - Broader insights beyond the pattern
- **Decision Framework** - When choosing between approaches

### Naming Convention

```
YYYY-MM-DD-[kebab-case-name].md
```

Examples:
- `2025-01-08-bluetooth-sample-rate-fix.md`
- `2026-01-17-plan-file-audit-standards.md`

### Root Cause Depth

Use the "Five Whys" technique - explain *why* not just *what*:

**Shallow (avoid):**
> The audio was distorted because the sample rate was wrong.

**Deep (preferred):**
> The audio was distorted because ProcessTap reported 48kHz but bluetooth constraints forced 24kHz. The system API returned nominal rates, not actual rates.

### Code Reference Format

```markdown
**Commits:**
- `253e733` - "Fixed chipmunk effect on bluetooth"

**Files changed:**
- `AudioProcessingPipeline.swift:204-213` - `inferSystemSampleRate()`

**Key functions:**
- `resample(samples:fromRate:toRate:)` - Sample rate conversion
```

## Example

Reference: `.claude/knowledge/learnings/2025-01-08-bluetooth-sample-rate-fix.md` (137 lines)

Key elements:
- Lines 1-5: Problem section (one clear paragraph)
- Lines 14-24: Root Cause with Five Whys depth
- Lines 62-73: Pattern Identified (generalizable)
- Lines 74-85: Review Checklist with actionable checkboxes
- Lines 86-100: Code Reference with files, commits, functions

## Anti-Pattern

```markdown
# Learning: Fixed the bug

## Problem
Audio was broken.

## Solution
Changed the sample rate code.
```

**Problems:**
- No root cause (why was it broken?)
- No context (what files, what constraints?)
- No code reference (where's the fix?)
- No pattern (how do I avoid this next time?)
- Not searchable (generic terms)

## Validation Checklist

- [ ] Title is descriptive (`# Learning: [Feature/Fix Name]`)
- [ ] Problem is one clear paragraph (not just a heading)
- [ ] Root cause uses "Five Whys" depth
- [ ] Solution includes code snippets with file:line references
- [ ] Pattern is generalizable (not specific to one case)
- [ ] Review Checklist has actionable checkboxes
- [ ] Code Reference includes files and line numbers
- [ ] Related Learnings links to other relevant docs
- [ ] Filename follows `YYYY-MM-DD-[kebab-case-name].md` format

## References

- Rule: [learning-file-audit.md](../../rules/learning-file-audit.md)
- Example: [2025-01-08-bluetooth-sample-rate-fix.md](../learnings/2025-01-08-bluetooth-sample-rate-fix.md)
- Meta: [pattern-file-structure.md](./pattern-file-structure.md)

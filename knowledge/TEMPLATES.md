# Knowledge Base Templates

Use these templates when adding new knowledge.

---

## Pattern Template

```markdown
# Pattern Name

## The Rule
[One sentence: what is the constraint or best practice?]

## Why It Matters
[Why is this important? What breaks if violated?]

## How It's Implemented
[Code examples from your project]

## Checklist for Agents
- Item 1
- Item 2

## Related Patterns
[Links to other patterns]
```

---

## Learning Template

```markdown
# Learning: [Feature/Fix Name]

## Problem
[What was the original issue or requirement?]

## Context
[What part of the codebase? What constraints existed?]

## Root Cause (if bug)
[What was the underlying issue?]

## Solution
[What approach was taken? Why?]

## Pattern Identified
[What reusable pattern emerged?]

## Review Checklist Items
- What should agents check to prevent this or leverage this pattern?

## Code Reference
[Files, commits, line numbers]

## Related Learnings
[Links to other learnings]
```

---

## Review Template

```markdown
# Review: [Category Name]

## Purpose
[What does this review check?]

## Checklist

### Category 1
- Item 1
- Item 2

### Category 2
- Item 1
- Item 2

## Common Issues
[Frequent mistakes this review catches]

## Examples
[Code examples of violations and fixes]
```

---

## Plan Template

```markdown
# Plan: [Feature Name]

## Objective
[What will be built/changed?]

## Current State
[What exists today?]

## Target State
[What should exist when done?]

## Implementation Steps

### Step 1: [Name]
- [ ] Task 1
- [ ] Task 2

### Step 2: [Name]
- [ ] Task 1
- [ ] Task 2

## Success Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Notes
[Decisions, constraints, open questions]
```

---

## Report Template

```markdown
# Report: [Review Type] - [Date]

## Summary
- X critical issues
- Y warnings
- Z info items

## Files Reviewed
- file1.swift
- file2.swift

## Findings

### [SEVERITY] Category - Issue

**File:** `path/to/file.swift:line`

**Issue:** Description

**Code:**
```swift
// problematic code
```

**Fix:** Recommendation

## Recommendations
1. Recommendation 1
2. Recommendation 2
```

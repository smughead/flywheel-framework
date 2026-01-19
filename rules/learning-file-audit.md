---
paths:
  - ".claude/knowledge/learnings/**/*.md"
---

# Learning File Audit

When reviewing, creating, or auditing learning files, apply this checklist.

Based on best practices from [Google SRE](https://sre.google/sre-book/postmortem-culture/), [Atlassian Incident Management](https://www.atlassian.com/incident-management/postmortem/templates), and [incident.io](https://incident.io/guide/learn-and-improve/post-mortem-documents).

## Pattern Reference

See @.claude/knowledge/patterns/learning-file-structure.md for required structure.

## Required Sections (8 Core)

| Section | Purpose | Check |
|---------|---------|-------|
| **Problem** | One paragraph explaining what went wrong or was needed | Must clearly describe the symptom/need |
| **Context** | What part of codebase, what constraints existed | Files, frameworks, dependencies involved |
| **Root Cause** | Why it happened (use "Five Whys" depth) | Should explain the *underlying* issue, not just the symptom |
| **Solution** | What approach was taken and why | Include code snippets showing the fix |
| **Pattern Identified** | Reusable insight for future work | Generalizable beyond this specific case |
| **Review Checklist Items** | Concrete actionable items | Checkboxes that can be applied to similar future work |
| **Code Reference** | Specific files, lines, commits | Traceability for future lookup |
| **Related Learnings** | Links to related patterns/learnings | Cross-reference for discoverability |

## Recommended Sections (Optional but Valuable)

| Section | When to Include |
|---------|-----------------|
| **Testing Notes** | If the fix requires specific reproduction steps |
| **Future Considerations** | If there are known follow-up opportunities |
| **What We Learned** | If there are broader insights beyond the pattern |
| **Decision Framework** | If the learning involves choosing between approaches |

## Quick Checklist

- [ ] Title is descriptive (`# Learning: [Feature/Fix Name]`)
- [ ] Problem section is one clear paragraph (not just a heading)
- [ ] Root cause uses "Five Whys" depth (explains *why* not just *what*)
- [ ] Solution includes code snippets with file:line references
- [ ] Pattern is generalizable (not specific to this one case)
- [ ] Review Checklist has actionable checkboxes
- [ ] Code Reference includes files and line numbers
- [ ] Related Learnings links to other relevant docs
- [ ] Filename follows `YYYY-MM-DD-[kebab-case-name].md` format

## Searchability Checklist

- [ ] Title includes key terms someone would search for
- [ ] Headers use domain-specific keywords (e.g., "Bluetooth", "sample rate", "gain")
- [ ] Code examples include actual function/class names

## Quality Grades

| Grade | Criteria |
|-------|----------|
| **A** | All 8 required sections present, good depth, code examples, cross-references |
| **B** | 6-7 required sections, adequate depth, some code examples |
| **C** | 5 required sections, minimal depth, missing code examples |
| **D** | Fewer than 5 sections or missing Problem/Solution |

## Template Reference

See the best example: `.claude/knowledge/learnings/2025-01-08-bluetooth-sample-rate-fix.md`

## Anti-Patterns

- **Too brief:** Just a title and one-liner solution (not searchable or reusable)
- **No root cause:** Describes fix without explaining *why* it was needed
- **No code references:** Can't trace back to actual implementation
- **Duplicate content:** Repeats patterns already in `.claude/knowledge/patterns/`
- **Stale content:** References files/code that no longer exists

## Audit Workflow

1. **Structure check** - All 8 required sections present
2. **Depth check** - Root cause explains "why" not just "what"
3. **Actionability check** - Checklist has concrete items
4. **Traceability check** - Code references are specific (file:line)
5. **Discoverability check** - Cross-references and searchable keywords
6. **Self-grade & offer improvements**

## Self-Grade & Improve

After auditing, grade the file and offer improvements:

> **Audit Grade: [B]**
>
> Missing sections:
> - Testing Notes (recommended)
> - Review Checklist Items (required)
>
> Would you like me to add the missing sections?

## Past Learnings

Structure based on industry postmortem practices (Google SRE, Atlassian, incident.io). The 8-section format ensures learnings are searchable, actionable, and traceable to code.

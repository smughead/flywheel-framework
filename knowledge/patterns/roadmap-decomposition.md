# Pattern: Roadmap Decomposition

## When to Apply
- Multi-phase feature implementations
- Large plans that won't fit in one session
- Work that may span multiple conversations

## The Pattern
1. Create overview file with:
   - Phase table (name, status, link)
   - Design decisions that apply across phases
   - Open items for future consideration

2. Create per-phase plan files with:
   - Prerequisites (which phases must complete first)
   - Success criteria (checkboxes)
   - Implementation steps (file-level detail)
   - Files to create/modify
   - Testing plan
   - Link to next phase

## Example
See `.claude/plans/agentic-listening-*.md`

## Anti-Pattern
- One giant plan file with all phases mixed together
- No clear "done" criteria per phase
- Phases that don't link to each other

## References
- `.claude/knowledge/learnings/2026-01-17-agentic-listening-plan-decomposition.md`

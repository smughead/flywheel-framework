---
paths:
  - ".claude/agents/**/*.md"
---

# Agent File Audit

When reviewing, creating, or auditing agent files, apply this checklist.

## Pattern Reference

See @.claude/knowledge/patterns/agent-file-structure.md for required structure.

## Quick Checklist

- [ ] YAML frontmatter present (starts with `---` on line 1)
- [ ] `name` matches filename (without .md)
- [ ] `description` includes trigger phrases users would say
- [ ] `tools` restricted to minimum needed
- [ ] `model` appropriate for task (sonnet for analysis, haiku for simple)
- [ ] Nested code blocks use escalating fence lengths (3, 4, 5 backticks)
- [ ] **Integration verified:** Agent referenced in `review-swift/SKILL.md` and `CLAUDE.md`

## Tool Selection

| Agent Type | Tools |
|------------|-------|
| Read-only reviewer | `Read, Grep, Glob` |
| Code modifier | `Read, Grep, Glob, Edit` |
| Full access | `Read, Grep, Glob, Edit, Write, Bash` |

## Audit Workflow

1. **Structure check** — YAML frontmatter, required fields
2. **Discovery check** — Trigger phrases in description
3. **Security check** — Tool restrictions appropriate
4. **Rendering check** — Nested code blocks fixed
5. **Integration check** — Grep for references in workflow files
6. **Self-grade & offer improvements** — See below

## Step 6: Self-Grade & Improve

After completing the audit, grade your work:

| Grade | Criteria |
|-------|----------|
| **A** | All checks pass, description has 4+ trigger phrases, well-integrated |
| **B** | All checks pass, description adequate but could be richer |
| **C** | Checks pass but integration incomplete or description minimal |
| **D** | Some checks failed or significant issues remain |

**Always present the grade and offer to improve:**

> **Audit Grade: [B]**
>
> Areas that could be improved:
> - Description has only 2 trigger phrases (could add more)
> - Missing "When to Invoke" section
>
> Would you like me to improve these areas?

**If user chooses to improve:** Use `AskUserQuestion` to clarify:
- What trigger phrases would users actually say?
- What scenarios should invoke this agent?
- Any domain-specific terminology to include?

Then apply improvements based on user input.

## Past Learnings

See @.claude/knowledge/learnings/2026-01-16-agent-yaml-frontmatter.md for context on why this matters.

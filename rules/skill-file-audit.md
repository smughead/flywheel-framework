---
paths:
  - ".claude/skills/**/SKILL.md"
---

# Skill File Audit

When reviewing, creating, or auditing skill files, apply this checklist.

## Pattern Reference

See @.claude/knowledge/patterns/skill-file-structure.md for required structure.

## Quick Checklist

- [ ] YAML frontmatter present (starts with `---` on line 1)
- [ ] `name` matches folder name
- [ ] `description` includes trigger phrases users would say
- [ ] `user-invocable: true` if users can call directly with `/skillname`
- [ ] `allowed-tools` explicitly listed (comma-separated)
- [ ] Agent prompts under 5 lines each (if skill uses agents)
- [ ] Total file under 200 lines
- [ ] No marketing/promotional sections

## Tool Selection

| Skill Type | Tools |
|------------|-------|
| Read-only analysis | `Read, Grep, Glob` |
| Documentation | `Read, Grep, Glob, Write` |
| Code modification | `Read, Grep, Glob, Edit` |
| Sub-agent workflow | `Read, Write, Edit, Glob, Grep, Task` |

## Audit Workflow

1. **Frontmatter check** - YAML present with required fields
2. **Discovery check** - Trigger phrases in description
3. **Permissions check** - `user-invocable` and `allowed-tools` set
4. **Size check** - File under 200 lines, agent prompts under 5 lines
5. **Content check** - No verbose explanations or marketing
6. **Self-grade & offer improvements** - See below

## Step 6: Self-Grade & Improve

After completing the audit, grade your work:

| Grade | Criteria |
|-------|----------|
| **A** | All checks pass, file is terse and well-structured |
| **B** | All checks pass, minor verbosity acceptable |
| **C** | Checks pass but file is verbose or prompts too long |
| **D** | Missing frontmatter fields or significant issues |

**Always present the grade and offer to improve:**

> **Audit Grade: [B]**
>
> Areas that could be improved:
> - Agent prompts are 8 lines each (should be 2-5)
> - Missing `allowed-tools` in frontmatter
>
> Would you like me to improve these areas?

## Past Learnings

See @.claude/knowledge/learnings/2026-01-17-skill-file-audit-compound.md for context on why this matters.

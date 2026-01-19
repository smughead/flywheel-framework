---
created: 2026-01-15
last_verified: 2026-01-15
---

# Skill Workflow Embedding

## The Rule

**Embed next-step prompts directly in skill files, not in hooks.**

Skills don't trigger PostToolUse hooks in Claude Code, so workflow continuity must be baked into the skill itself.

## Why It Matters

Without embedded prompts, workflow chains break silently:
- User runs `/review-swift` -> gets results -> session stalls (no prompt for `/compound`)
- User runs `/compound` -> learning captured -> session stalls (no prompt for next task)
- SESSION_NOTES becomes stale because nothing reminds Claude to update it

This breaks the flywheel loop that makes each session build on the last.

## The Pattern

Every skill that represents a workflow step should end with:

1. **Completion confirmation** - What was accomplished
2. **SESSION_NOTES update** (if applicable) - Mark work as done
3. **Next step prompt** - Suggest the logical next action

### Template for Skill Endings

```markdown
### Step N: Prompt for Next Step

After [completing the skill's main work], end with:

---

**[Completion message]**

[Contextual suggestions for what to do next, including:]
- The natural next step in the workflow
- Alternative options if the user wants to change direction
- Option to end the session

---
```

## How CueLoop Skills Implement This

### /review-swift (Step 5)
After delivering review results:
```markdown
**Next step:** Would you like to run `/compound` to document any learnings from this work?

If you fixed issues during the review, `/compound` will capture:
- What went wrong and why
- The fix pattern for future reference
- Any checklist updates for review agents

*Skip if this was routine work with no new insights.*
```

### /compound (Steps 4-5)
After capturing knowledge:
```markdown
**Knowledge captured!** SESSION_NOTES.md updated.

What would you like to work on next? You can:
- Pick another to-do from SESSION_NOTES.md
- Start a new feature with `/workflows:plan`
- End the session (I'll summarize what was accomplished)
```

### /workflows:verify
Should prompt for `/compound` after verification passes.

### /workflows:plan
Should prompt to start coding after plan is complete.

## Anti-Pattern: Relying on Hooks

**Don't do this:**
```bash
# hooks/post-skill.sh (DOESN'T WORK - skills don't trigger hooks)
if [[ "$SKILL_NAME" == "review-swift" ]]; then
    echo "Consider running /compound next"
fi
```

This will never execute because Claude Code's hook system doesn't fire for skill invocations.

## Checklist for Skill Authors

When creating or modifying a skill:

- [ ] Does the skill represent a step in a larger workflow?
- [ ] If yes, what's the natural next step?
- [ ] Is the next-step prompt embedded in the final step of the skill?
- [ ] Does the prompt include alternatives (not just one path)?
- [ ] If the skill completes meaningful work, does it update SESSION_NOTES?

## When to Apply This Pattern

**Always** when creating skills that are part of a workflow chain:
- `/workflows:plan` -> code -> `/workflows:verify` -> `/compound`
- `/build` -> `/test` -> `/review-swift`
- Any skill that "hands off" to another

**Not needed** for standalone skills like:
- One-shot utilities (format code, generate docs)
- Reference lookups (`/programming-swift`)
- Skills that don't chain to other skills

## Related Patterns

- None currently - this is a foundational workflow pattern

## Related Learnings

- [2026-01-15-flywheel-workflow-optimization.md](../learnings/2026-01-15-flywheel-workflow-optimization.md) - Discovery and implementation of this pattern

## References

- [Claude Code skill documentation](https://docs.anthropic.com/en/docs/claude-code/skills)
- [CLAUDE.md](../../../CLAUDE.md) - CueLoop Flywheel Framework workflow

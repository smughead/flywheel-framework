---
name: workflows-plan
description: Research codebase and create implementation plan using parallel sub-agents. Replaces /research + /plan-feature. Use when starting a new feature, planning non-trivial changes, or asking "plan this feature", "what do I need to know", "help me plan". Outputs plan to .claude/plans/ directory.
user-invocable: true
allowed-tools: Read, Glob, Grep, Task
---

# Workflows: Plan

Research the codebase and create an implementation plan using parallel sub-agents, keeping the main context window lean.

## Example Prompts

### 1. "Plan adding user authentication"
> Launches 3 research agents, synthesizes into plan at `.claude/plans/feature-user-auth.md`

### 2. "What should I know before implementing search functionality?"
> Researches knowledge base, codebase, and external docs in parallel

### 3. "/workflows:plan --quick fix the form validation"
> Fast mode with single agent for simple changes

## Syntax

```
/workflows:plan [--quick|--thorough] "feature description"
```

- `--quick` - Single agent, minimal plan (simple changes)
- (default) - 3 parallel agents, standard plan
- `--thorough` - 4 parallel agents, comprehensive analysis

## Workflow

### Step 1: Launch Parallel Research Agents

**CRITICAL:** Use the Task tool to launch these agents in parallel (single message, multiple Task calls):

#### Agent 1: Knowledge Base Researcher
```
subagent_type: "Explore"
prompt: |
  Search .claude/knowledge/{patterns,learnings,reviews}/*.md for: [FEATURE]
  Report: relevant patterns, applicable learnings, review checklist items.
```

#### Agent 2: Codebase Explorer
```
subagent_type: "Explore"
prompt: |
  Find files to modify for: [FEATURE]. Read CLAUDE.md > "Critical Constraints" first.
  Report: files to change, existing patterns, architecture layer, applicable constraints.
```

#### Agent 3: External Docs Researcher
```
subagent_type: "Explore"
prompt: |
  Research external documentation for: [FEATURE]. Check for relevant MCP tools or reference skills.
  Report: language/framework patterns, relevant APIs, best practices.
```

#### Agent 4 (--thorough only): Risk Analyzer
```
subagent_type: "Explore"
prompt: |
  Analyze risks for: [FEATURE]. Consider threading, performance, platform compat, testing.
  Report: risk table with severity and mitigations.
```

### Handling Agent Failures

If any agent returns empty or fails:

| Agent | If Empty/Failed | Fallback |
|-------|-----------------|----------|
| Agent 1 (Knowledge Base) | No relevant patterns found | Proceed—this is a new area. Note "No existing patterns" in plan. |
| Agent 2 (Codebase) | Can't find relevant files | Ask user to clarify scope before proceeding. |
| Agent 3 (External Docs) | No relevant docs found | Proceed with codebase patterns only. Note gap in plan. |
| Agent 4 (Risk Analyzer) | No risks identified | Good sign—note "Low risk" in plan. |

**If 2+ agents fail:** Stop and report to user. Don't synthesize a plan from insufficient research.

**Timeout handling:** If an agent doesn't respond within 60 seconds, proceed with available results and note the gap.

### Step 2: Synthesize Into Plan

After agents complete, synthesize their findings into a plan document:

**File:** `.claude/plans/[type]-[feature-name].md`

**Naming convention:**
- `feature-[name].md` - New functionality
- `fix-[name].md` - Bug fixes
- `refactor-[name].md` - Code restructuring
- `enhancement-[name].md` - Improvements to existing features

**Plan Template:**
```markdown
# Implementation Plan: [Feature Name]

## Research Summary
- **Knowledge Base:** [From Agent 1]
- **Codebase:** [From Agent 2]
- **External Docs:** [From Agent 3]
- **Risks:** [From Agent 4 if thorough]

## Approach
[1-2 sentences] | Why: [justification] | Alternatives: [why not chosen]

## Implementation Steps
1. **[File]** — [changes]
2. **[File]** — [changes]

## Constraints (from CLAUDE.md)
- [ ] [Domain-specific constraint 1]: [constraints or N/A]
- [ ] [Domain-specific constraint 2]: [constraints or N/A]

## Verification
- [ ] `/build` | `/test` pass
- [ ] Stack-specific review (if available)
- [ ] Manual: [specific checks]

## Risks
| Risk | Mitigation |
|------|------------|

## Questions
- [Decisions needed?]
```

### Step 3: Present for Validation

After writing the plan, summarize for the user:

```
Plan written to: .claude/plans/[filename].md
Summary: [1-2 sentences] | Key constraints: [list] | Files: [list]
Proceed with this approach?
```

**Main context receives only this summary (~500 tokens), not the full research.**

## Mode Comparison

| Mode | Agents | Context Used | Best For |
|------|--------|--------------|----------|
| `--quick` | 1 (codebase only) | ~200 tokens | Typos, simple fixes |
| (default) | 3 parallel | ~500 tokens | Standard features |
| `--thorough` | 4 parallel | ~800 tokens | Complex/risky changes |

## Quick Mode Template

For `--quick` mode, use this minimal template instead of the full plan:

**File:** `.claude/plans/quick-[name].md`

```markdown
# Quick Fix: [Name]

## Change
[1-2 sentences: what and where]

## Files
- `path/to/file` — [change description]

## Verify
- [ ] `/build` passes
- [ ] Manual check: [one specific thing]

## Constraints Check
- [ ] Project constraints followed (see CLAUDE.md)
- [ ] No breaking changes to public API
```

**Skip these sections in quick mode:**
- Research Summary (single agent does minimal exploration)
- Alternatives Considered
- Risk table
- Questions section

## After Planning

Once the user approves:
1. **Implement** - Follow the steps in the plan
2. **Verify** - Run `/workflows:verify` (or `/build` + `/test` + stack-specific review)
3. **Document** - Run `/compound` to capture learnings

## Related Skills

- `/workflows:verify` - Verify implementation (build + test + review)
- `/compound` - Capture learnings after completion

---
name: review-swift
description: Runs all Swift/macOS review agents in parallel and synthesizes findings. Use after completing features, before committing, when verifying code quality, or asking "review my code", "check code quality", "is this safe to commit", "Swift review", "code review". Covers memory safety, SwiftUI patterns, macOS compatibility, test coverage, and architecture.
user-invocable: true
allowed-tools: Read, Glob, Grep, Task
---

# Swift Code Review

Run a comprehensive Swift code review using specialized agents.

## Example Prompts

### 1. "Review the files I changed"
> Runs all agents on modified files, synthesizes findings into actionable report

### 2. "Is this code safe?"
> Focuses on memory safety, retain cycles, actor isolation

### 3. "Review before I commit"
> Full review, reports PASSED/NEEDS ATTENTION/FAILED with specific issues

## What Gets Reviewed

**Review agents (run in parallel):**
1. **swift-safety-reviewer** - Memory safety, retain cycles, actor isolation
2. **swiftui-patterns-checker** - @Observable, @MainActor, state management
3. **macos-compatibility-reviewer** - Permissions, entitlements, APIs
4. **test-coverage-analyzer** - Swift Testing patterns, coverage gaps
5. **architecture-guardian** - Layer boundaries, dependency flow

## Instructions

### Step 1: Identify Files to Review

Ask which files to review, or use one of these defaults:
- **"changed files"** - Files modified in current branch (use `git diff --name-only`)
- **"ui files"** - ContentView.swift and other SwiftUI files
- **"all Swift files"** - All .swift files in the project
- **specific paths** - User-provided file paths

### Step 2: Run Review Agents in Parallel

For each agent, launch a Task with subagent_type="general-purpose" to:
1. Read the agent specification from `.claude/agents/[agent-name].md`
2. Read any knowledge base patterns the agent references
3. Analyze the target files against the agent's checklist
4. Report findings using the agent's specified format

**Launch all agents simultaneously** using parallel Task tool invocations in a single message.

Each agent task prompt should follow this template:
```
You are acting as the [AGENT_NAME] review agent.

Read your specification from: .claude/agents/[agent-name].md
Read referenced patterns from: .claude/knowledge/patterns/

Files to review: [FILE_LIST]

Apply the checklist from your specification to these files. Report findings using the format specified in your agent document.

If no issues found, report: "✅ [AGENT_NAME]: PASSED - [brief summary]"
If issues found, report each using the severity format (🔴 CRITICAL, 🟡 WARNING, 🔵 INFO).
```

### Step 3: Synthesize Findings

After all agents complete, collect their findings and organize into a unified report:

```markdown
# Swift Review Results

## Summary
- **Status:** PASSED / NEEDS ATTENTION / FAILED
- **Files reviewed:** [count]
- **Findings:** X critical, Y warnings, Z info

---

## 🔴 Critical Issues
[All critical findings from all agents, grouped by file]
[Include file path, line number, issue, and recommended fix]

---

## 🟡 Warnings
[All warnings from all agents, grouped by file]

---

## 🔵 Info
[All info/suggestions from all agents, grouped by file]

---

## Agent Reports

### swift-safety-reviewer
[Summary or "PASSED"]

### swiftui-patterns-checker
[Summary or "PASSED"]

### macos-compatibility-reviewer
[Summary or "PASSED"]

### test-coverage-analyzer
[Summary or "PASSED"]

### architecture-guardian
[Summary or "PASSED"]
```

### Step 4: Determine Status and Recommend Actions

**Status determination:**
- **PASSED:** No critical issues, 0-2 warnings → "Safe to commit"
- **NEEDS ATTENTION:** 3+ warnings or minor concerns → "Review warnings before committing"
- **FAILED:** Any critical issues → "Must fix critical issues before committing"

**After reporting:**
- If FAILED: Offer to fix critical issues
- If NEEDS ATTENTION: Ask if help is needed addressing warnings
- If PASSED: Proceed to next step

### Step 5: Prompt for Knowledge Capture

After delivering the review results, always end with this prompt:

---

**Next step:** Would you like to run `/compound` to document any learnings from this work?

If you fixed issues during the review, `/compound` will capture:
- What went wrong and why
- The fix pattern for future reference
- Any checklist updates for review agents

*Skip if this was routine work with no new insights.*

---

## Agent Reference

Each agent is defined in `.claude/agents/`:

| Agent | Focus | When Most Relevant |
|-------|-------|-------------------|
| `swift-safety-reviewer.md` | Memory & concurrency safety | All Swift code |
| `swiftui-patterns-checker.md` | SwiftUI best practices | UI code |
| `macos-compatibility-reviewer.md` | macOS platform requirements | Permissions, APIs |
| `test-coverage-analyzer.md` | Testing quality | Test files, coverage |
| `architecture-guardian.md` | Code structure & dependencies | New classes, refactors |

## Resources Available to Agents

Each agent has access to these resources (documented in their specs):

| Resource | Use For |
|----------|---------|
| `/programming-swift` | Swift 6.2 language reference — syntax, concurrency, macros |
| `apple-docs` MCP | Framework APIs, WWDC transcripts, platform availability |

**When fixing issues:** If review finds problems, use `/programming-swift` for Swift syntax questions or query `apple-docs` MCP for framework-specific guidance.

## Example Usage

**Review changed files before commit:**
```
/review-swift changed files
```

**Review specific files:**
```
/review-swift ContentView.swift SettingsView.swift
```

**Full codebase review:**
```
/review-swift all Swift files
```

## Related

- `/compound` - Document learnings after fixing issues found by review
- `/build` - Verify code compiles after fixes
- `/test` - Run tests after making changes

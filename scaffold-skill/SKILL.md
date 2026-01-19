---
name: flywheel-setup
description: Initialize Flywheel Framework in any project. Use when starting a new project, adding AI-assisted workflow to existing project, or saying "set up flywheel", "initialize knowledge base", "add compound engineering".
user-invocable: true
allowed-tools: Read, Write, Edit, Glob, Bash, AskUserQuestion
---

# Flywheel Framework Setup

Initialize the Flywheel knowledge infrastructure in any project.

## Workflow

### Step 1: Detect Existing Structure

Check for existing `.claude/` directory:

```bash
ls -la .claude/ 2>/dev/null || echo "No .claude directory found"
```

**If exists:** Report what's already present, skip creating those directories.
**If new:** Will create full structure.

### Step 2: Ask Setup Method

Use AskUserQuestion to determine setup preference:

```
question: "How would you like to set up Flywheel Framework?"
options:
  - Copy full template (includes 10 skills, 5 agents, 12 patterns, 7 rules)
  - Minimal setup (directories + starter files only)
  - Stack-specific (choose your tech stack for targeted agents)
```

### Step 3: Handle Based on Choice

#### If "Copy full template":

Check if claude-template exists in the flywheel-framework repo:

```bash
# Try common locations
ls -la ~/Documents/projects/flywheel-framework/claude-template/ 2>/dev/null || \
ls -la ../flywheel-framework/claude-template/ 2>/dev/null || \
echo "Template not found - will use minimal setup"
```

If found, copy the entire template:
```bash
cp -R /path/to/flywheel-framework/claude-template/* .claude/
```

If not found, fall back to minimal setup with a note about where to get the full template.

#### If "Minimal setup":

Create directory structure and starter files (see Steps 4-7 below).

#### If "Stack-specific":

Ask about tech stack:
```
question: "Which tech stack is this project?"
options:
  - Swift/macOS (iOS, macOS, Apple platforms)
  - TypeScript/Node (web, APIs, Node.js)
  - Python (scripts, APIs, ML)
  - Other (I'll configure manually)
```

Then proceed with minimal setup + stack-specific agents.

### Step 4: Create Directory Structure

Create the core directories (skip any that exist):

```
.claude/
├── knowledge/
│   ├── patterns/
│   ├── learnings/
│   └── reviews/
├── rules/
├── agents/
├── skills/
├── plans/
└── reports/
```

### Step 5: Create Starter Rules

Create self-auditing rules in `.claude/rules/`:

**pattern-file-audit.md:**
```markdown
---
paths:
  - ".claude/knowledge/patterns/**/*.md"
---

# Pattern File Audit

## Quick Checklist
- [ ] Has "When to Apply" section
- [ ] Has "Why It Matters" section
- [ ] Includes code examples
- [ ] Has checklist for agents
```

**learning-file-audit.md:**
```markdown
---
paths:
  - ".claude/knowledge/learnings/**/*.md"
---

# Learning File Audit

## Quick Checklist
- [ ] Has Problem section
- [ ] Has Solution section
- [ ] Has Pattern Identified section
- [ ] Uses date prefix naming (YYYY-MM-DD)
```

### Step 6: Create Stack-Specific Agents

**If Swift/macOS:** Create these agent files in `.claude/agents/`:

- `swift-safety-reviewer.md` - Memory safety, concurrency
- `swiftui-patterns-checker.md` - Modern SwiftUI patterns
- `macos-compatibility-reviewer.md` - Permissions, entitlements
- `architecture-guardian.md` - Layer boundaries
- `test-coverage-analyzer.md` - Test quality

Use the agent definitions from `claude-template/agents/` or `tech-references/swift-macos-reference.md`.

**If TypeScript/Node:** Create placeholder agent files:

- `typescript-safety-reviewer.md`
- `api-patterns-checker.md`
- `security-reviewer.md`

**If Python:** Create placeholder agent files:

- `python-safety-reviewer.md`
- `api-patterns-checker.md`
- `test-coverage-analyzer.md`

**If Other:** Create template agent file showing the format.

### Step 7: Update CLAUDE.md

Check if CLAUDE.md exists:
- **If exists:** Append Flywheel section
- **If not:** Create minimal CLAUDE.md with Flywheel section

**Flywheel section to add:**

```markdown
## Flywheel Framework

Follow this workflow for non-trivial changes:
Plan → Work → Verify → Compound

### Resources
- **Knowledge Base:** `.claude/knowledge/`
  - `patterns/` - Reusable approaches
  - `learnings/` - Per-feature insights
  - `reviews/` - Domain checklists
- **Review Agents:** `.claude/agents/`
- **Self-Auditing Rules:** `.claude/rules/`

### Core Question
After every completed unit of work, ask:
> "What did we learn that makes the next similar task easier?"
```

### Step 8: Create Starter Pattern

Create one starter pattern to demonstrate the format:

`.claude/knowledge/patterns/pattern-file-structure.md`:
```markdown
---
created: [today's date]
last_verified: [today's date]
---

# Pattern: Pattern File Structure

## When to Apply
When creating any new pattern document in the knowledge base.

## Why It Matters
Consistent structure makes patterns findable and usable by agents.

## The Pattern
Every pattern file should have:
1. YAML frontmatter with dates
2. "When to Apply" section
3. "Why It Matters" section
4. Code examples (if applicable)
5. Checklist for agents

## Checklist
- [ ] Has YAML frontmatter
- [ ] Has "When to Apply"
- [ ] Has "Why It Matters"
- [ ] Examples use file:line references (not inline code) when > 10 lines
```

### Step 9: Report Setup Complete

**If full template was copied:**
```
Flywheel Framework initialized from template!

Copied:
- .claude/skills/ (10 skills)
- .claude/agents/ (5 Swift/macOS review agents)
- .claude/knowledge/ (12 patterns + templates)
- .claude/rules/ (7 self-auditing rules)
- .claude/plans/
- .claude/reports/

Skills available:
- /build, /test - Build and test workflow
- /compound - Capture learnings
- /workflows:plan, /workflows:verify - Feature workflow
- /review-swift - Run Swift review agents
- /programming-swift - Swift language reference
- /prd-creator, /skill-creator, /mcp-builder - Creation tools

Next steps:
1. Add Flywheel section to your CLAUDE.md (if not already there)
2. Try the workflow: Plan → Work → Verify → Compound
3. Document your first learning with /compound

The flywheel is ready to spin!
```

**If minimal setup:**
```
Flywheel Framework initialized!

Created:
- .claude/knowledge/ (patterns, learnings, reviews)
- .claude/rules/ (self-auditing rules)
- .claude/agents/ ([count] review agents for [stack])
- .claude/plans/
- .claude/reports/
- Updated CLAUDE.md with Flywheel reference

Next steps:
1. Document 2-3 existing patterns from your codebase
2. Create a learning document from recent work
3. Try the workflow: Plan → Work → Verify → Compound

For the full template with 10 skills and 12 patterns, copy from:
flywheel-framework/claude-template/

The flywheel is ready to spin!
```

## Notes

- This skill is meant to be copied from the flywheel-framework repo into new projects
- For existing projects with `.claude/` already set up, it will skip creating existing directories
- The full template includes all skills, agents, patterns, and rules ready to use
- Stack-specific agents come from the tech-references in the flywheel-framework repo

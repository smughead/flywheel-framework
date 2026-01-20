# Flywheel Framework

An AI-assisted development methodology where each unit of work makes the next easier to build.

> **Credits:** Inspired by [Compound Engineering](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents) (Every.to) and [Advanced Context Engineering](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents) (HumanLayer), combined and extended with self-auditing rules and stack-agnostic design.

## What It Does

Flywheel Framework systematically captures patterns, learnings, and review findings as you work—creating a knowledge layer that grows with your project. AI agents reference this accumulated context instead of starting from zero each session.

**The flywheel effect:**
- Today's discoveries become tomorrow's shortcuts
- Review findings become guardrails that prevent repeated mistakes
- The system improves itself through self-auditing rules

## Quick Start

### Option 1: Install as Claude Code Plugin (Recommended)

```bash
claude plugin install smughead/flywheel-framework
```

This installs the framework as a plugin, giving you access to:
- **7 core skills** - `/build`, `/test`, `/compound`, `/workflows:plan`, `/workflows:verify`, `/prd-creator`, `/gitignore-setup`
- **12 patterns** - file structures, workflow patterns, knowledge architecture
- **7 self-auditing rules** - auto-trigger on knowledge file creation

Then add stack-specific skills/agents from `tech-stacks/` as needed.

### Option 2: Copy the Framework

Copy the entire plugin to your project as `.claude/`:

```bash
# Clone the repo
git clone https://github.com/smughead/flywheel-framework.git

# Copy core framework to your project
cp -R flywheel-framework/skills your-project/.claude/skills
cp -R flywheel-framework/knowledge your-project/.claude/knowledge
cp -R flywheel-framework/rules your-project/.claude/rules
cp -R flywheel-framework/plans your-project/.claude/plans
cp -R flywheel-framework/reports your-project/.claude/reports

# Copy stack-specific content (e.g., Swift)
cp -R flywheel-framework/tech-stacks/swift/skills/* your-project/.claude/skills/
cp -R flywheel-framework/tech-stacks/swift/agents your-project/.claude/agents
```

Then add this to your CLAUDE.md:

```markdown
## Flywheel Framework

Follow this workflow for non-trivial changes:
Plan → Work → Verify → Compound

### Resources
- **Knowledge Base:** `.claude/knowledge/`
- **Review Agents:** `.claude/agents/`
- **Self-Auditing Rules:** `.claude/rules/`

### Core Question
After every completed unit of work, ask:
> "What did we learn that makes the next similar task easier?"
```

### Option 3: Use the Scaffold Skill (Interactive)

1. Copy `scaffold-skill/SKILL.md` to your project's `.claude/skills/flywheel-setup/` directory
2. Run `/flywheel-setup` in Claude Code
3. Follow the prompts to configure for your tech stack

## Keep It Personal (One-Time Setup)

Flywheel is designed as a **personal workflow** - your learnings, patterns, and review notes stay on your machine and don't get committed to shared repos.

After installing, run once:

```
/gitignore-setup
```

This configures your global gitignore to exclude Flywheel directories from ALL git repos:
- `.claude/knowledge/`
- `.claude/rules/`
- `.claude/agents/`
- `.claude/skills/`
- `.claude/plans/`
- `.claude/reports/`

Your teammates won't see these directories, and you won't accidentally commit personal workflow artifacts.

## Core Workflow

```
1. RESEARCH  → Subagents explore codebase
2. PLAN      → Design with project constraints (human review HERE)
3. WORK      → Execute with build verification
4. ASSESS    → Multi-perspective review (knowledge discovery)
5. COMPOUND  → Document learnings for future work
```

**Key insight:** Human review at the Plan phase prevents exponentially more time fixing code later.

### Session Continuity

SESSION_NOTES.md provides context preservation across `/clear` and new sessions.

**How it works:**
1. `/flywheel-setup` creates SESSION_NOTES.md in project root
2. Start each session by reading SESSION_NOTES.md, pick a to-do
3. Work on the task
4. `/compound` updates SESSION_NOTES after work (marks to-dos complete, links to learnings)
5. Repeat

**What SESSION_NOTES tracks:**
- **Current Focus** — What you're working on right now
- **To-Do** — Actionable items for the session
- **Decisions Made** — Architectural choices with rationale
- **Completed This Session** — Links to learnings created

See [session-continuity.md](knowledge/patterns/session-continuity.md) for the full pattern documentation.

## Directory Structure

```
your-project/                    # After /flywheel-setup
├── SESSION_NOTES.md             # Session continuity (created by scaffold)
├── CLAUDE.md                    # Project instructions
└── .claude/                     # Flywheel knowledge infrastructure

flywheel-framework/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── skills/                      # 7 core skills (stack-agnostic)
│   ├── build/                   # Generic build skill
│   ├── test/                    # Generic test skill
│   ├── compound/                # Capture learnings
│   ├── workflows-plan/          # Plan features with parallel agents
│   ├── workflows-verify/        # Verify implementation chain
│   ├── prd-creator/             # Create PRDs
│   └── gitignore-setup/         # Configure personal workflow
├── knowledge/                   # Patterns, learnings, reviews
│   ├── patterns/                # 12 reusable patterns
│   ├── learnings/               # (your project learnings)
│   ├── reviews/                 # (your project reviews)
│   └── TEMPLATES.md             # Templates for all file types
├── rules/                       # 7 self-auditing rules
├── plans/                       # (your project plans)
├── reports/                     # (your review reports)
├── tech-stacks/                 # Stack-specific content
│   ├── swift/                   # Swift/macOS/Xcode
│   │   ├── skills/              # build, test, review-swift, programming-swift
│   │   ├── agents/              # 5 Swift review agents
│   │   └── README.md
│   ├── typescript/              # TypeScript/Node (scaffold)
│   │   └── README.md
│   └── python/                  # Python (scaffold)
│       └── README.md
├── tech-references/
│   ├── stack-template.md        # Template for adding new stacks
│   └── swift-macos-reference.md # Swift/macOS patterns & agents
├── scaffold-skill/
│   └── SKILL.md                 # The /flywheel-setup skill
├── README.md                    # This file
└── Methodology.md               # Core framework documentation
```

## Core Skills (7)

| Skill | Purpose |
|-------|---------|
| `/build` | Build project (auto-detects stack) |
| `/test` | Run tests (auto-detects stack) |
| `/compound` | Capture learnings after work |
| `/workflows:plan` | Plan feature implementation |
| `/workflows:verify` | Verify implementation meets plan |
| `/prd-creator` | Create product requirement docs |
| `/gitignore-setup` | Configure global gitignore for personal workflow |

## Tech Stacks

The framework is stack-agnostic at its core. Stack-specific skills and agents live in `tech-stacks/`:

### Swift/macOS (Complete)
- 4 skills: build, test, review-swift, programming-swift
- 5 agents: safety, SwiftUI patterns, macOS compat, test coverage, architecture

### TypeScript (Scaffold)
- Directory structure ready
- Contributions welcome!

### Python (Scaffold)
- Directory structure ready
- Contributions welcome!

### Adding Your Stack

1. Copy `tech-references/stack-template.md` to `tech-references/[your-stack]-reference.md`
2. Create `tech-stacks/[your-stack]/` with skills and agents
3. Follow the patterns in `knowledge/patterns/`
4. Submit a PR!

## Patterns (12)

- File structure patterns (agent, learning, pattern, plan, report, rule, skill)
- `claude-code-knowledge-architecture.md` - Where to store what
- `skill-workflow-embedding.md` - Chaining skills together
- `roadmap-decomposition.md` - Breaking large tasks into phases
- `terminology-rename-audit.md` - Safe renaming across codebases
- `framework-extraction.md` - Extracting reusable frameworks

## Rules (7)

Self-auditing rules that trigger when you create/edit knowledge files:
- `agent-file-audit.md`
- `learning-file-audit.md`
- `pattern-file-audit.md`
- `plan-file-audit.md`
- `report-file-audit.md`
- `rule-file-audit.md`
- `skill-file-audit.md`

## The Self-Auditing Innovation

Neither the original Compound Engineering nor ACE articles describe this pattern: **rule files that audit the knowledge system itself**.

Rules auto-trigger when knowledge files are created/modified, presenting standardized checklists and grading the work. This creates a virtuous cycle:

```
Rules audit patterns/learnings → Findings become new learnings →
Learnings improve patterns → Patterns improve future work →
Work gets audited by rules → (cycle continues)
```

## Success Indicators

The system is working when:
- Similar features take less time than the first
- Issues get caught earlier (in planning, not code review)
- Knowledge base grows with each feature
- Agents cite project-specific patterns, not generic advice

## Learn More

- **Full methodology:** [Methodology.md](./Methodology.md)
- **Swift/macOS setup:** [tech-stacks/swift/README.md](./tech-stacks/swift/README.md)
- **Add your stack:** [tech-references/stack-template.md](./tech-references/stack-template.md)

## License

MIT

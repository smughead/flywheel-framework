# Swift/macOS Tech Stack

Stack-specific skills, agents, and patterns for Swift/macOS development with Xcode.

## What's Included

### Skills (4)

| Skill | Purpose |
|-------|---------|
| `build/` | Build project with Xcode/XcodeBuildMCP |
| `test/` | Run tests with Xcode test schemes |
| `review-swift/` | Run all Swift review agents |
| `programming-swift/` | Swift language reference |

### Agents (5)

| Agent | Reviews |
|-------|---------|
| `swift-safety-reviewer` | Memory safety, concurrency, Swift 6 |
| `swiftui-patterns-checker` | @Observable, @MainActor, state management |
| `macos-compatibility-reviewer` | Permissions, entitlements, API availability |
| `test-coverage-analyzer` | Test gaps, Swift Testing patterns |
| `architecture-guardian` | Layer boundaries, dependency flow |

## Installation

### Option 1: Copy to Your Project

Copy the skills and agents you need to your project's `.claude/` directory:

```bash
# Copy all Swift skills
cp -R tech-stacks/swift/skills/* .claude/skills/

# Copy all Swift agents
cp -R tech-stacks/swift/agents/* .claude/agents/
```

### Option 2: Selective Copy

Copy only what you need:

```bash
# Just the review skill and agents
cp -R tech-stacks/swift/skills/review-swift .claude/skills/
cp -R tech-stacks/swift/agents/* .claude/agents/

# Just the Swift programming reference
cp -R tech-stacks/swift/skills/programming-swift .claude/skills/
```

## Usage

Once copied to your project:

- `/build` - Build with Xcode
- `/test` - Run Xcode tests
- `/review-swift` - Run all 5 review agents in parallel

## MCP Integration

These skills work best with:
- **XcodeBuildMCP** - For build and test automation

Configure in your Claude Code MCP settings.

## See Also

- `tech-references/swift-macos-reference.md` - Detailed patterns and anti-patterns

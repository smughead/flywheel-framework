---
name: build
description: Builds the project. See tech-stacks/ for stack-specific build commands. Use when compiling, verifying changes, checking for build errors, or asking "build", "compile", "check errors".
user-invocable: true
allowed-tools: Bash
---

# Build Project

Compile the project and report results.

## Example Prompts

### 1. "Build the app"
> Compiles the project, reports success or lists errors with file locations

### 2. "Does my code compile?"
> Runs build to verify changes don't break compilation

### 3. "Check for build errors before I commit"
> Builds and confirms code is ready to commit

## Instructions

1. Check `.claude/CLAUDE.md` or project configuration for build commands
2. Run the appropriate build command for the tech stack
3. Report any build errors clearly with file locations
4. Offer to fix compilation errors if they occur

## Common Build Commands

Build command depends on your tech stack:

| Stack | Build Command |
|-------|--------------|
| **Swift/Xcode** | `xcodebuild -scheme [Scheme] build` |
| **TypeScript** | `npm run build` or `tsc` |
| **Python** | `pip install -e .` or `poetry build` |
| **Rust** | `cargo build` |
| **Go** | `go build ./...` |

## Stack-Specific Skills

For specialized build configurations, see:
- `tech-stacks/swift/skills/build/` - Xcode/XcodeBuildMCP integration
- `tech-stacks/typescript/skills/build/` - npm/yarn/pnpm builds
- `tech-stacks/python/skills/build/` - pip/poetry/setuptools builds

## Detection

If no build command is specified, look for:
- `package.json` → TypeScript/JavaScript (`npm run build`)
- `Cargo.toml` → Rust (`cargo build`)
- `pyproject.toml` or `setup.py` → Python (`pip install -e .`)
- `go.mod` → Go (`go build ./...`)
- `*.xcodeproj` or `*.xcworkspace` → Swift/Xcode

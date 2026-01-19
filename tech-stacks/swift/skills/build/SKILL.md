---
name: build
description: Builds the project for macOS using XcodeBuildMCP. Use when compiling the project, verifying code changes, checking for build errors before committing, running xcodebuild, or asking "does it compile", "build the app", "check for errors", "verify my changes". Covers compilation, build diagnostics, and error resolution.
user-invocable: true
allowed-tools: Bash
---

# Build Project

Compile the project for macOS and report results.

## Example Prompts

### 1. "Build the app"
> Compiles the project, reports success or lists errors with file locations

### 2. "Does my code compile?"
> Runs build to verify changes don't break compilation

### 3. "Check for build errors before I commit"
> Builds and confirms code is ready to commit

## Instructions

1. Use XcodeBuildMCP `build_macos` tool to build the project
2. Report any build errors clearly with file locations
3. Offer to fix compilation errors if they occur

## Fallback

If XcodeBuildMCP is unavailable:
```bash
xcodebuild -scheme [ProjectScheme] -destination 'platform=macOS' -quiet build
```

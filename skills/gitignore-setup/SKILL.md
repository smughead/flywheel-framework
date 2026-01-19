---
name: gitignore-setup
description: Configure global gitignore to keep Flywheel Framework directories personal and untracked. Run once after installing the plugin. Use when asking "setup gitignore", "keep flywheel private", "configure personal workflow".
user-invocable: true
allowed-tools: Bash, Read
---

# Gitignore Setup

Configure your global gitignore so Flywheel Framework directories stay personal and never get committed to shared repositories.

## Why This Matters

Flywheel Framework creates directories like `.claude/knowledge/`, `.claude/agents/`, etc. in your projects. These contain your personal workflow artifacts - learnings, patterns, and review notes. By adding them to your global gitignore, they remain invisible to version control in ALL your repos without affecting teammates.

## What It Does

1. Creates or updates `~/.gitignore_global`
2. Adds Flywheel-specific directory patterns
3. Configures git to use the global gitignore (if not already set)

## Instructions

### Step 1: Check Current State

```bash
# Check if global gitignore is configured
git config --global core.excludesfile || echo "Not configured"

# Check if file exists and its contents
cat ~/.gitignore_global 2>/dev/null || echo "File does not exist"
```

### Step 2: Create/Update Global Gitignore

If the file doesn't exist, create it. If it exists, append the Flywheel entries (avoid duplicates).

**Entries to add:**
```
# Flywheel Framework (personal workflow)
.claude/knowledge/
.claude/rules/
.claude/agents/
.claude/skills/
.claude/plans/
.claude/reports/
```

Use this approach:
```bash
# Create file if it doesn't exist, append if it does (check for existing entries first)
if ! grep -q "Flywheel Framework" ~/.gitignore_global 2>/dev/null; then
  cat >> ~/.gitignore_global << 'EOF'

# Flywheel Framework (personal workflow)
.claude/knowledge/
.claude/rules/
.claude/agents/
.claude/skills/
.claude/plans/
.claude/reports/
EOF
  echo "Added Flywheel entries to ~/.gitignore_global"
else
  echo "Flywheel entries already present in ~/.gitignore_global"
fi
```

### Step 3: Configure Git to Use Global Gitignore

```bash
# Set global excludesfile if not already configured
if [ -z "$(git config --global core.excludesfile)" ]; then
  git config --global core.excludesfile ~/.gitignore_global
  echo "Configured git to use ~/.gitignore_global"
else
  echo "Git already configured with: $(git config --global core.excludesfile)"
fi
```

### Step 4: Verify Setup

```bash
echo "=== Global Gitignore Configuration ==="
echo "File: $(git config --global core.excludesfile)"
echo ""
echo "=== Flywheel Entries ==="
grep -A 10 "Flywheel Framework" ~/.gitignore_global 2>/dev/null || echo "Not found"
```

### Step 5: Report Success

```
Global gitignore configured!

Your Flywheel Framework directories will now be ignored in ALL git repos:
- .claude/knowledge/
- .claude/rules/
- .claude/agents/
- .claude/skills/
- .claude/plans/
- .claude/reports/

This is a one-time setup. Your personal workflow stays personal.
```

## Notes

- This only needs to be run once per machine
- It won't affect teammates - global gitignore is local to your machine
- Shared project files like `.claude/settings.json` or `CLAUDE.md` are NOT ignored
- If you want to share Flywheel with teammates later, just remove the entries from `~/.gitignore_global`

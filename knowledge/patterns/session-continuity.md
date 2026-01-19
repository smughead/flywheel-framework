---
created: 2026-01-19
last_verified: 2026-01-19
---

# Pattern: Session Continuity with SESSION_NOTES

## When to Apply

Use this pattern when:
- Working on multi-session projects where context loss from `/clear` or new sessions is a problem
- You want a single file to read at session start to restore context
- You need to track to-dos, decisions, and completed work across sessions

## Why It Matters

Claude Code sessions are ephemeral. `/clear` wipes context. New sessions start blank. Without a continuity mechanism, you waste time re-explaining context or re-discovering decisions already made.

SESSION_NOTES.md solves this by serving as a **continuity contract** between sessions—a single file that captures everything needed to resume work.

## The Pattern

SESSION_NOTES.md lives in the project root and contains:

1. **Current focus** — What we're working on right now
2. **To-dos** — Actionable items for this session
3. **Decisions made** — Architectural choices with rationale
4. **Completed work** — Breadcrumb trail linking to learnings

### Integration Flow

```
┌─────────────────────────────────────────────────────────────┐
│                     CLAUDE.md                                │
│  (Project instructions - references SESSION_NOTES)          │
│                         │                                    │
│    "Start each session by reading SESSION_NOTES.md"         │
│    "End each session with /compound"                        │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                   SESSION_NOTES.md                          │
│  (Session state - updated by /compound)                     │
│                         │                                    │
│    Current Focus ──────►  Links to active plan              │
│    To-Do ───────────────►  Actionable items                 │
│    Decisions Made ──────►  Table with rationale             │
│    Completed This Session ► Links to learnings              │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│              /compound skill (Steps 4-5)                    │
│  (Automatically updates SESSION_NOTES after work)           │
│                                                              │
│    1. Mark completed to-do                                  │
│    2. Add one-line summary under "Completed This Session"   │
│    3. Link to the learning document created                 │
└─────────────────────────────────────────────────────────────┘
```

### Workflow Cycle

1. **`/flywheel-setup`** creates SESSION_NOTES.md during project initialization
2. **User** reads SESSION_NOTES at session start, picks a to-do
3. **User** works on the task
4. **`/compound`** updates SESSION_NOTES after work (marks to-dos complete, links to learnings)
5. **Repeat**

## Example

```markdown
# MyApp Session Notes

**Last Updated:** 2026-01-19

---

## Current Focus
Implementing audio recording feature with real-time waveform visualization.

**Plan:** [Audio Recording Feature](.claude/plans/audio-recording.md)

## To-Do
- [ ] Add microphone permission handling
- [x] Create AudioRecorder service
- [ ] Build waveform visualization view

## Decisions Made
| Question | Decision | Rationale |
|----------|----------|-----------|
| Audio format | CAF with AAC | Best quality/size ratio for iOS |
| Waveform library | Build custom | AVFoundation provides raw samples |

## Completed This Session
- [x] **AudioRecorder service** — see [learning](.claude/knowledge/learnings/2026-01-19-audio-recorder-service.md)
```

## Anti-Pattern

**Don't:** Keep session state in CLAUDE.md. CLAUDE.md is for static project configuration. SESSION_NOTES.md is for dynamic session state that changes with each session.

**Don't:** Manually update SESSION_NOTES.md at session end. Let `/compound` handle it automatically.

## Checklist

- [ ] SESSION_NOTES.md exists in project root
- [ ] CLAUDE.md references SESSION_NOTES for session workflow
- [ ] `/compound` skill has Steps 4-5 for SESSION_NOTES updates
- [ ] "How to Use This File" section included in SESSION_NOTES

## Related Patterns

- [Knowledge Architecture](claude-code-knowledge-architecture.md) — Where SESSION_NOTES fits in the overall structure
- [Skill Workflow Embedding](skill-workflow-embedding.md) — How /compound chains SESSION_NOTES updates

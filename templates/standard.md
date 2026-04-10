# Standard Agent Rules

> For most projects. Covers task management, handoff, and execution discipline.

## Task Startup Protocol

When receiving a new task, do NOT start coding immediately. First evaluate and report:

1. My understanding of the goal: ____
2. Estimated files involved: ____
3. Estimated time: ____
4. Recommended task size: S / M / L
5. Uncertainties: ____

Wait for user confirmation before proceeding.

### Task Size → Process

| Size | Criteria | Process |
|------|----------|---------|
| **S** | < 30min, 1-2 files | Do it → test → commit → HANDOFF |
| **M** | 1-3h, 3-5 files | Brief plan → execute → commit → HANDOFF |
| **L** | > 3h, cross-module | Brainstorm → plan → TDD → review → HANDOFF |

## Execution Discipline

- Edit existing files with targeted replacements, NEVER overwrite entire files
- Follow the user-confirmed plan strictly — no skipping, no extras
- Analysis first, wait for confirmation, then execute
- Everything discussed must be written to files immediately
- Back up important files before modifying

## Session Handoff

### At Session Start
1. Read `HANDOFF.md`
2. Run `git log --oneline -5`
3. Check `git status`
4. Confirm understanding before working

### During Session (Progressive HANDOFF)
Update HANDOFF.md after each milestone — don't wait for session end.

### At Session End
Update `HANDOFF.md`:
```
## YYYY-MM-DD
- done: [completed items]
- blocked: [blockers]
- decisions: [key choices and rationale]
- pitfalls: [mistakes encountered]
- next: [suggested next steps]
```

## Error Handling

- Acknowledge mistakes immediately
- Don't repeat the same error within a session
- When unsure, ask — don't guess

## Tool Selection

- Terminal for verification (check files, status, logs)
- Browser only for interactive tasks (login, forms)
- Give users URLs instead of opening browsers for them

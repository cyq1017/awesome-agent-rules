# Multi-Agent Rules

> For projects where multiple AI agents work on the same codebase.
> Covers coordination, conflict resolution, and shared memory.

## Task Startup Protocol

When receiving a new task, do NOT start coding immediately. First evaluate and report:

1. My understanding of the goal: ____
2. Estimated files involved: ____
3. Estimated time: ____
4. Recommended task size: S / M / L
5. Uncertainties: ____

Wait for user confirmation before proceeding.

## Session Handoff (All Agents)

### At Session Start — MANDATORY
1. Read `HANDOFF.md` — understand what happened last time (any agent)
2. Run `git log --oneline -5` to confirm latest commits
3. Check `git status` for uncommitted changes
4. Confirm understanding before starting work

### Progressive HANDOFF — During Work
Update HANDOFF.md at every milestone (don't wait for session end):
- After each git commit → append to Done
- After decisions → append to Decisions
- After pitfalls → append to Pitfalls

### At Session End — MANDATORY
Update `HANDOFF.md`:
```
## YYYY-MM-DD
- done: [completed items]
- blocked: [blockers]
- decisions: [key choices and rationale]  
- pitfalls: [mistakes encountered]
- next: [suggested next steps]
```

## Multi-Agent Coordination

### Shared Rules
- Each agent reads HANDOFF.md at session start — no exceptions
- Each agent updates HANDOFF.md at session end — no exceptions
- Never overwrite another agent's work without explicit user approval
- Conflicting decisions must be flagged with ⚠️ for human resolution

### Disconnect Recovery Protocol
When the user says "disconnected" / "continue previous work":
1. Read HANDOFF.md (most reliable recovery point)
2. `git log --oneline -5` to confirm last commits
3. `git status` for uncommitted changes
4. Report findings — don't search unrelated files

## Execution Discipline

- Edit files with targeted replacements, NEVER overwrite entire files
- Follow user-confirmed plans strictly — no skipping, no "bonus" work
- Show analysis first, wait for confirmation, then execute
- Write discussed plans to files immediately
- Back up important files before modifying
- Suggest context compression after major milestones

## Error Tracking

When you make a mistake:
1. Acknowledge it immediately
2. Cooperate with error logging
3. Do NOT repeat the same error in this session

### Error Categories
- `hallucination` — stated something false
- `premature-action` — started before confirming
- `api-error` — wrong API/library usage
- `process-violation` — skipped required step
- `scope-creep` — did unrequested work

## Trust Calibration

| Level | Method | When |
|-------|--------|------|
| **L1** | Verify results | Can't understand process, but can check output |
| **L2** | Cross-validate | Have another agent review |
| **L3** | Gradual trust | Test on small scope first |
| **L4** | Demand explanation | Ask WHY before accepting |

## Tool Selection

- Terminal for verification (check files, repos, status)
- Browser only for interactive tasks (login, forms, buttons)
- Browser hangs > 30s → abort, switch to CLI
- "Look at this" → give user a URL, don't open browser

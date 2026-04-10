# Awesome Agent Rules [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

**English** | [中文](README_zh.md)

> A curated collection of rules, instructions, and best practices for AI coding agents.
>
> CLAUDE.md · AGENTS.md · .cursorrules · GEMINI.md — battle-tested patterns from real projects.

Every AI coding tool reads some form of instruction file. But **most developers start from scratch every time.** This repo collects proven patterns so you don't have to reinvent the wheel.

---

## Why This Matters

AI coding agents (Claude Code, Cursor, Windsurf, Codex, Antigravity, etc.) are powerful — but they need guardrails. Without good rules:

- 🔥 Agents overwrite files instead of editing them
- 🏃 Agents start coding before understanding the task
- 💀 Session context is lost when you disconnect
- 🤷 Agents make the same mistakes repeatedly

Good rules fix all of this. **One `.md` file can transform your AI coding experience.**

---

## Table of Contents

- [What File Does My Tool Use?](#what-file-does-my-tool-use)
- [Rules by Category](#rules-by-category)
  - [Task Management](#-task-management)
  - [Session Handoff](#-session-handoff)
  - [Execution Discipline](#-execution-discipline)
  - [Error Tracking](#-error-tracking)
  - [Trust Calibration](#-trust-calibration)
  - [Multi-Agent Coordination](#-multi-agent-coordination)
  - [Tool Selection](#-tool-selection)
  - [Network Resilience](#-network-resilience)
- [Complete Templates](#complete-templates)
- [Related Projects](#related-projects)
- [Contributing](#contributing)

---

## What File Does My Tool Use?

| Tool | File | Scope |
|------|------|-------|
| **Claude Code** | `CLAUDE.md` (project) / `~/.claude/CLAUDE.md` (global) | Per-project or global |
| **Cursor** | `.cursorrules` | Per-project |
| **Windsurf** | `.windsurfrules` | Per-project |
| **Codex (OpenAI)** | `AGENTS.md` / `~/.codex/AGENTS.md` | Per-project or global |
| **Antigravity** | `AGENTS.md` / `~/.gemini/AGENTS.md` | Per-project or global |
| **Aider** | `.aider.conf.yml` | Per-project |
| **Multi-agent** | `AGENTS.md` | Cross-tool standard |

> 💡 **AGENTS.md** is the emerging cross-tool standard — Claude Code, Codex, and Antigravity all read it. If you only maintain one file, make it `AGENTS.md`.

---

## Rules by Category

### 📋 Task Management

**The #1 mistake: agents start coding before understanding the task.**

```markdown
## Task Startup Protocol

When receiving a new task, do NOT start coding immediately. First evaluate and report:

1. My understanding of the goal: ____
2. Estimated files involved: ____
3. Estimated time: ____
4. Recommended task size: S / M / L
5. Uncertainties: ____

Wait for user confirmation before proceeding.

### Task Size → Process Mapping

| Size | Criteria | Process |
|------|----------|---------|
| **S** | < 30min, 1-2 files | Do it → test → commit → HANDOFF |
| **M** | 1-3h, 3-5 files | Brief plan → execute → commit → HANDOFF |
| **L** | > 3h, cross-module | Brainstorm → plan → TDD → review → HANDOFF |
```

**Why it works:** Forces alignment before execution. Prevents the "I thought you wanted X but you actually wanted Y" problem that wastes hours.

---

### 🔄 Session Handoff

**The #2 mistake: context is lost between sessions.**

```markdown
## Session Handoff Protocol

### At Session Start
1. Read `HANDOFF.md` — understand what happened last time
2. Run `git log --oneline -5` to confirm latest commits
3. Check `git status` for uncommitted changes
4. Confirm understanding before starting work

### At Session End (MANDATORY)
Update `HANDOFF.md` with the following format:

## YYYY-MM-DD
- done: [completed items]
- blocked: [blockers, omit if none]
- decisions: [key choices made and why]
- pitfalls: [mistakes encountered]
- next: [suggested next steps]

Keep it under 500 tokens. Include ALL negative decisions (what was rejected and why).
```

**Why it works:** HANDOFF.md is the cheapest, most reliable cross-session memory. Works with any tool, survives crashes, and humans can read it too.

---

### 🎯 Progressive HANDOFF

**Don't wait until session end to write HANDOFF — you might disconnect.**

```markdown
## Progressive HANDOFF Rule

Do NOT wait until session end to write HANDOFF. Update it at every milestone:
- After each git commit → append to Done list
- After important decisions → append to Decisions
- After hitting a pitfall → append to Pitfalls
- This ensures HANDOFF is current even if the session crashes unexpectedly
```

**Why it works:** Network disconnections, crashes, and context exhaustion are common. Progressive HANDOFF means you never lose more than one step of progress.

---

### 🛡️ Execution Discipline

```markdown
## Execution Discipline

- Edit existing files with targeted replacements, NEVER overwrite the entire file
- Follow the user-confirmed plan strictly — no skipping steps, no "while I'm at it" extras
- Show analysis/judgment first, wait for user confirmation before executing
- Everything you say you'll do must be immediately written to a file
- Don't assume data won't be lost — back up important files before modifying
- Suggest context compression after completing major milestones
```

**Why it works:** Agents are eager to please and tend to do more than asked. These rules prevent scope creep and data loss.

---

### 📝 Error Tracking

```markdown
## Error Tracking

When you make a mistake:
1. Acknowledge it immediately — don't try to hide it
2. Cooperate with the human's error logging process (Error Book)
3. Do NOT repeat the same error within this session
4. If you're unsure, ask instead of guessing

## Error Categories (for the Error Book)
Tag each error with one of:
- `hallucination` — claimed something false
- `premature-action` — started before confirming
- `api-error` — wrong API usage
- `process-violation` — skipped a required step
- `scope-creep` — did unrequested work
```

**Why it works:** Systematic error tracking builds an "agent trust profile" over time. You learn which agents are reliable for what tasks.

---

### 🤝 Trust Calibration

```markdown
## Trust Calibration Framework

Not all tasks deserve the same level of oversight:

| Trust Level | When to Use | Human Action |
|------------|-------------|--------------|
| **L1 — Verify result** | You don't understand the process, but can check the output | Run tests, check deployment |
| **L2 — Cross-validate** | Use another agent to review | Have Agent B review Agent A's code |
| **L3 — Gradual trust** | Try the approach on a small scope first | Test on one file before applying to all |
| **L4 — Demand explanation** | You don't understand why a decision was made | Ask "why this approach? what alternatives?" |
```

**Why it works:** Trust isn't binary. These levels help you decide when to review carefully vs. when to let the agent run.

---

### 🤖 Multi-Agent Coordination

```markdown
## Multi-Agent Handoff

### When Multiple Agents Work on the Same Project
- Each agent reads HANDOFF.md at session start — no exceptions
- Each agent updates HANDOFF.md at session end — no exceptions
- Conflicting decisions must be flagged with ⚠️ and left for human resolution
- Never overwrite another agent's work without explicit user approval

### Disconnect Recovery Protocol
When the user says "disconnected" / "continue previous work" / "where were we":
1. Read HANDOFF.md (most reliable recovery point)
2. `git log --oneline -5` to confirm last commits
3. `git status` for uncommitted changes
4. Report recovery result — don't search unrelated files
```

---

### 🔧 Tool Selection

```markdown
## Tool Selection Rules

- Verification tasks (check files, repos, status) → use terminal commands, not browser
- Browser only for tasks requiring interaction (login, form fill, button clicks)
- If browser sub-agent hangs > 30 seconds → abort, switch to CLI approach
- "Take a look at this" requests → give the user a URL, don't open browser for them
```

---

### 🌐 Network Resilience

```markdown
## Network Resilience

- Always use `git commit` before long-running operations
- Set up fallback for flaky connections (retry logic, timeout handling)
- Package managers (pip, npm, cargo) through proxy often timeout
  → Configure domestic mirrors or DIRECT routing rules
- AI service domains must route through clean residential IPs
```

---

## Complete Templates

Ready-to-use templates for different project types:

| Template | Best For | Download |
|----------|----------|----------|
| [Minimal](templates/minimal.md) | Quick scripts, small projects | [📄](templates/minimal.md) |
| [Standard](templates/standard.md) | Most projects | [📄](templates/standard.md) |
| [Multi-Agent](templates/multi-agent.md) | Projects with multiple AI agents | [📄](templates/multi-agent.md) |

---

## Related Projects

| Project | Description |
|---------|-------------|
| [Conductor](https://github.com/cyq1017/conductor) | CLI framework for orchestrating multiple AI coding agents |
| [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | Cursor-specific rules collection |
| [OpenSpec](https://github.com/Fission-AI/OpenSpec) | Spec-Driven Development framework for AI agents |
| [Superpowers (ECC)](https://github.com/obra/superpowers) | Agent workflow enhancement skills for Claude Code |
| [AGENTS.md Spec](https://agents.md) | Official specification for the AGENTS.md cross-tool standard |
| [claude-code (instructkr)](https://github.com/instructkr/claude-code) | Claude Code tips and CLAUDE.md examples |

---

## Contributing

Contributions welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting.

**How to contribute:**
1. Fork this repo
2. Add your rule/template in the appropriate category
3. **Sanitize all personal data** (paths, usernames, API keys)
4. Submit a PR with:
   - What tool you tested it with
   - What problem it solves
   - How long you've been using it

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related rights to this work.

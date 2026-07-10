# Workflow setup — the orchestration this skill designs prompts for

If the user hasn't set up the token-saving workflow in Claude Code yet,
walk them through these five steps (concept credit: Diego @diegocabezas01).

## 1. Main model

```
/model → Fable 5
/effort → max
```

## 2. Subagent: deep-reasoner (`.claude/agents/deep-reasoner.md`)

```markdown
---
name: deep-reasoner
description: Use for reasoning-heavy phases: architecture, debugging complex issues, algorithm design. Think thoroughly, return a concise conclusion the orchestrator can act on.
model: opus
---

You are the deep-reasoning subagent. Read the relevant files yourself —
your context is disposable, the orchestrator's is not. Your FINAL answer
must be a concise, actionable conclusion plus a short "why". No reasoning
dumps; cite file:line instead of pasting code.
```

## 3. Subagent: fast-worker (`.claude/agents/fast-worker.md`)

```markdown
---
name: fast-worker
description: Use for mechanical tasks: boilerplate, tests, formatting, simple edits. Execute efficiently.
model: sonnet
---

You are the fast-worker subagent. Execute efficiently; do not over-think
or expand scope — if the task needs a design decision, stop and report
back. FINAL answer = terse completion report: files changed + anything
that failed.
```

## 4. Codex plugin (optional but recommended)

Prerequisite: `npm install -g @openai/codex` and log in. Then in Claude Code:

```
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/codex:setup
```

## 5. CLAUDE.md orchestration section (paste verbatim)

```markdown
## Orchestration workflow

You (Fable) are the orchestrator. Plan, decompose, synthesize.
Reasoning-heavy phases → deep-reasoner
Mechanical work → fast-worker
Codex (/codex:rescue --background) is a cracked engineer on par with
deep-reasoner, from a different perspective. Treat as a peer, not a
reviewer.
High-stakes decisions: task Opus + Codex on the same problem in
parallel, synthesize the best of both, without showing either the
other's answer. Keep your own context lean.
```

Verification: run a small task with the tech-lead template and confirm
(a) a plan appears before execution, (b) Task(deep-reasoner)/Task(fast-worker)
calls actually happen, (c) subagent reports are terse.

---
name: multi-model-orchestrator
description: >-
  Run complex development tasks as a multi-model orchestration: the main model
  (Fable 5, max reasoning) acts as tech-lead orchestrator and delegates to
  three executors — deep-reasoner (Opus) for reasoning-heavy phases,
  fast-worker (Sonnet) for mechanical work, and Codex CLI as a peer senior
  engineer with a different perspective. Saves orchestrator (Fable) token usage
  while keeping quality high. TRIGGER whenever the user asks to "orchestrate"
  a task, says "用 orchestrator 模式", "分派給 subagent", "delegate this",
  "當 tech lead", "act as tech lead", "省 Fable 用量", "multi-model", "讓 Opus
  想、Sonnet 做", or gives any large multi-phase coding task (new feature,
  refactor, complex debugging, architecture + implementation) where planning,
  deep reasoning, and mechanical execution can be split. Also trigger when the
  user asks for a "second opinion from Codex" or wants two models to solve the
  same problem in parallel.
---

# Multi-Model Orchestrator

You are the **orchestrator** — a tech lead, not an individual contributor.
Your job: plan, decompose, delegate, synthesize, verify. Your context and
tokens are the most expensive resource in this setup, so you delegate all
heavy lifting and keep only conclusions.

## The team

| Role | Who | Use for |
|------|-----|---------|
| Orchestrator | You (main model, max reasoning) | Planning, decomposition, synthesis, final decisions |
| deep-reasoner | Opus subagent | Architecture, complex debugging, algorithm design, tricky trade-offs |
| fast-worker | Sonnet subagent | Boilerplate, tests, formatting, simple/mechanical edits, repetitive changes |
| Codex peer | `/codex:rescue --background` (Codex CLI) | Fresh-perspective problems; a peer senior engineer, NOT a reviewer |

## Workflow

1. **Show your plan first.** Before executing, present a short numbered plan
   with, per step: which executor gets it and how you'll verify it. Wait for
   the user's go-ahead unless they already said to proceed.
2. **Decompose by cognitive load, not by file.** Ask of each step: does this
   need deep thought (→ deep-reasoner), or is the answer already obvious and
   it just needs doing (→ fast-worker)? If a step mixes both, split it.
3. **Delegate with tight briefs.** Every delegation includes: goal, relevant
   files/constraints, expected output format, and the instruction to return a
   *concise conclusion you can act on* — not a full exploration transcript.
4. **Synthesize and verify yourself.** You own the final answer. Check that
   delegated results are consistent with each other and with the codebase
   before integrating.

## Delegation briefs

**To deep-reasoner** (reasoning-heavy):

```
Goal: [the decision/diagnosis needed]
Context: [files, constraints, what's been tried]
Think thoroughly, but return a concise conclusion the orchestrator can act
on: recommendation + key reasoning + risks. No need to show all work.
```

**To fast-worker** (mechanical):

```
Task: [exact change, exact files]
Follow existing style. Execute efficiently — no exploration beyond what the
task needs. Report: files changed + how you verified.
```

**To Codex** (fresh perspective): run `/codex:rescue --background` with the
problem statement and context. Frame it as a peer engineer solving the
problem independently — do not tell it what deep-reasoner concluded.

## High-stakes protocol (parallel blind solve)

For decisions that are expensive to get wrong (architecture choices, subtle
production bugs, security-relevant design):

1. Give deep-reasoner AND Codex the **same problem, same context, in
   parallel** — neither sees the other's answer. Blind independence is the
   point: it surfaces disagreement you'd never see in a review chain.
2. Compare their conclusions yourself. Agreement → high confidence, proceed.
   Disagreement → the diff between their answers is exactly where the risk
   lives; resolve it explicitly (a third targeted question is fine).
3. Synthesize the best of both. Never just pick one wholesale without
   stating why.

## Context hygiene

- Keep your own context lean: request conclusions, not transcripts.
- Don't re-read files a subagent already summarized unless verifying a
  suspicious claim.
- If a subagent's answer is vague or hedged, re-delegate with a sharper
  brief rather than doing the work yourself.

## Fallbacks

- Codex CLI not installed / `/codex:rescue` fails → say so, and run the
  high-stakes protocol with two independent deep-reasoner passes (separate
  subagents, blind to each other) instead.
- Task too small to orchestrate (single obvious edit) → just do it directly
  and say orchestration wasn't warranted. Delegation overhead must pay for
  itself.

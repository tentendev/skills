---
name: token-saving-prompt-enhancer
description: Turn a rough idea, a requirements list, or a painful existing prompt into ONE copy-paste-ready "Enhanced Prompt" for the token-saving multi-model orchestration workflow in Claude Code (Fable 5 orchestrator + deep-reasoner/Opus + fast-worker/Sonnet + Codex peer). Asks 2-3 clarifying questions in Traditional Chinese, then outputs a decomposition table, token-risk analysis, an English Enhanced Prompt with a delegation plan, and usage notes. TRIGGER when the user says "幫我設計 prompt", "設計 Enhanced Prompt", "幫我拆解這個需求", "這個 prompt 很燒 token", "省 token 的 prompt", "幫我把想法變成 prompt", "orchestration prompt", "enhance this prompt for Claude Code", or pastes an idea / feature request / bug description / existing prompt and asks how to run it efficiently in Claude Code. Also trigger on mentions of deep-reasoner, fast-worker, Codex delegation, or the tech-lead prompt template. NOT for general prompt polishing unrelated to Claude Code (use prompt-optimizer).
---

# Token-Saving Prompt Enhancer

Design prompts for a Claude Code setup where the most expensive model only
directs, and cheaper models do the work. A well-designed Enhanced Prompt can
cut token spend dramatically; a naive one silently burns the orchestrator's
context. Your job is to make the efficient version the default.

## The target workflow you are designing for

The user's Claude Code is configured as:

| Role | Model | Duty |
|------|-------|------|
| Orchestrator | Fable 5 (max reasoning) | Plan, decompose, synthesize. Context must stay lean — it is the most expensive model. |
| deep-reasoner subagent | Opus | Architecture, complex debugging, algorithm design. Returns concise conclusions only. |
| fast-worker subagent | Sonnet | Boilerplate, tests, formatting, simple multi-file edits. |
| Codex (`/codex:rescue --background`) | OpenAI Codex CLI | Peer senior engineer with a different perspective — a peer, not a reviewer. Costs zero Claude tokens. |

High-stakes (hard-to-reverse) decisions get the double-blind pattern: task
deep-reasoner + Codex on the same problem in parallel, neither sees the
other's answer, the orchestrator synthesizes the best of both.

If the user seems not to have this workflow set up yet, point them to
`references/workflow-setup.md` (or offer to walk them through it) before
designing prompts that assume it.

## Input types you will receive

- **A. Rough idea** — "我想做一個…". Mostly underspecified; clarify first.
- **B. Requirements** — a feature list, bug description, or refactor goal.
- **C. Prompt pain points** — an existing prompt that wastes tokens, drifts
  off-goal, or produces bloated output. Diagnose before rewriting.

## Five token-saving principles — encode ALL of them in every Enhanced Prompt

These are the reason this skill exists. A prompt missing any of them leaks
tokens in a way the user won't notice until the bill arrives:

1. **The orchestrator never reads large files or logs itself.** Subagents
   read and return summaries with file:line references. Reading a 2,000-line
   file in the orchestrator poisons its context for the rest of the session.
2. **Every delegated subtask specifies a return format with a size cap**
   (e.g. "return: root cause + fix location, ≤10 lines"). Without a cap,
   Opus happily returns its whole chain of reasoning.
3. **Plan-first gate**: require "Show me your plan first, then execute."
   A wrong direction caught at plan time costs hundreds of tokens; caught
   mid-execution it costs tens of thousands.
4. **Right-sized delegation**: reasoning → deep-reasoner; mechanical →
   fast-worker; fresh perspective → Codex. Never send mechanical work to
   Opus (wasteful) or design decisions to Sonnet (unreliable).
5. **Verifiable success criteria** so the orchestrator can loop and verify
   on its own instead of coming back to ask.

## Workflow

### Phase 1 — Clarify (default)

Ask at most 2–3 questions, all in ONE message, in Traditional Chinese
(Taiwan usage). Only ask what actually changes the design: success criteria,
scope/constraints, which files or systems are involved, and whether any
decision is high-stakes. Anything you can reasonably assume, assume it and
label it as an assumption later — the user came here to save tokens, not to
fill in a questionnaire. If the input is already fully specified, skip
Phase 1 entirely.

### Phase 2 — Decompose & design

Output exactly these four sections, in this order:

**1. 拆解摘要** (Traditional Chinese)
- 目標（一句話）＋ 成功標準
- 已知限制／假設（自行假設的部分標明「假設」）
- 子任務分派表：| 子任務 | 派給誰 | 為什麼 | 回傳格式與上限 |

**2. Token 風險點** (Traditional Chinese)
2–4 bullets: where this task would waste tokens if prompted naively, and
which guardrail in your Enhanced Prompt closes each hole.

**3. Enhanced Prompt** (English, ONE fenced code block)
Fill this skeleton concretely — no placeholders left behind:

```
Goal: <one-sentence goal + measurable success criteria>
Context: <files, systems, constraints, what NOT to touch>

You're the lead. Show me your plan first, then execute.

Delegation plan:
- <subtask> → deep-reasoner: <instruction>
  (return: <format>, ≤<N> lines)
- <subtask> → fast-worker: <instruction>
  (return: files changed + anything that failed)
- <subtask> → Codex (/codex:rescue --background): <instruction>
[If a high-stakes decision exists:]
- High-stakes — <decision>: task deep-reasoner + Codex in parallel,
  double-blind; synthesize the best of both.

Execution rules:
- Never read large files yourself; delegate reading + summarizing.
- Subagents return conclusions only, within their line caps.
- Keep your own context lean.

Verify: <concrete verification step(s)>
Deliverable: <what the user should receive at the end>
```

**4. 使用提醒** (Traditional Chinese)
1–3 short notes: when to re-run, what to watch during execution, or which
assumption to correct if wrong.

### Special case: input type C (existing-prompt pain points)

Add a「問題診斷」section BEFORE the 拆解摘要: quote the 2–4 worst spots of
the original prompt and name which of the five principles each violates.
Seeing the diagnosis teaches the user to spot these patterns themselves —
that's worth more than the rewrite. Then rewrite as usual.

## Example

**Input (type A):**
> 我想幫我的 Ghost 部落格做「相關文章推薦」區塊，不用外部付費服務。

**Phase 1 (one message):** 推薦邏輯偏好？文章數量級？原生 theme 還是 headless？

**Phase 2 (after answers):** 拆解摘要 assigns 演算法選型 → deep-reasoner
(return: 建議方案＋理由, ≤12 lines), API 讀取與快取實作 → fast-worker,
設計盲點 → Codex (return: risk list, ≤8 lines); Token 風險點 flags "letting
the orchestrator read all post content would blow its context → fast-worker
builds a summary index instead"; then the filled-in English Enhanced Prompt;
then 使用提醒 e.g. "文章超過 500 篇時改用 embedding 方案，需重產 Prompt".

## Rules

- All explanation, questions, and analysis: Traditional Chinese (Taiwan
  usage). The Enhanced Prompt itself: English, in a single code block —
  it matches the workflow's original template and executes most reliably.
- Simplicity first: if the task is too small to justify orchestration
  (a one-file trivial edit), say so plainly and give a short single-model
  prompt instead. Forcing delegation onto trivial work costs MORE tokens
  than it saves — the opposite of this skill's purpose.
- One Enhanced Prompt per request; offer variants only if asked.
- Never invent files, APIs, or constraints the user didn't mention. Mark
  values only the user can supply as `<fill in>` and list them explicitly
  in 使用提醒.

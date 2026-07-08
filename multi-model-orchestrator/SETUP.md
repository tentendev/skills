# Setup — Multi-Model Orchestrator (Claude Code)

## 1. 主模型設為 Fable 5、reasoning 開到 max

在 Claude Code 中：

```
/model        → 選 Fable 5
/effort       → max
```

## 2. 安裝兩個 subagent

把 `agents/deep-reasoner.md` 和 `agents/fast-worker.md` 複製到：

- 全域：`~/.claude/agents/`
- 或單一專案：`<project>/.claude/agents/`

（也可用 `/agents` 介面手動建立，內容照抄兩個檔案。）

## 3. 安裝 Codex plugin

先在電腦上安裝 OpenAI Codex CLI（`npm install -g @openai/codex` 並登入），
然後在 Claude Code 輸入：

```
/plugin marketplace add openai/codex-plugin-cc
/plugin install codex@openai-codex
/codex:setup
```

## 4. 安裝 skill

把 `multi-model-orchestrator/` 資料夾複製到：

- 全域：`~/.claude/skills/multi-model-orchestrator/`
- 或單一專案：`<project>/.claude/skills/multi-model-orchestrator/`

## 5. 把 orchestration 規則寫進 CLAUDE.md

把 `CLAUDE-md-snippet.md` 的「Orchestration workflow」段落貼到專案的
`CLAUDE.md`。skill 負責「怎麼做」，CLAUDE.md 片段確保每個 session 都
記得「預設就這樣做」。

## 6. 使用（tech-lead prompt 模板）

```
Goal: [你要的結果]
Context: [檔案、限制條件]
You're the lead. Delegate reasoning to deep-reasoner, grunt work to
fast-worker, fresh-perspective problems to Codex. Show me your plan first,
then execute.
```

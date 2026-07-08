# Tenten Skills

[English](./README.md) · **繁體中文** · [简体中文](./README.zh-CN.md) · [日本語](./README.ja.md)

[Tenten](https://tenten.co) 每天實際在用的 Agent Skills，公開分享。

## 為什麼做這個 repo

在 Tenten，我們相信 Agent Skills 正在成為知識分享的新單位。一個寫得好的 skill，能把有經驗的人處理一件事的方式——流程、判斷、地雷——封裝成任何 AI agent 都能重複執行的形式。

我們在內部為真實工作設計、測試、迭代這些 skills：工程、專案管理、行銷、內容、客戶交付。這個 repo 是這些成果的公開長期紀錄——之後所有能公開的 skills 都會持續收錄到這裡，讓這個集合不斷成長。

公開分享，是因為 skills 在開放環境中會變得更好。歡迎 fork、改成適合你們技術棧的版本，也歡迎回報問題。

## 快速開始

用 [skills.sh](https://skills.sh) 安裝器：

```bash
npx skills@latest add tentenco/skills
```

或手動安裝——把任一 skill 資料夾複製到 agent 的 skills 目錄：

```bash
# Claude Code（全域）
cp -r skills/engineering/multi-model-orchestrator ~/.claude/skills/

# 或單一專案
cp -r skills/engineering/multi-model-orchestrator <project>/.claude/skills/
```

部分 skills 附帶額外元件（subagent 定義、CLAUDE.md 片段）。需要額外步驟的 skill 資料夾內都有 `SETUP.md`。

## Repo 結構

```
skills/
└── <category>/                  # engineering、pm、marketing、productivity…
    └── <skill-name>/
        ├── SKILL.md             # skill 本體（frontmatter + 指令）
        ├── SETUP.md             # 安裝步驟（如有需要）
        ├── agents/              # 附帶的 subagent 定義（如有）
        ├── scripts/             # 附帶的可執行工具（如有）
        └── 使用說明.html         # 繁體中文使用說明，含可複製的 prompt 範例
```

每個 skill 都是自包含的：一個資料夾、所需的一切，加上一份含 prompt 範例、真人可讀的使用說明。

## Skills 目錄

### Engineering（工程）

| Skill | 說明 |
|-------|------|
| [multi-model-orchestrator](./skills/engineering/multi-model-orchestrator/SKILL.md) | 把複雜開發任務交給多模型團隊執行：主模型擔任 tech-lead 指揮者，深度推理派給 Opus subagent、機械性工作派給 Sonnet subagent、需要不同視角的問題交給 Codex 盲測同儕。省下頂級模型用量，品質不打折。 |

*之後發布更多 skills 時，會陸續增加 PM、行銷、生產力等分類。*

## 授權

MIT — 見 [LICENSE](./LICENSE)。

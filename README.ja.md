# Tenten Skills

[English](./README.md) · [繁體中文](./README.zh-TW.md) · [简体中文](./README.zh-CN.md) · **日本語**

[Tenten](https://tenten.co) が日々の実務で使っている Agent Skills を、パブリックに公開しています。

## このリポジトリの目的

Tenten では、Agent Skills がナレッジ共有の新しい単位になりつつあると考えています。よく書かれた skill は、経験者の仕事の進め方——プロセス、判断基準、落とし穴——を、どんな AI エージェントでも再現可能な形にパッケージ化したものです。

私たちは、エンジニアリング、プロジェクト管理、マーケティング、コンテンツ制作、クライアントワークといった実務のために、社内で skills を設計・テスト・改善しています。このリポジトリはその成果の公開アーカイブです。今後、公開可能なすべての skills を継続的にここへ記録していくため、コレクションは増え続けます。

公開する理由はシンプルで、skills はオープンな場でこそ磨かれるからです。ぜひ fork して、自分たちのスタックに合わせて改変し、問題があれば教えてください。

## クイックスタート

[skills.sh](https://skills.sh) インストーラーを使う場合：

```bash
npx skills@latest add tentenco/skills
```

手動インストールの場合——任意の skill フォルダをエージェントの skills ディレクトリへコピーします：

```bash
# Claude Code（グローバル）
cp -r skills/engineering/multi-model-orchestrator ~/.claude/skills/

# プロジェクト単位
cp -r skills/engineering/multi-model-orchestrator <project>/.claude/skills/
```

一部の skills には追加コンポーネント（subagent 定義、CLAUDE.md スニペット）が付属します。追加手順が必要な skill のフォルダには `SETUP.md` があります。

## リポジトリ構成

```
skills/
└── <category>/                  # engineering、pm、marketing、productivity…
    └── <skill-name>/
        ├── SKILL.md             # skill 本体（frontmatter + 指示）
        ├── SETUP.md             # インストール手順（必要な場合）
        ├── agents/              # 付属の subagent 定義（ある場合）
        ├── scripts/             # 付属の実行スクリプト（ある場合）
        └── 使用說明.html         # 繁体字中国語の使用ガイド（プロンプト例付き）
```

各 skill は自己完結型です：1 フォルダに必要なものすべてと、コピペで使えるプロンプト例付きのガイドが入っています。

## Skills カタログ

### Engineering（エンジニアリング）

| Skill | 説明 |
|-------|------|
| [multi-model-orchestrator](./skills/engineering/multi-model-orchestrator/SKILL.md) | 複雑な開発タスクをマルチモデルのチームで実行：メインモデルがテックリードとして指揮を執り、深い推論は Opus subagent へ、機械的な作業は Sonnet subagent へ、別視点が必要な問題は Codex にブラインドで委任。トップティアモデルの使用量を節約しつつ品質を維持します。 |

*今後 skills の公開に合わせて、PM・マーケティング・生産性などのカテゴリを追加していきます。*

## ライセンス

MIT — [LICENSE](./LICENSE) を参照。

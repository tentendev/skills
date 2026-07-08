# Tenten Skills

**English** · [繁體中文](./README.zh-TW.md) · [简体中文](./README.zh-CN.md) · [日本語](./README.ja.md)

Agent Skills we build and use every day at [Tenten](https://tenten.co) — shared in public.

## Why this repo exists

At Tenten, we believe Agent Skills are becoming the new unit of shared know-how. A well-written skill captures how an experienced person approaches a task — the process, the judgment calls, the pitfalls — in a form any AI agent can execute repeatably.

We design, test, and iterate on skills internally for our real work: engineering, project management, marketing, content, and client delivery. This repo is our public, permanent record of that work. Every skill we're able to share will land here over time, so the collection keeps growing.

We share them because skills get better in the open. Fork them, adapt them to your own stack, and tell us what breaks.

## Quickstart

Install with the [skills.sh](https://skills.sh) installer:

```bash
npx skills@latest add tentenco/skills
```

Or install manually — copy any skill folder into your agent's skills directory:

```bash
# Claude Code (global)
cp -r skills/engineering/multi-model-orchestrator ~/.claude/skills/

# or per-project
cp -r skills/engineering/multi-model-orchestrator <project>/.claude/skills/
```

Some skills ship extra pieces (subagent definitions, CLAUDE.md snippets). Each skill folder has a `SETUP.md` when extra steps are needed.

## Repo structure

```
skills/
└── <category>/                  # engineering, pm, marketing, productivity…
    └── <skill-name>/
        ├── SKILL.md             # the skill itself (frontmatter + instructions)
        ├── SETUP.md             # install steps, if any
        ├── agents/              # bundled subagent definitions, if any
        ├── scripts/             # bundled executable helpers, if any
        └── 使用說明.html         # Traditional Chinese usage guide with prompt examples
```

Every skill is self-contained: one folder, everything it needs, plus a human-readable guide with copy-paste prompt examples.

## Skills catalog

### Engineering

| Skill | Description |
|-------|-------------|
| [multi-model-orchestrator](./skills/engineering/multi-model-orchestrator/SKILL.md) | Run complex dev tasks as a multi-model team: the main model acts as tech-lead orchestrator, delegating deep reasoning to an Opus subagent, mechanical work to a Sonnet subagent, and fresh-perspective problems to Codex as a blind peer. Saves top-tier model usage without losing quality. |

*More categories (PM, marketing, productivity) will appear as we publish more skills.*

## License

MIT — see [LICENSE](./LICENSE).

# claude-autonomous-kit

**Give Claude Code persistent memory across sessions. No code required.**

Claude Code is powerful, but every time you close the terminal, it forgets everything. Your context, decisions, and progress — gone. You spend the first 10 minutes of every session re-explaining what happened yesterday.

This template fixes that with a minimal file structure that turns Claude Code from a disposable chat into a **project companion that remembers**.

## The Problem

```
Monday:    "Hey Claude, let's continue the blog migration"
           → "I don't have any context about a blog migration.
              Could you fill me in?"
           → 10 minutes re-explaining everything

Tuesday:   Same thing again.

Wednesday: You give up and just start from scratch each time.
```

## The Fix

```
Monday:    /onboard → Claude reads memory, picks up instantly
           Work normally
           /handover → Claude saves progress

Tuesday:   /onboard → "We migrated 3 of 8 pages yesterday.
                        The auth page had CSS issues. Want to tackle that?"
```

## Why Not Just Use Claude's Built-in Memory?

Claude Code has `~/.claude/` auto-memory, but it's designed for **user preferences** (tone, style, habits) — not **project state**. Here's how they differ:

| | Built-in Memory | This Kit |
|---|---|---|
| Lives in | `~/.claude/` (your machine) | Your repo (portable, shareable) |
| Stores | User preferences, feedback | Project state, decisions, learnings |
| Shared with team | No | Yes (it's just files in git) |
| Structured workflow | No | /onboard + /handover |
| Works across machines | No | Yes (it's in the repo) |

**They're complementary.** Use built-in memory for your preferences. Use this kit for project continuity.

## How It Works

```
your-project/
├── CLAUDE.md                 ← Instructions for Claude (auto-loaded every session)
├── memory/
│   ├── handovers/            ← Session notes (short-term memory)
│   ├── learnings/            ← Lessons learned (long-term memory)
│   └── decisions/            ← Decision log with reasoning
└── .claude/
    └── skills/
        ├── onboard/SKILL.md  ← /onboard: load memory at session start
        └── handover/SKILL.md ← /handover: save progress at session end
```

That's it. No dependencies. No build step. Just Markdown files that Claude reads and writes.

## Quick Start

```bash
# Clone the template
git clone https://github.com/uzucky/claude-autonomous-kit.git my-project
cd my-project

# Edit CLAUDE.md with your project details
# (English template: CLAUDE.example.md, Japanese: CLAUDE.md)

# Start Claude Code and load memory
claude
/onboard
```

Or click **"Use this template"** on GitHub to create your own repo.

## What's Inside

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Project instruction manual for Claude. Edit this first. |
| `CLAUDE.example.md` | English template with guided comments |
| `/onboard` | Slash command — reads memory at session start |
| `/handover` | Slash command — saves progress at session end |
| `memory/handovers/` | Short-term: what happened last session |
| `memory/learnings/` | Long-term: patterns, preferences, domain knowledge |
| `memory/decisions/` | Audit trail: what was decided and why |

## Who Is This For?

- **Solo developers** who want session continuity without setup
- **Non-engineers** using Claude Code for writing, research, project management
- **Teams** who want AI to maintain shared institutional knowledge
- **Anyone** tired of re-explaining context every session

## Customization

**Add your own rules** to `CLAUDE.md`:
```markdown
## Rules
- Always respond in English
- Never deploy without asking first
- Keep responses under 200 words
```

**Add slash commands** — create `.claude/skills/your-skill/SKILL.md`:
```markdown
# /weekly-report
Compile this week's progress from memory/handovers/ into a summary.
Focus on completed items, blockers, and next steps.
```

**Add memory categories** — create folders under `memory/`:
```
memory/
├── handovers/
├── learnings/
├── decisions/
├── research/      ← your custom category
└── contacts/      ← your custom category
```

## Built From Production

This isn't theoretical. It's extracted from an autonomous AI business system that has run 24/7 for 3 months — managing trading bots, publishing 50+ articles, and shipping 6 products across hundreds of Claude Code sessions.

The memory architecture is what makes session-to-session continuity work. We stripped it down to the essential pattern anyone can use.

[Read more about the system](https://note.com/suzuidaki)

## License

MIT — use it however you want.

---

<details>
<summary>🇯🇵 日本語</summary>

## 非エンジニアのためのClaude Codeテンプレート

**Claude Codeを「使い捨てチャット」から「育つ相棒」に変える、最小構成のテンプレートです。**

### これは何？

Claude Codeは、ターミナルで動くAIアシスタントです。
普通に使うと、セッションが切れるたびにAIは全てを忘れます。

このテンプレートは、**AIに「記憶」を持たせる仕組み**です。

- `CLAUDE.md` = AIへの指示書（毎回自動で読み込まれる）
- `memory/` = AIの記憶置き場（セッションをまたいで引き継がれる）
- `.claude/skills/` = AIに教えた「やり方」（コマンドで呼び出せる）

### なぜClaude標準のメモリだけでは不十分？

Claudeは `~/.claude/` に自動メモリを持っていますが、それは**ユーザーの好み**（口調、スタイル）用です。**プロジェクトの状態**（昨日何をしたか、何を決めたか）は保持しません。

このキットは「プロジェクトの記憶」をリポジトリ内に保存します。チームで共有でき、マシンを変えても引き継げます。

### セットアップ

```bash
git clone https://github.com/uzucky/claude-autonomous-kit.git my-project
cd my-project
# CLAUDE.mdを編集して、claudeコマンドで起動
```

### 使い方

1. `/onboard` でセッション開始（記憶を読み込む）
2. 普通にAIと会話・作業
3. `/handover` でセッション終了（記憶を保存）

</details>

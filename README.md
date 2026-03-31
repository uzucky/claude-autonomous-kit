# claude-autonomous-kit

**Give Claude Code persistent memory across sessions. No code required.**

Claude Code is powerful, but every time you close the terminal, it forgets everything. Your context, decisions, and progress — gone.

This template fixes that with a minimal file structure that turns Claude Code from a disposable chat into a **project companion that remembers**.

## The Problem

```
Monday:    "Hey Claude, let's work on my blog migration"
           → Great session, lots of progress

Tuesday:   "Let's continue the blog migration"
           → "I don't have any context about a blog migration.
              Could you fill me in?"
           → 20 minutes re-explaining everything
```

## The Fix

```
Monday:    /onboard → Claude reads memory, picks up where you left off
           Work normally
           /handover → Claude saves progress

Tuesday:   /onboard → "I see we migrated 3 of 8 pages yesterday.
                        The auth page had CSS issues. Let's tackle that first."
```

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
# (replace the <!-- comments --> with your info)

# Start Claude Code
claude

# Load memory
/onboard
```

Or click **"Use this template"** on GitHub to create your own repo.

## What's Inside

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Your project's instruction manual for Claude. Edit this first. |
| `/onboard` | Slash command that reads all memory files at session start |
| `/handover` | Slash command that saves session progress for next time |
| `memory/handovers/` | Short-term: what happened last session |
| `memory/learnings/` | Long-term: patterns, preferences, domain knowledge |
| `memory/decisions/` | Audit trail: what was decided and why |

## Who Is This For?

- Anyone using Claude Code (Pro or Max plan)
- Non-engineers who want structured AI collaboration
- Researchers, writers, project managers, freelancers
- Teams who want AI to maintain institutional knowledge

## Customization

**Add instructions** to `CLAUDE.md`:
```markdown
## Rules
- Always respond in English
- Never write code without asking first
- Keep responses under 200 words
```

**Add skills** — create `.claude/skills/your-skill/SKILL.md`:
```markdown
# /weekly-report
Compile this week's progress from memory/handovers/ into a summary.
```

**Add memory categories** — create new folders under `memory/`:
```
memory/
├── handovers/
├── learnings/
├── decisions/
├── research/      ← your custom category
└── contacts/      ← your custom category
```

## Built From Production

This isn't theoretical. It's extracted from an [autonomous AI business system](https://note.com/suzuidaki) that runs 24/7 — managing trading bots, content publishing, and product development with persistent memory across hundreds of sessions.

We stripped it down to the essential pattern that works for any project.

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

詳しくは上の英語セクションを参照してください。

</details>

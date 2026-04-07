# Claude Autonomous Kit

**Run Claude Code 24/7 on your Mac.** A battle-tested framework for autonomous AI operations, built by a non-engineer who runs 26 automated systems with 2 hours of daily oversight.

## What This Is

A complete framework that turns your Mac into an always-on AI workstation. Claude Code runs autonomously on a schedule, picks up tasks from GitHub Issues, manages its own git workflow, monitors its own health, and hands off context between sessions — all without human intervention.

```
You (2 hrs/day) ──── GitHub Issues ────► Claude Code (22 hrs/day)
                         ▲                      │
                         │                      ▼
                    AI Reports ◄──── Commits, PRs, Results
```

## Architecture

Three layers, each with different failure modes and recovery patterns:

```
Layer 3: AI Execution (session-based, ephemeral)
  autonomous.sh    → scheduled daily Claude Code session
  check_tasks.sh   → polls GitHub Issues every 30min, triggers sessions

Layer 2: Maintenance Daemons (always-on)
  git_hygiene.sh   → cleans branches, protects uncommitted work
  healthcheck.sh   → monitors everything, creates alerts as GitHub Issues
  nosleep          → keeps Mac awake (caffeinate + pmset)

Layer 1: Your Daemons (add your own)
  your-bot.sh      → whatever you want to run on a schedule
```

## Key Innovations

### Safety Commits (not stash)
Git stash accumulates and gets lost. Safety commits go to dedicated branches, visible in git history, impossible to lose.

### Append-Only Action Log
`action_registry.py` writes JSONL. No merge conflicts (`.gitattributes merge=union`). Idempotency checking prevents double-execution.

### Inter-Session Task Queue
`task_queue_manager.py` survives session crashes. Tasks auto-retry with exponential backoff. Priority-based scheduling.

### Conflict-Aware Auto-Merge
PRs auto-merge via GitHub API. On conflict: auto-rebase with file-specific resolution rules. Falls back gracefully.

### Session Handover Hooks
`session-end.sh` extracts structured data from transcripts (not AI summaries). `session-start.sh` injects it into the next session. Zero context loss.

### Scope Guard
Protects immutable files from AI modification. Hard blocks via `exit 2` in PreToolUse hooks.

## Quick Start

### Prerequisites
- macOS (uses launchd for scheduling)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed (`npm install -g @anthropic-ai/claude-code`)
- A GitHub repository with a Personal Access Token
- Claude Max subscription or API key

### Installation

```bash
# 1. Clone this repo (or copy files into your project)
git clone https://github.com/YOUR_USER/claude-autonomous-kit.git
cd claude-autonomous-kit

# 2. Copy the framework into your project
cp -r scripts/ /path/to/your/repo/autonomous/
cp -r hooks/ /path/to/your/repo/.claude/hooks/
cp -r utils/ /path/to/your/repo/autonomous/
cp templates/CLAUDE.md.template /path/to/your/repo/CLAUDE.md

# 3. Configure
cp .env.example /path/to/your/repo/.env
# Edit .env with your GitHub PAT and repo details

# 4. Edit CLAUDE.md to match your project
# (This is your AI's instruction manual — customize it)

# 5. Run setup
cd /path/to/your/repo
bash autonomous/setup.sh
```

### Manual Test Run

```bash
# Run one autonomous session manually
bash autonomous/autonomous.sh

# Check task queue
python3 autonomous/task_queue_manager.py list /path/to/your/repo

# View health status
bash autonomous/healthcheck.sh
```

## File Reference

### Scripts (`scripts/`)

| File | Purpose | Runs |
|------|---------|------|
| `autonomous.sh` | Main execution engine. Pulls latest, runs `claude -p`, commits, creates PR, auto-merges | Daily 7AM + on-demand |
| `check_tasks.sh` | Polls GitHub Issues with `ai-task` label. Triggers `autonomous.sh` when found | Every 30 min |
| `healthcheck.sh` | Monitors all infrastructure. Creates GitHub Issues on failure | Every hour |
| `git_hygiene.sh` | Cleans stale branches, protects uncommitted work, syncs master | Every 2 hours |
| `setup.sh` | One-time installer. Registers all launchd jobs | Manual, once |

### Hooks (`hooks/`)

| File | Event | Purpose |
|------|-------|---------|
| `session-start.sh` | SessionStart | Injects state, handover, and weekly protocol triggers |
| `session-end.sh` | SessionEnd | Extracts structured handover from transcript |
| `scope-guard.sh` | PreToolUse (Edit/Write) | Blocks modification of protected files |

### Utilities (`utils/`)

| File | Purpose |
|------|---------|
| `action_registry.py` | Append-only action log. Idempotency checking. JSONL format |
| `task_queue_manager.py` | Priority task queue with claim/complete/interrupt/retry |

### Templates (`templates/`)

| File | Purpose |
|------|---------|
| `CLAUDE.md.template` | AI instruction file — customize for your project |
| `prompt.md.template` | Headless session prompt — what Claude does each run |
| `SESSION_STATE.md.template` | Session state file — current tasks and context |

### LaunchAgent Plists (`plists/`)

| File | Schedule | Purpose |
|------|----------|---------|
| `com.autonomous.main.plist` | Daily 7:00 AM | Main AI session |
| `com.autonomous.checker.plist` | Every 30 min | Issue polling |
| `com.autonomous.healthcheck.plist` | Every hour | Health monitoring |
| `com.autonomous.git-hygiene.plist` | Every 2 hours | Git maintenance |
| `com.autonomous.nosleep.plist` | Always-on | Prevent Mac sleep |

## How It Works

### Daily Flow

```
7:00 AM  ─► autonomous.sh starts
              ├─ Safety commit any dirty state
              ├─ Clean stale branches
              ├─ git pull --ff-only
              ├─ Claim highest-priority task from queue
              ├─ Run: claude -p <prompt> --max-turns 35 --max-budget-usd 5
              ├─ Create feature branch + PR
              ├─ Auto-merge (squash)
              └─ Return to master, clean state

Every 30m ─► check_tasks.sh
              ├─ Check GitHub Issues with 'ai-task' label
              ├─ If found + cooldown passed → run autonomous.sh
              ├─ If no tasks + idle > 8hrs → run autonomous.sh (surplus mode)
              └─ Otherwise → exit silently

Every 1h  ─► healthcheck.sh
              ├─ Check last execution time
              ├─ Check for hung processes
              ├─ Check daemon health
              └─ Alert via GitHub Issue if problems found

Every 2h  ─► git_hygiene.sh
              ├─ Delete stale claude/* branches
              ├─ Protect uncommitted changes
              └─ Sync master
```

### Communication: GitHub Issues as Message Board

- **`ai-task` label**: Instructions TO the AI. Create an Issue with this label, and the next `check_tasks.sh` run will trigger an autonomous session to handle it.
- **`ai-report` label**: Reports FROM the AI. Health alerts, completion reports, proposals.

### Budget Protection

Runs are budget-capped to protect your Claude subscription:
- Day sessions: $5 max, 35 turns
- Night sessions: $8 max, 50 turns
- Monthly: ~$100 total (fits Claude Max)

## Configuration

### Environment Variables (`.env`)

```bash
GITHUB_PERSONAL_ACCESS_TOKEN=ghp_xxxxx  # Required for PR creation
GITHUB_REPO=your-user/your-repo         # Your repository
```

### Customization Points

1. **`CLAUDE.md`**: Your AI's instruction manual. Define what it should do, what it shouldn't touch, and how it should communicate.

2. **`prompt.md`**: The prompt sent to `claude -p` each session. Define the autonomous behavior here.

3. **`SESSION_STATE.md`**: Current state file. Updated in real-time during sessions. The AI reads this at startup to know what to do.

4. **Budget/Turns**: Edit `autonomous.sh` variables `MAX_BUDGET` and `MAX_TURNS`.

5. **Schedule**: Edit plist files for different timing (see `setup.sh`).

6. **Protected Files**: Edit `scope-guard.sh` to guard additional files.

## Weekly Protocols (Optional)

The framework supports day-of-week triggers via `session-start.sh`. Define recurring tasks:

```bash
# In session-start.sh:
case $DOW in
  1) PROTOCOL_MSG="Monday: Run optimization protocol" ;;
  3) PROTOCOL_MSG="Wednesday: Analyze metrics and improve" ;;
  7) PROTOCOL_MSG="Sunday: Self-improvement and maintenance" ;;
esac
```

Create protocol files (e.g., `protocols/monday.md`) with step-by-step instructions the AI follows autonomously.

## FAQ

**Q: Does this work on Linux?**
A: The scripts are macOS-specific (launchd, caffeinate, pmset). For Linux, replace launchd with systemd/cron and remove sleep prevention.

**Q: How much does it cost?**
A: With Claude Max ($100/month), the default budget caps keep total spend within the subscription. With API keys, expect ~$3-8/day depending on session count.

**Q: Can the AI break things?**
A: The framework has multiple safety layers: budget caps, turn limits, scope guards, safety commits, and auto-merge (never force-push to main). But always review PRs initially until you trust the setup.

**Q: What if a session crashes?**
A: `task_queue_manager.py` marks the task as interrupted and re-queues it at highest priority. `git_hygiene.sh` protects any uncommitted work. `healthcheck.sh` alerts you if sessions stop running.

## Origin

This framework was extracted from a production system that runs 26 autonomous bots (trading, content, monitoring, optimization) on a MacBook Air 2015 with macOS 11. The owner — a non-engineer — manages it with 2 hours of daily smartphone oversight. The AI handles the other 22 hours.

## License

MIT

# Project Instructions

<!-- Replace this with your project name (e.g., "My Freelance Business") -->

---

## About This Project

<!-- Describe your project in 1-3 sentences. -->
<!-- Example: "A personal blog focused on productivity and tech reviews." -->

---

## Instructions for You (the AI)

- Respond in English
- If you use technical terms, add a brief explanation
- When unsure, ask — don't guess and proceed
- Record important learnings and decisions in `memory/`

<!-- Add your own rules below -->
<!-- Examples: -->
<!-- - Keep responses under 200 words -->
<!-- - Never push code without asking first -->
<!-- - Use casual tone, not corporate-speak -->

---

## Directory Structure

```
├── CLAUDE.md              ← This file (loaded automatically every session)
├── memory/
│   ├── handovers/         ← Session notes (created by /handover)
│   ├── learnings/         ← Lessons learned, patterns discovered
│   └── decisions/         ← Important decisions with reasoning
└── .claude/
    └── skills/            ← AI skills (/onboard, /handover)
```

<!-- Add project-specific folders if needed -->
<!-- Examples: drafts/, research/, output/ -->

---

## Available Commands

| Command | When to Use | What It Does |
|---------|-------------|--------------|
| `/onboard` | Start of session | Loads memory, picks up where you left off |
| `/handover` | End of session | Saves progress for next time |

---

## Principles

<!-- Add project-specific principles. Delete if not needed. -->

1. **Record everything important**: What isn't in `memory/` will be lost
2. **Ask before assuming**: Confirm unclear requirements
3. **Be direct**: Short, clear answers over verbose explanations

---

## About the Owner

<!-- Help the AI tailor its approach to you -->

- **Availability**: <!-- e.g., weekday evenings, 1 hour -->
- **Technical level**: <!-- e.g., non-engineer, knows terminal basics -->
- **Goal**: <!-- e.g., build a side income of $500/month -->

# VantEdge Skills

AI-powered development tools for the VantEdge team. One installer sets up everything — [gstack](https://github.com/garrytan/gstack) (the engineering workflow), Floki (your personal Second Brain), and our custom skills.

Works with Claude Code, the desktop app, or the VS Code extension. Non-technical team members welcome — the installer asks a few questions and handles the rest.

## Install

Open Claude Code (or a terminal) and paste:

```
git clone https://github.com/devesh-pixel/vantedge-skills.git ~/.claude/skills/vantedge && ~/.claude/skills/vantedge/setup
```

That's it. The installer will:
1. Check your system (git, bun, Claude Code)
2. Ask what you want to install (everything, just gstack, just Floki, etc.)
3. Install gstack and build the browser engine
4. Scaffold your Second Brain vault
5. Symlink all skills into Claude Code
6. Optionally set up sound notifications

## What's Inside

### Floki — Your Second Brain (8 skills)

A personal knowledge vault that lives on your computer as plain markdown files. Save what you learn, capture ideas, and search your own notes later — Claude reads and writes to it for you. Works with [Obsidian](https://obsidian.md) if you want a visual UI.

| Skill | What it does |
|---|---|
| `/dump` | Save anything to your vault — auto-categorizes, creates wiki pages, links related knowledge |
| `/idea` | Flesh out an app or product idea — asks questions, saves a structured plan |
| `/educate` | Explain any code file in plain English with line-by-line walkthrough |
| `/floki-learn` | Save what `/educate` taught you as a permanent learning note |
| `/floki-recall` | Search and chat with your Second Brain — answers from YOUR notes, not the internet |
| `/floki-todo` | Simple task management — add, complete, or list todos in your vault |
| `/floki-lint` | Audit vault health — find broken links, missing frontmatter, orphan files |
| `/brief` | Morning briefing — process inbox, summarize tasks, show recent activity |

### Custom Skills (3 skills)

| Skill | What it does |
|---|---|
| `/plan` | Create a structured implementation plan with intuitive (non-technical) + technical flow |
| **`/karpathy`** | **The most important skill.** Activates Andrej Karpathy's 4 principles for disciplined AI coding |
| `/browser-automation` | Selenium-based persistent Chrome session — launch once, connect from many scripts without re-login |

#### Why `/karpathy` matters

Without it, Claude tends to over-engineer, guess when it's unsure, add features you didn't ask for, and "improve" code it shouldn't be touching. `/karpathy` fixes all of that. Run it at the start of any serious task and Claude will:

1. **Think before coding** — state assumptions, ask when unsure, push back if a simpler approach exists
2. **Simplicity first** — minimum code that solves the problem, no speculative features, no abstractions for single-use code
3. **Surgical changes** — touch only what it must, don't "improve" adjacent code
4. **Goal-driven** — show what changed, prove the goal was met, no unnecessary output

This is the difference between Claude writing 200 lines of over-abstracted code vs. 50 clean lines that do the job.

### gstack — AI Engineering Workflow (37 skills)

An open-source toolkit by [Garry Tan](https://github.com/garrytan/gstack) (YC CEO) that gives Claude specialized roles — it can open a real browser to test your app, review code before you ship, run security audits, brainstorm product strategy, and more. Installed automatically by the setup script.

#### Idea & Strategy
| Skill | What it does |
|---|---|
| `/office-hours` | YC-style brainstorming — six forcing questions to pressure-test an idea before code |
| `/plan-ceo-review` | Founder-mode plan review — rethink the problem, find the 10-star product |
| `/plan-eng-review` | Eng manager review — lock architecture, data flow, edge cases before coding |
| `/plan-design-review` | Rate each design dimension 0-10, explain what a 10 looks like, fix the plan |
| `/plan-devex-review` | Developer experience review for APIs/CLIs/SDKs — personas, friction, benchmarks |
| `/autoplan` | Run all four reviews (CEO, design, eng, DX) back-to-back, one command |

#### Design
| Skill | What it does |
|---|---|
| `/design-consultation` | Build a complete design system from scratch — typography, color, spacing, motion |
| `/design-shotgun` | Generate multiple design variants, open a comparison board, collect feedback |
| `/design-html` | Turn an approved mockup into production HTML/CSS — zero deps, text reflows |
| `/design-review` | Designer's eye QA on a live site — finds spacing issues, hierarchy problems, fixes them |

#### Browser & Testing
| Skill | What it does |
|---|---|
| `/browse` | GStack's browser engine — navigate URLs, click, fill forms, screenshot, ~100ms/command |
| `/open-gstack-browser` | Launch a visible AI-controlled Chromium with a live activity sidebar |
| `/setup-browser-cookies` | Import cookies from your real Chrome for testing authenticated pages |
| `/pair-agent` | Give another AI agent scoped access to your browser via a setup key |
| `/qa` | Full QA loop — find bugs in a real browser, fix them in code, re-verify |
| `/qa-only` | Same QA testing but report-only — bug report with screenshots, no fixes |
| `/benchmark` | Performance regression detection — page load, Core Web Vitals, before/after |
| `/canary` | Post-deploy monitoring — watch production for errors, perf regressions |
| `/devex-review` | Live DX audit — actually tries the getting-started flow, times it, screenshots errors |

#### Code Quality & Safety
| Skill | What it does |
|---|---|
| `/review` | Pre-landing PR review — SQL safety, trust boundary violations, structural issues |
| `/investigate` | Systematic debugging — investigate, analyze, hypothesize, implement. No fixes without root cause |
| `/health` | Code quality dashboard — type checker, linter, tests, dead code — weighted 0-10 score |
| `/cso` | Chief Security Officer mode — secrets, supply chain, CI/CD, OWASP, STRIDE threat modeling |
| `/codex` | OpenAI Codex wrapper — independent code review, adversarial challenge, or consult |

#### Ship & Deploy
| Skill | What it does |
|---|---|
| `/ship` | Full ship workflow — merge base, tests, review diff, bump version, push, create PR |
| `/land-and-deploy` | Merge the PR, wait for CI, deploy, verify production health |
| `/setup-deploy` | Configure your deploy platform (Fly, Render, Vercel, etc.) for automatic deploys |
| `/document-release` | Post-ship docs update — syncs README/ARCHITECTURE/CHANGELOG to match what shipped |
| `/retro` | Weekly engineering retrospective — commit history, per-person breakdowns, trends |

#### Session & Safety
| Skill | What it does |
|---|---|
| `/careful` | Warns before destructive commands (rm -rf, DROP TABLE, force-push) |
| `/freeze` | Lock file edits to one directory — hard block, not just a warning |
| `/unfreeze` | Remove the freeze boundary |
| `/guard` | Activate both `/careful` + `/freeze` — maximum safety mode |
| `/checkpoint` | Save working state so you can resume exactly where you left off |
| `/learn` | Review, search, prune what gstack has learned across sessions |
| `/gstack-upgrade` | Upgrade gstack to latest version |

## How Floki Works

Floki is a file-based Second Brain that lives at `~/SecondBrain/` (or wherever you chose during setup). It's just markdown files in folders — works with Obsidian, VS Code, or any text editor.

### Vault Structure

```
SecondBrain/
├── _schema/          ← rules for how content is organized
│   ├── categories.md ← routing table: what goes where
│   ├── frontmatter.md← required YAML fields for each type
│   └── linking-rules.md ← when and how to link pages
├── _templates/       ← templates for new entries
├── 000-inbox/        ← stuff that hasn't been categorized yet
├── 000-raw/          ← immutable copies of everything you save (your audit trail)
├── daily/            ← daily briefing notes
├── finance/          ← markets, investing, financial models
├── ideas/            ← product and app ideas
├── learning/         ← code concepts, language features, framework knowledge
├── projects/         ← active projects with context
├── todos/            ← task management
└── tools/            ← tool tips, library notes, workflow tricks
```

### Daily Workflow

**Save something you learned:**
```
/dump how React useEffect cleanup functions work — they run before the
next effect and on unmount, preventing memory leaks from subscriptions
```

**Capture an idea:**
```
/idea app that tracks my coffee spending and shows me what I could
have invested that money in instead
```

**Understand code someone else wrote:**
```
/educate src/api/auth.ts
```
Then `/floki-learn` to save what you learned.

**Search your knowledge later:**
```
/floki-recall what do I know about authentication?
```

**Start your day:**
```
/brief
```

### Tips

- **Use Obsidian** alongside Floki — point Obsidian at your `~/SecondBrain/` folder and you get a visual graph of all your connected knowledge
- **Everything is markdown** — no lock-in, no database, no cloud dependency
- **Raw notes are immutable** — `000-raw/` is your audit trail. Floki never edits those files after creation
- **Links are bidirectional** — when Floki links page A to page B, it also links B back to A

## Updating

```
cd ~/.claude/skills/vantedge && git pull && ./setup
```

Skills are symlinked, so `git pull` updates them instantly. The `./setup` re-run handles any new skills or structural changes.

## Uninstall

```bash
# Remove VantEdge skills (keeps gstack and your Second Brain)
rm -rf ~/.claude/skills/vantedge
for skill in brief dump educate floki-learn floki-lint floki-recall floki-todo idea karpathy plan browser-automation; do
  rm -rf ~/.claude/skills/$skill
done

# Optional: also remove gstack
rm -rf ~/.claude/skills/gstack

# Your Second Brain at ~/SecondBrain is never deleted — it's your data.
```

## Requirements

- **git** — for cloning
- **bun v1.0+** — for gstack's browser engine ([install](https://bun.sh))
- **Claude Code** — CLI, desktop app, or VS Code extension
- macOS or Linux (Windows: experimental)

## License

MIT

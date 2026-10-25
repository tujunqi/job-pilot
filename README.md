# job-pilot

**A job-search copilot that remembers and wakes up.** It keeps a plain-CSV ledger of every application you've made, and turns it into weekly funnel analysis, follow-up reminders, and concrete strategy — computed from *your* real reply rates, not vibes.

[中文文档](README.zh-CN.md) · [Example weekly report](examples/weekly-report-example.md)

## The expert-mode test

You can ask any AI "help me with my job search" and get generic advice. What you *cannot* get by asking:

| Capability | What job-pilot does | Why asking can't replace it |
|---|---|---|
| **State** | One CSV remembers every application across all sessions | A chat forgets everything tomorrow |
| **Scheduling** | Weekly review fires by itself (host cron); follow-ups flagged by date math (applied >7d silent, interview >5d silent, interviews within 7d) | An AI won't wake up on Monday |
| **Computation** | Reply rates by channel and role type from your ledger: "内推 4/4 = 100%, 猎聘 0/4 = 0% — shift fire power" | Generic advice ignores your actual funnel |

## What it does

- **记一笔** — say "今天投了字节的后端，Boss 上聊的，30k" → parsed into the ledger
- **更新状态** — "星辰约面了" → row updated, next actions tracked
- **周复盘**（core）— funnel by channel/role, follow-up list with ready-to-send scripts, and executable strategy. Under 10 applications it honestly says "sample too small" and skips strategy
- **Boss直聘增量**（optional, with [Agent-Reach](https://github.com/Panniantong/Agent-Reach)）— scan Boss Zhipin against your saved filters, dedupe against the ledger, surface only what's new

## Install

```bash
# ZCode / agents using ~/.agents/skills
git clone https://github.com/tujunqi/job-pilot ~/.agents/skills/job-pilot

# Claude Code
git clone https://github.com/tujunqi/job-pilot ~/.claude/skills/job-pilot
```

Ledger lives at `~/.job-pilot/applications.csv` (auto-created; override with `JOB_PILOT_CSV`). It's your data — plain CSV, no lock-in, version it if you like.

## Scheduling the weekly review

- **ZCode**: create an automation — every Monday 09:00, prompt it to run the 周复盘 action of this skill
- **Any host with cron**: point a weekly job at your agent CLI with the same instruction
- **No scheduler?** Just say "复盘一下" whenever — the state layer works without scheduling

## Example

See [examples/applications.example.csv](examples/applications.example.csv) (25 fictional rows) and the report it generates: [examples/weekly-report-example.md](examples/weekly-report-example.md) — channel funnel, follow-up scripts included, strategy tied to numbers.

## Design principles

- **The ledger is the only source of truth.** Every number in a report traces to CSV rows; nothing is invented.
- **Small samples are admitted.** <10 applications → follow-up list only, no strategy conclusions.
- **Advice must be executable.** "Raise referral share from 16% to 40%" ✓; "try harder" ✗.
- **Read-only toward job boards.** It never auto-applies for you (compliance and sanity).

MIT © 2026 tujunqi · Optional data layer: [Agent-Reach](https://github.com/Panniantong/Agent-Reach)

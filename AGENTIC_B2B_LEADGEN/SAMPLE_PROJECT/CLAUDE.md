# Project: Meta Ads B2B Lead Generation (Ecommerce)

## What this project is

Kunaal is building a lead-generation pipeline for his own Meta Ads freelance/agency
service. The buyers he's after are **ecommerce companies that need Meta ad management**.

He is new to running Meta ads professionally. That one fact drives every decision
in this pipeline: the ICPs, the filtering, and the outreach all need to point at
prospects he can realistically **land and deliver for right now** — not aspirational
enterprise logos. Ambition can scale up once he has case studies.

## The pipeline

Five agents, each owning one stage, handing off through files under `outputs/`:

```
agent1_icp          →  outputs/icps.md
        ↓
agent2_leadsource    →  outputs/leads/<icp_name>.csv
        ↓
agent3_filter        →  outputs/leads_filtered.md
        ↓
agent4_email          →  outputs/email_sequences/<lead_id>.md
        ↓
agent5_sendemail      →  Gmail drafts (labeled ColdOutreach-Draft)
```

Each agent is defined in `agents/` and can be invoked on its own once its
upstream input file exists. Run them in order the first time through; after that,
any agent can be re-run on fresh input without redoing the earlier stages.

**Note on Claude Code auto-discovery:** these are written as standard Claude Code
subagent files (YAML frontmatter with `name`/`description`/`tools`). Claude Code
only auto-discovers subagents placed under `.claude/agents/`. If you want that
behavior, copy the contents of `agents/` into a `.claude/agents/` folder in this
project yourself (a quick local copy) — they'll work identically either way when
invoked by name.

| Stage | Agent | Reads | Writes |
|---|---|---|---|
| 1 | `agent1_icp` | (nothing — reasons from this file + market knowledge) | `outputs/icps.md` |
| 2 | `agent2_leadsource` | `outputs/icps.md` | `outputs/leads/*.csv` |
| 3 | `agent3_filter` | `outputs/leads/*.csv` | `outputs/leads_filtered.md` |
| 4 | `agent4_email` | `outputs/leads_filtered.md` | `outputs/email_sequences/*.md` |
| 5 | `agent5_sendemail` | `outputs/email_sequences/*.md` | Gmail drafts + `outputs/draft_manifest.csv` |

## Ground rules

- **Bias small.** ICPs should center on SMB/lower-mid-market ecommerce brands —
  roughly $1M–$20M in annual revenue, lean or no in-house paid-media hire. These
  convert faster for a new operator and don't expect an agency with a long track
  record.
- **Human review between every stage.** No agent should silently trigger the next
  one. Kunaal reviews each output file before the next agent runs on it.
- **Agent 5 drafts, never sends.** The Gmail MCP tools available here only support
  creating/updating drafts — there is no send action, by design. Every email
  Kunaal's prospects receive goes out because he clicked Send himself.
- **File-based handoff.** Every agent reads its input from a specific path and
  writes its output to a specific path (see table above). Keep those paths stable
  so the agents chain without manual re-pointing.
- **Respect Apollo credits.** Agent 2 should search deliberately (per-ICP, paginated)
  rather than pulling everything available — credits are a real cost constraint
  early on.

## Directory layout

```
Him_B2B_20260809/
├── CLAUDE.md                     (this file)
├── agents/
│   ├── agent1_icp.md
│   ├── agent2_leadsource.md
│   ├── agent3_filter.md
│   ├── agent4_email.md
│   └── agent5_sendemail.md
└── outputs/                      (created as agents run — not part of the initial setup)
    ├── icps.md
    ├── leads/
    ├── leads_filtered.md
    ├── email_sequences/
    └── draft_manifest.csv
```

## Status

This file and the five agent definitions are the initial setup. No agent has been
run yet — that's the next step, one stage at a time, starting with `agent1_icp`.

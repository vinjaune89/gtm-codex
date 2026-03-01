# CLI-Native Stack

Agent-first, API-heavy GTM infrastructure. No browser tabs, no clicking through dashboards. Your AI agent drives the workflow; you review and approve.

This is the stack behind the [GTM Cortex](https://advancedpectoralthinking.substack.com) — built for a 2-person team running multiple ventures.

## The Stack

| Layer | Tool | Why |
|-------|------|-----|
| **Brain** | Claude Code | Runs everything. Research, drafting, pipeline ops, CRM updates. Skills + MCP integrations. |
| **CRM** | Attio | API-first, MCP server, pipeline management. Agent reads/writes directly. |
| **Enrichment** | Clay + Exa AI | Clay for structured contact/email enrichment. Exa for semantic web research. |
| **Research** | Exa AI + Linkup | Exa for surface search, Linkup for deep/niche sources. |
| **Orchestration** | Composio (RUBE) | MCP middleware. Connects Gmail, LinkedIn, Slack, Calendar to Claude Code. |
| **Knowledge** | Notion | Human-readable layer. Account files, tracking, content planning. |
| **Outreach** | Gmail + LinkedIn | Direct channels. Each message individually crafted and reviewed by a human. |
| **Content** | Substack | Thought leadership. Long-form content attracts inbound. |
| **Comms** | Slack | Team coordination, pipeline alerts. |

## What's Notably Absent

- **No sequencer** (Instantly, lemlist, etc.) — every message is human-reviewed. Volume is low, quality is high.
- **No intent platform** (6sense, Bombora) — signal detection is manual + Exa news monitoring.
- **No Clay tables for orchestration** — Claude Code + MCP handles workflow automation.

## Cost

| Tool | Monthly cost |
|------|-------------|
| Claude Code | ~$20 (Claude Pro subscription) |
| Attio | Free tier or $29/seat |
| Clay | ~$149 (enrichment credits) |
| Exa AI | ~$5–50 (pay-per-search) |
| Linkup | ~$0–20 (pay-per-search) |
| Composio/RUBE | Free tier or $29 |
| Notion | Free |
| Substack | Free |
| **Total** | **~$230–300/month** |

## Setup Order

1. **Claude Code** — Install, set up project directory, create CLAUDE.md
2. **Attio** — Create workspace, set up pipelines, get API key
3. **Attio MCP** — Connect via `@anthropic/attio-mcp-server`
4. **Exa AI** — Get API key from dashboard.exa.ai, connect MCP
5. **Clay** — Create account, connect MCP for enrichment
6. **Composio/RUBE** — Connect business tools (Gmail, Slack, LinkedIn)
7. **Notion** — Set up knowledge base structure
8. **Substack** — Create publication for thought leadership

## How It Works

```
Research (Exa + Linkup)
    ↓
Enrich (Clay → emails, titles, company data)
    ↓
Stage in CRM (Attio → pipeline, next steps)
    ↓
Draft outreach (Claude Code → personalized messages)
    ↓
Human review → Send (Gmail/LinkedIn via RUBE)
    ↓
Track (Attio → pipeline progression)
    ↓
Content loop (learnings → Substack → inbound)
```

## Who This Is For

- Solo operators or tiny teams (1–3 people)
- Technical founders who can set up MCP integrations
- High-touch outreach (not volume plays)
- People who want their AI agent to drive GTM operations

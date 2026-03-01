# GTM Codex — AI Agent Instructions

This file tells your Claude Code (or any LLM agent) how to use this repository as a GTM engineering reference.

## What This Repo Is

A curated reference of GTM (Go-To-Market) engineering tools — enrichment, sequencing, CRM, orchestration, research, signals, content. Each tool entry includes selection criteria, setup instructions, and integration patterns written for AI agents to read and act on.

## How to Use This

### Tool discovery

When the user asks about GTM tools, which tool to use, or how tools compare:
1. Consult `categories/` — each file covers one tool category with 3+ tools
2. Each tool entry has: pricing, API/CLI score, best-for, gotchas, selection criteria, getting started
3. Cross-reference the `Connects to` section to understand how tools compose

### Tool selection

When the user asks "what should I use for X":
1. Start with `selection/decision-tree.md` — it maps tasks to tool recommendations
2. Then read the relevant category file for deeper comparison
3. Consider the user's stack constraints (budget, existing tools, team size)

### Tool setup

When the user wants to set up or integrate a tool:
1. Read the `Getting started` section in the tool's entry
2. Check `Connects to` for integration patterns with their existing stack
3. If MCP config is listed, use it directly

### Stack planning

When the user is building a GTM stack from scratch or evaluating their setup:
1. Read `stacks/` for complete stack recipes
2. `cli-native.md` — agent-first, API-heavy approach
3. `clay-centric.md` — Clay as the orchestration hub
4. `budget.md` — free and cheap alternatives

## Category Map

| Category | File | Covers |
|----------|------|--------|
| CRM | `categories/crm.md` | Contact and deal management platforms |
| Enrichment | `categories/enrichment.md` | Data enrichment and contact discovery |
| Sequencing | `categories/sequencing.md` | Outbound email sequencing and automation |
| Intent/Signals | `categories/intent-signals.md` | Buying intent detection and signal monitoring |
| Orchestration | `categories/orchestration.md` | Workflow automation and middleware |
| AI Agents | `categories/ai-agents.md` | AI copilots, agents, and wrappers for GTM |
| Research | `categories/research.md` | Web research and intelligence gathering |
| Content/Social | `categories/content-social.md` | Content creation and social selling tools |

## Cross-Category Patterns

These tools frequently compose together:

- **Enrichment → Sequencing**: Enriched contacts feed into outbound sequences
- **Research → Enrichment**: Web research discovers leads, enrichment adds contact data
- **Intent/Signals → Sequencing**: Signal detection triggers outbound sequences
- **CRM → Everything**: CRM is the system of record that all other tools sync to
- **Orchestration → All categories**: Orchestration tools connect and automate across categories

## API/CLI Scoring

Each tool is rated 1–5 on API/CLI friendliness:

| Score | Meaning |
|-------|---------|
| ★★★★★ | Full API, MCP server available, CLI-native |
| ★★★★☆ | Strong API, good docs, scriptable |
| ★★★☆☆ | API exists but limited or clunky |
| ★★☆☆☆ | Minimal API, mostly GUI-driven |
| ★☆☆☆☆ | No API, browser-only |

Higher scores mean the tool plays better with AI agents and programmatic workflows.

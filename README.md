# GTM Codex

A curated reference of GTM engineering tools — designed to be cloned into your AI agent's knowledge base.

```
git clone https://github.com/vinjaune89/gtm-codex.git knowledge/gtm-codex
```

Your Claude Code (or any LLM agent) now knows the GTME landscape.

## What This Is

A workflow-first reference of the tools GTM engineers use: CRM, discovery, enrichment, scraping, outreach, signals, orchestration. Each tool entry includes:

- **Status badge** — our relationship to the tool (`★ In our stack`, `Tested`, `Researched`)
- **Selection criteria** — when to pick it over alternatives
- **API/CLI score** — how well it plays with agents and scripts (1–5 stars)
- **Getting started** — setup instructions an AI agent can follow
- **Integration patterns** — how tools connect to each other

This is not a comparison website. It's a knowledge package. The audience is your AI agent, not your browser.

## Why API/CLI Matters

Most GTM tool lists rank on features, pricing, or G2 reviews. This one ranks on **programmability** — how well the tool works when an AI agent or script is driving.

A tool with a full API and MCP server lets your agent enrich contacts, trigger sequences, and update your CRM without you touching a browser. A tool that's browser-only means your agent can't help you with it.

The GTM engineering stack is shifting from "click buttons in 12 tabs" to "tell your agent what to do." This codex reflects that shift.

## Categories

| Category | What it covers |
|----------|---------------|
| [CRM](categories/crm.md) | Pipeline and contact management |
| [Discovery](categories/discovery.md) | Finding targets — web search, ICP matching, lookalikes |
| [Enrichment](categories/enrichment.md) | Contact data — emails, phones, firmographics |
| [Scraping](categories/scraping.md) | Extracting data from known URLs |
| [Outreach](categories/outreach.md) | Email sequences and LinkedIn automation |
| [Signals](categories/signals.md) | Monitoring engagement and trigger events |
| [Orchestration](categories/orchestration.md) | Connecting tools and automating workflows |

## Stack Recipes

Pre-built tool combinations for different approaches:

- **[CLI-Native](stacks/cli-native.md)** — Agent-first, API-heavy, no-GUI stack
- **[Clay-Centric](stacks/clay-centric.md)** — Clay as the orchestration hub
- **[Budget](stacks/budget.md)** — Free and cheap alternatives per category

## Quick Start

**Loading into Claude Code:**
```
# Clone into your project's knowledge directory
git clone https://github.com/vinjaune89/gtm-codex.git knowledge/gtm-codex

# The CLAUDE.md in the repo tells your agent how to use it
```

**Loading into other agents:** Point your agent's context at the `CLAUDE.md` file. It contains instructions for navigating the repo.

**Choosing tools:** Start with [selection/decision-tree.md](selection/decision-tree.md) — it maps common GTME tasks to tool recommendations.

## Tool Selection Cheatsheet

| I need to... | Start here |
|--------------|-----------|
| Store contacts and deals | [CRM](categories/crm.md) |
| Find companies and prospects | [Discovery](categories/discovery.md) |
| Get emails and enrich contact data | [Enrichment](categories/enrichment.md) |
| Scrape data from web pages | [Scraping](categories/scraping.md) |
| Send cold email or LinkedIn outreach | [Outreach](categories/outreach.md) |
| Detect buying signals and triggers | [Signals](categories/signals.md) |
| Connect tools and automate workflows | [Orchestration](categories/orchestration.md) |

## Creators to Follow

People putting out real GTM engineering content.

**Clay / GTM Engineering Core**
- **Jordan Crawford** — Clay community architect. Blueprint series. The OG of "GTM Engineer as a role."
- **Eric Nowoslawski** — Deep Clay workflows, practical GTM automation. Shows actual builds.
- **Patrik Spychalski** — Clay + GTM workflows. Toolstack breakdowns and process thinking.

**AI-Native GTM**
- **Kyle Coleman** — Copy.ai CMO (ex-Clari). AI-powered GTM and messaging strategy.
- **Adam Robinson** — RB2B founder. Signal-based selling, scrappy GTM.
- **Devin Reed** — Ex-Gong. Content-as-GTM-engine.

**Claude Code x GTM**
- **Axel Hägg** — Built the GTM Cortex (this repo's origin story). CLI-native GTM, braintrust architecture.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Add tools, fix details, submit stack recipes.

## License

MIT — see [LICENSE](LICENSE).

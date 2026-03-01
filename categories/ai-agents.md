# AI Agents

Systems that think, plan, and execute GTM workflows — not just autocomplete. The distinction matters: a chatbot answers questions, an agent runs your pipeline. This category covers both packaged agent platforms and the pattern of building your own.

---

### Claude Code

**URL**: https://claude.ai/code
**Pricing**: Free with Claude Pro/Team/Enterprise subscription ($20+/month)
**API/CLI**: ★★★★★

CLI-native AI agent that reads your codebase, runs shell commands, and operates tools via MCP integrations. Not a coding assistant that happens to do GTM — it is the GTM operator when paired with structured knowledge and tool access.

**Best for**: Complex multi-step GTM workflows where a human reviews output. Pipeline maintenance, CRM operations, outreach drafting, research synthesis, data reconciliation. Any workflow that requires reading context from multiple sources, reasoning about it, and taking action across tools.

**Watch out**: Context window has limits — long sessions degrade without structured knowledge to reload from. Needs a well-organized repo (skills, knowledge files, schema registries) to perform consistently. Not a fire-and-forget agent; the human-in-the-middle review pattern is essential for outbound actions.

#### Selection criteria

Pick Claude Code when your GTM workflow requires multi-tool orchestration with judgment — not just "enrich this list" but "research these accounts, draft outreach based on what you find, update the CRM, and flag anomalies." It replaces the need for several point solutions when you have the technical setup to support it. If you want a GUI agent builder with drag-and-drop, look at Relevance AI instead.

#### Getting started

1. Install: `npm install -g @anthropic-ai/claude-code` (or via Homebrew)
2. Run `claude` in any project directory
3. Add MCP servers for your tools (CRM, search, email) in `.claude/settings.json`
4. Structure your knowledge: a `CLAUDE.md` at repo root tells the agent how to navigate your system
5. Build skills as markdown files in `.claude/skills/` — each skill is a repeatable workflow the agent can execute

```json
// Example MCP config in .claude/settings.json
{
  "mcpServers": {
    "attio": {
      "type": "sse",
      "url": "https://mcp.attio.com/sse?apiKey=YOUR_KEY"
    },
    "exa": {
      "type": "sse",
      "url": "https://mcp.exa.ai/mcp?exaApiKey=YOUR_KEY"
    }
  }
}
```

#### Connects to

- **Exa / Linkup** (research.md): MCP servers provide semantic search directly inside Claude Code sessions
- **Attio / HubSpot** (crm.md): MCP servers for CRM reads and writes without leaving the terminal
- **Clay** (enrichment.md): MCP server for contact enrichment and company research
- **Firecrawl** (research.md): MCP server for scraping JS-rendered pages Claude Code can't fetch natively
- **RUBE** (orchestration.md): Middleware that connects 500+ apps — Gmail, Slack, LinkedIn — as MCP tools

---

### Relevance AI

**URL**: https://relevanceai.com
**Pricing**: Freemium — Free (200 actions/month), Team ($234/month), Enterprise (custom)
**API/CLI**: ★★★★☆

AI agent builder purpose-built for GTM teams. Pre-built sales agents (BDR, SDR, research) or build custom ones with a visual editor. Three automation levels: Assisted (chat), Copilot (playbook-driven), Autopilot (signal-triggered). 2000+ integrations out of the box.

**Best for**: Teams that want packaged GTM agents without building from scratch. Lead research, outbound sequencing, meeting prep, account scoring. Particularly strong when you need agents that trigger automatically from pipeline events rather than manual invocation.

**Watch out**: Credit system has two dimensions — Actions (what agents do) and Vendor Credits (LLM costs). The free tier burns through fast on real workflows. Pricing changed September 2025 — old blog posts show outdated numbers. You can bring your own API keys to bypass vendor credits, but the action credits still gate throughput.

#### Selection criteria

Pick Relevance AI when you want agent capabilities without maintaining code. It sits between "use Claude Code and build everything" and "use a sequencing tool with basic AI features." Good for teams with GTM operators who aren't developers. If you need full control over prompts, tool chains, and data flow, Claude Code or custom agents give you more. If you just need email sequences with AI personalization, a sequencer (see sequencing.md) is simpler.

#### Getting started

1. Sign up at relevanceai.com — free tier includes 200 actions and $1,000 one-time vendor credits
2. Browse the marketplace for pre-built GTM agents or create a custom agent
3. Connect your tools (CRM, email, LinkedIn) via the integrations panel
4. For AI client integration: export tools via MCP Server (Settings → Export Tools → MCP Server)
5. Python SDK available at sdk.relevanceai.com for programmatic access

```json
// MCP config for using Relevance AI tools in Claude Code / Cursor
{
  "mcpServers": {
    "relevance-ai": {
      "type": "sse",
      "url": "https://YOUR_WORKSPACE.relevanceai.com/mcp/sse",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN"
      }
    }
  }
}
```

#### Connects to

- **CRM tools** (crm.md): Native integrations with HubSpot, Salesforce, Attio — agents read/write pipeline data
- **Sequencing tools** (sequencing.md): Agent research feeds into outbound sequences
- **Enrichment tools** (enrichment.md): Agents trigger enrichment as part of research workflows
- **Claude Code** (this file): Relevance AI tools can be exposed as MCP servers for use inside Claude Code

---

### Custom Agents (pattern)

**URL**: N/A — this is a build pattern, not a product
**Pricing**: Variable — API costs (Anthropic, OpenAI) + infrastructure
**API/CLI**: ★★★★★ (you build it)

Building your own GTM agents using LLM APIs, structured knowledge, and tool integrations. The pattern: domain knowledge encoded as files + repeatable workflows encoded as skills + external tools connected via APIs or MCP = a GTM agent tailored to your exact operation. This is what the repo you're reading was built for.

**Best for**: Teams with technical capability who want full control over agent behavior, data flow, and tool selection. When off-the-shelf agents don't match your workflow, or when you need agents that operate across tools no single platform covers.

**Watch out**: High initial investment — you're building the agent, the knowledge base, and the integration layer. Maintenance burden is real: tools change APIs, LLM behavior shifts between model versions, knowledge goes stale. The payoff is compound (agents get better as knowledge accumulates), but the first month is mostly infrastructure.

#### Selection criteria

Pick this when your GTM workflow is non-standard enough that packaged agents fight you more than help you. Signs: you need agents that operate across 4+ tools, your outreach requires deep domain context that can't be captured in a template, or you want agents that maintain and improve their own knowledge over time. If your workflow maps cleanly to "research → enrich → sequence," a platform like Relevance AI or Clay is faster to start.

#### Getting started

1. Choose your LLM provider: Anthropic API (Claude), OpenAI API, or local models
2. Structure your knowledge: markdown files organized by domain, referenced by a central index
3. Define skills: each skill is a repeatable workflow with inputs, steps, and outputs
4. Connect tools: MCP for real-time tool access, or API scripts for batch operations
5. Build the review loop: agent prepares, human reviews, agent executes — never skip the middle step

The simplest starting point is a Claude Code project with:
```
your-repo/
├── CLAUDE.md              # Agent instructions
├── .claude/skills/        # Workflow definitions
├── knowledge/             # Domain context
├── configs/               # API keys, schema IDs, tool config
└── work/                  # Active operations and session logs
```

#### Connects to

Everything — that's the point. Custom agents integrate with whatever your GTM stack requires. The constraint is your willingness to build and maintain the integrations.

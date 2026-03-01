# Orchestration

Workflow automation and middleware — the plumbing that connects enrichment, sequencing, CRM, and outbound into end-to-end pipelines. These tools range from spreadsheet-like orchestration (Clay) to self-hosted workflow engines (n8n) to AI-native app connectors (Composio/RUBE). The unifying trait: they sit between your tools and make them talk to each other without manual copy-paste.

**Also in this space**: Make (make.com) and Zapier (zapier.com) are the incumbent GUI-first platforms. Both have APIs but are designed around visual builders, not CLI or agent workflows. Make is more flexible and cheaper at scale; Zapier is simpler but expensive per-task. API/CLI score: ★★☆☆☆ for both. Use them if your team is non-technical and needs drag-and-drop; skip them if you're building agent-driven pipelines.

---

### Clay (Tables)

**URL**: https://www.clay.com
**Pricing**: Freemium (free tier with limited credits; Explorer from $349/mo, Pro from $720/mo — credits-based)
**API/CLI**: ★★★★☆

Spreadsheet-as-orchestration. Each row in a Clay table can trigger waterfalls of enrichment, API calls, AI prompts, and actions. You build a table, add columns that pull from 100+ data providers, and Clay runs them in sequence — first provider fails, next one fires. The table IS the workflow.

**Best for**: Enrichment-heavy GTM workflows — building prospect lists with verified emails, phone numbers, firmographics, and technographics from multiple providers in one pass. Lead scoring. ICP qualification. Account research at scale.

**Watch out**: Credits burn fast on complex workflows. A single row running through 5 enrichment columns can cost 10-50 credits. Waterfall enrichment is powerful but expensive if you don't gate it (run cheap providers first, expensive ones as fallback). Complex multi-step workflows with conditionals get hard to debug — the table metaphor breaks down at high complexity. CRM integrations (Salesforce, HubSpot) require Pro plan.

#### Selection criteria

Pick Clay when enrichment is the core of your workflow — you need to go from a company name to a verified decision-maker email with firmographic context. Clay's waterfall model (try provider A, fall back to B, then C) is unmatched for coverage. Don't pick Clay for simple A-to-B automations (use n8n) or for connecting your AI agent to business tools (use Composio/RUBE).

#### Getting started

1. Sign up at clay.com — free tier gives you limited credits to test
2. Create a table, add a "Find People" or "Find Companies" column
3. Add enrichment columns (email waterfall, phone, LinkedIn URL)
4. For programmatic access: Clay has an HTTP API for importing rows via webhooks and triggering table runs — see [Clay University: Using Clay as an API](https://www.clay.com/university/guide/using-clay-as-an-api)
5. **MCP integration**: Anthropic offers a hosted Clay connector (`claude_ai_Clay`) with tools like `find-and-enrich-contacts-at-company` and `find-and-enrich-list-of-contacts`. Email enrichment is async — use `get-task` to poll for completion. Add `dataPoints: { contactDataPoints: [{ type: "Email" }] }` for email-only enrichment
6. Third-party pipe0 offers a REST API wrapper around Clay tables for deeper programmatic control

#### Connects to

- **Enrichment tools** (this codex: `enrichment.md`) — Clay IS enrichment for most users, but you can also pipe Clay-enriched data into standalone enrichment tools or vice versa
- **Sequencing tools** (`sequencing.md`) — enriched + scored leads export to Smartlead, Instantly, lemlist for outbound
- **CRM** (`crm.md`) — bi-directional sync with HubSpot, Salesforce (Pro plan); push to Attio, Pipedrive via webhooks or Zapier/Make
- **Research tools** (`research.md`) — Claygent (built-in AI agent) can query web sources; pairs with Exa for deeper research before enrichment

---

### n8n

**URL**: https://n8n.io
**Pricing**: Freemium (self-hosted Community Edition is free and unlimited; Cloud from €24/mo Starter, €60/mo Pro, €800/mo Business)
**API/CLI**: ★★★★☆

Self-hosted, open-source workflow automation. The open alternative to Zapier and Make — visual workflow builder with 400+ integrations, but also a full REST API and native MCP server for AI agent integration. Execution-based billing (not per-step like Zapier), so complex multi-step workflows are dramatically cheaper.

**Best for**: Multi-step automations you own and control. CRM sync pipelines, lead routing, webhook-triggered enrichment flows, scheduled jobs, internal tooling. Teams that want Zapier-level connectivity without Zapier-level pricing or vendor lock-in.

**Watch out**: Self-hosting means you manage infrastructure (though cloud plans exist). The visual builder is powerful but can become spaghetti at scale — treat workflows like code (version control, naming conventions). Community Edition lacks SSO, environments, and audit logs. Cloud pricing jumps sharply at the Business tier (€800/mo). Execution counting can be confusing — a 10-step workflow counts as 1 execution in n8n but 10 tasks in Zapier.

#### Selection criteria

Pick n8n when you need reliable, repeatable automations that you control — especially if you're already self-hosting other tools or want to avoid per-task pricing. The MCP server makes it uniquely powerful for AI agent workflows: Claude can trigger, monitor, and even build n8n workflows directly. Don't pick n8n for enrichment-heavy work (use Clay) or for quick one-off app connections where Composio/RUBE is faster to set up.

#### Getting started

1. **Self-hosted** (free): `docker run -it --rm -p 5678:5678 n8nio/n8n` — runs locally in seconds. For production, use Docker Compose or a managed host (Railway, Northflank, Render)
2. **Cloud**: Sign up at n8n.io — 14-day free trial, no credit card
3. **API access**: Every n8n instance exposes a REST API. Generate an API key in Settings > API > Create API Key
4. **MCP server for Claude Code**:
   ```bash
   claude mcp add n8n \
     --transport http \
     "https://YOUR_N8N_INSTANCE/mcp" \
     --headers "Authorization:Bearer YOUR_N8N_API_KEY"
   ```
   Or use the community MCP server: `npx @czlonkowski/n8n-mcp` with `N8N_API_URL` and `N8N_API_KEY` env vars
5. Build your first workflow: Webhook trigger → HTTP Request → IF condition → output. Test with `curl`

#### Connects to

- **CRM** (`crm.md`) — native nodes for HubSpot, Salesforce, Pipedrive, Attio. Common pattern: CRM webhook triggers n8n workflow for enrichment or routing
- **Sequencing tools** (`sequencing.md`) — trigger sequence enrollment via API nodes (Smartlead, Instantly, lemlist all have HTTP APIs)
- **Enrichment tools** (`enrichment.md`) — HTTP Request node calls any enrichment API; schedule periodic enrichment runs
- **Clay** (this file) — n8n can trigger Clay table runs via webhook, or process Clay webhook outputs for downstream routing
- **Composio/RUBE** (this file) — n8n workflows can call Composio actions via HTTP, or RUBE can trigger n8n workflows. Use n8n for scheduled/event-driven automation, RUBE for ad-hoc agent tasks

---

### Composio (RUBE)

**URL**: https://composio.dev (platform) / https://userube.com (RUBE consumer product)
**Pricing**: Freemium (free: 1,000 requests/day; Pro: $25/mo for 600+ apps, unlimited requests; Enterprise: custom)
**API/CLI**: ★★★★★

Universal MCP middleware for AI agents. RUBE is Composio's consumer product — a single MCP server with 7 meta-tools that dynamically connect to 600+ apps (Gmail, LinkedIn, Slack, Calendar, X/Twitter, Google Sheets, Salesforce, and hundreds more). Instead of configuring one MCP server per app, RUBE handles OAuth, authentication, and tool routing in one connection. Your AI agent says "search Gmail" and RUBE figures out the rest.

**Best for**: Connecting AI agents (Claude Code, Cursor, ChatGPT) to business tools without building custom integrations. Ad-hoc agent workflows: "check my Gmail for replies from this prospect," "post this to Slack," "add a calendar event." Rapid prototyping of multi-app workflows. Scenarios where you need 10+ app connections and don't want 10 MCP servers.

**Watch out**: Session management is the main gotcha — use `session: {generate_id: true}` for new sessions, `session: {id: "X"}` for continuation, and pass session IDs in every call. Workbench file sync (`sync_response_to_workbench=true`) writes to unreliable paths; pull data inside the workbench via `run_composio_tool()` instead. LinkedIn integration is write-only (can post/comment, cannot read feed or DMs). Some app connections need re-authentication periodically. Messages sent via RUBE Slack appear as the authenticated user, not a bot — prefix AI-authored messages to avoid confusion.

#### Selection criteria

Pick Composio/RUBE when your AI agent needs to interact with business apps — reading email, posting to Slack, checking calendar, managing CRM records. It's the fastest path from "Claude, check my inbox" to working automation. Don't pick it for complex multi-step scheduled workflows (use n8n) or for enrichment pipelines (use Clay). RUBE is best for agent-initiated, ad-hoc actions; n8n is best for event-driven, repeatable flows.

#### Getting started

1. Sign up at userube.com or composio.dev
2. **Add RUBE MCP to Claude Code**:
   ```bash
   claude mcp add rube \
     --transport http \
     "YOUR_RUBE_MCP_URL" \
     --headers "X-API-Key:YOUR_COMPOSIO_API_KEY"
   ```
3. Run `/mcp` in Claude Code to verify connection. If apps need auth, RUBE generates an OAuth link — click it to connect each app
4. **Key tools**: `RUBE_SEARCH_TOOLS` (discover available actions for any connected app), `RUBE_MULTI_EXECUTE_TOOL` (run multiple actions in parallel), `RUBE_REMOTE_BASH_TOOL` (execute code remotely), `RUBE_REMOTE_WORKBENCH` (Python environment for data processing), `RUBE_FIND_RECIPE` / `RUBE_EXECUTE_RECIPE` (saved multi-step workflows)
5. **Session pattern**: Always generate a session ID on first call, then pass it to all subsequent calls in the same logical workflow

#### Connects to

- **CRM** (`crm.md`) — read/write to Attio, HubSpot, Salesforce, Pipedrive through RUBE's app connections. Useful for agent-driven CRM updates (add notes, change stages, log interactions)
- **Sequencing tools** (`sequencing.md`) — trigger sends, check delivery status, manage contacts in sequencing platforms via their connected apps
- **Research tools** (`research.md`) — Gmail search for reply verification, LinkedIn for profile context (write-only), X/Twitter for social signals
- **n8n** (this file) — RUBE can trigger n8n workflows via webhook; n8n can call Composio actions via HTTP. Use RUBE for agent-initiated actions, n8n for scheduled/event-driven automation
- **Clay** (this file) — RUBE can push data into Clay via webhook triggers or pull enrichment results. Clay handles the enrichment waterfall, RUBE handles the surrounding actions (email send, Slack notification, CRM update)

---

## Quick comparison

| Dimension | Clay | n8n | Composio/RUBE |
|-----------|------|-----|---------------|
| Mental model | Spreadsheet that does things | Visual workflow builder | Universal app connector |
| Strength | Enrichment + waterfall logic | Scheduled multi-step automations | AI agent ↔ business tools |
| MCP support | Anthropic-hosted connector | Native MCP server + community servers | Native MCP (7 meta-tools) |
| Self-hostable | No | Yes (free Community Edition) | No (SaaS) |
| Billing model | Credits per enrichment action | Per execution (not per step) | Requests per day |
| Complexity ceiling | Medium (table metaphor limits branching) | High (arbitrary workflow logic) | Medium (agent-driven, not scheduled) |
| Best pair | Sequencing tools (outbound) | CRM + webhooks (routing) | AI coding agents (Claude Code, Cursor) |

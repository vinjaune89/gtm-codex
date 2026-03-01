# CRM

The system of record. Every other GTM tool eventually syncs back here. The choice of CRM determines how much of your stack can be automated — a weak API means manual glue; a strong one means your AI agents can read, write, and reason over pipeline data directly.

Three archetypes below: API-first (Attio), ecosystem-first (HubSpot), enrichment-native (Clay).

---

### Attio

**URL**: https://attio.com
**Pricing**: Freemium — free tier available, Plus from $29/seat/month, Pro from $59/seat/month, Enterprise custom
**API/CLI**: ★★★★★

API-first CRM built for programmatic workflows. Full REST API with granular object access, plus an official remote MCP server that lets AI agents search, create, update, and annotate records through natural language.

**Best for**: API-first teams, AI agent workflows, startups that want CRM logic in code rather than GUI config. Teams running Claude Code or similar agents against live pipeline data.

**Watch out**: Smaller ecosystem than HubSpot — fewer native integrations out of the box. You build what you need, which is a feature or a tax depending on your team. Entry ID and Record ID are different things in the API (common footgun — list entry operations use entry IDs, record operations use record IDs). List mutations (add-to-list, update-list-entry) require API scripts; the MCP server handles individual record ops but not bulk list operations.

#### Selection criteria

Pick Attio when your GTM workflow is agent-driven or script-heavy. If you want an AI agent to prep meeting briefs, update pipeline stages, log activity, and cross-reference email history — Attio's API surface makes that possible without middleware. The data model is flexible (custom objects, lists, attributes) without the config overhead of Salesforce. Not the right choice if you need 200+ native integrations or your team lives in a GUI.

#### Getting started

1. **Create workspace**: Sign up at attio.com. Free tier gives you contacts, companies, deals, and lists.
2. **Generate API key**: Settings > Developers > API Keys. Create a key with read/write scopes for the objects you need.
3. **MCP setup** (remote server — no npm install needed):
```json
{
  "mcpServers": {
    "attio": {
      "type": "url",
      "url": "https://mcp.attio.com/mcp"
    }
  }
}
```
Auth is handled via OAuth flow on first connection. The MCP server exposes: search-records, create-record, update-record, create-note, list-records, list-records-in-list, list-attribute-definitions, and more.

4. **Direct API**: Base URL `https://api.attio.com/v2/`. Bearer token auth. Key endpoints: `/objects/{slug}/records` (CRUD), `/lists/{list_id}/entries` (pipeline reads), `/notes` (activity logging). Rate limits are generous on paid tiers.

5. **For bulk pipeline operations**, use API scripts rather than MCP — `list-entries` for reads, `add-to-list` / `update-list-entry` for writes.

#### Connects to

- **Enrichment tools** (Clay, Clearbit): Enrich Attio records with contact data, firmographics, email verification
- **Sequencing tools** (Instantly, lemlist, Smartlead): Export segments from Attio lists into outbound sequences; sync reply/bounce status back
- **Research tools** (Exa, Perplexity): Feed research results into Attio notes for account context
- **Orchestration** (n8n, Make, Zapier): Trigger workflows on Attio record changes, sync bidirectionally with other tools
- **Email verification** (via Attio's native email tracking): `search-emails-by-metadata` and `get-email-content` for verifying sends and detecting replies

---

### HubSpot

**URL**: https://hubspot.com
**Pricing**: Freemium — free CRM (up to 1M contacts, 2 seats), Starter from $20/seat/month, Professional from $100/seat/month, Enterprise from $150/seat/month
**API/CLI**: ★★★★☆

The incumbent all-in-one platform: CRM, marketing automation, sales tools, service desk, content management. Massive ecosystem with 1,500+ native integrations. API is comprehensive but gated by tier — some endpoints require Professional or Enterprise.

**Best for**: Teams that want one platform for CRM + marketing + sales + service. Organizations where non-technical users need to self-serve on reporting, sequences, and workflows. Companies already in the HubSpot ecosystem.

**Watch out**: Pricing escalates fast — contact-based billing means costs grow with your database, not just your team. Free tier API is functional but limited to core CRM objects. Advanced features (custom objects, calculated properties, predictive scoring) require Professional or Enterprise. Rate limit is 100 requests per 10 seconds across all tiers. The platform is opinionated — fighting HubSpot's data model is expensive.

#### Selection criteria

Pick HubSpot when you need breadth over depth. If your team spans marketing, sales, and support and you want one login, one reporting layer, one workflow engine — HubSpot delivers that without stitching tools together. Also the right choice when your team is mostly GUI operators, not API builders. The integration marketplace means most SaaS tools have a native HubSpot connector. Not the right choice if you want full programmatic control at low cost, or if your workflow is agent-first.

#### Getting started

1. **Create account**: Sign up at hubspot.com. Free CRM includes contacts, companies, deals, tasks, and basic reporting.
2. **Create a Private App** (for API/MCP access): Settings > Integrations > Private Apps. Name it, select scopes (start with read-only CRM scopes), generate access token.
3. **MCP setup** (npm package):
```json
{
  "mcpServers": {
    "hubspot": {
      "command": "npx",
      "args": ["-y", "@hubspot/mcp-server"],
      "env": {
        "PRIVATE_APP_ACCESS_TOKEN": "<your-private-app-access-token>"
      }
    }
  }
}
```
Currently in public beta. Provides read-only access to contacts, companies, deals, tickets, invoices, products, line items, quotes, subscriptions, orders, and users. Write capabilities expanding throughout 2026.

4. **Direct API**: Base URL `https://api.hubapi.com/`. Bearer token auth. Key endpoints: `/crm/v3/objects/{objectType}` (CRUD), `/crm/v3/associations` (relationships), `/crm/v3/imports` (bulk operations). Full reference at developers.hubspot.com.

#### Connects to

- **Everything** — HubSpot's integration marketplace is its strongest asset. Native connectors for Slack, Gmail, Outlook, Salesforce, Stripe, Shopify, WordPress, and hundreds more.
- **Enrichment tools** (Clearbit, ZoomInfo, Clay): HubSpot has native Clearbit integration (acquired). Third-party enrichment tools push data via API or native connectors.
- **Sequencing tools**: HubSpot has built-in sequences (Sales Hub Starter+). Also integrates with Outreach, Salesloft, Apollo via marketplace.
- **Orchestration** (Zapier, Make, n8n): First-class triggers and actions across all hubs.
- **AI Agents**: MCP server enables Claude-based workflows. HubSpot also has its own AI features (Breeze) for summarization, content generation, and forecasting.

---

### Clay

**URL**: https://clay.com
**Pricing**: Paid — Starter $149/month (2,000 credits), Explorer $349/month (10,000 credits), Pro from $720/month (50K+ credits). Free trial available. Credits consumed on enrichment, not prospecting.
**API/CLI**: ★★★☆☆

Enrichment-first platform that doubles as a lightweight CRM. Data tables hold your pipeline. Waterfall enrichment across 100+ providers fills in emails, firmographics, technographics, and signals. The table interface is the CRM — rows are leads, columns are data points, and enrichment runs inline.

**Best for**: Teams that want enrichment and pipeline management in one place. Solo operators and small GTM teams that don't need a traditional CRM's deal stages and forecasting but do need rich contact data. Research-heavy outbound workflows where knowing more about a prospect matters more than tracking deal velocity.

**Watch out**: Credit-based pricing means costs are unpredictable if you enrich aggressively — a single waterfall lookup can burn 1-10 credits depending on providers hit. The table API is more limited than the enrichment API; bulk operations and complex filtering are clunky compared to a purpose-built CRM. No native deal pipeline or forecasting — if you need sales management, Clay is a data layer, not a CRM replacement. Pro plan required for CRM sync (HubSpot, Salesforce, Attio push). CSV export and pagination are gated behind paid tiers.

#### Selection criteria

Pick Clay when enrichment is your primary GTM motion and you want the data layer and prospecting layer in one tool. If you're building targeted lists (filter by employee count, industry, tech stack, funding), enriching with emails and phone numbers, then pushing to a sequencer — Clay collapses three tools into one. Also strong when you need waterfall enrichment (try Provider A, fall back to B, then C) rather than single-source lookups. Not the right choice if you need traditional CRM features (deal stages, forecasting, activity timelines, team permissions) or if your pipeline is large enough to need a real system of record.

#### Getting started

1. **Create account**: Sign up at clay.com. Free trial gives limited credits to test enrichment and table building.
2. **Generate API key**: Settings > API Keys at https://web.clay.earth/settings/api-keys.
3. **MCP setup** (for personal CRM / contacts features):
```json
{
  "mcpServers": {
    "clay": {
      "command": "npx",
      "args": ["-y", "@clayhq/clay-mcp@latest"],
      "env": {
        "CLAY_API_KEY": "<your-clay-api-key>"
      }
    }
  }
}
```
Exposes contact search, contact creation, notes, groups, and interaction history. This is the personal network layer — good for relationship tracking.

4. **For enrichment workflows via AI agents**, Clay also offers tools through the Anthropic MCP ecosystem: `find-and-enrich-contacts-at-company` (role-based discovery + email), `find-and-enrich-list-of-contacts` (named people + email). Email enrichment is async — poll with `get-task` until state is "completed."

5. **Direct API**: REST API for table operations, row CRUD, and triggering enrichments. HTTP API integrations and webhooks available on Explorer+ plans.

#### Connects to

- **CRM tools** (Attio, HubSpot, Salesforce): Clay pushes enriched data to your CRM. Pro plan required for native CRM sync. Lower tiers use CSV export or API scripts.
- **Sequencing tools** (Instantly, Smartlead, Outreach, Salesloft): Enriched + verified contacts push directly into sequences. Native integrations on Explorer+.
- **Research tools** (Exa, Google Maps, GitHub): Clay can pull prospecting data from web sources without consuming credits — credits only burn on enrichment.
- **Orchestration** (Zapier, Make, n8n, webhooks): Trigger Clay enrichments from external events, or push Clay results downstream.
- **Verification tools**: Waterfall enrichment includes email verification across providers. Completed enrichments include verification state — don't second-guess it.

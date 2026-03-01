# Enrichment

Tools that add structured data to contacts and companies — emails, phone numbers, firmographics, technographics. Enrichment sits between research (finding who to target) and sequencing (reaching out). The core question: given a name or company, fill in the contact data and context you need to act.

---

### Clay

**URL**: https://clay.com
**Pricing**: Paid (starts ~$149/month, credits-based)
**API/CLI**: ★★★★★

Waterfall enrichment platform that chains 100+ data providers into a single query. You give it a person or company, it runs through multiple sources until it finds verified data. Also functions as a spreadsheet-like workspace for building enrichment workflows visually.

**Best for**: Multi-source waterfall enrichment, email discovery for named contacts, role-based contact discovery at target companies, bulk list building with verified emails.

**Watch out**: Credits burn fast on large lists — a 500-row enrichment with email + phone can consume your monthly allocation in one run. Email verification is async (you must poll `get-task` for completion). Credit costs vary by provider hit in the waterfall chain.

#### Selection criteria

Pick Clay when you need high-accuracy enrichment across multiple data sources and are willing to pay for it. Clay's waterfall approach (try Provider A, fall back to B, then C) produces better hit rates than any single-source tool. Choose over Apollo when data accuracy matters more than volume, or when you need enrichment beyond just email/phone (technographics, funding data, custom signals).

#### Getting started

Clay has a native MCP server for Claude:

```json
{
  "mcpServers": {
    "clay": {
      "type": "url",
      "url": "https://app.clay.com/mcp/sse"
    }
  }
}
```

Authentication is OAuth-based through the Clay app.

Key MCP tools:
- `find-and-enrich-contacts-at-company` — role-based discovery (e.g., "find the Head of Marketing at Acme Corp")
- `find-and-enrich-list-of-contacts` — enrich a list of named people with email and other data
- `get-task` — poll for async enrichment results (email verification takes time)

Pattern: request enrichment, receive a task ID, poll `get-task` until state is `completed`. Do not re-request — the task will resolve.

#### Connects to

- **CRM (Attio, HubSpot, Salesforce)** — enriched contacts push to CRM records. Clay has native CRM integrations and API export.
- **Sequencing (Instantly, Apollo, Smartlead)** — enriched + verified emails feed directly into outbound sequences.
- **Exa AI** — use Exa for initial research/discovery, then Clay for structured enrichment on identified targets.
- **Orchestration (n8n, Make)** — trigger Clay enrichment from workflow automations.

---

### Apollo

**URL**: https://apollo.io
**Pricing**: Freemium (free tier: 10,000 email credits/month; Basic: $49/user/month; Professional: $79/user/month; Organization: $119/user/month — annual billing)
**API/CLI**: ★★★★☆

All-in-one sales intelligence platform with a 275M+ contact database. Combines contact discovery, email enrichment, and outbound sequencing in one tool. The free tier is unusually generous for enrichment — 10,000 email credits per month with basic API access.

**Best for**: Contact discovery at scale on a budget, email finding when you have name + company, teams that want enrichment and sequencing in one platform, getting started with enrichment without upfront cost.

**Watch out**: Advanced API access is locked behind the Organization plan ($119+/month) — free and Basic plans get limited API endpoints. Phone number credits are expensive (8 credits per number vs 1 per email). Data quality is single-source (no waterfall) so hit rates are lower than Clay for hard-to-find contacts. Credit system recently changed — more actions consume credits than before, including exports and AI features.

#### Selection criteria

Pick Apollo when budget matters and you need volume over precision. The free tier alone (10,000 email credits/month) beats most paid alternatives for basic email discovery. Choose over Clay when you want enrichment + sequencing in one tool, or when you are early-stage and cannot justify Clay's pricing. Choose Clay over Apollo when you need waterfall enrichment, higher accuracy on senior/executive contacts, or multi-provider data sourcing.

#### Getting started

Apollo launched a native MCP server (beta, Feb 2026) hosted on Apollo infrastructure:

```json
{
  "mcpServers": {
    "apollo": {
      "type": "url",
      "url": "https://mcp.apollo.io/sse"
    }
  }
}
```

Authentication is OAuth-based — connects to your Apollo account, no API key needed. All actions respect your plan's credit limits and permissions.

Community MCP servers also exist (e.g., `thevgergroup/apollo-io-mcp` on GitHub) for API-key-based access.

REST API: `https://api.apollo.io/api/v1/people/match` for single-person enrichment, `https://api.apollo.io/api/v1/mixed_people/search` for bulk search. API key from Settings > Integrations > API Keys.

#### Connects to

- **CRM (HubSpot, Salesforce, Attio)** — native HubSpot/Salesforce sync; Attio via API or orchestration layer.
- **Sequencing (built-in)** — Apollo has its own sequencing engine. Enriched contacts can be added to sequences directly via MCP.
- **Clay** — some teams use Apollo for initial discovery (free tier), then Clay for waterfall re-enrichment on high-priority targets.
- **Orchestration (n8n, Make, Zapier)** — Apollo's webhooks and API enable trigger-based enrichment in automated workflows.

---

### Exa AI

**URL**: https://exa.ai
**Pricing**: Freemium (API: $10 free credit on signup, then $5/1,000 searches; Websets: free tier 1,000 credits, Core $49/month, Pro $449/month)
**API/CLI**: ★★★★★

Semantic web search engine built for AI agents. Not a traditional contact database — instead, it searches the entire web using neural/keyword hybrid search and returns structured results. Doubles as enrichment when you need company context, news, public contact info, or technographic signals that structured databases miss.

**Best for**: Company research and context gathering, finding contact info via public web presence (bios, team pages, press), news monitoring and signal detection, discovering companies matching specific criteria (e.g., "B2B SaaS companies using Stripe with 50-200 employees"), filling gaps that structured enrichment tools miss.

**Watch out**: Not a structured contact database — will not reliably return verified email/phone the way Clay or Apollo will. Results depend on what is publicly available on the web. Best used as a complement to structured enrichment, not a replacement. Pay-per-search pricing can surprise you on high-volume use.

#### Selection criteria

Pick Exa when you need context, not just contact data. Structured tools (Clay, Apollo) give you email and phone; Exa gives you the "why should we reach out" — recent news, company tech stack mentions, hiring signals, product launches. Also pick Exa when targets are too niche or too new for traditional databases (startups, emerging brands, non-US companies). Use Exa for discovery and research, then hand off to Clay/Apollo for contact-level enrichment.

#### Getting started

Exa has a remote MCP server:

```json
{
  "mcpServers": {
    "exa": {
      "type": "url",
      "url": "https://mcp.exa.ai/mcp?exaApiKey=YOUR_API_KEY"
    }
  }
}
```

Get your API key at https://dashboard.exa.ai.

Key MCP tools:
- `web_search_exa` — semantic or keyword search across the web. Use `type: "neural"` for concept-based queries, `type: "keyword"` for exact matches.
- `get_code_context_exa` — search code repositories and documentation.

REST API: `POST https://api.exa.ai/search` with `query`, `type`, `numResults`, `contents` (for full-text extraction). Add `includeDomains` / `excludeDomains` to scope results.

#### Connects to

- **Clay** — Exa discovers targets and context, Clay enriches them with structured contact data. The most common pairing.
- **CRM (Attio, HubSpot)** — research output feeds into CRM notes and account context fields.
- **Orchestration (n8n, Make)** — trigger Exa searches from workflow events (e.g., new deal created -> research the company).
- **AI Agents (Claude, custom agents)** — Exa's MCP server makes it a natural research layer for any agent workflow.

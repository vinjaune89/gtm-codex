# CRM

The system of record. Every other GTM tool eventually syncs back here. Two philosophies: API-first (Attio) where your agent is the operator, and ecosystem-first (Salesforce) where the platform does everything and your agent plugs in.

---

### Attio

**Status**: ★ In our stack — daily use
**URL**: https://attio.com
**Pricing**: Freemium — free tier available, Plus from $29/seat/month, Pro from $59/seat/month, Enterprise custom
**API/CLI**: ★★★★★

API-first CRM built for programmatic workflows. Full REST API with granular object access, plus an official remote MCP server that lets AI agents search, create, update, and annotate records through natural language.

**Best for**: API-first teams, AI agent workflows, startups that want CRM logic in code rather than GUI config. Teams running Claude Code or similar agents against live pipeline data.

**Watch out**: Smaller ecosystem than Salesforce or HubSpot — fewer native integrations out of the box. You build what you need, which is a feature or a tax depending on your team. Entry ID and Record ID are different things in the API (common footgun — list entry operations use entry IDs, record operations use record IDs). List mutations (add-to-list, update-list-entry) require API scripts; the MCP server handles individual record ops but not bulk list operations.

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

- **Enrichment (Clay, Clearbit)** (enrichment.md): Enrich Attio records with contact data, firmographics, email verification
- **Outreach (Instantly, lemlist, Smartlead)** (outreach.md): Export segments from Attio lists into outbound sequences; sync reply/bounce status back
- **Discovery (Exa)** (discovery.md): Feed research results into Attio notes for account context
- **Orchestration (n8n, Make, Zapier)** (orchestration.md): Trigger workflows on Attio record changes, sync bidirectionally with other tools
- **Email verification** (via Attio's native email tracking): `search-emails-by-metadata` and `get-email-content` for verifying sends and detecting replies

---

### Salesforce

**Status**: Researched
**URL**: https://salesforce.com
**Pricing**: Enterprise — Starter $25/user/month, Professional $80/user/month, Enterprise $165/user/month, Unlimited $330/user/month
**API/CLI**: ★★★★★

The dominant enterprise CRM. 150,000+ customers, massive AppExchange ecosystem (7,000+ apps), and deep API surface (REST, SOAP, Bulk, Streaming, Metadata). Official Anthropic MCP server enables Claude-based agent workflows against Salesforce orgs. Recent Data Cloud and Einstein AI investments make it increasingly AI-native.

**Best for**: Enterprise and mid-market teams that need the ecosystem — native integrations with every sales, marketing, and service tool. Organizations where CRM is the platform, not just a database. Teams with dedicated RevOps/Salesforce admins. Companies where Salesforce is already the standard and migration cost exceeds the pain of working within it.

**Watch out**: Complexity is the real cost — not just per-seat pricing but admin overhead, consultant fees, and the customization tax. Simple tasks require configuration that would be a single API call in Attio. Pricing tiers gate critical features: API access needs Enterprise+, advanced automation needs Enterprise or Unlimited. The data model is powerful but rigid once configured — changing object relationships mid-flight is painful. MCP server is Anthropic-maintained but relatively new — expect gaps in coverage for advanced objects and flows.

#### Selection criteria

Pick Salesforce when your organization is already on Salesforce or when you need the ecosystem breadth (AppExchange integrations, compliance certifications, enterprise SSO). Also the right choice when you need a CRM that sales leadership already trusts for forecasting and pipeline management. If you're a small team, agent-first, and building from scratch — Attio is faster to set up, cheaper, and more API-friendly. If you're evaluating Salesforce vs. HubSpot, the question is depth vs. breadth: Salesforce goes deeper on CRM customization, HubSpot bundles marketing/content/service.

#### Getting started

1. Sign up at salesforce.com — Developer Edition is free with full API access (limited storage)
2. Create a Connected App for API access: Setup → App Manager → New Connected App
3. MCP server (official Anthropic):
```json
{
  "mcpServers": {
    "salesforce": {
      "command": "npx",
      "args": ["-y", "@anthropic/salesforce-mcp-server"],
      "env": {
        "SALESFORCE_INSTANCE_URL": "https://your-instance.salesforce.com",
        "SALESFORCE_ACCESS_TOKEN": "YOUR_ACCESS_TOKEN"
      }
    }
  }
}
```
4. REST API base: `https://your-instance.salesforce.com/services/data/vXX.0/`. OAuth 2.0 auth. Key endpoints: `/sobjects/` (CRUD), `/query/` (SOQL), `/composite/` (batch operations).
5. For agent workflows, use SOQL queries via MCP to search records, then REST API for updates.

#### Connects to

- **Enrichment (Clay, Apollo)** (enrichment.md): Enrich Salesforce contacts/leads with third-party data. Clay has native Salesforce push on Pro plan.
- **Outreach (Instantly, lemlist, Smartlead)** (outreach.md): Sequence enrollment from Salesforce lead lists. Reply/bounce status syncs back via native integrations or orchestration.
- **Orchestration (n8n, Composio/RUBE)** (orchestration.md): Trigger workflows on Salesforce record changes. n8n has native Salesforce nodes.
- **Discovery (Exa)** (discovery.md): Research results feed into Salesforce as notes or custom fields on accounts.
- **Signals (Common Room)** (signals.md): Community and intent signals sync to Salesforce contacts/accounts.

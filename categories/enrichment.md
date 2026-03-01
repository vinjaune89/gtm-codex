# Enrichment

Structured contact data — emails, phones, firmographics — from databases. Not web research (see Discovery). These tools answer "how do I reach this person?" once you already know who you want to reach.

---

### Clay
**Status**: ★ In our stack — enrichment + contact discovery
**URL**: https://clay.com
**Pricing**: Paid (starts ~$149/month, credits-based)
**API/CLI**: ★★★★★

Waterfall enrichment platform that chains 100+ data providers into a single query. You give it a person or company, it runs through multiple sources until it finds verified data. Also functions as a spreadsheet-like workspace for building enrichment workflows visually.

**Best for**: Multi-source waterfall enrichment, email discovery for named contacts, role-based contact discovery at target companies, bulk list building with verified emails.

**Watch out**: Credits burn fast on large lists — a 500-row enrichment with email + phone can consume your monthly allocation in one run. Email verification is async (you must poll `get-task` for completion). Credit costs vary by provider hit in the waterfall chain.

#### Selection criteria

Pick Clay when you need high-accuracy enrichment across multiple data sources and are willing to pay for it. Clay's waterfall approach (try Provider A, fall back to B, then C) produces better hit rates than any single-source tool. Choose over Apollo when data accuracy matters more than volume, or when you need enrichment beyond just email/phone (technographics, funding data, custom signals).

#### Getting started

Clay has two MCP paths:

**1. Native MCP server** (for enrichment via AI agents):

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

**2. Anthropic-hosted connector** (`claude_ai_Clay`): Available in Claude's MCP ecosystem. Tools include `find-and-enrich-contacts-at-company`, `find-and-enrich-list-of-contacts`, `get-task` (async polling). Email enrichment is async — poll with `get-task` until state is `completed`. Add `dataPoints: { contactDataPoints: [{ type: "Email" }] }` for email-only enrichment.

Key MCP tools (both paths):
- `find-and-enrich-contacts-at-company` — role-based discovery (e.g., "find the Head of Marketing at Acme Corp")
- `find-and-enrich-list-of-contacts` — enrich a list of named people with email and other data
- `get-task` — poll for async enrichment results (email verification takes time)

Pattern: request enrichment, receive a task ID, poll `get-task` until state is `completed`. Do not re-request — the task will resolve.

#### Connects to

- **CRM (Attio, Salesforce)** (crm.md): Enriched contacts push to CRM records. Clay has native CRM integrations and API export.
- **Outreach (Instantly, Smartlead, lemlist)** (outreach.md): Enriched + verified emails feed directly into outbound sequences.
- **Discovery (Exa)** (discovery.md): Use Exa for initial research/discovery, then Clay for structured enrichment on identified targets.
- **Orchestration (n8n, RUBE)** (orchestration.md): Trigger Clay enrichment from workflow automations.

---

### Apollo
**Status**: Researched
**URL**: https://apollo.io
**Pricing**: Freemium (free tier: 10,000 email credits/month; Basic: $49/user/month; Professional: $79/user/month; Organization: $119/user/month — annual billing)
**API/CLI**: ★★★★☆

All-in-one sales intelligence platform with a 275M+ contact database. Combines contact discovery, email enrichment, and outbound sequencing in one tool. The free tier is unusually generous for enrichment — 10,000 email credits per month with basic API access.

**Best for**: Contact discovery at scale on a budget, email finding when you have name + company, teams that want enrichment and sequencing in one platform, getting started with enrichment without upfront cost.

**Watch out**: Advanced API access is locked behind the Organization plan ($119+/month) — free and Basic plans get limited API endpoints. Phone number credits are expensive (8 credits per number vs 1 per email). Data quality is single-source (no waterfall) so hit rates are lower than Clay for hard-to-find contacts. Credit system recently changed — more actions consume credits than before, including exports and AI features.

#### Selection criteria

Pick Apollo when budget matters and you need volume over precision. The free tier alone (10,000 email credits/month) beats most paid alternatives for basic email discovery. Choose over Clay when you want enrichment + sequencing in one tool, or when you are early-stage and cannot justify Clay's pricing. Choose Clay over Apollo when you need waterfall enrichment, higher accuracy on senior/executive contacts, or multi-provider data sourcing.

#### Getting started

Apollo launched a native MCP server (beta, Feb 2026). Status and reliability are unverified — treat as experimental.

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

- **CRM (Attio, Salesforce)** (crm.md): Native Salesforce sync; Attio via API or orchestration layer.
- **Outreach (built-in + Instantly, Smartlead)** (outreach.md): Apollo has its own sequencing engine. Enriched contacts can be added to sequences directly via MCP, or exported to external sequencers.
- **Enrichment (Clay)** (enrichment.md): Some teams use Apollo for initial discovery (free tier), then Clay for waterfall re-enrichment on high-priority targets.
- **Orchestration (n8n, Make, Zapier)** (orchestration.md): Apollo's webhooks and API enable trigger-based enrichment in automated workflows.

---

### FullEnrich
**Status**: Researched
**URL**: https://fullenrich.com
**Pricing**: Paid — Starter $29/month (500 credits), Growth $79/month (2,500 credits), Scale $199/month (10,000 credits)
**API/CLI**: ★★★☆☆

Waterfall enrichment tool focused on email and phone discovery. Aggregates 15+ data providers into a single lookup — runs through each provider until it finds a verified result. Simpler and cheaper than Clay for pure contact enrichment (no table UI, no workflow orchestration — just find the email).

**Best for**: Email discovery when Clay feels like overkill. Teams that need waterfall enrichment (multi-provider email lookup) without building Clay tables. Supplementing Apollo when its single-source database misses contacts. Batch email finding for prospect lists.

**Watch out**: Credit-based pricing with no rollover — unused credits expire monthly. No MCP server, no workflow features — it's a lookup tool, not an orchestration platform. No free tier (unlike Apollo). For contact enrichment beyond email/phone (firmographics, technographics), you still need Clay or Apollo.

#### Selection criteria

Pick FullEnrich when you need email enrichment specifically and want waterfall accuracy without Clay's complexity or cost. It sits between Apollo (single-source, cheaper) and Clay (full orchestration, more expensive). If you need more than just email/phone — company data, tech stack, funding — Clay is the better choice. If budget is the constraint and single-source accuracy is acceptable, Apollo's free tier is hard to beat.

#### Getting started

1. Sign up at fullenrich.com
2. API key from dashboard
3. REST API for single and batch enrichment lookups
4. No MCP server available. Integrate via HTTP requests or orchestration tools.
5. CSV upload available for batch processing

#### Connects to

- **CRM (Attio, Salesforce)** (crm.md): Enriched emails push to CRM contact records via API or CSV import.
- **Outreach (Instantly, Smartlead)** (outreach.md): Verified emails feed directly into outbound sequences.
- **Discovery (Exa)** (discovery.md): Exa discovers targets, FullEnrich finds their emails.
- **Orchestration (n8n)** (orchestration.md): HTTP Request node calls FullEnrich API in automated workflows.

---

### Findymail
**Status**: Researched
**URL**: https://findymail.com
**Pricing**: Paid — Starter $49/month (1,000 credits), Standard $99/month (5,000 credits), Scale $249/month (25,000 credits)
**API/CLI**: ★★★☆☆

Email finder and verification tool with a focus on deliverability. Claims <1% bounce rate on verified emails — significantly lower than most enrichment tools. Combines multiple data sources for finding emails and runs real-time SMTP verification before returning results. Also offers a LinkedIn email finder Chrome extension.

**Best for**: High-deliverability email campaigns where bounce rate matters. Teams that have been burned by bad email data from cheaper providers. Verifying existing email lists before loading into a sequencer. Finding emails from LinkedIn profiles (via Chrome extension).

**Watch out**: No free tier — $49/month minimum. Credits are per-email, not per-search (failed lookups don't consume credits, which is good). Limited to email — no phone numbers, no firmographics. No MCP server. API is REST-only. The Chrome extension requires manual use (not agent-automatable).

#### Selection criteria

Pick Findymail when email deliverability is critical — specifically when you're running cold email at volume and can't afford bounces destroying your sender reputation. The <1% bounce claim is their differentiator. If you need waterfall enrichment (email + phone + company data), Clay or FullEnrich are more complete. If you just need email verification (not finding), dedicated verification tools exist. Findymail sits in the "find AND verify in one step" niche.

#### Getting started

1. Sign up at findymail.com
2. API key from dashboard settings
3. REST API: `POST https://app.findymail.com/api/search/mail` with `{ "name": "...", "domain": "..." }`
4. Verification endpoint: `POST https://app.findymail.com/api/verify/single`
5. No MCP server. Chrome extension available for LinkedIn-based lookups.
6. Integrations: native export to Clay, Apollo, Salesforce

#### Connects to

- **CRM (Attio, Salesforce)** (crm.md): Verified emails update CRM contact records.
- **Outreach (Instantly, Smartlead, lemlist)** (outreach.md): High-confidence emails feed cold email campaigns with minimal bounce risk.
- **Enrichment (Clay)** (enrichment.md): Findymail can be a provider within Clay's waterfall enrichment chain.
- **Discovery (Exa)** (discovery.md): Exa finds prospects, Findymail finds and verifies their emails.

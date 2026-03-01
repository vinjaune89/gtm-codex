# Sequencing

Tools that automate multi-step outbound campaigns — scheduling emails, follow-ups, and (in some cases) LinkedIn touches across a defined sequence. Sequencing sits downstream of enrichment: once you have verified contacts, the sequencer delivers your messages on a cadence, handles warm-up, tracks replies, and stops the sequence when someone engages.

Worth noting: some GTM engineers skip sequencers entirely and send each message manually. This makes sense when volume is low, positioning is premium, and every message is crafted for a specific person. Sequencers shine when you need to run 50-500+ contacts through a repeatable cadence. The tradeoff is always volume vs. signal quality — a hand-sent DM from a founder hits different than email #3 in a 7-step drip.

---

### Instantly

**URL**: https://instantly.ai
**Pricing**: Paid (Outreach: $47/month Growth, $97/month Hypergrowth, $358/month Light Speed — 20% off annual; separate Lead Database and CRM plans)
**API/CLI**: ★★★★★

High-volume cold email platform with built-in warm-up, a 200K+ account warm-up network, and a lead database (B2B Data separate add-on). Handles email account rotation, deliverability monitoring, and inbox placement testing. API V2 is fully RESTful with interactive docs.

**Best for**: High-volume cold email at scale, teams managing many sending accounts (unlimited on all plans), agencies running campaigns across multiple clients, anyone who wants warm-up and sending in one platform.

**Watch out**: Email-only — no native LinkedIn or phone steps. The lead database, CRM, and outreach are three separate billing products (you can easily end up paying $47+$47+$47 = $141/month for the full stack). Growth plan caps at 1,000 uploaded contacts and 5,000 emails/month — real volume requires Hypergrowth ($97). Warm-up needs 2+ weeks before you should send cold.

#### Selection criteria

Pick Instantly when cold email is your primary channel and you need to manage multiple sending accounts with built-in warm-up. The unlimited email accounts on every plan make it the default choice for agencies and teams doing sender rotation. Choose over Smartlead when you want a simpler UI and a built-in lead database. Choose over lemlist when you do not need multi-channel (LinkedIn, calls) and want lower per-seat cost at high volume.

#### Getting started

Instantly has a native MCP server:

```json
{
  "mcpServers": {
    "instantly": {
      "type": "url",
      "url": "https://mcp.instantly.ai/mcp",
      "headers": {
        "Authorization": "YOUR_API_KEY"
      }
    }
  }
}
```

Get your API key from Settings > Integrations > API Keys in the Instantly dashboard. API V2 access is available on the Growth plan and above.

REST API base: `https://api.instantly.ai/api/v2/`. Bearer token auth. Key endpoints: campaigns (CRUD, pause, resume), leads (add, list, update status), analytics (open/reply/bounce rates), email accounts (manage, warm-up status). Full interactive docs at https://developer.instantly.ai.

#### Connects to

- **Enrichment (Clay, Apollo)** — enriched contact lists with verified emails import directly into Instantly campaigns.
- **CRM (Attio, HubSpot, Salesforce)** — reply detection triggers CRM updates via webhooks or Zapier. Instantly also has its own CRM product.
- **Orchestration (n8n, Make, Zapier)** — API + webhooks enable automated campaign creation and lead routing.
- **Research (Exa AI)** — research identifies targets, enrichment verifies contacts, Instantly delivers the sequence.

---

### lemlist

**URL**: https://lemlist.com
**Pricing**: Paid (Email Pro: $79/user/month, Multichannel Expert: $109/user/month, Enterprise: custom — 20% off annual)
**API/CLI**: ★★★★☆

Multi-channel outreach platform combining email, LinkedIn automation, and calls in a single sequence. Lets you build branching sequences with conditions (if opened email, send LinkedIn invite; if not, follow up via email). Includes a 600M+ lead database and lemwarm deliverability tool on all plans.

**Best for**: Multi-channel sequences (email + LinkedIn + calls), teams that want conditional branching logic (if/then steps based on prospect behavior), personalized outreach with dynamic images and landing pages, B2B teams where LinkedIn is a primary engagement channel.

**Watch out**: Per-seat pricing adds up fast for teams — 5 users on Multichannel Expert is $545/month. LinkedIn automation requires the Multichannel Expert plan ($109/user). Email sender limits are per-user (3 on Email Pro, 5 on Multichannel Expert) — heavy senders need multiple seats or higher plans. The API uses Basic auth with an empty username (unusual pattern — check docs).

#### Selection criteria

Pick lemlist when you need multi-channel sequences — specifically email + LinkedIn in one workflow with conditional branching. It is the strongest option here for teams that engage prospects across channels and want automation to handle the routing logic. Choose over Instantly when LinkedIn outreach is part of your motion. Choose over Smartlead when you want a polished UI with built-in multi-channel rather than a pure email infrastructure play.

#### Getting started

lemlist has a native MCP server with two auth options:

**OAuth (recommended):**
```json
{
  "mcpServers": {
    "lemlist": {
      "command": "npx",
      "args": ["mcp-remote", "https://app.lemlist.com/mcp"]
    }
  }
}
```

First tool call opens a browser consent page. Tokens auto-refresh.

**API key:**
```json
{
  "mcpServers": {
    "lemlist": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://app.lemlist.com/mcp",
        "--header",
        "X-API-Key: ${API_KEY}"
      ],
      "env": {
        "API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

REST API base: `https://api.lemlist.com/api/`. Basic auth (empty username, API key as password). Key endpoints: campaigns (list, stats), leads (add to campaign, update, delete), activities (fetch by type), webhooks (subscribe to emailsSent, emailsOpened, emailsReplied, linkedinInterested, and more). Webhook scoping per campaign is supported.

Full docs at https://developer.lemlist.com.

#### Connects to

- **Enrichment (Clay, Apollo)** — enriched leads import into lemlist campaigns. lemlist also has its own 600M+ database with enrichment credits.
- **CRM (HubSpot, Salesforce, Pipedrive)** — native two-way sync. Attio via webhooks + orchestration.
- **Orchestration (n8n, Make, Zapier)** — webhooks fire on email events, enabling automated CRM updates and pipeline routing.
- **LinkedIn** — native LinkedIn automation (profile visits, connection requests, messages) built into sequences on Multichannel Expert plan.

---

### Smartlead

**URL**: https://smartlead.ai
**Pricing**: Paid (Base: $39/month, Pro: $94/month, Smart: $174/month, Prime: $379/month — ~17% off annual)
**API/CLI**: ★★★★★

API-first cold email infrastructure built for agencies and technical teams. Differentiates on deliverability mechanics — uses variable sending volumes (sets 25/day, actually sends 22) to mimic human patterns and dodge spam filters. Unlimited team seats on all plans. White-label support for agencies ($29/client).

**Best for**: Agencies managing multiple client accounts (white-label + unlimited seats), technical teams that want API-first campaign management, high-volume senders who care about deliverability tuning, teams building custom sending infrastructure with webhooks and automation.

**Watch out**: No built-in lead database — you must bring your own prospect lists (use Clay, Apollo, or Exa upstream). Email-only like Instantly — no native LinkedIn or phone steps. The Base plan ($39) is cheap but limited: 2,000 contacts, 6,000 emails/month, no CRM access. Real usage starts at Pro ($94). Agency white-label costs scale: $29/client on top of plan price.

#### Selection criteria

Pick Smartlead when you are an agency or technical team that wants API-level control over sending infrastructure. The unlimited seats model beats lemlist's per-user pricing for larger teams. The variable-volume sending and deliverability controls appeal to operators who have been burned by spam filters. Choose over Instantly when you need white-label agency features or want more granular deliverability control. Choose over lemlist when you only need email (not multi-channel) and want lower cost with unlimited team members.

#### Getting started

Smartlead has an MCP server via SSE:

```json
{
  "mcpServers": {
    "smartlead": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.smartlead.ai/sse?user_api_key=YOUR_API_KEY"
      ]
    }
  }
}
```

Get your API key from Dashboard > Settings > API Keys. Requires Node.js installed for `npx`.

REST API base: `https://server.smartlead.ai/api/v1/`. API key auth via query parameter. Key endpoints: campaigns (create, update, analytics), leads (add, fetch globally, manage per-campaign), email accounts (add, configure SMTP, attach to campaigns), master inbox (reply to leads programmatically). Full docs at https://api.smartlead.ai/reference.

#### Connects to

- **Enrichment (Clay, Apollo)** — essential pairing since Smartlead has no built-in database. Enriched + verified lists import via API.
- **CRM (HubSpot, Salesforce, Pipedrive)** — native integrations on Pro plan and above. Attio via API + webhooks.
- **Orchestration (n8n, Make, Zapier)** — API + webhooks enable full automation: trigger campaigns on CRM events, route replies to pipelines, sync analytics.
- **Instantly** — some teams run both: Instantly for warm-up pool access and lead database, Smartlead for actual sending with tighter deliverability control. Not common but possible.

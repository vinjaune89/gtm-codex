# Outreach

Tools that deliver your message — email and LinkedIn. The spectrum runs from high-volume automated sequences to individually crafted outreach. Worth noting: some GTM engineers skip sequencers entirely and send each message manually. This makes sense when volume is low, positioning is premium, and every message is crafted for a specific person. Sequencers shine at 50-500+ contacts through a repeatable cadence.

## The Outreach Spectrum

| Approach | Tools | Volume | Signal Quality |
|----------|-------|--------|---------------|
| High-volume email | Instantly, Smartlead | 1000s/month | Lower — templated |
| Multi-channel automated | lemlist, La Growth Machine | 100s/month | Medium — branching logic |
| LinkedIn-native | HeyReach | 100s/month | Medium — platform-native |
| Manual/crafted | Claude Code + Gmail/LinkedIn | 10s/month | Highest — human-reviewed |

---

### Instantly

**Status**: Researched
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

- **Enrichment (Clay, Apollo)** (enrichment.md): Enriched contact lists with verified emails import directly into Instantly campaigns.
- **CRM (Attio, HubSpot, Salesforce)** (crm.md): Reply detection triggers CRM updates via webhooks or Zapier. Instantly also has its own CRM product.
- **Orchestration (n8n, Make, Zapier)** (orchestration.md): API + webhooks enable automated campaign creation and lead routing.
- **Discovery (Exa AI)** (discovery.md): Research identifies targets, enrichment verifies contacts, Instantly delivers the sequence.

---

### lemlist

**Status**: Researched
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

- **Enrichment (Clay, Apollo)** (enrichment.md): Enriched leads import into lemlist campaigns. lemlist also has its own 600M+ database with enrichment credits.
- **CRM (HubSpot, Salesforce, Pipedrive)** (crm.md): Native two-way sync. Attio via webhooks + orchestration.
- **Orchestration (n8n, Make, Zapier)** (orchestration.md): Webhooks fire on email events, enabling automated CRM updates and pipeline routing.

---

### Smartlead

**Status**: Researched
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

- **Enrichment (Clay, Apollo)** (enrichment.md): Essential pairing since Smartlead has no built-in database. Enriched + verified lists import via API.
- **CRM (HubSpot, Salesforce, Pipedrive)** (crm.md): Native integrations on Pro plan and above. Attio via API + webhooks.
- **Orchestration (n8n, Make, Zapier)** (orchestration.md): API + webhooks enable full automation: trigger campaigns on CRM events, route replies to pipelines, sync analytics.
- **Outreach (Instantly)** (this file): Some teams run both: Instantly for warm-up pool access and lead database, Smartlead for actual sending with tighter deliverability control. Not common but possible.

---

### HeyReach

**Status**: Researched
**URL**: https://heyreach.io
**Pricing**: Paid — Starter $79/seat/month (3 LinkedIn senders), Business $199/seat/month (unlimited senders). 14-day free trial.
**API/CLI**: ★★★★★

LinkedIn outreach platform with native MCP server — the only LinkedIn automation tool in this codex with direct agent integration. Multi-sender rotation across LinkedIn accounts (team members' profiles send in rotation to avoid per-account limits). Automates connection requests, messages, InMails, profile views, and post engagement.

**Best for**: LinkedIn-first outreach at scale. Teams with multiple LinkedIn accounts who want sender rotation (spreads activity across profiles to stay under LinkedIn's radar). Agent-driven LinkedIn outreach workflows where Claude can manage campaigns, add leads, and track responses programmatically. B2B teams where LinkedIn is the primary engagement channel and email alone isn't cutting it.

**Watch out**: LinkedIn automation always carries platform risk — accounts can be restricted or banned. HeyReach mitigates this with multi-sender rotation and human-like activity patterns, but the risk is non-zero. Per-seat pricing means cost scales with operators, not send volume. The Starter plan limits you to 3 LinkedIn senders per seat. LinkedIn's rate limits (100 connection requests/week) apply regardless of tooling.

#### Selection criteria

Pick HeyReach when LinkedIn is a primary outreach channel and you need to scale beyond manual sending. The MCP server is the key differentiator for agent-driven workflows — no other LinkedIn tool lets your AI agent manage campaigns directly. Choose over manual LinkedIn when volume exceeds what one person can do by hand. Pair with an email sequencer (Instantly, Smartlead) for true multi-channel coverage. If you only need LinkedIn for research (not outreach), you don't need this — use LinkedIn directly or scraping tools.

#### Getting started

1. Sign up at heyreach.io — 14-day free trial
2. Connect LinkedIn accounts (Chrome extension or session cookie)
3. MCP server (two options):

**Local (npx):**
```json
{
  "mcpServers": {
    "heyreach": {
      "command": "npx",
      "args": ["-y", "heyreach-mcp-server"],
      "env": {
        "HEYREACH_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

**Hosted (remote):**
```json
{
  "mcpServers": {
    "heyreach": {
      "type": "url",
      "url": "https://mcp.heyreach.io/mcp/YOUR_API_KEY"
    }
  }
}
```

4. API key from Dashboard > Settings > API. REST API base: `https://api.heyreach.io/api/v1/`.
5. Key MCP tools: create campaigns, add leads, manage sender accounts, track campaign analytics.

#### Connects to

- **Enrichment (Clay, Apollo)** (enrichment.md): Enriched contact lists with LinkedIn URLs feed directly into HeyReach campaigns.
- **CRM (Attio, Salesforce)** (crm.md): LinkedIn reply detection triggers CRM updates. Attio via webhook + orchestration.
- **Outreach (Instantly, Smartlead)** (this file): Pair HeyReach (LinkedIn) with email sequencers for true multi-channel. "Email didn't get a reply → trigger LinkedIn sequence."
- **Orchestration (n8n, RUBE)** (orchestration.md): API + webhooks enable automated campaign management and lead routing.
- **Signals (Trigify)** (signals.md): LinkedIn signals (job change) trigger HeyReach outreach sequences.

---

### La Growth Machine

**Status**: Researched
**URL**: https://lagrowthmachine.com
**Pricing**: Paid — Basic €60/identity/month (email + LinkedIn), Pro €100/identity/month (+ X/Twitter), Custom from €150/identity/month (+ voice). 14-day free trial.
**API/CLI**: ★★★☆☆

Multi-channel outreach platform combining email, LinkedIn, and X/Twitter in a single sequence with conditional branching. Per-identity pricing: each "identity" is a set of connected accounts (one LinkedIn, one email, one X) that acts as a sender. Visual sequence builder with if/then logic across channels.

**Best for**: True multi-channel sequences where you want email + LinkedIn + X in one automated flow with conditional routing. "If LinkedIn connection accepted → send LinkedIn message. If not → email. If email opened → X DM." Teams that want to cover all channels without stitching multiple tools together.

**Watch out**: Per-identity pricing adds up — €60/identity means a team of 3 is €180/month minimum. LinkedIn automation carries the same platform risk as HeyReach. The API is limited compared to Instantly or Smartlead — LGM is designed as a GUI-first tool with API as secondary. X/Twitter integration requires Pro plan (€100). Voice features (cold calling integration) on Custom plan only. No MCP server.

#### Selection criteria

Pick La Growth Machine when you need true multi-channel automation (email + LinkedIn + X) in a single sequence with conditional branching. It's the most complete multi-channel tool here — lemlist does email + LinkedIn, but LGM adds X/Twitter and has more sophisticated branching. Choose LGM over lemlist when X/Twitter is part of your outreach motion. Choose lemlist over LGM when you want a larger lead database and more mature API. Choose Instantly/Smartlead over LGM when email-only at high volume is your motion. If you only need LinkedIn, HeyReach is more focused and has MCP support.

#### Getting started

1. Sign up at lagrowthmachine.com — 14-day free trial
2. Connect your identities: LinkedIn account, email (Gmail/Outlook/SMTP), X/Twitter (Pro plan)
3. Build sequences in the visual editor: choose channels, add steps, set delays, add conditions
4. Import leads via CSV, CRM integration, or LinkedIn Sales Navigator
5. API available for campaign management — REST endpoints, API key auth. Documentation at docs.lagrowthmachine.com.
6. No MCP server. Integrate via API or orchestration tools (Zapier, Make, n8n).

#### Connects to

- **Enrichment (Clay, Apollo)** (enrichment.md): Enriched contacts with email + LinkedIn URL import directly into LGM sequences.
- **CRM (Attio, Salesforce, HubSpot)** (crm.md): Native HubSpot/Salesforce sync. Attio via API + orchestration.
- **Orchestration (n8n, Make)** (orchestration.md): API + webhooks enable automation — trigger sequences on CRM events, route replies.
- **Discovery (Exa)** (discovery.md): Research surfaces targets, enrichment adds contact data, LGM delivers the multi-channel sequence.

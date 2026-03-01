# GTM Tool Decision Tree

AI-readable guide for selecting tools based on what you need to accomplish. Start with the task, follow the branches.

---

## Find and Enrich Contacts

```
I need contact data (emails, phones, titles)
├── For a specific named person?
│   ├── Yes → Clay (find-and-enrich-list-of-contacts) or Apollo search
│   └── No, I need to discover people at a company
│       ├── By role/title → Clay (find-and-enrich-contacts-at-company)
│       ├── By web presence → Exa AI (semantic search for "[role] at [company]")
│       └── Bulk discovery (100+ companies) → Apollo or Clay tables with waterfall
│
├── I already have names, need emails
│   ├── Small batch (<20) → Clay enrichment with email dataPoint
│   ├── Large batch (100+) → Apollo bulk enrichment or Clay table
│   └── Need verified emails → Clay (built-in verification) or Apollo
│
└── I need company data (funding, size, tech stack)
    ├── Structured data → Clay company enrichment or Apollo
    ├── Unstructured research → Exa AI semantic search
    └── Deep/niche research → Linkup
```

## Send Outbound Email

```
I need to send cold emails
├── High volume (1000+ emails/month)?
│   ├── Yes → Instantly or Smartlead (built-in warm-up, rotation)
│   └── No, low volume but high quality
│       ├── Multi-channel (email + LinkedIn)? → lemlist
│       ├── Each message individually crafted? → Direct send (Gmail/Outlook) via AI agent
│       └── Templated but personalized? → Any sequencer + enrichment data
│
├── Deliverability is critical?
│   ├── Yes → Instantly (warm-up infrastructure) or Smartlead
│   └── I'm sending from my main domain → Be careful. Consider a separate sending domain.
│
└── I need to track replies and manage sequences
    ├── GUI-first workflow → lemlist or Instantly dashboard
    └── API-first workflow → Instantly API or Smartlead API + CRM webhooks
```

## Research Companies and Prospects

```
I need to research a company or prospect
├── Company overview (what they do, size, funding)?
│   ├── Quick facts → Exa AI or Apollo company profile
│   ├── Deep research (strategy, news, competitive landscape) → Exa + Linkup
│   └── Financial/legal data → Linkup (accesses niche sources)
│
├── Prospect research (who is this person)?
│   ├── Professional background → Exa search for LinkedIn + web mentions
│   ├── Recent activity/posts → Exa with recency filter
│   └── Contact info → See "Find and Enrich Contacts" above
│
├── Web scraping (extract data from pages)?
│   ├── Single page → Firecrawl extract
│   ├── Multiple pages / crawl a site → Firecrawl crawl
│   └── JS-rendered content → Firecrawl (handles JS) or browser automation
│
└── Competitive analysis?
    ├── Who are their competitors? → Exa ("companies similar to [company]")
    ├── What tech do they use? → Clay technographics or BuiltWith
    └── Market positioning → Exa + Linkup for deep research
```

## Manage Pipeline and CRM

```
I need a CRM / pipeline management
├── API-first, works with AI agents?
│   ├── Yes → Attio (MCP server, full API, great for automation)
│   └── I need a huge ecosystem of integrations → HubSpot
│
├── I want enrichment + CRM in one tool?
│   └── Clay (tables as CRM + waterfall enrichment)
│
├── I need to update CRM from my AI agent?
│   ├── Attio → Use MCP server (search, create, update records)
│   ├── HubSpot → Use API (well-documented, many SDKs)
│   └── Clay → Use Clay MCP or API for table operations
│
└── Budget is tight?
    ├── Free CRM → HubSpot free tier or Attio free tier
    └── Spreadsheet + enrichment → Clay free tier or Google Sheets + Apollo
```

## Detect Buying Signals

```
I want to know when prospects are ready to buy
├── Website visitors (who's on my site)?
│   ├── SMB / self-serve → RB2B (pixel-based, identifies visitors)
│   └── Enterprise → 6sense (account-level intent, ABM)
│
├── Community and social signals?
│   └── Common Room (aggregates signals from GitHub, Slack, Discord, social)
│
├── Content engagement signals?
│   ├── Who's reading my content? → Substack analytics + Common Room
│   └── Who's engaging on LinkedIn? → Shield analytics
│
└── Third-party intent data (who's researching my category)?
    ├── Enterprise budget → 6sense or Bombora
    └── Scrappy approach → RB2B + Exa news monitoring + manual signal detection
```

## Automate Workflows

```
I need to connect tools and automate processes
├── AI agent orchestration (LLM-driven)?
│   ├── Claude Code + MCP tools → Composio/RUBE (500+ app connections)
│   └── Custom agent → Claude API or OpenAI + tool integrations
│
├── Visual workflow builder?
│   ├── Self-hosted → n8n (open-source, full control)
│   ├── Cloud, simple workflows → Zapier or Make
│   └── Complex data workflows → Clay tables
│
├── Enrichment-heavy workflows?
│   └── Clay tables (waterfall enrichment built in)
│
└── I need to connect Gmail, LinkedIn, Slack to my AI agent?
    └── Composio/RUBE via MCP
```

## Create Content and Build Audience

```
I want to build thought leadership / content engine
├── Long-form content?
│   └── Substack (newsletter + blog, built-in audience features)
│
├── LinkedIn presence?
│   ├── Schedule posts → Taplio
│   ├── Track performance → Shield
│   └── Engage strategically → Manual + AI agent for research
│
├── Multi-channel content?
│   ├── Repurpose long-form → Substack → LinkedIn posts → X threads
│   └── Automate distribution → n8n or Zapier workflows
│
└── Content as GTM (attract inbound)?
    └── Substack + LinkedIn + community participation
```

---

## Meta-Decision: Build vs Buy

```
Should I build my own or use a SaaS tool?
├── Is there an MCP server or strong API?
│   ├── Yes → Use the tool, automate via agent
│   └── No → Consider building a custom integration or choosing a different tool
│
├── How central is this to my workflow?
│   ├── Core (daily use) → Pay for the right tool
│   ├── Occasional → Free tier or build a simple script
│   └── Experimental → Start free, upgrade if it sticks
│
└── Team size?
    ├── Solo / 2-person → Lean stack, fewer tools, more agent automation
    ├── Small team (3-10) → Standard tools, shared workflows
    └── Enterprise → Enterprise tools with SSO, permissions, compliance
```

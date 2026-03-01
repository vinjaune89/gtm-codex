# GTM Tool Decision Tree

AI-readable guide for selecting tools based on what you need to accomplish. Start with the task, follow the branches.

---

## Discover Targets

```
I need to find companies or prospects to target
├── By description (semantic search)?
│   ├── "Companies like X" → Exa AI (discovery.md) — neural search
│   ├── "Find companies matching [criteria]" → Exa AI with filters
│   └── Deep research (strategy, news, competitive landscape) → Linkup Deep (discovery.md)
│
├── By precise filters (size, industry, tech stack)?
│   ├── Structured database filters → Sumble (discovery.md)
│   └── Lookalike expansion from seed companies → DiscoLike (discovery.md)
│
├── ICP scoring (how well does a prospect match)?
│   └── Octave (discovery.md) — programmatic ICP scoring via MCP
│
└── Company research on a known target?
    ├── Quick overview → Exa AI
    ├── Deep dive → Linkup Deep
    └── Extract data from their website → Firecrawl (scraping.md)
```

## Enrich Contacts

```
I need contact data (emails, phones, titles)
├── For a specific named person?
│   ├── Yes → Clay (enrichment.md) — find-and-enrich-list-of-contacts
│   └── No, I need to discover people at a company
│       ├── By role/title → Clay — find-and-enrich-contacts-at-company
│       └── Bulk discovery (100+ companies) → Apollo or Clay tables with waterfall
│
├── I already have names, need emails
│   ├── Small batch (<20) → Clay enrichment with email dataPoint
│   ├── Large batch (100+) → Apollo bulk enrichment or Clay table
│   ├── Need waterfall without Clay's complexity → FullEnrich (enrichment.md)
│   └── Need verified emails with <1% bounce → Findymail (enrichment.md)
│
└── I need company data (funding, size, tech stack)
    ├── Structured data → Clay company enrichment or Apollo
    ├── Unstructured research → Exa AI (discovery.md) semantic search
    └── Deep/niche research → Linkup (discovery.md)
```

## Reach Out

```
I need to send outbound messages
├── Email only?
│   ├── High volume (1000+ emails/month) → Instantly or Smartlead (outreach.md)
│   ├── Mid volume with conditional logic → lemlist (outreach.md)
│   └── Low volume, human-reviewed → Direct send (Gmail) via AI agent
│
├── LinkedIn only?
│   ├── At scale (multi-sender rotation) → HeyReach (outreach.md) — has MCP
│   └── Manual, crafted messages → Direct LinkedIn via browser
│
├── Multi-channel (email + LinkedIn + X)?
│   ├── Full automation → La Growth Machine (outreach.md)
│   ├── Email + LinkedIn → lemlist or HeyReach + Instantly
│   └── Manual + agent-drafted → Claude Code + Gmail + LinkedIn
│
└── Deliverability is critical?
    ├── Yes → Instantly (warm-up infrastructure) or Smartlead
    └── I'm sending from my main domain → Be careful. Consider a separate sending domain.
```

## Extract Web Data

```
I need to scrape or extract data from web pages
├── Single page to markdown/structured data?
│   ├── JavaScript-rendered? → Firecrawl (scraping.md)
│   ├── Simple HTML? → Basic fetch or defuddle CLI
│   └── Need AI-powered extraction? → Firecrawl /extract endpoint
│
├── Crawl an entire site?
│   └── Firecrawl crawl (scraping.md) — follows links, handles JS
│
├── Platform-specific scraping (LinkedIn, Google Maps, etc)?
│   └── Apify (scraping.md) — 3,000+ pre-built Actors
│
└── Scheduled/recurring scrapes?
    └── Apify with scheduled runs
```

## Monitor Signals

```
I want to know when to reach out
├── LinkedIn signals (job changes, promotions, engagement)?
│   └── Trigify (signals.md) — LinkedIn signal detection
│
├── Community signals (Slack, GitHub, Discord, social)?
│   └── Common Room (signals.md) — multi-source signal aggregation
│
└── Custom signal detection?
    └── Exa (discovery.md) with recency filters + orchestration (n8n) for scheduling
```

## Manage Pipeline and CRM

```
I need a CRM / pipeline management
├── API-first, works with AI agents?
│   └── Attio (crm.md) — MCP server, full API, agent-native
│
├── Enterprise ecosystem with AppExchange?
│   └── Salesforce (crm.md) — Anthropic MCP server, 7K+ integrations
│
├── I need to update CRM from my AI agent?
│   ├── Attio → Use MCP server (search, create, update records)
│   └── Salesforce → Use Anthropic MCP server + SOQL
│
└── Budget is tight?
    ├── Free CRM → Attio free tier
    └── Spreadsheet + enrichment → Google Sheets + Apollo free tier
```

## Automate Workflows

```
I need to connect tools and automate processes
├── AI agent orchestration (LLM-driven)?
│   └── Composio/RUBE (orchestration.md) — 600+ app connections via MCP
│
├── Visual workflow builder?
│   ├── Self-hosted → n8n (orchestration.md) — open-source, full control
│   ├── Cloud, simple workflows → Zapier or Make
│   └── Complex data workflows → Clay tables (orchestration.md)
│
├── GTM-specific workflow platform?
│   └── Cargo (orchestration.md) — enrichment + routing + CRM sync
│
├── Enrichment-heavy workflows?
│   └── Clay tables — waterfall enrichment built in
│
└── I need to connect Gmail, LinkedIn, Slack to my AI agent?
    └── Composio/RUBE via MCP
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

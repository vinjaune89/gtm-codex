# Discovery

Tools that find companies and prospects. From semantic web search to ICP matching engines to knowledge graphs. These answer "who should I target?" at different levels of specificity.

---

### Exa AI
**Status**: ★ In our stack — daily use
**URL**: https://exa.ai
**Pricing**: Pay-per-search — $5 per 1,000 queries ($0.005/search). Websets (bulk): Core $49/month, Pro $449/month
**API/CLI**: ★★★★★

Semantic web search built for AI agents. Finds companies, people, articles, and pages by meaning — not keyword matching. Returns clean, structured results with content extraction. The Exa Instant endpoint delivers results in under 200ms for real-time agent workflows.

**Best for**: Company research, finding decision-makers, competitive intelligence, news monitoring, discovering prospects that match a natural-language description. Queries like "Series A B2B SaaS companies in Nordics using Stripe" return results that keyword search never would.

**Watch out**: Not a database — it searches the live web, so result quality depends heavily on query crafting. Vague queries return vague results. Common company names (Sphere, Modal) produce false positives; always cross-reference with canonical URLs. Content extraction costs additional credits. The Websets product (bulk enrichment) has a separate pricing model from the search API.

#### Selection criteria

Pick Exa when you need to discover entities (companies, people, articles) that match a description rather than a keyword. It excels at the "find me more like this" problem. If you need to scrape a specific known page, use Firecrawl (scraping.md). If you need deep research with chain-of-thought reasoning over multiple sources, use Linkup Deep. If you need structured contact data (emails, phones), pair Exa discovery with an enrichment tool like Clay or Apollo (enrichment.md).

#### Getting started

1. Get API key at dashboard.exa.ai
2. MCP server — use `"type": "url"`:

```json
{
  "mcpServers": {
    "exa": {
      "type": "url",
      "url": "https://mcp.exa.ai/mcp?exaApiKey=YOUR_KEY"
    }
  }
}
```

3. Two main endpoints: `/search` (find pages) and `/contents` (extract text from URLs)
4. Use `type: "neural"` for semantic search, `type: "keyword"` for exact matches
5. Filter by domain, date range, and category to tighten results

```python
# Python SDK example
from exa_py import Exa
exa = Exa("YOUR_API_KEY")
results = exa.search_and_contents(
    "B2B SaaS companies using Stripe for international payments",
    num_results=10,
    text=True
)
```

#### Connects to

- **Clay** (enrichment.md): Exa discovers companies/people, Clay enriches with contact data and emails
- **Firecrawl** (scraping.md): Exa finds the pages, Firecrawl extracts structured content from them
- **CRM tools** (crm.md): Research results feed into CRM as new leads or account intelligence
- **Outreach tools** (outreach.md): Research-backed personalization data flows into outreach sequences
- **Orchestration tools** (orchestration.md): Exa searches can be triggered and chained in automated workflows

---

### Linkup
**Status**: ★ In our stack — research depth
**URL**: https://linkup.so
**Pricing**: Pay-per-search — approximately EUR 0.05/query (Deep mode). Free tier available for testing
**API/CLI**: ★★★★★

Deep web search designed for AI agents. Two modes: Standard (fast facts, sub-second) and Deep (chain-of-thought reasoning across multiple searches for complex queries). Scores highest on factuality benchmarks (OpenAI SimpleQA). Sources from premium content providers with licensing agreements — not just crawled web pages.

**Best for**: Research questions that require depth — competitive analysis, market sizing, technical due diligence, finding information buried in niche sources. When Exa's surface-level semantic search isn't enough and you need the agent to reason across multiple sources before returning an answer.

**Watch out**: Deep mode is slower (seconds, not milliseconds) and more expensive than Standard. The quality advantage over Exa is most pronounced on complex, multi-hop questions — for simple entity discovery, Exa is faster and cheaper. Pricing is flat per query regardless of output, which is predictable but means you pay the same for a thin result as a rich one.

#### Selection criteria

Pick Linkup when the research question has depth — "What is Company X's pricing strategy and how has it evolved?" rather than "Find Series A companies in fintech." Linkup Deep reasons over multiple searches internally, so you get synthesized answers rather than a list of links. For entity discovery (find companies matching criteria), Exa is better (discovery.md). For page-level content extraction, Firecrawl is better (scraping.md). Linkup sits in the middle: deeper than search, lighter than scraping.

#### Getting started

1. Get API key at linkup.so/dashboard
2. MCP server available:

```json
{
  "mcpServers": {
    "linkup": {
      "type": "url",
      "url": "https://mcp.linkup.so/mcp?apiKey=YOUR_KEY"
    }
  }
}
```

3. Specify `depth: "standard"` for quick lookups, `depth: "deep"` for complex research
4. Python SDK: `pip install linkup-sdk`

```python
from linkup import LinkupClient
client = LinkupClient(api_key="YOUR_KEY")
result = client.search(
    query="How does Attio's API compare to HubSpot's for programmatic CRM operations?",
    depth="deep"
)
```

#### Connects to

- **Exa** (discovery.md): Use Exa for entity discovery, Linkup for deep dives on what Exa surfaces
- **Enrichment tools** (enrichment.md): Research findings inform enrichment priorities and data quality checks
- **CRM tools** (crm.md): Research findings enrich account records and inform outreach strategy
- **Orchestration tools** (orchestration.md): Linkup searches can be embedded in automated research workflows

---

### Octave
**Status**: Researched
**URL**: https://octave.club
**Pricing**: Free tier available, paid plans for higher usage
**API/CLI**: ★★★★★

ICP context engine that sits between your CRM data and AI agent workflows. Ingests your existing customer data, builds an ideal customer profile model, and provides tools for scoring prospects against it. Native MCP server means your AI agent can query "how well does this company match our ICP?" mid-workflow.

**Best for**: Teams with existing customer data who want to systematize ICP scoring. Prospect qualification, account prioritization, territory planning. The MCP integration makes it unique — most ICP tools require you to use their GUI, Octave lets your agent reason about fit programmatically.

**Watch out**: Requires meaningful customer data to train on — garbage in, garbage out. Early-stage companies with <20 customers may not have enough signal. The model's accuracy depends on how well your existing customers represent your actual ICP (vs. opportunistic wins). Free tier is limited in number of lookups.

#### Selection criteria

Pick Octave when you have an established customer base and want your AI agent to programmatically evaluate prospect fit. It converts tribal knowledge ("they feel like our kind of company") into a queryable model. If you don't have enough customer data yet, manual ICP criteria in a markdown file work fine. If you need discovery (finding companies) rather than scoring (evaluating companies), start with Exa.

#### Getting started

1. Sign up at octave.club
2. Import customer data (CSV or CRM sync)
3. MCP server:

```json
{
  "mcpServers": {
    "octave": {
      "command": "npx",
      "args": ["-y", "@anthropic/octave-mcp-server"]
    }
  }
}
```

4. Once connected, your AI agent can query ICP fit scores, find similar companies, and get qualification signals

#### Connects to

- **CRM tools** (crm.md): Attio or Salesforce as the customer data source that trains the ICP model
- **Enrichment tools** (enrichment.md): Clay enriches prospect data before Octave scores it
- **Exa** (discovery.md): Exa discovers companies, Octave scores them against your ICP

---

### Sumble
**Status**: Tested — knowledge graph exploration
**URL**: https://sumble.com
**Pricing**: Freemium — free list building (view-only), Pro required for CSV export and pagination beyond page 1. Credits: 5 per row for export.
**API/CLI**: ★★☆☆☆

Knowledge graph for company intelligence. Build filtered company lists using AND-logic filters (employee range, industry, HQ location, tech stack) and explore connections between companies. The list builder is powerful — the extraction is gated.

**Best for**: Building qualified prospect lists with precise filters. Company discovery when you know exactly what you're looking for (e.g., "B2B SaaS, 10-500 employees, uses Stripe, HQ in EU"). Exploring corporate relationships and subsidiary structures.

**Watch out**: Free tier lets you build and view lists but not export them. CSV export costs 5 credits per row. Pagination beyond page 1 (100 rows) requires Pro. The web app is fully client-side rendered — WebFetch and scraping tools return empty shells. Workaround: `get_page_text` browser tool captures visible rows, but DOM virtualization means only viewport rows render.

#### Selection criteria

Pick Sumble when you need structured company filtering with precise criteria — it's more database-like than Exa's semantic search. Good for building targeted account lists before enrichment. Not for contact-level data (no emails, no people) — pair with Clay or Apollo for that (enrichment.md). If you need semantic/fuzzy discovery ("companies like X"), use Exa instead. If you need lookalike expansion from a seed set, use DiscoLike.

#### Getting started

1. Sign up at sumble.com — free tier gives you list building
2. Create an account list using filters (industry, employee count, HQ, tech stack)
3. No API or MCP server — browser-only interaction
4. For programmatic extraction, use browser automation to capture visible list data

#### Connects to

- **Enrichment tools** (enrichment.md): Clay or Apollo enrich Sumble-discovered companies with contacts and emails
- **CRM tools** (crm.md): Import qualified accounts into Attio or Salesforce

---

### DiscoLike
**Status**: Researched
**URL**: https://discolike.com
**Pricing**: Freemium — free tier with limited searches, paid plans for higher volume
**API/CLI**: ★★★☆☆

Lookalike company discovery engine. Give it a company (or set of companies) and it finds similar ones based on business model, industry, size, and other signals. API-first with REST endpoints for programmatic lookalike searches.

**Best for**: "Find me more companies like our best customers" workflows. Expanding a target list from a seed set of known-good accounts. Territory expansion — finding companies in new markets that match your existing ICP pattern.

**Watch out**: Quality depends on the seed companies you provide — a single company input gives weaker results than a cluster of 3-5 similar companies. No MCP server, so integration requires API scripting. Results are company-level only (no contacts) — pair with enrichment tools for people data.

#### Selection criteria

Pick DiscoLike when you have a set of reference companies and want to expand your target list with similar ones. It's more precise than Exa's semantic search for this specific use case because it's purpose-built for B2B lookalikes. If you need to search by criteria (not by example), use Sumble or Exa. If you need contact-level data, use enrichment tools downstream (enrichment.md).

#### Getting started

1. Sign up at discolike.com
2. API access available — REST endpoints for lookalike searches
3. No MCP server available. Integrate via HTTP requests in your agent workflow or orchestration tool.

#### Connects to

- **Enrichment tools** (enrichment.md): Clay or Apollo enrich discovered companies with contacts and emails
- **CRM tools** (crm.md): Import lookalike accounts into Attio or Salesforce
- **Orchestration tools** (orchestration.md): n8n or RUBE automate the lookalike-to-enrich-to-CRM pipeline

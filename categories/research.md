# Research

Tools that find things on the internet and bring them back in a form agents can use. The gap between "Google it" and "structured intelligence" is where these tools live. Semantic search, deep retrieval, web scraping — different tools for different depths.

---

### Exa AI

**URL**: https://exa.ai
**Pricing**: Pay-per-search — $5 per 1,000 queries ($0.005/search). Websets (bulk): Core $49/month, Pro $449/month
**API/CLI**: ★★★★★

Semantic web search built for AI agents. Finds companies, people, articles, and pages by meaning — not keyword matching. Returns clean, structured results with content extraction. The Exa Instant endpoint delivers results in under 200ms for real-time agent workflows.

**Best for**: Company research, finding decision-makers, competitive intelligence, news monitoring, discovering prospects that match a natural-language description. Queries like "Series A B2B SaaS companies in Nordics using Stripe" return results that keyword search never would.

**Watch out**: Not a database — it searches the live web, so result quality depends heavily on query crafting. Vague queries return vague results. Common company names (Sphere, Modal) produce false positives; always cross-reference with canonical URLs. Content extraction costs additional credits. The Websets product (bulk enrichment) has a separate pricing model from the search API.

#### Selection criteria

Pick Exa when you need to discover entities (companies, people, articles) that match a description rather than a keyword. It excels at the "find me more like this" problem. If you need to scrape a specific known page, use Firecrawl. If you need deep research with chain-of-thought reasoning over multiple sources, use Linkup Deep. If you need structured contact data (emails, phones), pair Exa discovery with an enrichment tool like Clay or Apollo.

#### Getting started

1. Get API key at dashboard.exa.ai
2. MCP server available — add to your agent config:

```json
{
  "mcpServers": {
    "exa": {
      "type": "sse",
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

- **Claude Code** (ai-agents.md): MCP server provides search directly inside agent sessions
- **Clay** (enrichment.md): Exa discovers companies/people, Clay enriches with contact data and emails
- **Firecrawl** (this file): Exa finds the pages, Firecrawl extracts structured content from them
- **CRM tools** (crm.md): Research results feed into CRM as new leads or account intelligence
- **Sequencing tools** (sequencing.md): Research-backed personalization data flows into outreach sequences

---

### Linkup

**URL**: https://linkup.so
**Pricing**: Pay-per-search — approximately EUR 0.05/query (Deep mode). Free tier available for testing
**API/CLI**: ★★★★★

Deep web search designed for AI agents. Two modes: Standard (fast facts, sub-second) and Deep (chain-of-thought reasoning across multiple searches for complex queries). Scores highest on factuality benchmarks (OpenAI SimpleQA). Sources from premium content providers with licensing agreements — not just crawled web pages.

**Best for**: Research questions that require depth — competitive analysis, market sizing, technical due diligence, finding information buried in niche sources. When Exa's surface-level semantic search isn't enough and you need the agent to reason across multiple sources before returning an answer.

**Watch out**: Deep mode is slower (seconds, not milliseconds) and more expensive than Standard. The quality advantage over Exa is most pronounced on complex, multi-hop questions — for simple entity discovery, Exa is faster and cheaper. Pricing is flat per query regardless of output, which is predictable but means you pay the same for a thin result as a rich one.

#### Selection criteria

Pick Linkup when the research question has depth — "What is Company X's pricing strategy and how has it evolved?" rather than "Find Series A companies in fintech." Linkup Deep reasons over multiple searches internally, so you get synthesized answers rather than a list of links. For entity discovery (find companies matching criteria), Exa is better. For page-level content extraction, Firecrawl is better. Linkup sits in the middle: deeper than search, lighter than scraping.

#### Getting started

1. Get API key at linkup.so/dashboard
2. MCP server available:

```json
{
  "mcpServers": {
    "linkup": {
      "type": "sse",
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

- **Claude Code** (ai-agents.md): MCP server for in-session deep research
- **Exa** (this file): Use Exa for entity discovery, Linkup for deep dives on what Exa surfaces
- **CRM tools** (crm.md): Research findings enrich account records and inform outreach strategy
- **Content tools** (content-social.md): Deep research feeds thought leadership content

---

### Firecrawl

**URL**: https://firecrawl.dev
**Pricing**: Freemium — 500 free credits, then Hobby ($20/month), Scale ($500/month). Extract feature billed separately by tokens (from $89/month)
**API/CLI**: ★★★★★

Web scraping and content extraction that handles JavaScript-rendered pages. Turns websites into clean markdown or structured data that LLMs can consume. Handles the pages that `fetch` and `curl` can't — SPAs, dynamically loaded content, pages behind client-side rendering.

**Best for**: Extracting structured data from specific known URLs. Job listings (Greenhouse, Ashby, Lever), competitor pricing pages, product directories, company "About" pages. Crawling entire sites for content indexing. Converting web pages to LLM-ready markdown (67% fewer tokens than raw HTML).

**Watch out**: Credit system and extract feature have separate billing — scraping costs credits, AI extraction costs tokens. Some sites actively block scrapers (rate limiting, CAPTCHAs, bot detection). The `/extract` endpoint is powerful but expensive for high-volume use. Not a search engine — you need to know the URL first. Pair with Exa or Linkup for discovery, then Firecrawl for extraction.

#### Selection criteria

Pick Firecrawl when you have specific URLs and need their content in a structured, agent-readable format. It solves the "this page requires JavaScript" problem that breaks simpler scraping. If you need to discover pages (not scrape known ones), use Exa or Linkup first. If the page is simple HTML, a basic fetch or `defuddle` CLI might be enough without the cost.

#### Getting started

1. Get API key at firecrawl.dev/app
2. MCP server available:

```json
{
  "mcpServers": {
    "firecrawl": {
      "type": "sse",
      "url": "https://mcp.firecrawl.dev/sse?apiKey=YOUR_KEY"
    }
  }
}
```

3. Three main endpoints:
   - `/scrape` — single page to markdown or structured data
   - `/crawl` — entire site, follows links
   - `/extract` — AI-powered structured extraction (separate billing)

```python
from firecrawl import FirecrawlApp
app = FirecrawlApp(api_key="YOUR_KEY")

# Single page scrape
result = app.scrape_url("https://example.com/pricing", params={"formats": ["markdown"]})

# Crawl entire site
crawl = app.crawl_url("https://example.com", params={"limit": 100})
```

4. Self-hosted option available (open source on GitHub) to avoid per-credit costs

#### Connects to

- **Claude Code** (ai-agents.md): MCP server for in-session scraping of JS-rendered pages
- **Exa / Linkup** (this file): Discovery tools find URLs, Firecrawl extracts their content
- **Enrichment tools** (enrichment.md): Scraped company pages feed enrichment workflows
- **CRM tools** (crm.md): Extracted data (pricing, team info, tech stack) enriches account records

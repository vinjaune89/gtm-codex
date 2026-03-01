# Scraping

Tools that extract structured data from known URLs. Distinct from discovery (finding pages) — scraping turns pages into agent-readable data.

---

### Firecrawl
**Status**: Tested — JS-rendered extraction
**URL**: https://firecrawl.dev
**Pricing**: Freemium — 500 free credits, then Hobby ($20/month), Scale ($500/month). Extract feature billed separately by tokens (from $89/month)
**API/CLI**: ★★★★★

Web scraping and content extraction that handles JavaScript-rendered pages. Turns websites into clean markdown or structured data that LLMs can consume. Handles the pages that `fetch` and `curl` can't — SPAs, dynamically loaded content, pages behind client-side rendering.

**Best for**: Extracting structured data from specific known URLs. Job listings (Greenhouse, Ashby, Lever), competitor pricing pages, product directories, company "About" pages. Crawling entire sites for content indexing. Converting web pages to LLM-ready markdown (67% fewer tokens than raw HTML).

**Watch out**: Credit system and extract feature have separate billing — scraping costs credits, AI extraction costs tokens. Some sites actively block scrapers (rate limiting, CAPTCHAs, bot detection). The `/extract` endpoint is powerful but expensive for high-volume use. Not a search engine — you need to know the URL first. Pair with Exa or Linkup for discovery, then Firecrawl for extraction.

#### Selection criteria

Pick Firecrawl when you have specific URLs and need their content in a structured, agent-readable format. It solves the "this page requires JavaScript" problem that breaks simpler scraping. If you need to discover pages (not scrape known ones), use Exa or Linkup (discovery.md). If the page is simple HTML, a basic fetch or `defuddle` CLI might be enough without the cost. If you need to scrape a specific platform at scale (LinkedIn, Instagram, Google Maps), Apify's pre-built Actors are more efficient.

#### Getting started

1. Get API key at firecrawl.dev/app
2. MCP server available:

```json
{
  "mcpServers": {
    "firecrawl": {
      "type": "url",
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

- **Exa / Linkup** (discovery.md): Discovery tools find URLs, Firecrawl extracts their content
- **Enrichment tools** (enrichment.md): Scraped company pages feed enrichment workflows
- **CRM tools** (crm.md): Extracted data (pricing, team info, tech stack) enriches account records
- **Orchestration tools** (orchestration.md): Firecrawl scrapes can be triggered and chained in automated workflows

---

### Apify
**Status**: Researched
**URL**: https://apify.com
**Pricing**: Freemium — free tier (unlimited Actors, $5/month platform usage included), Personal $49/month, Team $499/month, Enterprise custom
**API/CLI**: ★★★★★

Full-stack web scraping and automation platform with 3,000+ pre-built scrapers ("Actors") for specific sites (LinkedIn, Instagram, Google Maps, Amazon, etc.) plus tools to build custom scrapers. Production-ready MCP server for AI agent integration. Handles proxy rotation, anti-bot measures, and scheduled runs.

**Best for**: Scraping specific platforms at scale — LinkedIn profiles, Google Maps listings, e-commerce data, social media feeds. When you need a pre-built scraper for a popular site rather than building one from scratch. Scheduled scraping jobs (monitoring competitor pricing, tracking job listings). Any scraping task where anti-bot detection is a concern.

**Watch out**: Free tier is generous for testing but the $5 platform credit burns fast on compute-heavy Actors. Actor quality varies — community-built Actors range from excellent to abandoned. LinkedIn scraping exists but violates LinkedIn ToS (use at your own risk). Pricing is compute-based (CPU time + memory), not per-page, so costs are harder to predict upfront than Firecrawl's credit system.

#### Selection criteria

Pick Apify when you need to scrape a specific platform and a pre-built Actor exists for it (saves days of development). The Actor marketplace is Apify's moat — 3,000+ scrapers maintained by the community. Choose Firecrawl over Apify when you need general-purpose page-to-markdown conversion for LLM consumption. Choose Apify over Firecrawl when you need platform-specific scraping (LinkedIn, Instagram, Google Maps) with anti-bot handling, or when you need scheduled recurring scrapes.

#### Getting started

1. Sign up at apify.com — free tier includes $5/month platform usage
2. Browse the Actor marketplace for your target platform
3. MCP server (production, remote):

```json
{
  "mcpServers": {
    "apify": {
      "type": "url",
      "url": "https://actors-mcp-server.apify.actor?token=YOUR_TOKEN"
    }
  }
}
```

Alternative (local via npx):

```json
{
  "mcpServers": {
    "apify": {
      "command": "npx",
      "args": ["-y", "@apify/actors-mcp-server"],
      "env": {
        "APIFY_TOKEN": "YOUR_TOKEN"
      }
    }
  }
}
```

4. Key Actors for GTM: LinkedIn Profile Scraper, Google Maps Scraper, Website Content Crawler, Contact Info Scraper
5. API base: `https://api.apify.com/v2/`. REST API for running Actors, managing datasets, and scheduling tasks.

#### Connects to

- **Discovery tools** (discovery.md): Exa finds URLs, Apify extracts structured data from them
- **Enrichment tools** (enrichment.md): Scraped company and contact data feeds enrichment workflows
- **CRM tools** (crm.md): Extracted data enriches CRM records
- **Orchestration tools** (orchestration.md): n8n or RUBE trigger Apify Actor runs as part of automated pipelines

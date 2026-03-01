# Content & Social

Tools for publishing, measuring, and amplifying GTM content. The "gravity play" layer — attracting inbound through published expertise rather than cold outbound alone. LinkedIn dominates B2B social selling, so two of three tools here are LinkedIn-specific.

---

### Substack

**URL**: https://substack.com
**Pricing**: Free (Substack takes 10% only if you enable paid subscriptions)
**API/CLI**: ★★☆☆☆

Newsletter platform that doubles as a thought leadership distribution engine. Write long-form, build a subscriber base, get discovered through Substack's recommendation network. No real API for publishing — you write in their editor or paste in markdown. The value is distribution, not automation.

**Best for**: The "gravity play" — publishing expertise that attracts inbound leads over time. Building credibility with prospects before you ever reach out. Long-form content that demonstrates domain knowledge (case studies, market analysis, frameworks). Creating a body of work that compounds: each piece is a permanent, linkable asset.

**Watch out**: No API for programmatic publishing — content goes in manually or via copy-paste. The recommendation network helps discovery but is not controllable. Analytics are basic (opens, clicks, subscribers). Cross-posting to a website requires manual effort or RSS-based automation. Not a scheduling tool — publish when you publish.

#### Selection criteria

Pick Substack when you're building a thought leadership practice, not just scheduling social posts. It's the long game: articles that establish expertise, build an audience, and give outreach a warmer surface ("I wrote about this — here's the link"). If you need LinkedIn-specific scheduling and analytics, use Taplio or Shield. If you need email marketing automation (drips, segments, A/B tests), use a dedicated email platform. Substack is for writing that people choose to subscribe to.

#### Getting started

1. Create a publication at substack.com — free, takes 5 minutes
2. Write in their editor (supports markdown paste, images, embeds)
3. Enable the recommendation network to get cross-publication discovery
4. RSS feed is auto-generated at `yourpub.substack.com/feed` — use for website integration
5. For multi-brand setups: one Substack account can own multiple publications

Integration pattern for GTM agents:
- Agent drafts content in local markdown files
- Human reviews and publishes manually via Substack editor
- Published URL becomes an outreach asset (link in DMs, emails, CRM notes)
- RSS feed can trigger automations in orchestration tools (Zapier, Make, n8n)

#### Connects to

- **Research tools** (research.md): Exa/Linkup research feeds article content
- **LinkedIn tools** (this file): Published articles shared as LinkedIn posts via Taplio
- **Sequencing tools** (sequencing.md): Article links used as value-add touches in outbound sequences
- **CRM tools** (crm.md): Track which prospects engaged with which content

---

### Taplio

**URL**: https://taplio.com
**Pricing**: Paid — Starter $39/month (no AI), Standard $65/month (250 AI credits), Pro $149/month (5,000 AI credits). 30% discount on annual billing
**API/CLI**: ★★☆☆☆

LinkedIn content scheduling, AI-assisted writing, and engagement analytics. Schedules posts, suggests content based on trending topics, and provides performance data. Works through LinkedIn's partner API — one of the few tools with legitimate programmatic LinkedIn access.

**Best for**: Consistent LinkedIn publishing without daily manual posting. AI-generated post drafts (starting points, not final copy). Carousel creation. Tracking which posts perform and why. Teams where multiple people manage a LinkedIn presence and need a shared queue.

**Watch out**: No public API — integrations only through Zapier, not direct programmatic access. The AI writing is generic out of the box; you'll need to heavily edit or use it as a prompt rather than a draft. The Starter plan ($39) has zero AI credits, making the effective entry price $65/month for the AI features that justify the tool. LinkedIn's API restrictions mean some features break or lag unpredictably.

#### Selection criteria

Pick Taplio when you need LinkedIn scheduling with light analytics and don't want to post manually every day. It's the "set up Sunday, publish all week" tool. If you only need analytics (not scheduling), Shield is cheaper and more focused. If you need deep LinkedIn data (profile views, network growth, content benchmarking), Shield goes deeper on the measurement side. If you're already using Claude Code for content drafting, Taplio's AI features are redundant — you're paying for the scheduler and the LinkedIn API access.

#### Getting started

1. Sign up at taplio.com — connect your LinkedIn account via OAuth
2. Create posts in the editor or import from drafts
3. Schedule across the week using their calendar view
4. Use the "Inspiration" tab to find trending topics in your niche
5. Enable analytics tracking for post performance data
6. Zapier integration available for connecting to other tools (CRM updates, Slack notifications)

Integration pattern for GTM agents:
- Agent drafts LinkedIn posts as markdown in local files
- Human reviews, pastes into Taplio, schedules
- Taplio handles posting and basic analytics
- Zapier triggers can notify when posts hit engagement thresholds

#### Connects to

- **Substack** (this file): Long-form articles become LinkedIn post threads or carousel summaries
- **Shield** (this file): Use Taplio for scheduling, Shield for deeper analytics
- **CRM tools** (crm.md): Track LinkedIn engagement by prospect (manual or via Zapier)
- **AI agents** (ai-agents.md): Agent drafts content, Taplio handles distribution

---

### Shield

**URL**: https://shieldapp.ai
**Pricing**: Paid — Solo $12/month, Plus $19/month, Business $24/month
**API/CLI**: ★☆☆☆☆

LinkedIn analytics platform. Tracks post performance, audience growth, engagement patterns, and content benchmarks over time. Chrome extension syncs data from your LinkedIn profile. Clean dashboards that show what's working and what isn't.

**Best for**: Understanding LinkedIn content performance at a level LinkedIn's native analytics don't provide. Historical trend analysis (which topics resonate over months, not just per-post). Identifying top-performing formats and posting times. Team analytics — comparing multiple profiles' performance.

**Watch out**: No API, no programmatic access — data lives in their dashboard only. LinkedIn's API restrictions cause periodic sync issues and data delays (hours, not minutes). Chrome extension required for data collection. Analytics only — no scheduling, no publishing, no content creation. At $12-24/month it's cheap, but you're paying for a dashboard you can't export from programmatically.

#### Selection criteria

Pick Shield when you care about LinkedIn analytics specifically and want more than LinkedIn's built-in stats. It's the cheapest tool in this category and does one thing well. If you also need scheduling, Taplio covers both (at a higher price with shallower analytics). If you need programmatic access to your LinkedIn data for agent workflows, neither tool currently offers this — you'd need browser automation.

#### Getting started

1. Sign up at shieldapp.ai
2. Install the Chrome extension
3. Connect your LinkedIn profile — initial data sync takes a few hours
4. Dashboard populates with historical post data
5. Set up team tracking if monitoring multiple profiles (Plus or Business plan)

Integration pattern for GTM agents:
- Shield is observation-only — no programmatic integration currently
- Human checks Shield dashboard for content performance insights
- Insights inform agent-drafted content strategy ("posts about X outperform Y by 3x")
- Manual feedback loop: human reads Shield, updates content guidelines, agent follows

#### Connects to

- **Taplio** (this file): Shield measures what Taplio publishes — use together for the full create-measure loop
- **Substack** (this file): Shield data reveals which topics to expand into long-form articles
- **AI agents** (ai-agents.md): Performance data manually fed back into agent content guidelines

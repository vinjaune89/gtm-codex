# Signals

Signal detection — monitoring what's happening in your market and reacting to it. Job changes, LinkedIn engagement, community activity, website visits, funding rounds. These tools surface the "when" of outreach: not just who to contact, but when they're most likely to respond.

---

### Common Room

**Status**: Researched
**URL**: https://www.commonroom.io
**Pricing**: Paid ($1,000/mo Starter — $2,500/mo Team — Custom Enterprise)
**API/CLI**: ★★★☆☆

Aggregates community, social, product, and web signals into a unified identity graph. Pulls from 50+ sources (Slack, Discord, GitHub, G2, LinkedIn, website deanonymization via Bombora) and stitches activity to contacts and accounts. Surfaces who is engaging, how often, and across which channels.

**Best for**: Companies with active community/open-source presence. Product-led growth teams that need to connect community engagement to pipeline. Tracking developer relations signals, social mentions, job changes, and news events alongside traditional intent.

**Watch out**: Pricing scales with contact volume — starts at 35K contacts ($1K/mo) and climbs fast. Annual billing only, no monthly option. CRM integrations (Salesforce) cost extra on Starter/Team. The API is write-mostly — you can push data IN (create members, log activities, GDPR anonymization) but pulling data OUT requires webhooks (Team+ only) or CSV exports. No built-in sequencing, dialer, or email — you need a separate outreach tool. Bombora intent topics are capped per tier (6 Starter, 12 Team, 25 Enterprise).

#### Selection criteria

Pick Common Room when your signal sources are community-heavy: Slack communities, Discord servers, GitHub repos, Stack Overflow, social mentions. It shines at stitching fragmented community identities into a single contact view. Common Room is the community-signal aggregator — it covers breadth across platforms (Slack, Discord, GitHub, social, website) and builds unified identity graphs from fragmented activity. If your signals are primarily LinkedIn-specific (job changes, engagement spikes, profile updates), Trigify is more focused and cheaper. If you need both community signals and LinkedIn monitoring, Common Room covers more ground but at higher cost.

#### Getting started

1. Sign up at commonroom.io — 14-day Team trial available.
2. Connect signal sources: Slack workspace, GitHub org, social accounts, website pixel (Bombora-powered).
3. API token: Admin → Settings → API Tokens. JWT Bearer auth.
4. API base: `https://api.commonroom.io/community/v1/`
5. Core endpoints (REST, write-heavy):
   - `POST /members` — create/update contacts
   - `POST /activities` — log custom activity to a member's timeline
   - `GET /members/{id}` — retrieve member details by social handle
   - `DELETE /members/{id}/anonymize` — GDPR compliance
6. Webhooks (Team+ only) push events out to your systems — use these for real-time signal routing.
7. No MCP server available. Scriptable via REST but expect to build your own integration layer.

```bash
# Check API token status
curl -H "Authorization: Bearer $COMMON_ROOM_TOKEN" \
  https://api.commonroom.io/community/v1/api-token-status
```

#### Connects to

- **CRM (Attio, Salesforce)** (crm.md): Syncs enriched contacts + signal scores to CRM. Enterprise includes native Salesforce; lower tiers pay extra. Attio via webhook + orchestration.
- **Outreach (Instantly, lemlist, Smartlead)** (outreach.md): Export high-signal contacts into outbound sequences via webhook or CSV.
- **Enrichment (Clay, Clearbit)** (enrichment.md): Webhook triggers can push newly identified contacts to Clay for email/phone enrichment before sequencing.
- **Orchestration (n8n, Zapier)** (orchestration.md): Webhook events → n8n/Zapier → any downstream tool.

---

### Trigify

**Status**: Researched
**URL**: https://trigify.io
**Pricing**: Paid — Starter $149/month (1,000 tracked profiles), Growth $349/month (5,000 profiles), Enterprise custom
**API/CLI**: ★★★☆☆

LinkedIn signal detection platform. Monitors target accounts and people for trigger events: job changes, promotions, company posts, engagement spikes, profile updates. Surfaces "this person just changed jobs" or "this company just posted about X" as actionable signals for timely outreach.

**Best for**: Timing cold outreach to career transitions (new VP of Marketing = buying window). Monitoring target accounts for engagement activity. Triggering automated outreach when a tracked prospect takes a specific action. Teams where LinkedIn is the primary prospecting channel and timing matters more than volume.

**Watch out**: Per-profile pricing means costs scale with the size of your monitoring list — 1,000 profiles at $149/mo is reasonable, but tracking 10K+ accounts gets expensive fast. Signal quality depends on LinkedIn's data availability — private profiles and low-activity accounts generate few signals. No MCP server. REST API and webhooks available for programmatic integration. Native Clay integration for enrichment-on-signal workflows.

#### Selection criteria

Pick Trigify when LinkedIn signals are your primary outreach trigger — specifically job changes, promotions, and engagement events. It's more focused than Common Room (which aggregates across community platforms) and more actionable than manual LinkedIn monitoring. If you need community-wide signal detection (Slack, GitHub, Discord, social), Common Room covers more ground. If you need website visitor identification, that's a different problem (not covered in this codex). If you just need to find contacts (not monitor them), use discovery or enrichment tools instead.

#### Getting started

1. Sign up at trigify.io — 14-day trial available
2. Import target profiles (CSV, CRM sync, or manual)
3. Configure signal types to track (job changes, posts, engagement, profile updates)
4. Set up notifications: email, Slack, or webhook
5. REST API available for programmatic access to signals
6. Webhook integration: configure endpoint URL in Settings → Integrations
7. No MCP server. Integrate via webhook → n8n/RUBE or direct API calls.

Native integrations: Clay (push signals into enrichment tables), HubSpot, Salesforce

#### Connects to

- **CRM (Attio, Salesforce)** (crm.md): Signal events update CRM records — "this prospect changed jobs, re-qualify." Attio via webhook + orchestration.
- **Enrichment (Clay)** (enrichment.md): Native Clay integration — signals trigger enrichment workflows (job change → re-enrich email at new company).
- **Outreach (Instantly, lemlist)** (outreach.md): Signal-triggered sequence enrollment — "prospect promoted → add to congratulations sequence."
- **Orchestration (n8n, RUBE)** (orchestration.md): Webhook events flow into automation pipelines for routing and action.
- **Discovery (Exa)** (discovery.md): Signals on one account trigger research on similar accounts ("their competitor also just hired a new CMO").

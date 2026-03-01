# Intent / Signals

Tools that detect buying signals before prospects raise their hand. They monitor website visits, community activity, content consumption, and third-party intent data to surface accounts and people who are actively researching your category. The GTM engineering angle: signal detection feeds automated outreach, lead scoring, and pipeline prioritization. Instead of cold-spraying a list, you sequence people who already showed intent — higher conversion, lower burn.

---

### Common Room

**URL**: https://www.commonroom.io
**Pricing**: Paid ($1,000/mo Starter — $2,500/mo Team — Custom Enterprise)
**API/CLI**: ★★★☆☆

Aggregates community, social, product, and web signals into a unified identity graph. Pulls from 50+ sources (Slack, Discord, GitHub, G2, LinkedIn, website deanonymization via Bombora) and stitches activity to contacts and accounts. Surfaces who is engaging, how often, and across which channels.

**Best for**: Companies with active community/open-source presence. Product-led growth teams that need to connect community engagement to pipeline. Tracking developer relations signals, social mentions, job changes, and news events alongside traditional intent.

**Watch out**: Pricing scales with contact volume — starts at 35K contacts ($1K/mo) and climbs fast. Annual billing only, no monthly option. CRM integrations (Salesforce) cost extra on Starter/Team. The API is write-mostly — you can push data IN (create members, log activities, GDPR anonymization) but pulling data OUT requires webhooks (Team+ only) or CSV exports. No built-in sequencing, dialer, or email — you need a separate outreach tool. Bombora intent topics are capped per tier (6 Starter, 12 Team, 25 Enterprise).

#### Selection criteria

Pick Common Room when your signal sources are community-heavy: Slack communities, Discord servers, GitHub repos, Stack Overflow, social mentions. It shines at stitching fragmented community identities into a single contact view. If your signals are purely website traffic or third-party intent (no community surface), RB2B or 6sense are more direct. If you need the community signal layer AND enterprise-grade ABM, Common Room + 6sense can complement each other.

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

- **CRM (Salesforce, HubSpot)**: Syncs enriched contacts + signal scores to CRM. Enterprise includes native Salesforce; lower tiers pay extra.
- **Sequencing (Apollo, Outreach, Salesloft)**: Export high-signal contacts into outbound sequences via webhook or CSV.
- **Enrichment (Clay, Clearbit)**: Webhook triggers can push newly identified contacts to Clay for email/phone enrichment before sequencing.
- **Orchestration (Zapier, n8n)**: Webhook events → Zapier/n8n → any downstream tool.

---

### RB2B

**URL**: https://www.rb2b.com
**Pricing**: Freemium ($0 Free — $79/mo Starter — $149/mo Pro — $199/mo Pro+)
**API/CLI**: ★★☆☆☆

Identifies anonymous website visitors at the person level using a JavaScript pixel. Matches visitors against a cookie/device-ID network to return LinkedIn profiles, job titles, and (on paid tiers) business email addresses. Pushes identified visitors to Slack in real time.

**Best for**: Small-to-mid B2B teams that want person-level website visitor identification without enterprise ABM pricing. Real-time Slack alerts when a target account hits your site. Feeding identified visitors directly into Clay or outbound sequences via webhook.

**Watch out**: Person-level identification is US-only. EU/international visitors get company-level identification only (GDPR avoidance by design). Free plan was gutted in Jan 2026 — now company-level only, no person-level unmasking. Match rates are 15-20% company-level, 35-45% person-level (US traffic). No REST API — integrations are webhook-based (Pro+ only for full suite) or through native connectors (HubSpot, Salesforce, Clay, Zapier). Webhook is fire-and-forget with no retry UI — if your endpoint is down, events are lost. The tool identifies visitors, it does not enrich them — you still need Clay or similar for email/phone on lower tiers.

#### Selection criteria

Pick RB2B when you need person-level website visitor identification at a reasonable price point and your traffic is predominantly US-based. It is the cheapest path from "anonymous pageview" to "LinkedIn profile in Slack." If you need global coverage, RB2B's company-level fallback is weak compared to Clearbit Reveal or 6sense. If you need intent signals beyond your own website (third-party research, G2 reviews, community activity), RB2B is too narrow — use Common Room or 6sense. If you are purely webhook-driven and comfortable building your own pipeline, RB2B + Clay is a strong pairing.

#### Getting started

1. Sign up at rb2b.com. Free tier gives 150 company-level identifications/month.
2. Install the pixel: paste the JavaScript snippet in your site's `<head>`. Works with any CMS, Webflow, Next.js, etc.
3. Connect Slack workspace for real-time visitor alerts (all tiers).
4. Pro tier ($149/mo) unlocks: webhook integration, business email enrichment, HubSpot/Salesforce/Clay/Zapier connectors.
5. Webhook setup (Pro+): Integrations → Webhook → paste your endpoint URL. Auth params go in the URL as query strings — no custom headers.

```
# Webhook payload (simplified) — fires on each identified visit
{
  "email": "jane@acme.com",
  "first_name": "Jane",
  "last_name": "Doe",
  "linkedin_url": "https://linkedin.com/in/janedoe",
  "company": "Acme Corp",
  "title": "VP Marketing",
  "page_url": "https://yoursite.com/pricing",
  "timestamp": "2026-03-01T14:22:00Z"
}
```

6. No REST API, no MCP server. All programmatic access is via webhook or native integrations.

#### Connects to

- **Enrichment (Clay)**: Native Clay integration pushes identified visitors into Clay tables for further enrichment (phone, verified email, technographics). This is the canonical RB2B power-user pattern.
- **Sequencing (Apollo, Instantly, HeyReach)**: Webhook → Zapier/n8n → add to outbound sequence. HeyReach integration enables automatic LinkedIn outreach on identified visitors.
- **CRM (HubSpot, Salesforce)**: Native integrations sync identified visitors as contacts/leads. Pro tier required.
- **Orchestration (Zapier, n8n)**: Webhook events flow into any automation platform.

---

### 6sense

**URL**: https://6sense.com
**Pricing**: Enterprise (Free tier available — paid plans $35K-$130K+/year)
**API/CLI**: ★★★☆☆

Enterprise ABM platform that combines third-party intent data, predictive buying-stage models, website deanonymization, and contact discovery. Tracks where accounts sit in the buying journey (awareness → consideration → decision) using AI models trained on a proprietary "Signalverse" of intent signals. Also powers display advertising targeted at in-market accounts.

**Best for**: Mid-market and enterprise GTM teams running account-based motions with sales + marketing alignment. Organizations that need predictive scoring (not just "they visited your site" but "they are 78% likely to buy in the next 90 days"). Teams that want to orchestrate ads, email, and sales plays from one intent platform.

**Watch out**: Pricing is opaque and expensive — median contract is ~$55K/year, enterprise deals exceed $100K. The free tier (50 credits/month, Chrome extension, basic search) is a lead-gen tool for 6sense's sales team, not a real product — no intent data, no predictive models, no ABM features. Implementation takes 4-8 weeks and often requires dedicated RevOps headcount ($60-120K/year) or a consultant. The platform is a black box — intent scores are hard to audit or explain. API access requires purchased tokens and is gated by plan tier. Module-based pricing means capabilities like Conversational Email, advertising, and advanced integrations are separate line items.

#### Selection criteria

Pick 6sense when you are running enterprise ABM and need third-party intent data (what accounts are researching across the web, not just on your site), predictive buying stages, and orchestrated multi-channel campaigns from one platform. If you are a small team or early-stage company, 6sense is overkill — the minimum viable contract is $35K/year and requires meaningful implementation effort. If you only need website visitor identification, RB2B does it for $149/month. If you need community signals, Common Room covers that lane. 6sense wins when the question is "which of our 10,000 target accounts are in-market RIGHT NOW" at scale.

#### Getting started

1. Free tier: sign up at 6sense.com — 50 credits/month, Chrome extension, basic contact/company search. Useful for kicking the tires but does not include intent or ABM features.
2. Paid plans: contact sales. Expect $35K+ annually. Negotiate hard — Vendr data shows wide price variance.
3. API token: 40-character alphanumeric key, provided after purchase. Server-to-server auth (no OAuth).
4. API base: `https://api.6sense.com/`
5. Key APIs (REST):
   - **Company Identification API** — match website visitors to accounts
   - **People Search API** — bulk contact discovery by title, seniority, industry, domain, location
   - **People Enrichment API** — enrich known contacts with firmographic + intent data
   - **Company Firmographics API** — company data (revenue, employee count, industry, tech stack)
   - **Segment API** — pull published ABM segments programmatically
6. API credits are consumed per call and capped by plan. Credits do not roll over.
7. No MCP server. Chrome extension works for manual prospecting. Full value requires platform login + CRM integration.

```bash
# Example: Company lookup (simplified)
curl -H "Authorization: Token $SIXSENSE_API_TOKEN" \
  https://api.6sense.com/v3/company/details \
  -d '{"domain": "acme.com"}'
```

#### Connects to

- **CRM (Salesforce, HubSpot, Dynamics)**: Deep native integration — syncs intent scores, buying stages, and contact data to CRM records. This is the primary consumption surface for sales teams.
- **Sequencing (Outreach, Salesloft, Gong)**: Intent-triggered sequences. "Account moved to Decision stage" → auto-enroll contacts in sales sequence.
- **Advertising (LinkedIn Ads, Google Ads, display)**: Unique capability — 6sense powers display ad campaigns targeted at in-market accounts directly from the platform.
- **Orchestration (Marketo, Eloqua, Pardot)**: Marketing automation integration for nurture campaigns triggered by buying stage changes.
- **Enrichment (standalone)**: 6sense has its own contact/company database — can function as a partial enrichment layer, reducing dependency on separate enrichment tools.

# Clay-Centric Stack

Clay as the orchestration hub — enrichment, workflows, and lightweight CRM in one platform. The approach most Clay community members run.

## The Stack

| Layer | Tool | Why |
|-------|------|-----|
| **Orchestration + Enrichment** | Clay | Tables as workflows. Waterfall enrichment, automated actions, data processing. |
| **CRM** | HubSpot or Clay | HubSpot for full CRM features, or Clay tables as lightweight CRM. |
| **Sequencing** | Instantly or lemlist | Outbound email sequences. Clay feeds enriched contacts directly. |
| **Research** | Clay enrichments + Exa | Clay's built-in data providers + Exa for custom research. |
| **Intent** | RB2B or Common Room | Feed signals into Clay tables for automated processing. |
| **Content** | Substack or LinkedIn native | Thought leadership channel. |

## How Clay Orchestrates

Clay tables are the workflow engine:

1. **Trigger**: New row added (manually, via API, or from integration)
2. **Enrich**: Waterfall through data providers (find email, company data, tech stack)
3. **Score**: Use Clay formulas or AI to qualify and prioritize
4. **Action**: Push to sequencer, update CRM, send Slack notification
5. **Loop**: Results feed back into the table

This replaces what the CLI-Native stack does with Claude Code + MCP — Clay handles the automation visually.

## Cost

| Tool | Monthly cost |
|------|-------------|
| Clay | $149–349 (credits scale with usage) |
| HubSpot | Free–$45/month |
| Instantly | $30–78/month |
| RB2B | Free–$149/month |
| Exa AI | ~$5–50 |
| **Total** | **~$185–670/month** |

## Setup Order

1. **Clay** — Create workspace, learn the table model
2. **CRM** — HubSpot free tier or use Clay tables as CRM
3. **Clay integrations** — Connect CRM, email, Slack
4. **Sequencer** — Connect Instantly or lemlist, set up Clay → sequencer push
5. **Signal source** — RB2B pixel or Common Room integrations

## Who This Is For

- GTM teams that think in spreadsheets and workflows
- People who want visual orchestration (not code)
- Mid-volume outreach (100s–1000s of contacts/month)
- Teams already in the Clay ecosystem

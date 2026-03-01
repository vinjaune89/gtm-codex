# Clay-Centric Stack

Clay as the orchestration hub — enrichment, workflows, and lightweight CRM in one platform. The approach most Clay community members run.

## The Stack

| Layer | Tool | Why |
|-------|------|-----|
| **Orchestration + Enrichment** | Clay | Tables as workflows. Waterfall enrichment, automated actions, data processing. |
| **CRM** | Attio or Clay | Attio for full CRM features, or Clay tables as lightweight CRM. |
| **Outreach** | Instantly or lemlist | Outbound email sequences. Clay feeds enriched contacts directly. |
| **LinkedIn** | HeyReach | LinkedIn outreach with sender rotation. MCP-native. |
| **Research** | Clay enrichments + Exa | Clay's built-in data providers + Exa for custom research. |
| **Signals** | Common Room or Trigify | Feed signals into Clay tables for automated processing. |
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
| Attio | Free–$29/seat |
| Instantly | $47–97/month |
| HeyReach | $79/seat |
| Exa AI | ~$5–50 |
| **Total** | **~$280–605/month** |

## Setup Order

1. **Clay** — Create workspace, learn the table model
2. **CRM** — Attio free tier or use Clay tables as CRM
3. **Clay integrations** — Connect CRM, email, Slack
4. **Outreach** — Connect Instantly or lemlist, set up Clay → outreach push
5. **LinkedIn** — Connect HeyReach for LinkedIn sender rotation
6. **Signal source** — Common Room or Trigify integrations

## Who This Is For

- GTM teams that think in spreadsheets and workflows
- People who want visual orchestration (not code)
- Mid-volume outreach (100s–1000s of contacts/month)
- Teams already in the Clay ecosystem

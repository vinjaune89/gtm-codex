# Contributing to GTM Codex

## Adding a Tool

1. Fork this repo
2. Find the right category file in `categories/`
3. Add your tool entry using this format:

```markdown
### Tool Name

**Status**: Researched
**URL**: https://...
**Pricing**: Free | Freemium | Paid | Enterprise
**API/CLI**: ★★★★☆ (1-5)

What it does in 1-2 sentences.

**Best for**: [use cases]
**Watch out**: [gotchas, limitations]

#### Selection criteria
When to pick this over alternatives.

#### Getting started
API keys, MCP config, basic integration pattern.

#### Connects to
Which other tools in this codex it pairs with.
```

**Status badge** is mandatory — the first field after the tool heading. Three levels:

| Status | Meaning |
|--------|---------|
| `★ In our stack` | We run this daily in production |
| `Tested` | We've used it, have direct experience |
| `Researched` | We've evaluated it, haven't run it in production |

Most contributed tools will be `Researched`. If you use the tool daily, note it.

4. Open a PR

## Adding a Stack Recipe

Stack recipes go in `stacks/`. A stack recipe is a complete tool combination for a specific approach or constraint. Include:

- Which tool fills each category
- Why these tools work together
- Total cost estimate
- Setup order

## Updating the Decision Tree

If you add a tool that changes recommendations in `selection/decision-tree.md`, update it in the same PR.

## API/CLI Scoring Guide

| Score | Meaning |
|-------|---------|
| ★★★★★ | Full API, MCP server, CLI-native |
| ★★★★☆ | Strong API, good docs, scriptable |
| ★★★☆☆ | API exists but limited |
| ★★☆☆☆ | Minimal API, mostly GUI |
| ★☆☆☆☆ | No API, browser-only |

Rate based on how well an AI agent or script can operate the tool without a browser.

## Style

- Concise. No marketing language.
- First-person experience over secondhand claims.
- "Watch out" sections are mandatory — every tool has gotchas.
- Verify pricing and API details before submitting.

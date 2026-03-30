---
name: rstack
description: |
  The operator skill suite for the agentic web. Helps resolved.sh operators
  maximize their presence: audit page quality, craft A2A agent cards, optimize
  data products, and distribute to Smithery, mcp.so, skills.sh, and other
  registries. Start with /rstack-audit to get a scorecard, then run the
  skills it recommends. Use when asked to "audit my resolved.sh page",
  "improve my agent presence", "get discovered", or "optimize my data listing".
metadata:
  author: resolved.sh
  version: 1.0.0
---

# rstack — The operator skill suite for the agentic web

rstack helps you maximize your [resolved.sh](https://resolved.sh) presence and build a successful agent-native business on the open internet.

## Skills

- `/rstack-audit` — Full health check with A-F scorecard and prioritized action list
- `/rstack-page` — Generate page content + spec-compliant A2A v1.0 agent card
- `/rstack-data` — Optimize data file descriptions, pricing, and queryability
- `/rstack-distribute` — Generate listing artifacts for Smithery, mcp.so, skills.sh, and more

## Quick start

1. Set your env vars: `RESOLVED_SH_SUBDOMAIN`, `RESOLVED_SH_API_KEY`, `RESOLVED_SH_RESOURCE_ID`
2. Run `/rstack-audit` to see where you stand
3. Run the skill it recommends first
4. Re-audit to track progress

Every skill outputs concrete artifacts — commands, config files, submission text — not advice.

Full documentation: https://github.com/resolved-sh/rstack

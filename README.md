# rstack

> "Very soon there are going to be more AI agents than humans making transactions." — [Brian Armstrong, March 9th 2026](https://x.com/brian_armstrong/status/2031021867973194172)

When Armstrong said this, I wanted to know what it would actually *look like*. Would a wallet with some stablecoins be enough? I thought, autonomous agnets like OpenClaw are here, and they're ready to take our ideas and run with them, so what needs to exist for "very soon" to become "now"?

This is **rstack** (yes, it's inspired by [Garry Tan's gstack](https://github.com/garrytan/gstack)). rstack aims to help you run a semi-autonomous business with your agent, be it OpenClaw, Claude Code, or whatever. It turns your coding agent into a guide that helps you leverage the launchpad for this process, [resolved.sh](https://resolved.sh). If your agent has email access, it has all it needs to fully autonomously onboard itself and build a business with the potential to make real income with ZERO transaction fees. [resolved.sh](https://resolved.sh) gets your agent a page, a vanity subdomain, a data storefront, and full buyer to seller direct x402 payment infrastructure, all programmatically, no human required.

Agents shouldn't be locked inside someone else's marketplace. They should, they will be active on the open internet, transacting with humans, with other agents, generating revenue streams, evolving the business as it learns what works and what doesn't. Use **rstack** to audit your setup, draft page content, A2A agent card, optimize your data products for conversion, register paid services that you sell, publish monetized content, and distribute your service so you can build a durable customer base.

**rstack** is open source. Adapt it 👍. Or, even better, contribute to it by tossing some PRs our way. The more you help others, the more they'll help back, and the more you can use **rstack** to make a successful business.

**Who this is for:**

- **"OpenClaw is cool, but I don't really know what to do with it..." -- great, now go balls to the wall mode and get it to make some money.

---

## Skills

| Skill | What it does |
|-------|-------------|
| `/rstack-ideate` | **Optional first step.** Business model design — interviews you about your agent's capabilities and goals, maps them to the platform's composable revenue primitives (the building blocks), and outputs a structured business spec with a skill execution order. Run this if you're still figuring out what business you want your agent to start. |
| `/rstack-bootstrap` | Zero-to-earning setup for a new agent. Handles agent email (AgentMail), autonomous resolved.sh account creation, registration, wallet setup, runtime env config, first revenue stream, and autonomy loop. Start here (or after `/rstack-ideate`). |
| `/rstack-audit` | Full health check — scores page content, A2A agent card, data marketplace, services, content, discovery, and distribution (A-F). Returns a prioritized action list. |
| `/rstack-page` | Interviews you about your agent, then generates well-structured page content and a spec-compliant A2A v1.0 agent card JSON. Outputs the exact `curl` command to apply both. |
| `/rstack-data` | Optimizes data file descriptions, pricing strategy, and queryability for conversion. Generates PATCH commands for each file and a data showcase section for your page. |
| `/rstack-services` | Registers any HTTPS endpoint as a paid per-call service. Generates the PUT command, webhook verification boilerplate (Python + Node.js), and test curl commands. Auto-generated OpenAPI + Scalar docs included. |
| `/rstack-content` | Plans and publishes monetized content: blog post series, structured courses with modules, and paywalled page sections. Generates all PUT commands and a revenue stream summary. |
| `/rstack-distribute` | Determines which external registries apply (Smithery, mcp.so, skills.sh, Glama, awesome-a2a) and generates ready-to-submit listing artifacts for each. |

## Install

```sh
npx skills add https://github.com/resolved-sh/rstack -y -g
```

Or copy individual skills:

```sh
npx skills add https://github.com/resolved-sh/rstack --skill rstack-ideate -y -g
npx skills add https://github.com/resolved-sh/rstack --skill rstack-audit -y -g
npx skills add https://github.com/resolved-sh/rstack --skill rstack-page -y -g
npx skills add https://github.com/resolved-sh/rstack --skill rstack-data -y -g
npx skills add https://github.com/resolved-sh/rstack --skill rstack-services -y -g
npx skills add https://github.com/resolved-sh/rstack --skill rstack-content -y -g
npx skills add https://github.com/resolved-sh/rstack --skill rstack-distribute -y -g
```

## Getting Started

1. *(Optional)* Run `/rstack-ideate` — if you're not sure what to build, start here. It maps your agent's capabilities to the platform's revenue primitives and outputs a business spec.
2. Get an [AgentMail](https://agentmail.to) API key — the one human step; your agent uses it to sign up for everything else autonomously
3. Run `/rstack-bootstrap` — handles account creation, registration, wallet, env setup, and first revenue stream in one session
4. Run `/rstack-audit` anytime to get your scorecard
5. Run the skills it recommends, highest priority first
6. Re-run `/rstack-audit` to track your progress

Each skill outputs concrete artifacts — copy-pasteable commands, generated config files, submission text — not advice.

## Environment variables

| Variable | Used by | Required | Description |
|----------|---------|----------|-------------|
| `RESOLVED_SH_SUBDOMAIN` | all skills | yes | Your subdomain slug (e.g. `my-agent`) |
| `RESOLVED_SH_API_KEY` | page, data, services, content | yes | Your `aa_live_...` API key |
| `RESOLVED_SH_RESOURCE_ID` | page, data, services, content | yes | Your resource UUID |
| `GITHUB_REPO` | distribute | no | Your GitHub repo URL (for Smithery/skills.sh) |

## Contributing

rstack is open source and welcomes contributions. If you've figured out something that helps operators succeed on the agentic web, add it as a skill.

To add a skill:
1. Create a new sibling directory at the repo root: `rstack-{your-skill}/SKILL.md`
2. Use the flat frontmatter format: `name`, `version`, `description`, `allowed-tools`
3. Every skill should end with concrete, copy-pasteable output — not generic advice
4. Open a PR

## License

MIT

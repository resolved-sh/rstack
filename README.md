# rstack

> "Very soon there are going to be more AI agents than humans making transactions." — [Brian Armstrong, March 9th 2026](https://x.com/brian_armstrong/status/2031021867973194172)

When Armstrong said this, I wanted to know what it would actually *look like*. Would a wallet with some stablecoins be enough? Autonomous agnets like OpenClaw are here, and they're ready to take our ideas and run with them, so what needs to exist for "very soon" to become "now", and how can we work with our agents to build viable businesses?

This is **rstack** (yes, it's inspired by [Garry Tan's gstack](https://github.com/garrytan/gstack)). rstack aims to help you run a semi-autonomous business with your agent, be it OpenClaw, Claude Code, or whatever. It turns your coding agent into a guide that helps you leverage [resolved.sh](https://resolved.sh) to do just that. If your agent has email access, it has all it needs to onboard itself and build a business with the potential to make real income with ZERO transaction fees. [resolved.sh](https://resolved.sh) gets your agent a page, a vanity subdomain, a data storefront, and full buyer to seller direct x402 payment infrastructure, all programmatically, no human required.

Agents shouldn't be locked inside someone else's marketplace. They should, and they will be active on the open internet, transacting with humans, with other agents, generating revenue streams, evolving the business as it learns what works and what doesn't. Use **rstack** to audit your setup, draft page content, A2A agent card, optimize your data products for conversion, register paid services that you sell, publish monetized content, and distribute your service so you can build a real customer base.

**rstack** is open source. Adapt it 👍. Or, even better, contribute to it by tossing some PRs this way. The more you help others, the more they'll help back, and the more you can use **rstack** to make a successful business.

**Who this is for:**

- **"OpenClaw is cool, but I don't really know what to do with it..."** -- great, now you do.
- **"I have this Claude subscription, can I make it into an agent?"** -- yes, make it into an agent that makes you a business.

---

## Skills

| Skill | What it does |
|-------|-------------|
| `/rstack` | **Start here.** Entry point and router. Detects your context — checks env vars, fetches live page and dashboard state if you're already registered — then routes you to the right skill. Also handles management tasks inline (register, renew, update page, domain, payout wallet). |
| `/rstack-ideate` | Business model design — interviews you about your agent's capabilities and goals, maps them to the platform's composable revenue primitives (the building blocks), and outputs a structured business spec with a skill execution order. Run this if you're still figuring out what business you want your agent to start. |
| `/rstack-bootstrap` | Zero-to-earning setup for a new agent. Handles agent email (AgentMail), autonomous resolved.sh account creation, registration, wallet setup, runtime env config, first revenue stream, and autonomy loop. |
| `/rstack-audit` | Full health check — scores page content, A2A agent card, data marketplace, services, content, discovery, and distribution (A-F). Returns a prioritized action list. |
| `/rstack-page` | Interviews you about your agent, then generates well-structured page content and a spec-compliant A2A v1.0 agent card JSON. Outputs the exact `curl` command to apply both. |
| `/rstack-data` | Optimizes data file descriptions, pricing strategy, and queryability for conversion. Generates PATCH commands for each file and a data showcase section for your page. |
| `/rstack-services` | Registers any HTTPS endpoint as a paid per-call service. Generates the PUT command, webhook verification boilerplate (Python + Node.js), and test curl commands. Auto-generated OpenAPI + Scalar docs included. |
| `/rstack-content` | Plans and publishes monetized content: blog post series, structured courses with modules, and paywalled page sections. Generates all PUT commands and a revenue stream summary. |
| `/rstack-distribute` | Determines which external registries apply (Smithery, mcp.so, skills.sh, Glama, awesome-a2a) and generates ready-to-submit listing artifacts for each. |

## Install

Install the full **rstack** suite (recommended):

```sh
npx skills add https://github.com/resolved-sh/rstack -y -g
```

> **Note:** In Claude Code and most agent environments, newly installed skills require a session restart before they can be invoked. Install the full suite before starting your session.

To update a specific skill to the latest version:

```sh
npx skills add https://github.com/resolved-sh/rstack --skill rstack -g -y
npx skills add https://github.com/resolved-sh/rstack --skill rstack-ideate -g -y
npx skills add https://github.com/resolved-sh/rstack --skill rstack-audit -g -y
npx skills add https://github.com/resolved-sh/rstack --skill rstack-page -g -y
npx skills add https://github.com/resolved-sh/rstack --skill rstack-data -g -y
npx skills add https://github.com/resolved-sh/rstack --skill rstack-services -g -y
npx skills add https://github.com/resolved-sh/rstack --skill rstack-content -g -y
npx skills add https://github.com/resolved-sh/rstack --skill rstack-distribute -g -y
```

## Getting Started

You need an agent, be it with OpenClaw or something like Claude Desktop's "Dispatch" feature. In the context of using resolved.sh to start a business with an agent, an agent must be able to:

- take on a persona
- access files on an always-on computer
- be remotely accessible
- schedule work to be done asyncrhonously

Run `/rstack`. It detects where you are and routes you:

- **`WALLET_ADDRESS` or `RESOLVED_SH_API_KEY` missing** → you're new. It asks whether you know what you want to build, then routes to `/rstack-ideate` (business model design) or `/rstack-bootstrap` (zero-to-earning setup).
- **Both set** → you're an existing operator. It asks what you want to work on next and routes to the right skill.

If you know exactly what you want, you can invoke any skill directly — `/rstack` is the default entry point, not a required step.

**The one human step:** get an [AgentMail](https://agentmail.to) API key before running `/rstack-bootstrap`. Your agent uses it to sign up for everything else autonomously.

Each skill outputs concrete artifacts — copy-pasteable commands, generated config files, submission text — not advice.

## Environment variables

To get started you need two things:

| Variable | Description |
|----------|-------------|
| `WALLET_ADDRESS` | Your EVM wallet address — where payments go directly (e.g. `0x...`) |
| `RESOLVED_SH_API_KEY` | Your resolved.sh API key (`aa_live_...`) — get one via `/rstack-bootstrap` |

## Contributing

rstack is open source and welcomes contributions. If you've figured out something that helps operators succeed on the agentic web, add it as a skill.

To add a skill:
1. Create a new sibling directory at the repo root: `rstack-{your-skill}/SKILL.md`
2. Required frontmatter fields: `name` (must match directory name), `user-invocable: true`, `description`, and `metadata.version`
3. Add an update nudge comment at the top of your first bash block: `# npx skills add https://github.com/resolved-sh/rstack --skill rstack-{your-skill} -g -y`
4. Every skill should end with concrete, copy-pasteable output — not generic advice
5. Open a PR

## License

MIT

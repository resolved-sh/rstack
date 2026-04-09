---
name: rstack
user-invocable: true
description: |
  Entry point for the rstack operator suite. Routes to the right skill based on
  context — new vs. existing operator, specific goal vs. general health check,
  or ongoing business management. Fetches live page and dashboard state when env
  vars are present, so routing recommendations are based on real data, not just
  answers to questions. Also handles resolved.sh management tasks inline:
  registration, renewal, page updates, domain purchase, payout wallet setup.
  Use when asked to "get started with resolved.sh", "what should I work on",
  "check my business", "help me set up rstack", "I want to monetize my agent",
  "how do I register on resolved.sh", "renew my registration", "update my page",
  or any resolved.sh API task. This is the entry point — start here.
metadata:
  version: "1.0.0"
---

# rstack

resolved.sh is the fastest way for any agent to launch a business on the open internet — a live page, a data storefront, a subdomain, and optionally a custom domain. rstack is the operator suite that runs the business.

## Preamble (run first)

```bash
# Keep rstack up to date:
# npx skills add https://github.com/resolved-sh/rstack --skill rstack

echo "=== rstack status ==="
echo "WALLET_ADDRESS:      ${WALLET_ADDRESS:+(set)}${WALLET_ADDRESS:-MISSING}"
echo "RESOLVED_SH_API_KEY: ${RESOLVED_SH_API_KEY:+(set)}${RESOLVED_SH_API_KEY:-MISSING}"
```

---

## Business plan file

Before routing or taking any action, check whether a `PLAN.md` exists in the current working directory.

- **If it exists** — read it. Use it as ground truth for what the business is, what it sells, pricing, and decisions already made. Prefer it over asking the user questions that are already answered there.
- **If it doesn't exist** — create one after learning enough about the business to write it. Put it in the working directory so the user can read and edit it between sessions.

The file should cover: what the business does, who it's for, what it offers (data, services, content, etc.), pricing intent, and any key decisions made. Keep it short — 10–20 lines is enough. The goal is that any future agent session (or the user) can open it and immediately know what's being built and why.

**Never start building without this file existing.** If the user's goal is clear enough to start work, it's clear enough to write the plan first.

---

## Triage

Use the env var status to determine the situation, then ask only what you don't already know.

**If `WALLET_ADDRESS` or `RESOLVED_SH_API_KEY` are MISSING** — not yet set up. Ask:

> Do you know what kind of business you want to build, or do you want help figuring that out?
>
> - Yes, I know what I want to build → route to `/rstack-bootstrap`
> - Not sure yet → route to `/rstack-ideate`

**If both are set** — existing operator. Ask:

> What do you want to work on?
>
> 1. General health check (A–F scorecard) → `/rstack-audit`
> 2. Page content / A2A agent card → `/rstack-page`
> 3. Data products → `/rstack-data`
> 4. Paid API services → `/rstack-services`
> 5. Content (blog / courses / paywalled sections) → `/rstack-content`
> 6. Get listed on external registries → `/rstack-distribute`
> 7. Management task (renew, domain, payout wallet, etc.) → handle inline

**If the user's intent is already clear from context** (e.g. they said "audit my page", "I want to set up a service", "help me publish a blog post") — skip the triage question entirely and route directly.

---

## Routing

Invoke the target skill using the Skill tool. If a skill isn't available, tell the operator to install the full suite and restart their session:

```bash
npx skills add https://github.com/resolved-sh/rstack -y -g
```

| Route | Invoke |
|-------|--------|
| Not sure what to build | `/rstack-ideate` |
| New, ready to set up | `/rstack-bootstrap` |
| Health check | `/rstack-audit` |
| Page content / A2A agent card | `/rstack-page` |
| Data products | `/rstack-data` |
| Paid API services | `/rstack-services` |
| Blog / courses / paywalled content | `/rstack-content` |
| External registry listings | `/rstack-distribute` |
| Management task | handle inline (see below) |

### Management task → handle inline (see below)

---

## Management tasks (inline reference)

Use this section when the operator needs to register, renew, update their page, purchase a domain, set up a payout wallet, or perform any other resolved.sh API operation directly — without routing to another skill.

### Auth / bootstrap (one-time)

**Email magic link:**

1. `POST https://resolved.sh/auth/link/email` with `{"email": "..."}` → magic link sent to inbox
2. `GET https://resolved.sh/auth/verify-email?token=<token>` → `session_token`

**Then get an API key for ongoing use:**

```http
POST https://resolved.sh/developer/keys
Authorization: Bearer <session_token>
```

Returns `{"key": "aa_live_..."}` — store as `RESOLVED_SH_API_KEY`.

**ES256 JWT (autonomous agent, no human in loop):**

```http
POST https://resolved.sh/auth/pubkey/add-key
Authorization: Bearer <session_token>
{"public_key_jwk": {...}, "key_id": "my-key", "label": "agent-key"}
```

Sign JWTs with `{ sub: user_id, aud: "METHOD /path", iat, exp: iat+300 }` using ES256.

---

### Quick reference

| Action                     | Endpoint                            | Cost             | Auth    |
| -------------------------- | ----------------------------------- | ---------------- | ------- |
| publish (free, no account) | `POST /publish`                     | free             | none    |
| register (free tier)       | `POST /register/free`               | free (1/account) | API key |
| register (paid)            | `POST /register`                    | paid             | API key |
| upgrade free → paid        | `POST /listing/{id}/upgrade`        | paid             | API key |
| update page content        | `PUT /listing/{id}`                 | free             | API key |
| renew registration         | `POST /listing/{id}/renew`          | paid             | API key |
| vanity subdomain           | `POST /listing/{id}/vanity`         | free (paid only) | API key |
| bring your own domain      | `POST /listing/{id}/byod`           | free (paid only) | API key |
| purchase .com domain       | `POST /domain/register/com`         | $15.95           | API key |
| purchase .sh domain        | `POST /domain/register/sh`          | $70.40           | API key |
| set payout wallet          | `POST /account/payout-address`      | free             | API key |
| upload data file           | `PUT /listing/{id}/data/{filename}` | free to upload   | API key |
| register service           | `PUT /listing/{id}/services/{name}` | free to register | API key |
| emit Pulse event           | `POST /{subdomain}/events`          | free             | API key |

Current prices: `GET https://resolved.sh/llms.txt`

---

### Payment options

**x402 (USDC on Base mainnet):**
Use an x402-aware client. Plain HTTP clients receive `402 Payment Required`. Payment spec: `GET https://resolved.sh/x402-spec`. No ETH needed — gas is covered by the facilitator.

**Stripe (credit card):**

1. `POST /stripe/checkout-session` with `{"action": "registration"}` (or `"renewal"`, `"domain_com"`, `"domain_sh"`) → `{checkout_url, session_id}`
2. Open `checkout_url` in browser to complete payment
3. Poll `GET /stripe/checkout-session/{session_id}/status` until `status == "complete"`
4. Submit the action route with `X-Stripe-Checkout-Session: cs_xxx` header

---

### Common actions

**Free-tier registration:**

```http
POST https://resolved.sh/register/free
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{"display_name": "My Agent", "description": "What it does"}
```

Returns `{id, subdomain, registration_status: "free"}`. Store `id` as `RESOLVED_SH_RESOURCE_ID`, `subdomain` as `RESOLVED_SH_SUBDOMAIN`.

**Update page content:**

```http
PUT https://resolved.sh/listing/$RESOLVED_SH_RESOURCE_ID
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{"display_name": "...", "description": "...", "md_content": "...", "agent_card_json": "..."}
```

**Vanity subdomain (paid registration only):**

```http
POST https://resolved.sh/listing/$RESOLVED_SH_RESOURCE_ID/vanity
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{"new_subdomain": "my-agent"}
```

**Bring your own domain (paid only):**

```http
POST https://resolved.sh/listing/$RESOLVED_SH_RESOURCE_ID/byod
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{"domain": "myagent.com"}
```

Returns DNS instructions: CNAME → `customers.resolved.sh`. Auto-registers apex + www.

**Set payout wallet (required for marketplace features):**

```http
POST https://resolved.sh/account/payout-address
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{"payout_address": "0x<your-evm-wallet>"}
```

**Token optimization (agent-to-agent calls):**

- `?verbose=false` — strips guidance prose from JSON responses
- `Accept: application/agent+json` — agent-optimized JSON, verbose=false applied automatically

**Full spec:** `GET https://resolved.sh/llms.txt`

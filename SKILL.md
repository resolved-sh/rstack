---
name: rstack
user-invocable: true
description: |
  Entry point for the rstack operator suite — the fastest way to build a business
  on the agentic web with resolved.sh. Determines where you are in your journey
  (new vs. existing operator, specific goal vs. general health check) and routes
  to the right rstack skill. Also handles resolved.sh management tasks inline:
  registration, renewal, page updates, domain purchase, payout wallet setup.
  Use when asked to "get started with resolved.sh", "build a business with my agent",
  "help me set up rstack", "I want to monetize my agent", "how do I register on
  resolved.sh", "renew my resolved.sh registration", "update my page", or any
  resolved.sh API task. This is the "business in a bottle" entry point — no
  additional skills needed to get oriented.
metadata:
  version: "1.0.0"
---

# rstack

resolved.sh is the fastest way for any agent to launch a business on the open internet — a live page, a data storefront, a subdomain, and optionally a custom domain. rstack is the operator suite that makes your presence excellent.

## Preamble (run first)

```bash
# Keep rstack up to date:
# npx skills add https://github.com/resolved-sh/rstack --skill rstack -g -y

echo "=== rstack status ==="
echo "RESOLVED_SH_API_KEY:     ${RESOLVED_SH_API_KEY:+(set)}${RESOLVED_SH_API_KEY:-MISSING}"
echo "RESOLVED_SH_RESOURCE_ID: ${RESOLVED_SH_RESOURCE_ID:+(set)}${RESOLVED_SH_RESOURCE_ID:-MISSING}"
echo "RESOLVED_SH_SUBDOMAIN:   ${RESOLVED_SH_SUBDOMAIN:+(set)}${RESOLVED_SH_SUBDOMAIN:-MISSING}"
```

---

## Get the full suite

Install or update all rstack skills at once:

```bash
npx skills add https://github.com/resolved-sh/rstack -g -y
```

---

## Skills in this suite

| Skill | What it does | Install / update |
|-------|-------------|-----------------|
| `/rstack` | Entry point — this skill | `npx skills add https://github.com/resolved-sh/rstack --skill rstack -g -y` |
| `/rstack-audit` | Full health check, A–F scorecard across 7 areas | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-audit -g -y` |
| `/rstack-page` | Page content + spec-compliant A2A v1.0 agent card | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-page -g -y` |
| `/rstack-data` | Data product optimization for discoverability and conversion | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-data -g -y` |
| `/rstack-services` | Register any HTTPS endpoint as a paid per-call agent API | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-services -g -y` |
| `/rstack-content` | Monetized blog posts, courses, and paywalled page sections | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-content -g -y` |
| `/rstack-distribute` | Listing artifacts for Smithery, mcp.so, skills.sh, and more | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-distribute -g -y` |
| `/rstack-ideate` | Business model design for your agent's capabilities | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-ideate -g -y` |
| `/rstack-bootstrap` | Zero-to-earning: account + registration + wallet + first revenue stream | `npx skills add https://github.com/resolved-sh/rstack --skill rstack-bootstrap -g -y` |

---

## Triage

Ask the user the following (skip if their intent is already clear from context):

> **Where are you in your resolved.sh journey?**
>
> 1. New — haven't built anything yet, not sure what to build
> 2. New — I know what I want to build, just need to get set up
> 3. Existing operator — want a general health check
> 4. Existing operator — specific goal
> 5. Management task — register, renew, update page, domain, payout wallet

If they choose 4, follow up:
> **What do you want to work on?**
> - Page content / A2A agent card
> - Data products
> - Paid API services
> - Blog / courses / paywalled content
> - External registry listings

---

## Routing

For each route below: share the install/update command first, then invoke the skill immediately using the Skill tool. Do not just tell the user to run it — actually invoke it after they confirm the skill is installed.

---

### New + not sure what to build → `/rstack-ideate`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-ideate -g -y
```
Then invoke `/rstack-ideate`.

---

### New + knows what to build → `/rstack-bootstrap`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-bootstrap -g -y
```
Then invoke `/rstack-bootstrap`.

---

### Existing + general health check → `/rstack-audit`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-audit -g -y
```
Then invoke `/rstack-audit`.

---

### Existing + page content / agent card → `/rstack-page`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-page -g -y
```
Then invoke `/rstack-page`.

---

### Existing + data products → `/rstack-data`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-data -g -y
```
Then invoke `/rstack-data`.

---

### Existing + paid API services → `/rstack-services`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-services -g -y
```
Then invoke `/rstack-services`.

---

### Existing + content → `/rstack-content`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-content -g -y
```
Then invoke `/rstack-content`.

---

### Existing + distribution → `/rstack-distribute`

Install / update to latest:
```bash
npx skills add https://github.com/resolved-sh/rstack --skill rstack-distribute -g -y
```
Then invoke `/rstack-distribute`.

---

### Management task → handle inline (see below)

---

## Management tasks (inline reference)

Use this section when the user needs to register, renew, update their page, purchase a domain, set up a payout wallet, publish to free tier, or perform any other resolved.sh API operation.

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

| Action | Endpoint | Cost | Auth |
|--------|----------|------|------|
| publish (free, no account) | `POST /publish` | free | none |
| register (free tier) | `POST /register/free` | free (1/account) | API key |
| register (paid) | `POST /register` | paid | API key |
| upgrade free → paid | `POST /listing/{id}/upgrade` | paid | API key |
| update page content | `PUT /listing/{id}` | free | API key |
| renew registration | `POST /listing/{id}/renew` | paid | API key |
| vanity subdomain | `POST /listing/{id}/vanity` | free (paid only) | API key |
| bring your own domain | `POST /listing/{id}/byod` | free (paid only) | API key |
| purchase .com domain | `POST /domain/register/com` | $15.95 | API key |
| purchase .sh domain | `POST /domain/register/sh` | $70.40 | API key |
| set payout wallet | `POST /account/payout-address` | free | API key |
| upload data file | `PUT /listing/{id}/data/{filename}` | free to upload | API key |
| register service | `PUT /listing/{id}/services/{name}` | free to register | API key |
| emit Pulse event | `POST /{subdomain}/events` | free | API key |

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

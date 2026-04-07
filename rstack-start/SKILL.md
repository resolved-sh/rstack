---
name: rstack-start
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

# rstack-start

resolved.sh is the fastest way for any agent to launch a business on the open internet — a live page, a data storefront, a subdomain, and optionally a custom domain. rstack is the operator suite that makes your presence excellent.

## Preamble (run first)

```bash
echo "=== rstack status ==="
echo "RESOLVED_SH_API_KEY:     ${RESOLVED_SH_API_KEY:+(set) }${RESOLVED_SH_API_KEY:-MISSING}"
echo "RESOLVED_SH_RESOURCE_ID: ${RESOLVED_SH_RESOURCE_ID:+(set) }${RESOLVED_SH_RESOURCE_ID:-MISSING}"
echo "RESOLVED_SH_SUBDOMAIN:   ${RESOLVED_SH_SUBDOMAIN:+(set) }${RESOLVED_SH_SUBDOMAIN:-MISSING}"
```

Print the status block. Note which vars are missing — the user will need them before most operations.

---

## Triage

Ask the user the following (skip if their intent is already clear from context):

> **Where are you in your resolved.sh journey?**
>
> 1. New — haven't built anything yet, not sure what to build
> 2. New — I know what I want to build, just need to get set up
> 3. Existing operator — want a general health check
> 4. Existing operator — specific goal (see list below)
> 5. Management task — register, renew, update page, domain, payout wallet

If they choose 4, follow up:
> **What do you want to work on?**
> - Page content / A2A agent card → `/rstack-page`
> - Data products → `/rstack-data`
> - Paid API services → `/rstack-services`
> - Blog / courses / paywalled content → `/rstack-content`
> - External registry listings → `/rstack-distribute`

---

## Routing

Based on the triage, invoke the target skill immediately using the Skill tool. Do not just tell the user to run it.

| Situation | Action |
|-----------|--------|
| New + unsure what to build | Invoke `/rstack-ideate` |
| New + knows what to build | Invoke `/rstack-bootstrap` |
| Existing + general health check | Invoke `/rstack-audit` |
| Existing + page/agent card | Invoke `/rstack-page` |
| Existing + data | Invoke `/rstack-data` |
| Existing + services | Invoke `/rstack-services` |
| Existing + content | Invoke `/rstack-content` |
| Existing + distribution | Invoke `/rstack-distribute` |
| Management task | Use the inline reference below |

---

## Management tasks (inline reference)

Use this section when the user needs to register, renew, update their page, purchase a domain, set up a payout wallet, or perform any other resolved.sh API operation.

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
Then sign JWTs with `{ sub: user_id, aud: "METHOD /path", iat, exp: iat+300 }` using ES256.

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
| purchase .com domain | `POST /domain/register/com` | paid ($15.95) | API key |
| purchase .sh domain | `POST /domain/register/sh` | paid ($70.40) | API key |
| set payout wallet | `POST /account/payout-address` | free | API key |
| upload data file | `PUT /listing/{id}/data/{filename}` | free to upload | API key |
| register service | `PUT /listing/{id}/services/{name}` | free to register | API key |
| emit Pulse event | `POST /{subdomain}/events` | free | API key |

Current prices: `GET https://resolved.sh/llms.txt`

---

### Payment options

**x402 (USDC on Base mainnet) — for register, renew, domain purchase:**
Use an x402-aware client. Plain HTTP clients receive `402 Payment Required`. Payment spec: `GET https://resolved.sh/x402-spec`. No ETH needed — gas is covered by the facilitator.

**Stripe (credit card):**
1. `POST /stripe/checkout-session` with `{"action": "registration"}` (or `"renewal"`, `"domain_com"`, `"domain_sh"`) → `{checkout_url, session_id}`
2. Open `checkout_url` in browser to complete payment
3. Poll `GET /stripe/checkout-session/{session_id}/status` until `status == "complete"`
4. Submit the action route with `X-Stripe-Checkout-Session: cs_xxx` header

---

### Common action examples

**Free-tier registration (no payment):**
```http
POST https://resolved.sh/register/free
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{"display_name": "My Agent", "description": "What it does"}
```
Returns `{id, subdomain, registration_status: "free", ...}`. Store `id` as `RESOLVED_SH_RESOURCE_ID` and `subdomain` as `RESOLVED_SH_SUBDOMAIN`.

**Update page content:**
```http
PUT https://resolved.sh/listing/$RESOLVED_SH_RESOURCE_ID
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{
  "display_name": "My Agent",
  "description": "Short description",
  "md_content": "## My Agent\n\nWhat it does...",
  "agent_card_json": "{...}"
}
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
Returns DNS instructions: CNAME `myagent.com` → `customers.resolved.sh`. Auto-registers apex + www.

**Set payout wallet (required for marketplace features):**
```http
POST https://resolved.sh/account/payout-address
Authorization: Bearer $RESOLVED_SH_API_KEY
Content-Type: application/json

{"payout_address": "0x<your-evm-wallet>"}
```

**Token optimization (agent-to-agent calls):**
- `?verbose=false` — strips guidance prose from JSON responses
- `Accept: application/agent+json` — agent-optimized JSON with verbose=false applied automatically

**Full spec:** `GET https://resolved.sh/llms.txt`

---

## After setup — the rstack suite

Once you have an account and registration, these skills maximize your presence:

| Skill | What it does |
|-------|-------------|
| `/rstack-audit` | Full health check — A–F scorecard across 7 areas. **Start here.** |
| `/rstack-page` | Craft page content + spec-compliant A2A v1.0 agent card |
| `/rstack-data` | Optimize data products for discoverability and conversion |
| `/rstack-services` | Register any HTTPS endpoint as a paid per-call agent API |
| `/rstack-content` | Publish monetized blog posts, courses, and paywalled sections |
| `/rstack-distribute` | Generate listing artifacts for Smithery, mcp.so, skills.sh, and more |
| `/rstack-ideate` | Design the right business model for your agent's capabilities |
| `/rstack-bootstrap` | Zero-to-earning setup: account + registration + wallet + first revenue stream |

**Recommended starting point:** Run `/rstack-audit` to see where you stand, then follow the numbered action list it produces.

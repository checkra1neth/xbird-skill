---
name: xbird-rest-api
description: "Use when building backend services, autonomous agents, or programmatic integrations that need Twitter/X data via HTTP. Triggers: REST API, x402, USDC payment, BYOA credentials, encrypted credentials, per-request headers, Base mainnet, micropayment."
---

# xbird REST API — Twitter/X with x402 Micropayments

Pay-per-request Twitter/X API. Every call is metered via x402 (USDC on Base). Bring your own Twitter account (BYOA) — the server never stores plaintext credentials.

## When to Use

- Backend services or scripts that need Twitter data via HTTP
- Autonomous agents without MCP support
- Any language/framework (not just TypeScript)
- Direct programmatic integrations

**Don't use when:** Running inside Claude Code / Cursor / Windsurf (use MCP instead), or operating on Virtuals marketplace (use ACP instead).

## Setup

```bash
bun add @x402/fetch @x402/evm viem
```

```bash
# .env — Bun auto-loads this, no dotenv needed
PRIVATE_KEY=0x...          # Wallet private key for x402 payment signing
TWITTER_AUTH_TOKEN=...     # From x.com cookies (auth_token)
TWITTER_CT0=...            # From x.com cookies (ct0)
```

```typescript
import { wrapFetchWithPayment, x402Client } from "@x402/fetch";
import { registerExactEvmScheme } from "@x402/evm/exact/client";
import { privateKeyToAccount } from "viem/accounts";

const account = privateKeyToAccount(process.env.PRIVATE_KEY as `0x${string}`);
const client = new x402Client();
registerExactEvmScheme(client, { signer: account });
const paymentFetch = wrapFetchWithPayment(fetch, client);
```

`paymentFetch` is a drop-in `fetch` replacement that auto-handles x402 payment challenges.

## Authentication (BYOA)

**Mode 1 — Per-request headers** (simplest): pass `X-Twitter-Auth-Token` + `X-Twitter-CT0` on every request.

**Mode 2 — Encrypted credentials** (production): register once via `POST /api/accounts` with an encryption key, then send only `X-Encryption-Key` header. See `x402-flow.md` for detailed steps.

## Complete Example

```typescript
const SERVER = "https://xbirdapi.up.railway.app";

// Search with per-request headers
const res = await paymentFetch(
  `${SERVER}/api/search?q=${encodeURIComponent("AI agents")}&count=10`,
  {
    headers: {
      "X-Twitter-Auth-Token": process.env.TWITTER_AUTH_TOKEN!,
      "X-Twitter-CT0": process.env.TWITTER_CT0!,
    },
  },
);
const { data, cursor } = await res.json();
```

## Quick Reference

```
Server:   https://xbirdapi.up.railway.app
Response: { data: {...} } or { data: [...], cursor: "..." }
Error:    { error: "message" }
Undo:     POST to engage, DELETE to undo (like, retweet, bookmark)
Bulk:     Resolve handle → numeric ID via GET /api/users/:handle first
Pricing:  Read $0.001 | Search $0.005 | Bulk/Write $0.01 | Media $0.05
```

Full endpoint list: see `endpoints.md`. Payment flow details: see `x402-flow.md`.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Payment error "invalid_payload" | Wallet address == server payTo. EIP-3009 rejects self-payment. Use a different wallet. |
| Using handle for bulk endpoints | `/api/users/:id/tweets` needs numeric ID. Call `GET /api/users/:handle` first. |
| Empty USDC balance | Fund wallet with USDC on Base mainnet (`eip155:8453`). $0.10 = hundreds of calls. |
| Sending only one credential header | Both `X-Twitter-Auth-Token` AND `X-Twitter-CT0` required, or use `X-Encryption-Key`. |
| Wrong encryption key on request | Must match the key used during `POST /api/accounts` registration exactly. |
| Rate limit 429 | Twitter rate limit. Wait 1-2 minutes, retry. |

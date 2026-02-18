---
name: xbird-rest-api
description: "Use when building backend services, autonomous agents, or programmatic integrations that need Twitter/X data via HTTP. Triggers: REST API, x402, USDC payment, stateless token, encrypted credentials, per-request headers, Base mainnet, micropayment."
---

# xbird REST API — Twitter/X with x402 Micropayments

Pay-per-request Twitter/X API. Every call is metered via x402 (USDC on Base). Fully stateless — the server stores nothing. No database, no stored credentials.

## When to Use

- Backend services or scripts that need Twitter data via HTTP
- Autonomous agents without MCP support
- Any language/framework (not just TypeScript)
- Direct programmatic integrations

**Don't use when:** Running inside Claude Code / Cursor / Windsurf (use MCP instead), or operating on Virtuals marketplace (use ACP instead).

## Setup

```bash
# 1. Install xbird and generate a stateless token
npx @checkra1n/xbird login

# 2. Fund the wallet shown in login output with USDC on Base
```

The `login` command auto-detects your browser cookies, encrypts them into a self-contained stateless token locally, and saves it. Nothing is sent to the server.

## Authentication

**Mode 1 — Stateless token** (recommended): run `xbird login` to generate a token, then pass it via `X-Encryption-Key` header. The token is self-contained — the server decrypts per-request and stores nothing.

**Mode 2 — Per-request headers** (simplest): pass `X-Twitter-Auth-Token` + `X-Twitter-CT0` on every request.

## Complete Example

```typescript
import { wrapFetchWithPayment, x402Client } from "@x402/fetch";
import { registerExactEvmScheme } from "@x402/evm/exact/client";
import { privateKeyToAccount } from "viem/accounts";

const account = privateKeyToAccount(process.env.PRIVATE_KEY as `0x${string}`);
const client = new x402Client();
registerExactEvmScheme(client, { signer: account });
const paymentFetch = wrapFetchWithPayment(fetch, client);

const SERVER = "https://xbirdapi.up.railway.app";

// Search with stateless token
const res = await paymentFetch(
  `${SERVER}/api/search?q=${encodeURIComponent("AI agents")}&count=10`,
  {
    headers: {
      "X-Encryption-Key": process.env.XBIRD_TOKEN!,  // from xbird login
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
| Sending only one credential header | Both `X-Twitter-Auth-Token` AND `X-Twitter-CT0` required, or use `X-Encryption-Key` with stateless token. |
| Invalid token format | Token must be `xbird_sk_<key>.<ciphertext>.<iv>`. Re-run `npx @checkra1n/xbird login` to generate. |
| Rate limit 429 | Twitter rate limit. Wait 1-2 minutes, retry. |

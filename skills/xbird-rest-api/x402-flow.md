# x402 Payment Flow

Every `/api/*` endpoint is protected by x402 micropayments on Base mainnet (USDC).

## Sequence Diagram

```
Agent                          xbird Server                     Facilitator
  |                                |                                |
  |-- GET /api/search?q=bitcoin -->|                                |
  |                                |                                |
  |<-- 402 Payment Required -------|                                |
  |    (payment-required header:                                    |
  |     base64 JSON with amount,                                    |
  |     payTo, network, scheme)                                     |
  |                                                                 |
  |-- Signs EIP-3009 transferWithAuthorization (USDC) offline       |
  |                                                                 |
  |-- GET /api/search?q=bitcoin -->|                                |
  |    (x-payment header with      |                                |
  |     signed authorization)      |-- Verify payment ------------>|
  |                                |<-- Payment valid --------------|
  |                                |                                |
  |                                |-- Execute Twitter API call     |
  |                                |                                |
  |                                |-- Settle payment ------------->|
  |<-- 200 { data, cursor } -------|<-- Settlement confirmed -------|
```

## Steps

1. Agent sends request to endpoint
2. Server returns HTTP 402 with `payment-required` header (base64 JSON containing amount = resource rate × count, payTo address, network `eip155:8453`, scheme `exact`)
3. `@x402/fetch` automatically decodes the challenge, signs a USDC `transferWithAuthorization` (EIP-3009) using the wallet
4. Agent retries the request with `x-payment` header containing the signed authorization
5. Server verifies payment via the facilitator, executes the Twitter API call, settles the payment, and returns the result

All of this is transparent when using `wrapFetchWithPayment` — it handles steps 2-4 automatically.

## Stateless Token Flow

The server is fully stateless — no database, no stored credentials. Credentials are encrypted into a self-contained token.

### Option A — Use `xbird login` (recommended)

```bash
npx @checkra1n/xbird login
# Auto-detects browser cookies, encrypts locally, outputs token
# Token format: xbird_sk_<key_hex>.<ciphertext_b64>.<iv_b64>
```

Then pass the token on every request:

```typescript
const res = await paymentFetch(`${SERVER}/api/search?q=bitcoin&count=10`, {
  headers: {
    "X-Encryption-Key": process.env.XBIRD_TOKEN!,  // xbird_sk_...
  },
});
```

The server parses the token, decrypts credentials per-request with AES-256-GCM, executes the call, then discards everything. Zero server storage.

### Option B — Per-request headers (no token needed)

```typescript
const res = await paymentFetch(`${SERVER}/api/search?q=bitcoin&count=10`, {
  headers: {
    "X-Twitter-Auth-Token": process.env.TWITTER_AUTH_TOKEN!,
    "X-Twitter-CT0": process.env.TWITTER_CT0!,
  },
});
```

Credentials are sent in plaintext headers on each request. Simpler but less secure for production.

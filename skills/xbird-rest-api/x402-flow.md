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
2. Server returns HTTP 402 with `payment-required` header (base64 JSON containing amount, payTo address, network `eip155:8453`, scheme `exact`)
3. `@x402/fetch` automatically decodes the challenge, signs a USDC `transferWithAuthorization` (EIP-3009) using the wallet
4. Agent retries the request with `x-payment` header containing the signed authorization
5. Server verifies payment via the facilitator, executes the Twitter API call, settles the payment, and returns the result

All of this is transparent when using `wrapFetchWithPayment` — it handles steps 2-4 automatically.

## Encrypted Credentials (Mode 2)

Register credentials once, then only send the encryption key on each request. The server encrypts at rest with AES-256-GCM.

### Step 1 — Generate a 32-byte encryption key

```typescript
// Derive from wallet private key via HKDF (recommended)
import { hkdf } from "@noble/hashes/hkdf";
import { sha256 } from "@noble/hashes/sha256";

const keyBytes = hkdf(sha256, Buffer.from(PRIVATE_KEY.slice(2), "hex"), "xbird-encryption", "xbird-cred-key", 32);
const encryptionKey = Buffer.from(keyBytes).toString("hex"); // 64 hex chars

// Or generate a random key
const encryptionKey = Buffer.from(crypto.getRandomValues(new Uint8Array(32))).toString("hex");
```

### Step 2 — Register

```typescript
const res = await paymentFetch(`${SERVER}/api/accounts`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    authToken: process.env.TWITTER_AUTH_TOKEN,
    ct0: process.env.TWITTER_CT0,
    encryptionKey,  // 64 hex chars or 44 base64 chars (32 bytes)
  }),
});
// Response: { registered: true, encrypted: true, username: "yourhandle" }
```

### Step 3 — Use with encryption key header

```typescript
const res = await paymentFetch(`${SERVER}/api/search?q=bitcoin&count=10`, {
  headers: {
    "X-Encryption-Key": encryptionKey,
  },
});
```

The server maps your wallet address (from the x402 payment) to your registered credentials, decrypts them with the provided key, and executes the Twitter API call. A database breach yields only useless ciphertext.

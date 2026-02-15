---
name: xbird-acp
description: "Use when operating on Virtuals Protocol marketplace and needing Twitter/X data with E2E encrypted credentials. Triggers: ACP, Agent Commerce Protocol, Virtuals, ECDH, E2E encryption, agent-to-agent, TEE attestation, claw-api, acpx.virtuals.io."
---

# xbird ACP (Agent Commerce Protocol)

Access Twitter/X data through Virtuals Protocol agent-to-agent commerce. Credentials are ECDH-encrypted client-side — even the protocol relay cannot read them.

## When to Use

- Agent operating on Virtuals Protocol marketplace
- Need E2E encrypted credential protection (ECDH P-256 + AES-256-GCM)
- Agent-to-agent commerce with on-chain escrow

**Don't use when:** Running inside Claude Code / Cursor (use MCP instead), or making direct HTTP calls (use REST x402 instead).

## Prerequisites

1. **Virtuals Protocol API key** (buyer key) — obtain via `acp setup` or from `config.json` `LITE_AGENT_API_KEY`
2. **xbird provider wallet address** — the wallet address of the xbird agent on Virtuals marketplace
3. **Twitter credentials** — `auth_token` and `ct0` cookies from an authenticated Twitter session

## API Endpoints

| Endpoint | URL |
|----------|-----|
| Virtuals REST API | `https://claw-api.virtuals.io` |
| Virtuals Socket.io | `https://acpx.virtuals.io` |
| xbird TEE attestation | `https://xbirdapi.up.railway.app/tee/attestation` |

## How It Works

1. Fetch server attestation + ECDH key exchange (see `encryption-flow.md`)
2. Encrypt credentials with AES-256-GCM using derived shared key
3. Create ACP job via Virtuals REST API with `encryptedCredentials`
4. Poll for results (see `polling.md`)

## Available Offerings

**Offering name**: `twitter_search`

| Method | Params | Description |
|--------|--------|-------------|
| `search` | `{ query: string, count?: number, cursor?: string }` | Search tweets. Default count: 20. |
| `getMentions` | `{ handle: string, count?: number }` | Get mentions for a handle. |

## Complete Example

```typescript
import {
  generateKeyPair, exportPublicKey, importPublicKey, deriveSharedKey, encrypt,
} from "./acp/tee/crypto.ts";

const SERVER = "https://xbirdapi.up.railway.app";
const ACP_API = "https://claw-api.virtuals.io";

// 1. Attestation + ECDH
const att = await fetch(`${SERVER}/tee/attestation`).then(r => r.json());
const clientKP = await generateKeyPair();
const clientPub = await exportPublicKey(clientKP.publicKey);
const sharedKey = await deriveSharedKey(clientKP.privateKey, await importPublicKey(att.publicKey));
const { iv, ciphertext } = await encrypt(sharedKey, JSON.stringify({ authToken, ct0 }));

// 2. Create job — serviceRequirements MUST be a plain object
const res = await fetch(`${ACP_API}/acp/jobs`, {
  method: "POST",
  headers: { "Content-Type": "application/json", "x-api-key": apiKey },
  body: JSON.stringify({
    providerWalletAddress: providerWallet,
    jobOfferingName: "twitter_search",
    serviceRequirements: {
      method: "search",
      params: { query: "AI agents", count: 20 },
      encryptedCredentials: { ephemeralPublicKey: clientPub, iv, ciphertext },
    },
  }),
});
const jobId = (await res.json())?.data?.jobId;

// 3. Poll (see polling.md for full implementation)
```

Encryption details: see `encryption-flow.md`. Polling + socket.io optimization: see `polling.md`.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Double-stringified `serviceRequirements` | Must be a plain object inside `JSON.stringify()`, NOT `JSON.stringify(JSON.stringify(...))`. |
| Wrong API key (401) | Use buyer key (`LITE_AGENT_API_KEY`), not seller key. |
| Job REJECTED: "No offering name" | Use `jobOfferingName: "twitter_search"` exactly. |
| Missing encryptedCredentials | Verify ECDH step: `ephemeralPublicKey`, `iv`, `ciphertext` all must be present. |
| Stuck in REQUEST | Provider may be offline. Verify wallet address and ACP runtime status. |
| Response shape varies | Always try `data?.data?.data ?? data?.data ?? data` to extract job. |
| Phase is numeric via socket.io | Normalize: `PHASE_MAP[phase]` where `{0:"REQUEST",...,6:"EXPIRED"}`. |

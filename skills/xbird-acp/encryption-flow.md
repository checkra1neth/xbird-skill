# E2E Encryption Flow — ECDH P-256 + AES-256-GCM

All credentials are encrypted client-side before leaving your agent. The Virtuals relay never sees plaintext credentials.

## Step 1: Fetch server attestation

```typescript
const attestation = await fetch("https://xbirdapi.up.railway.app/tee/attestation")
  .then(r => r.json());
// Returns: { publicKey: string, codeHash: string, timestamp: number }
```

`publicKey` is the server's ECDH P-256 public key (base64, raw format). `codeHash` is a SHA-256 hash of the server source files -- use it to verify the server is running the expected code.

## Step 2: ECDH key exchange and encryption

```typescript
import {
  generateKeyPair,
  exportPublicKey,
  importPublicKey,
  deriveSharedKey,
  encrypt,
} from "./acp/tee/crypto.ts";

// Generate ephemeral client key pair (P-256)
const clientKP = await generateKeyPair();
const clientPub = await exportPublicKey(clientKP.publicKey);

// Import server public key and derive shared secret
const serverKey = await importPublicKey(attestation.publicKey);
const sharedKey = await deriveSharedKey(clientKP.privateKey, serverKey);

// Encrypt credentials with AES-256-GCM
const { iv, ciphertext } = await encrypt(
  sharedKey,
  JSON.stringify({ authToken: "<your-auth-token>", ct0: "<your-ct0>" }),
);
```

## Step 3: Build EncryptedCredentials object

```typescript
const encryptedCredentials = {
  ephemeralPublicKey: clientPub, // base64 P-256 raw public key
  iv,                            // base64 12-byte AES-GCM IV
  ciphertext,                    // base64 AES-256-GCM ciphertext
};
```

## Crypto Primitives Reference

All functions use the Web Crypto API (works in Bun, Node 20+, and browsers):

| Function | Purpose |
|----------|---------|
| `generateKeyPair()` | ECDH P-256 key pair, extractable, `deriveKey` usage |
| `exportPublicKey(key)` | Export CryptoKey to base64 raw format |
| `importPublicKey(base64)` | Import base64 raw P-256 public key |
| `deriveSharedKey(priv, pub)` | ECDH derive AES-256-GCM key |
| `encrypt(key, plaintext)` | AES-256-GCM encrypt, returns `{ iv, ciphertext }` (base64) |

Source: `src/acp/tee/crypto.ts`

## Security Guarantees

| Layer | Protection |
|-------|-----------|
| **ECDH P-256** | Ephemeral keys per request. Forward secrecy. |
| **AES-256-GCM** | Authenticated encryption before credentials leave buyer. |
| **TEE attestation** | Verify server code hash before trusting. |
| **Relay blindness** | `claw-api.virtuals.io` transports opaque ciphertext. |

**Protected:** Twitter auth_token and ct0 (E2E encrypted, only TEE can decrypt).
**NOT protected:** Query params, deliverable, job metadata (visible to relay for routing).

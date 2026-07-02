# xbird — Twitter/X for AI Agents

[![Website](https://img.shields.io/badge/xbird.dev-website-white)](https://xbird.dev)
[![npm](https://img.shields.io/npm/v/@checkra1n/xbird)](https://www.npmjs.com/package/@checkra1n/xbird)
[![smithery badge](https://smithery.ai/badge/checkra1neth/xbirdmcp)](https://smithery.ai/server/checkra1neth/xbirdmcp)
[![smithery skill](https://img.shields.io/badge/Smithery-skill-purple)](https://smithery.ai/skills/checkra1neth/xbird)
[![skills.sh](https://img.shields.io/badge/skills.sh-xbird-blue)](https://skills.sh/checkra1neth/xbird-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Agent skills that give AI agents **247 Twitter/X tools** — reading, searching, posting, engagement, media upload — across 3 integration protocols.

No Twitter developer account, no API keys, no OAuth.

## Skills

| Skill | Protocol | Best For |
|-------|----------|----------|
| [**xbird**](./skills/xbird/) | MCP (stdio) | Claude Code, Cursor, Windsurf — local MCP tools |
| [**xbird-rest-api**](./skills/xbird-rest-api/) | REST + x402 | Backend services, autonomous agents, any HTTP client |
| [**xbird-acp**](./skills/xbird-acp/) | ACP (Virtuals) | Agent-to-agent commerce on Virtuals Protocol marketplace |

### Which skill to use?

```
Running in Claude Code / Cursor / Windsurf?
  → xbird (MCP)

Building a backend service or autonomous agent?
  → xbird-rest-api (x402 micropayments, USDC on Base)

Operating on Virtuals Protocol marketplace?
  → xbird-acp (E2E encrypted credentials, ECDH + AES-256-GCM)
```

## Quick Install

### Claude Code Plugin

```bash
/plugin install checkra1neth/xbird-skill
```

### Agent Skill (works with 35+ agents)

```bash
npx skills add checkra1neth/xbird-skill
```

### MCP Server Only

**Bun (recommended)** — auto-detects browser cookies:
```bash
claude mcp add xbird -- bunx @checkra1n/xbird
```

**Node.js** — requires one-time login:
```bash
claude mcp add xbird -- npx @checkra1n/xbird
npx @checkra1n/xbird login  # paste auth_token + ct0 from x.com DevTools
```

A payment wallet is generated automatically on first run.

| | Bun (`bunx`) | Node.js (`npx`) |
|--|---|---|
| **Browser cookies** | Auto-detected | Manual login required |
| **Setup** | Install Bun, done | One-time `login` command |
| **Startup** | Faster | Slower (npm registry check) |

### CLI

```bash
# Save Twitter credentials locally
npx @checkra1n/xbird login
```

## How It Works

### MCP (Local)

```
AI Agent (Claude Code / Cursor / Windsurf)
  |  MCP stdio
@checkra1n/xbird (local process)
  |-- Auto-detect Twitter cookies from browser
  |-- Pay x402 --> xbird server (payment only)
  '-- Execute --> Twitter API (your residential IP)
```

The xbird server only verifies payments. All Twitter API calls happen locally from your machine.

### REST API (Stateless)

```
Your Backend / Agent
  |-- npx @checkra1n/xbird login  -->  generates stateless token locally
  |-- GET /api/search + token     -->  xbird server
  |<-- 402 Payment Required       <--  (challenge)
  |-- x-payment header (signed)   -->  (auto via @x402/fetch)
  |<-- 200 { data, cursor }       <--  (result)
```

Fully stateless server — no database, no stored credentials. The token is self-contained (`xbird_sk_<key>.<ciphertext>.<iv>`), decrypted per-request then discarded.

### ACP (Virtuals Protocol)

```
Buyer Agent
  |-- ECDH key exchange  -->  xbird TEE (attestation)
  |-- Encrypted credentials + job  -->  Virtuals relay (claw-api)
  |-- Job lifecycle: REQUEST → NEGOTIATION → TRANSACTION → COMPLETED
  |<-- Deliverable with results
```

End-to-end encrypted credentials (ECDH P-256 + AES-256-GCM). The Virtuals relay transports only opaque ciphertext.

## Pricing

Resource catalog billing (~2× cheaper than X API pay-per-use per unit):

| Resource | Rate/unit | Count-aware | Examples |
|----------|-----------|-------------|----------|
| `post_read` | $0.0025 | yes (default 20, max 100) | `get_tweet`, `search_tweets`, `get_replies` |
| `user_read` | $0.005 | yes | `get_user`, `get_followers` |
| `owned_read` | $0.001 | yes | `get_bookmarks`, `get_home_timeline` |
| `post_create` | $0.0075 | no | `post_tweet`, `reply_to_tweet` |
| `interaction_create` | $0.0075 | no | `like_tweet`, `follow_user` |
| `content_create` | $0.005 | no | `upload_media` |

### Comparison with X API

Example session (9 calls, `count=20` on search/lists):

| Call | xbird | X API |
|------|-------|-------|
| `search_tweets` count=20 | $0.05 | $0.10 |
| `get_tweet` ×3 | $0.0075 | $0.015 |
| `get_user` | $0.005 | $0.010 |
| `get_replies` count=20 | $0.05 | $0.10 |
| `post_tweet` | $0.0075 | $0.015 |
| `like_tweet` | $0.0075 | $0.015 |
| `upload_media` | $0.005 | $0.015 |
| **Total** | **~$0.13** | **~$0.26** |

Both bill per resource unit — xbird unit rates are ~2× lower.

## Security

| Protocol | Credential Protection |
|----------|----------------------|
| **MCP** | Auto-detected from browser, never leave your machine |
| **REST x402** | Fully stateless — encrypted in self-contained token, server stores nothing |
| **ACP** | ECDH P-256 + AES-256-GCM end-to-end encryption (relay-blind) |

## Skill Structure

Each skill follows [Anthropic's best practices](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/tutorials#create-custom-slash-commands) — concise SKILL.md (<500 words) with heavy reference in separate files.

```
skills/
├── xbird/                   # MCP (local tools)
│   ├── SKILL.md             # Setup + workflows + common mistakes
│   └── tools.md             # 247 MCP tools reference
├── xbird-rest-api/          # REST API + x402 micropayments
│   ├── SKILL.md             # Setup + auth + example + common mistakes
│   ├── endpoints.md         # Full endpoint tables with pricing
│   └── x402-flow.md         # Payment flow + stateless token
└── xbird-acp/               # ACP (Virtuals marketplace)
    ├── SKILL.md             # Setup + example + common mistakes
    ├── encryption-flow.md   # ECDH P-256 + AES-256-GCM details
    └── polling.md           # Job lifecycle + polling strategies
```

## Compatibility

| Client | Install Method |
|--------|---------------|
| **Claude Code** | `/plugin install checkra1neth/xbird-skill` |
| **Claude Desktop** | `npx @checkra1n/xbird` as MCP command |
| **Cursor** | `npx @checkra1n/xbird` as MCP command |
| **Windsurf** | `npx @checkra1n/xbird` as MCP command |
| **Smithery** | `npx -y @smithery/cli install @checkra1neth/xbirdmcp` |
| **Any MCP client** | `bunx @checkra1n/xbird` / `npx @checkra1n/xbird` |

## Distribution

| Platform | Install |
|----------|---------|
| **npm** | [`@checkra1n/xbird`](https://www.npmjs.com/package/@checkra1n/xbird) |
| **Claude Code Plugin** | `/plugin install checkra1neth/xbird-skill` |
| **skills.sh** | `npx skills add checkra1neth/xbird-skill` |
| **Smithery MCP** | [`@checkra1neth/xbirdmcp`](https://smithery.ai/server/checkra1neth/xbirdmcp) |
| **Smithery Skill** | [`checkra1neth/xbird`](https://smithery.ai/skills/checkra1neth/xbird) |
| **SkillsMP** | [skillsmp.com](https://skillsmp.com/) |
| **Website** | [xbird.dev](https://xbird.dev) |

## License

MIT

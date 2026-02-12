# xbird — Twitter/X for AI Agents

[![Website](https://img.shields.io/badge/xbird.dev-website-white)](https://xb1rd.vercel.app)
[![npm](https://img.shields.io/npm/v/@checkra1n/xbird)](https://www.npmjs.com/package/@checkra1n/xbird)
[![smithery badge](https://smithery.ai/badge/checkra1neth/xbirdmcp)](https://smithery.ai/server/checkra1neth/xbirdmcp)
[![smithery skill](https://img.shields.io/badge/Smithery-skill-purple)](https://smithery.ai/skills/checkra1neth/xbird)
[![skills.sh](https://img.shields.io/badge/skills.sh-xbird-blue)](https://skills.sh/checkra1neth/xbird-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Agent skill + MCP server that gives AI agents **30 Twitter/X tools** — reading, searching, posting, engagement, media upload — with per-call [x402](https://www.x402.org/) micropayments on Base.

No Twitter developer account. No API keys. No OAuth. Runs locally on your machine from a residential IP.

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

```bash
bunx @checkra1n/xbird
```

## Setup

### 1. Get Twitter Cookies

1. Open [x.com](https://x.com) in your browser
2. DevTools → Application → Cookies → `https://x.com`
3. Copy `auth_token` and `ct0` values

### 2. Get a Wallet

You need a wallet with USDC on Base for micropayments. Export the private key.

### 3. Configure MCP

Add to `~/.claude.json` (Claude Code) or MCP settings (Cursor/Windsurf):

```json
{
  "mcpServers": {
    "xbird": {
      "command": "bunx",
      "args": ["@checkra1n/xbird"],
      "env": {
        "XBIRD_AUTH_TOKEN": "your_auth_token",
        "XBIRD_CT0": "your_ct0",
        "XBIRD_PRIVATE_KEY": "0x_your_wallet_private_key"
      }
    }
  }
}
```

## How It Works

```
AI Agent (Claude Code / Cursor / Windsurf)
  |  MCP stdio
@checkra1n/xbird (local process)
  |-- Pay x402 --> xbird server (payment gateway)
  '-- Execute --> Twitter API (your local IP)
```

The xbird server only verifies payments. All Twitter API calls happen locally from your machine.

## Why xbird?

- **No API keys** — no Twitter developer account, no OAuth tokens, no rate limit management
- **3x cheaper than X API** — flat per-call pricing vs per-resource charges (see [comparison](#pricing-comparison))
- **Residential IP** — runs locally on your machine, no datacenter blocks
- **30 tools** — full coverage: read, search, post, engage, upload media
- **Zero config** — one install command, your agent handles payments automatically via x402
- **Works everywhere** — Claude Code, Cursor, Windsurf, and 35+ MCP-compatible agents

## Tools (30)

### Read — $0.001/call

| Tool | Description |
|------|-------------|
| `get_tweet` | Get a tweet by ID |
| `get_thread` | Get a tweet thread (conversation chain) |
| `get_replies` | Get replies to a tweet |
| `get_user` | Get user profile by handle |
| `get_user_about` | Get detailed user info |
| `get_current_user` | Get authenticated user's profile |
| `get_home_timeline` | Get home timeline |
| `get_news` | Get trending topics |
| `get_lists` | Get owned lists |
| `get_list_timeline` | Get tweets from a list |

### Search — $0.005/call

| Tool | Description |
|------|-------------|
| `search_tweets` | Search tweets by query |
| `get_mentions` | Get mentions for a user |

### Bulk — $0.01/call

| Tool | Description |
|------|-------------|
| `get_user_tweets` | Get tweets posted by a user |
| `get_followers` | Get a user's followers |
| `get_following` | Get users a user follows |
| `get_likes` | Get liked tweets |
| `get_bookmarks` | Get bookmarked tweets |
| `get_list_memberships` | Get list memberships |

### Write — $0.01/call

| Tool | Description |
|------|-------------|
| `post_tweet` | Post a new tweet |
| `reply_to_tweet` | Reply to a tweet |
| `post_thread` | Post a thread |
| `like_tweet` / `unlike_tweet` | Like / unlike |
| `retweet` / `unretweet` | Retweet / undo |
| `bookmark_tweet` / `unbookmark_tweet` | Bookmark / undo |
| `follow_user` / `unfollow_user` | Follow / unfollow |

### Media — $0.05/call

| Tool | Description |
|------|-------------|
| `upload_media` | Upload image/video for tweets |

## Pricing Comparison

Example agent session — 9 API calls in a typical workflow:

| Call | xbird | X API |
|------|-------|-------|
| `search_tweets` | $0.005 | $0.100 |
| `get_tweet` ×3 | $0.003 | $0.015 |
| `get_user` | $0.001 | $0.010 |
| `get_replies` | $0.001 | $0.100 |
| `post_tweet` | $0.010 | $0.010 |
| `like_tweet` | $0.010 | $0.015 |
| `upload_media` | $0.050 | $0.010 |
| **Total** | **$0.080** | **$0.260** |

**3.2x cheaper.** X API charges per resource fetched — a search returning 20 tweets costs 20x the per-tweet price. xbird charges a flat fee per call.

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
| **Website** | [xbird.dev](https://xb1rd.vercel.app) |

## License

MIT

# xbird — Twitter/X for AI Agents

[![npm](https://img.shields.io/npm/v/xbird-mcp)](https://www.npmjs.com/package/xbird-mcp)
[![skills.sh](https://img.shields.io/badge/skills.sh-xbird-blue)](https://skills.sh/checkra1neth/xbird-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Agent skill + MCP server that gives AI agents **30 Twitter/X tools** — reading, searching, posting, engagement, media upload — with per-call x402 micropayments on Base.

Runs locally on your machine using your Twitter cookies, so requests come from a residential IP (no datacenter blocks).

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
bunx xbird-mcp
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
      "args": ["xbird-mcp"],
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
xbird-mcp (local process)
  |-- Pay x402 --> xbird server (payment gateway)
  '-- Execute --> Twitter API (your local IP)
```

The xbird server only verifies payments. All Twitter API calls happen locally from your machine.

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

## Payments

Every tool call makes a micropayment via [x402](https://www.x402.org/) on Base (L2). Prices range from $0.001 (reading) to $0.05 (media upload).

## Distribution

| Platform | Install |
|----------|---------|
| **npm** | `bunx xbird-mcp` / `npx xbird-mcp` |
| **Claude Code Plugin** | `/plugin install checkra1neth/xbird-skill` |
| **skills.sh** | `npx skills add checkra1neth/xbird-skill` |
| **SkillsMP** | [skillsmp.com](https://skillsmp.com/) |
| **Smithery** | [smithery.ai](https://smithery.ai/) |

## License

MIT

---
name: xbird
description: "Use when the user asks to tweet, post threads, read tweets, search Twitter/X, check mentions, manage engagement (like/retweet/bookmark), lists, communities, or update profile. Triggers: twitter, tweet, post, thread, timeline, mentions, followers, bookmark."
---

# xbird — Twitter/X for AI Agents

247 MCP tools for Twitter/X with x402 micropayments. Runs locally from residential IP.

**Zero config** — auto-detects your browser session and generates a wallet. No API keys, no developer account.

## When to Use

- Running inside Claude Code, Cursor, or Windsurf
- Need direct MCP tool access to Twitter/X
- Local execution from your own IP

**Don't use when:** Building backend services or autonomous agents (use REST x402 instead), or operating on Virtuals marketplace (use ACP instead).

## Setup

```bash
claude mcp add xbird -- npx @checkra1n/xbird
```

That's it. xbird auto-detects your Twitter session from Chrome, Firefox, Edge, or Safari. A payment wallet is generated automatically on first run.

Full tool list: see `tools.md`.

## Common Workflows

**Post tweet with image:** `upload_media` (get `mediaId`) → `post_tweet` with `mediaIds: ["<id>"]`

**Get someone's tweets:** `get_user` (get numeric `userId`) → `get_user_tweets` with `userId`

**Search and engage:** `search_tweets` with query → `like_tweet` or `retweet` results

**Update profile:** `update_profile_image` + `update_profile` with new bio

## Quick Reference

```
Handles:    work with or without @ prefix
Pagination: list/search use count (default 20, max 100)
Media:      always upload first, then attach mediaId to tweet
Billing:    resource × count — post_read $0.0025 | user_read $0.005 | owned_read $0.001
              post_create $0.0075 | interaction_create $0.0075 | content_create $0.005
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using handle for bulk tools | `get_user_tweets`, `get_followers`, etc. need numeric `userId`. Call `get_user` first. |
| Posting thread with 1 tweet | `post_thread` requires minimum 2 tweets. Use `post_tweet` for single tweet. |
| Media not attached | Upload returns `mediaId` — must pass it in `mediaIds` array to `post_tweet`. |
| Rate limit error | Twitter rate limit. Wait 1-2 minutes, retry. |
| Cookies not detected | Login to x.com in any browser. xbird auto-detects Chrome, Firefox, Edge, Safari. |

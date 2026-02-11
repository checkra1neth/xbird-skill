---
name: xbird
description: "Use when the user asks to tweet, read tweets, search Twitter/X, check mentions, manage engagement (like/retweet/bookmark), or interact with Twitter accounts"
---

# xbird — Twitter/X for AI Agents

30 MCP tools for Twitter/X with x402 micropayments. Runs locally from residential IP.

## Prerequisites

The xbird MCP server must be configured. Add to `~/.claude.json`:

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

Get cookies from x.com: DevTools → Application → Cookies → copy `auth_token` and `ct0`.

## Available Tools

### Read ($0.001/call)
- `get_tweet` — get a tweet by ID
- `get_thread` — get full thread/conversation chain
- `get_replies` — get replies to a tweet
- `get_user` — get user profile by handle
- `get_user_about` — get detailed user info
- `get_current_user` — get authenticated user's profile
- `get_home_timeline` — get home timeline
- `get_news` — get trending topics
- `get_lists` — get owned lists
- `get_list_timeline` — get tweets from a list

### Search ($0.005/call)
- `search_tweets` — search tweets by query
- `get_mentions` — get mentions for a user

### Bulk ($0.01/call)
- `get_user_tweets` — get tweets by user (needs numeric userId)
- `get_followers` / `get_following` — followers/following lists
- `get_likes` — liked tweets
- `get_bookmarks` — bookmarked tweets
- `get_list_memberships` — lists user is a member of

### Write ($0.01/call)
- `post_tweet` — post a tweet (supports mediaIds)
- `reply_to_tweet` — reply to a tweet
- `post_thread` — post a multi-tweet thread (min 2 tweets)
- `like_tweet` / `unlike_tweet`
- `retweet` / `unretweet`
- `bookmark_tweet` / `unbookmark_tweet`
- `follow_user` / `unfollow_user`

### Media ($0.05/call)
- `upload_media` — upload image/video, returns mediaId

## Agent Usage Notes

- Handles: with or without `@` prefix
- Bulk tools (`get_user_tweets`, `get_followers`, `get_following`, `get_likes`) need numeric **userId**, not handle. Get it from `get_user` first.
- `post_thread` takes an array of strings, minimum 2
- `upload_media` returns a mediaId — pass it to `post_tweet` or `reply_to_tweet` via the `mediaIds` array
- Pagination: most list tools support `cursor` for next page
- `search_tweets` supports Twitter search operators: `from:user`, `to:user`, `since:2024-01-01`, `filter:media`, etc.
- All calls are paid via x402 micropayments on Base (USDC)

# xbird REST API — Endpoints Reference

Server: `https://xbirdapi.up.railway.app`

## Account Management — $0.001/call

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/accounts` | Register encrypted credentials |
| `GET` | `/api/accounts` | Check registration status |
| `DELETE` | `/api/accounts` | Remove registration |

## Read — $0.001/call

| Method | Endpoint | Query Params | Description |
|--------|----------|-------------|-------------|
| `GET` | `/api/tweets/:id` | — | Get tweet by ID |
| `GET` | `/api/tweets/:id/thread` | — | Get full thread/conversation |
| `GET` | `/api/tweets/:id/replies` | `count`, `cursor` | Get replies to a tweet |
| `GET` | `/api/users/:handle` | — | Get user profile by handle |
| `GET` | `/api/users/:handle/about` | — | Get detailed user info |
| `GET` | `/api/timeline/home` | `count`, `cursor`, `following` | Get home feed |
| `GET` | `/api/news` | `count`, `tab`, `aiOnly`, `withTweets`, `trendingOnly` | Get trending topics |
| `GET` | `/api/lists` | — | Get owned + membership lists |
| `GET` | `/api/lists/:id/tweets` | `count`, `cursor` | Get list timeline |

## Search — $0.005/call

| Method | Endpoint | Query Params | Description |
|--------|----------|-------------|-------------|
| `GET` | `/api/search` | `q` (required), `count`, `cursor` | Search tweets |
| `GET` | `/api/mentions/:handle` | `count` | Get mentions for a user |

Search supports Twitter operators: `from:user`, `to:user`, `since:2024-01-01`, `filter:media`, `-filter:retweets`.

## Bulk — $0.01/call

| Method | Endpoint | Query Params | Description |
|--------|----------|-------------|-------------|
| `GET` | `/api/users/:id/tweets` | `count`, `cursor` | Get user's tweets (numeric ID) |
| `GET` | `/api/users/:id/followers` | `count`, `cursor` | Get user's followers (numeric ID) |
| `GET` | `/api/users/:id/following` | `count`, `cursor` | Get who user follows (numeric ID) |
| `GET` | `/api/users/:id/likes` | `count`, `cursor` | Get user's liked tweets (numeric ID) |
| `GET` | `/api/bookmarks` | `count`, `cursor`, `folderId` | Get bookmarked tweets |

Bulk endpoints require **numeric user ID**, not handle. Resolve via `GET /api/users/:handle` first.

## Write — $0.01/call

| Method | Endpoint | Body | Description |
|--------|----------|------|-------------|
| `POST` | `/api/tweets` | `{ text, mediaIds? }` | Post a tweet |
| `POST` | `/api/tweets/:id/reply` | `{ text, mediaIds? }` | Reply to a tweet |
| `POST` | `/api/tweets/:id/like` | — | Like a tweet |
| `DELETE` | `/api/tweets/:id/like` | — | Unlike a tweet |
| `POST` | `/api/tweets/:id/retweet` | — | Retweet |
| `DELETE` | `/api/tweets/:id/retweet` | — | Undo retweet |
| `POST` | `/api/tweets/:id/bookmark` | — | Bookmark a tweet |
| `DELETE` | `/api/tweets/:id/bookmark` | — | Remove bookmark |
| `POST` | `/api/users/:handle/follow` | — | Follow a user |
| `DELETE` | `/api/users/:handle/follow` | — | Unfollow a user |

## Media — $0.05/call

| Method | Endpoint | Body | Description |
|--------|----------|------|-------------|
| `POST` | `/api/media` | `multipart/form-data` with `file` field, or raw binary with `Content-Type` | Upload media, returns `mediaId` |

Attach uploaded media to tweets via the `mediaIds` array in `POST /api/tweets` or `POST /api/tweets/:id/reply`.

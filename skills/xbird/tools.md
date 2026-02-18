# xbird MCP — Tools Reference

35 MCP tools for Twitter/X. All tools accept standard MCP parameters.

## Read — $0.001/call

| Tool | Description |
|------|-------------|
| `get_tweet` | Get tweet by ID |
| `get_thread` | Get full thread/conversation chain |
| `get_article` | Get full article/long-form post content |
| `get_replies` | Get replies to a tweet (supports `count`, `cursor`) |
| `get_user` | Get user profile by handle |
| `get_user_about` | Get detailed user info (bio, stats, links) |
| `get_current_user` | Get authenticated user's profile |
| `get_home_timeline` | Get home feed (supports `count`, `cursor`) |
| `get_news` | Get trending topics (tabs: `trending`, `forYou`, `news`, `sports`, `entertainment`) |
| `get_lists` | Get owned Twitter lists |
| `get_list_timeline` | Get tweets from a list by list ID |

## Search — $0.005/call

| Tool | Description |
|------|-------------|
| `search_tweets` | Search tweets. Supports operators: `from:user`, `to:user`, `since:2024-01-01`, `filter:media`, `-filter:retweets` |
| `get_mentions` | Get mentions for a handle |

## Bulk — $0.01/call

| Tool | Description |
|------|-------------|
| `get_user_tweets` | Get user's tweets. **Requires numeric `userId`** — get it from `get_user` first |
| `get_followers` | Get user's followers. **Requires numeric `userId`** |
| `get_following` | Get who user follows. **Requires numeric `userId`** |
| `get_likes` | Get user's liked tweets. **Requires numeric `userId`** |
| `get_bookmarks` | Get bookmarked tweets |
| `get_list_memberships` | Get lists user is a member of |

## Write — $0.01/call

| Tool | Description |
|------|-------------|
| `post_tweet` | Post a tweet. Pass `mediaIds` array to attach media |
| `reply_to_tweet` | Reply to a tweet by `replyToId` |
| `post_thread` | Post a thread — array of strings, **minimum 2 tweets** |
| `like_tweet` / `unlike_tweet` | Like or unlike by tweet ID |
| `retweet` / `unretweet` | Retweet or undo by tweet ID |
| `bookmark_tweet` / `unbookmark_tweet` | Bookmark or remove by tweet ID |
| `follow_user` / `unfollow_user` | Follow or unfollow by handle |

## Profile — $0.01/call

| Tool | Description |
|------|-------------|
| `update_profile` | Update bio/description text |
| `update_profile_image` | Update avatar — absolute file path to image |
| `update_profile_banner` | Update banner — absolute file path to image |
| `remove_profile_banner` | Remove banner image |

## Media — $0.05/call

| Tool | Description |
|------|-------------|
| `upload_media` | Upload image/video, returns `mediaId`. Pass it to `post_tweet` or `reply_to_tweet` via `mediaIds` |

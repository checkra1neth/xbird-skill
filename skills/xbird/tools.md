# xbird MCP Tools

> Bundled with the xbird agent skill. Auto-generated — run `bun run docs:tools` in the xbird repo to refresh.

**247 tools** · package `0.4.0`

Billing matches X API resources (pay per unit before execution). Prices are ~2× cheaper than official pay-per-use.

## `post_read` — $0.0025/unit

| Tool | Description |
|------|-------------|
| `add_content_disclosure` | Add a content disclosure to a tweet (e.g. AI-generated, paid promotion) |
| `browse_space_topics` | Browse Space topics (for the create-Space flow) |
| `can_tweet_be_media_note` | Check if a specific tweet is eligible to receive a Birdwatch / Community media note |
| `can_verify_phone` | Check if the authenticated user can verify their phone number for the blue checkmark |
| `check_verification` | Check verification status (alias) |
| `create_birdwatch_note` | Create a Birdwatch / Community Note on a tweet |
| `create_grok_conversation` | Create a new Grok AI conversation (returns conversationId; the response comes via SSE streaming, not GraphQL) |
| `create_highlight` | Create a Twitter Highlight (curated collection of tweets) |
| `disable_verified_phone_label` | Disable the verified phone label |
| `enable_logged_out_web_notifications` | Enable browser notifications for Twitter when not signed in |
| `enable_verified_phone_label` | Enable the verified phone label for your account (start blue checkmark process) |
| `enroll_coupon` | Enroll a coupon code for a discount on subscriptions |
| `get_account_label` | Get account label (verified phone, government, etc.) for a user |
| `get_ad_accounts` | Get advertising accounts associated with the authenticated user |
| `get_alt_text_prompt` | Get the user's alt-text prompt preference for image uploads |
| `get_alt_text_prompt_preference` | Get user's alt text prompt preference (whether to be reminded to add alt text) |
| `get_article` | Get the full article/long-form post content from a tweet. Pass the tweet ID that contains the article (not the article URL ID). |
| `get_available_locations` | Get list of available WOEID locations for trends (Worldwide, US, UK, etc.) |
| `get_budgets` | Get advertising budgets for the authenticated user's ad accounts |
| `get_coupons` | Get available coupon codes for Premium subscriptions |
| `get_data_saver` | Get the user's data saver mode setting |
| `get_data_saver_mode` | Get current data saver mode (low bandwidth) setting |
| `get_home_timeline` | Get home feed |
| `get_likes` | Get tweets liked by a user (alias for get_user_liked_tweets) |
| `get_mentions` | Get mentions for a user |
| `get_my_articles` | Get the authenticated user's Twitter Articles (long-form posts) |
| `get_payment_methods` | Get saved payment methods for the authenticated user |
| `get_personalization` | Get personalization settings (timeline, search, etc.) |
| `get_replies` | Get replies to a tweet |
| `get_similar_posts` | Get posts similar to a given tweet (Twitter's 'More like this') |
| `get_story_topic` | Get a Twitter Story topic by rest_id (embedded articles in tweets) |
| `get_subscription_checkout_url` | Get checkout URL to subscribe to a product |
| `get_subscription_product_details` | Get details for a specific subscription product (e.g. 'super-follows-monthly') |
| `get_supported_languages` | Get the list of supported translation languages (48 languages) |
| `get_thread` | Get a tweet thread (conversation chain) |
| `get_topic_by_id` | Get a topic by ID (name, description, follower count) |
| `get_topic_carousel` | Get featured topics carousel (Explore page) |
| `get_topics_to_follow_sidebar` | Get topics Twitter suggests you should follow |
| `get_tweet` | Get a tweet by ID |
| `get_tweet_likes_count` | Get just the like count for a tweet (faster than full analytics) |
| `get_tweet_quotes` | Get quote tweets of a tweet |
| `get_tweet_quotes_count` | Get just the quote tweets count for a tweet |
| `get_tweet_replies` | Get replies to a tweet |
| `get_tweet_replies_count` | Get just the replies count for a tweet |
| `get_tweet_retweets_count` | Get just the retweet count for a tweet |
| `get_tweet_stats` | Get aggregated stats for a tweet (full TweetStats object) |
| `get_tweet_views_count` | Get the view (impression) count for a tweet |
| `get_tweets_by_ids` | Get up to 100 tweets by IDs in a single request |
| `get_upsells` | Get Premium upsell offers available for the authenticated user |
| `get_verification_link_block` | Get verification link block info (state of the blue checkmark application) |
| `get_xchat_token` | Get XChat JWT token for WebSocket authentication |
| `home` | Get the authenticated user's home timeline (For You) |
| `mark_topic_not_interested` | Mark a topic as 'Not Interested' (reduces its presence in your feed) |
| `mute_list` | Mute a list (stop seeing its tweets in your home feed) |
| `post_note_tweet` | Post a long-form tweet (NoteTweet, up to 25,000 characters) |
| `post_poll` | Post a tweet with a poll attached |
| `publish_article` | Publish a draft article (make it live) |
| `put_client_education_flag` | Mark a client education tip as seen (for tooltips/educational content) |
| `search_bookmarks` | Search your bookmarked tweets |
| `search_communities` | Search for Twitter communities by keyword |
| `search_lists` | Search Twitter lists by keyword |
| `search_tweets` | Search for tweets. Supports operators: from:user, to:user, since:2024-01-01, until:2024-12-31, filter:media, -filter:retweets, has:images, has:videos, has:links, lang:en, min_faves:100, -is:retweet |
| `set_data_saver_mode` | Enable or disable data saver mode (reduces media quality for low bandwidth) |
| `set_default_share_target` | Set the default share target (Twitter DM, Bookmark folder, etc.) |
| `share_audio_space` | Share an audio Space (adds to your shared spaces) |
| `submit_search_feedback` | Submit feedback about a search result (helps Twitter improve search) |
| `submit_timeline_feedback` | Submit feedback about timeline (e.g. 'show more like this', 'less like this') |
| `switch_subscription_tier` | Switch the user's subscription tier (e.g. Basic → Premium) |
| `unmention_user_from_conversation` | Unmention a user from a conversation (remove @-mention) |
| `unmute_list` | Unmute a list |
| `unpublish_article` | Unpublish (archive) a published article |
| `unshare_audio_space` | Remove an audio Space from your shared spaces |
| `update_alt_text_prompt_preference` | Update alt text prompt preference (enable/disable reminder) |
| `update_article_content` | Update article body content |
| `update_article_cover` | Update article cover image |
| `update_article_title` | Update article title |
| `update_audio_spaces_sharing_preference` | Enable/disable sharing your audio Space listening activity with followers |

## `user_read` — $0.005/unit

| Tool | Description |
|------|-------------|
| `check_username_availability` | Check if a username is available and get suggested alternatives |
| `get_blocked_accounts` | Get accounts the authenticated user has blocked |
| `get_blocking` | Get accounts the authenticated user has blocked |
| `get_blue_verified_followers` | Get a Premium user's blue-verified (Twitter Blue) followers |
| `get_followers` | Get a user's followers |
| `get_following` | Get users that a user follows |
| `get_muted_accounts` | Get accounts the authenticated user has muted |
| `get_muting` | Get accounts the authenticated user has muted |
| `get_super_followers` | Get users subscribed to a user's Super Follows |
| `get_tweet_likers` | Get list of users who liked a tweet |
| `get_tweet_retweeters` | Get list of users who retweeted a tweet |
| `get_user` | Get user profile by handle |
| `get_user_about` | Get detailed user info (bio, stats, links) |
| `get_user_analytics` | Get user profile spotlights and stats (verified status, premium counts) |
| `get_user_articles` | Get a user's Twitter Articles (long-form posts) |
| `get_user_business_profile` | Get a user's business profile (Premium feature) |
| `get_user_claims` | Get verified account claims for a user (org affiliation, etc.) |
| `get_user_followers` | Get followers of a user by handle (alias for get_followers) |
| `get_user_following` | Get users that a user is following (by handle) |
| `get_user_highlights` | Get a user's Twitter Highlights (curated tweet collections) |
| `get_user_liked_tweets` | Get tweets liked by a user (only public likes) |
| `get_user_likes` | Get tweets liked by a user (by handle) |
| `get_user_media` | Get a user's media (images/videos posted) |
| `get_user_memberships` | Get communities a user is a member of |
| `get_user_overview` | Get user engagement overview (impressions, engagements, profile visits) for the last N days |
| `get_user_preferences` | Get user preferences (notifications, content, personalization settings) |
| `get_user_promotable_tweets` | Get tweets the user has promoted (paid) |
| `get_user_promoted_tweets` | Get posts the user has retweeted (alias for promoted) |
| `get_user_sessions` | Get active sessions for the authenticated user (security check) |
| `get_user_super_follow_tweets` | Get Super Follows-exclusive tweets from a user |
| `get_user_tweets` | Get tweets posted by a user |
| `get_user_tweets_and_replies` | Get a user's tweets including replies |
| `get_user_tweets_by_handle` | Get a user's tweets by handle (resolves handle → userId automatically) |
| `get_users_by_ids` | Get up to 100 users by numeric IDs in a single request |
| `get_users_by_screen_names` | Get up to 100 users by screen names (handles) in a single request |
| `get_who_to_follow` | Get suggested users to follow (Twitter's recommendations) |

## `owned_read` — $0.001/unit

| Tool | Description |
|------|-------------|
| `get_birdwatch_global_timeline` | Get the global Birdwatch / Community Notes timeline |
| `get_birdwatch_note` | Get a single Birdwatch / Community Note by ID |
| `get_birdwatch_note_translation` | Translate a Birdwatch / Community Note to a target language |
| `get_birdwatch_notes_for_tweet` | Get Birdwatch / Community Notes written about a specific tweet |
| `get_birdwatch_public_data` | Get Birdwatch public data bundle URLs (downloads of all notes/ratings for offline analysis) |
| `get_birdwatch_sign_up_eligibility` | Check if the authenticated user is eligible to sign up as a Birdwatch contributor |
| `get_birdwatch_suggestion_feedback_overview` | Get your earned Birdwatch rating overview (helpfulness score, impact) |
| `get_bookmark_folder_tweets` | Get tweets inside a bookmark folder |
| `get_bookmark_folders` | Get all bookmark folders for the authenticated user |
| `get_bookmarks` | Get bookmarked tweets |
| `get_business_team_timeline` | Get business team members' tweets (for business accounts) |
| `get_current_user` | Get authenticated user's profile |
| `get_dm_settings` | Get DM settings (who can send you DMs, AV call permissions, quality filter) |
| `get_draft_tweets` | List all saved draft tweets |
| `get_grok_home` | Get Grok home configuration (available models like grok-4-auto, grok-3, etc.) |
| `get_list_memberships` | Get lists the authenticated user is a member of |
| `get_lists` | Get owned Twitter lists (for the authenticated user) |
| `get_my_birdwatch_profile` | Get the authenticated user's Birdwatch / Community Notes contributor profile (alias, streak, etc.) |
| `get_notification_likes` | Get only 'likes' notifications (people who liked your tweets) |
| `get_notification_mentions` | Get only 'mentions' notifications (people who @mentioned you) |
| `get_notification_retweets` | Get only 'retweets' notifications (people who retweeted your tweets) |
| `get_notifications` | Get notifications (mentions, likes, retweets) |
| `get_owned_lists` | Get lists owned by the authenticated user (alias for get_lists) |
| `get_pinned_timelines` | Get the list of timelines pinned to the home sidebar (for the authenticated user) |
| `get_premium_hub` | Get Twitter Premium / Blue hub configuration (plans, pricing) |
| `get_premium_hub_config` | Get Premium hub configuration (deprecated alias — use get_premium_hub) |
| `get_profile_filter` | Get profile content filter (what types of content you see in your timeline) |
| `get_scheduled_tweets` | List all scheduled tweets (pending publication) |
| `get_tweet_analytics` | Get analytics for a tweet (views, likes, retweets, replies, quotes, bookmarks) |
| `get_tweet_bookmarks_count` | Get the bookmark count for a tweet |
| `get_tweet_engagement` | Get engagement metrics for a tweet (alias for get_tweet_analytics) |
| `get_tweet_engagement_timeline` | Get tweet engagement over time (daily impressions/engagements breakdown) |

## `list_read` — $0.0025/unit

| Tool | Description |
|------|-------------|
| `get_all_lists_for_user` | Get all lists (owned + member + pinned) for a user |
| `get_combined_lists` | Get combined lists view (owned + subscribed + pinned) for a user |
| `get_list` | Get list info by ID |
| `get_list_by_slug` | Get list info by slug + owner screen name |
| `get_list_members` | Get members of a list |
| `get_list_subscribers` | Get subscribers (followers) of a list |
| `get_list_tweets` | Get tweets from a list timeline |
| `get_membership_lists` | Get lists a user follows (is a member of) |

## `space_read` — $0.0025/unit

| Tool | Description |
|------|-------------|
| `get_audio_space` | Get audio Space details by ID (title, state, participants, host) |
| `get_space` | Get audio Space info by ID |

## `community_read` — $0.0025/unit

| Tool | Description |
|------|-------------|
| `get_community` | Get community info (rules, member count, banner) by ID |
| `get_community_about` | Get community about/description |
| `get_community_hashtags` | Get hashtags featured in a community |
| `get_community_members` | Get community members (admins + facepile) |
| `get_community_rules` | Get community rules |
| `get_community_tweets` | Get tweets posted in a community |

## `trend_read` — $0.005/unit

| Tool | Description |
|------|-------------|
| `get_explore` | Get the Explore 'For You' page (alias for get_explore_page) |
| `get_explore_page` | Get the Explore 'For You' page (curated news, stories, trends) |
| `get_explore_sidebar` | Get the Explore sidebar (trending topics, who to follow, news) |
| `get_local_trends` | Get currently trending topics for the user's location |
| `get_news` | Get trending topics and news (tabs: trending, forYou, news, sports, entertainment) |
| `get_trend_history` | Get history of a specific trend (when it trended, tweet count timeline) |
| `get_trend_relevant_users` | Get users relevant to a specific trend (suggested follows for a topic) |
| `get_trends_for_woeid` | Get trending topics for a specific Yahoo WOEID location |

## `dm_read` — $0.005/unit

| Tool | Description |
|------|-------------|
| `block_in_dms` | Block a user from sending you DMs (UI-only block requires confirm dialog) |
| `get_dm_call_setup` | Get voice/video call setup token for DMs |
| `get_dm_conversation` | Get messages in a DM conversation by conversation_id. Returns encrypted message events (content is E2E encrypted). |
| `get_dm_conversation_decrypted` | Get messages in a DM conversation with E2E decryption. Requires your XChat private key (base64 PKCS8). Use extract-xchat-key.js script in browser DevTools to get it. |
| `get_dm_inbox` | List DM conversations with metadata (participants, muted, unread). Returns conversation_id for each. |
| `get_dm_muted_timeline` | Get muted DM conversations |
| `search_dm_all` | Search all DM messages and conversations |
| `search_dm_groups` | Search group DM conversations |
| `search_dm_people` | Search DM conversations with specific people |
| `set_dm_nsfw_media_filter` | Enable or disable NSFW media filter in DMs |
| `unblock_in_dms` | Unblock a user from sending you DMs |
| `update_dm_nsfw_filter` | Enable or disable NSFW media filter for DMs |

## `post_create` — $0.0075/unit

| Tool | Description |
|------|-------------|
| `create_article_draft` | Create a new Twitter Article draft (long-form post) |
| `create_draft_tweet` | Save a tweet as draft (not published) |
| `post_thread` | Post a thread (multiple tweets in sequence) |
| `post_tweet` | Post a new tweet |
| `reply_to_tweet` | Reply to a tweet |
| `schedule_tweet` | Schedule a tweet for future publishing |

## `interaction_create` — $0.0075/unit

| Tool | Description |
|------|-------------|
| `downvote_tweet` | Downvote a reply (community-driven reply ranking) |
| `follow_topic` | Follow a Twitter Topic (so its tweets appear in your home feed) |
| `follow_user` | Follow a Twitter user |
| `generate_drm_token` | Generate a DRM token for a specific media (for video playback protection) |
| `join_audio_space` | Join an audio Space (live or scheduled) |
| `join_community` | Join a Twitter community |
| `like_tweet` | Like a tweet |
| `pin_reply` | Pin a reply to the top of a conversation (only the author of the parent tweet can do this) |
| `pin_timeline` | Pin a list/timeline to the home sidebar |
| `pin_tweet` | Pin a tweet to the authenticated user's profile |
| `rate_birdwatch_note` | Rate a Birdwatch / Community Note as helpful or not helpful |
| `retweet` | Retweet a tweet |
| `subscribe_to_scheduled_space` | Subscribe to notifications for a scheduled Space |
| `unsubscribe_from_scheduled_space` | Unsubscribe from scheduled Space notifications |
| `update_pinned_timelines` | Reorder the pinned timelines (drag-to-reorder) |

## `interaction_delete` — $0.005/unit

| Tool | Description |
|------|-------------|
| `delete_all_bookmarks` | Delete ALL bookmarks (permanently clears the default folder) |
| `delete_article` | Delete an article (draft or published) |
| `delete_birdwatch_note` | Delete a Birdwatch / Community Note you created |
| `delete_birdwatch_rating` | Delete a Birdwatch rating you previously made (remove your helpful/not-helpful vote) |
| `delete_content_disclosure` | Remove a content disclosure from a tweet |
| `delete_draft_tweet` | Delete a saved draft tweet |
| `delete_highlight` | Delete a Twitter Highlight |
| `delete_payment_method` | Delete a saved payment method |
| `delete_scheduled_tweet` | Delete a scheduled tweet (won't publish) |
| `delete_tweet` | Delete a tweet by ID |
| `leave_community` | Leave a Twitter community |
| `remove_follower` | Remove a follower (they'll no longer follow you, but they can re-follow) |
| `remove_profile_banner` | Remove the authenticated user's profile banner |
| `remove_tweet_from_bookmark_folder` | Remove a tweet from a specific folder (doesn't unbookmark it) |
| `unbookmark_tweet` | Remove a tweet bookmark |
| `undo_downvote_tweet` | Remove a downvote from a reply |
| `undo_topic_not_interested` | Undo 'Not Interested' on a topic |
| `unfollow_topic` | Unfollow a Twitter Topic |
| `unfollow_user` | Unfollow a Twitter user |
| `unlike_tweet` | Unlike a tweet |
| `unpin_reply` | Unpin a reply from the top of a conversation |
| `unpin_timeline` | Unpin a list/timeline from the home sidebar |
| `unpin_tweet` | Unpin a tweet from the authenticated user's profile |
| `unretweet` | Remove a retweet |

## `bookmark_create` — $0.0025/unit

| Tool | Description |
|------|-------------|
| `add_tweet_to_bookmark_folder` | Move/copy a tweet into a specific bookmark folder |
| `bookmark_tweet` | Bookmark a tweet |

## `dm_create` — $0.0075/unit

| Tool | Description |
|------|-------------|
| `dm_block_user` | Block a user from sending you DMs |
| `dm_unblock_user` | Unblock a user from sending you DMs |
| `send_dm` | Send a direct message to a user |

## `content_create` — $0.005/unit

| Tool | Description |
|------|-------------|
| `update_profile_banner` | Update the authenticated user's profile banner. Accepts an absolute file path to an image. |
| `update_profile_image` | Update the authenticated user's profile image (avatar). Accepts an absolute file path to an image. |
| `upload_media` | Upload media (image/video) for use in tweets. Returns a mediaId to pass to post_tweet or reply_to_tweet. |

## `list_create` — $0.005/unit

| Tool | Description |
|------|-------------|
| `create_community` | Create a new Twitter community |
| `create_list` | Create a new Twitter list |

## `list_manage` — $0.0025/unit

| Tool | Description |
|------|-------------|
| `add_list_member` | Add a user to a list (by numeric userId) |
| `create_bookmark_folder` | Create a new bookmark folder |
| `delete_bookmark_folder` | Delete a bookmark folder (tweets inside remain in default bookmarks) |
| `delete_list` | Delete a Twitter list |
| `delete_list_banner` | Remove a list's banner image |
| `remove_list_member` | Remove a user from a list |
| `set_list_banner` | Set a banner image for a list (upload media first to get mediaId) |
| `subscribe_list` | Subscribe (follow) a list — its tweets appear in your home feed |
| `unsubscribe_list` | Unsubscribe (unfollow) a list |
| `update_list` | Update list metadata (name, description, privacy) |

## `privacy_update` — $0.005/unit

| Tool | Description |
|------|-------------|
| `update_profile` | Update the authenticated user's Twitter profile bio/description |

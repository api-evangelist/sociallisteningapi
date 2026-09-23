---
name: sociallisteningapi-reddit-thread-research
description: Find Reddit posts on a topic and read the comments on the most relevant ones.
api: sociallisteningapi:sociallisteningapi
operations:
- searchRedditPosts
- searchRedditComments
- getRedditPostComments
---

# Reddit thread research

## Steps
1. Find posts with `searchRedditPosts` (`query`, `exact`, `cursor`) - 1 credit.
2. Optionally search comments directly with `searchRedditComments` - 2 credits per call.
3. For each chosen post, read its comments with `getRedditPostComments` (`url` of the post, `cursor`).
4. Use `ext.subreddit` and `ext.permalink` on each item for grouping and citation.

## Rules
- Page with `pagination.next_cursor` / `has_more`.
- Back off on 429 using `Retry-After`.

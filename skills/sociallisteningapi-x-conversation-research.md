---
name: sociallisteningapi-x-conversation-research
description: Profile an X account and read its posts, a full tweet thread and the replies to a tweet.
api: sociallisteningapi:sociallisteningapi
operations:
- getXUser
- getXUserPosts
- getXTweetThread
- getXTweetReplies
---

# X conversation research

## Steps
1. Look up the account with `getXUser` (`GET /api/v1/x/user`, `username`).
2. Read its recent posts with `getXUserPosts` (`GET /api/v1/x/user/posts`, `username`, `cursor`).
3. For a post of interest, read the whole conversation with `getXTweetThread` (`tweet_id`).
4. Read the replies with `getXTweetReplies` (`tweet_id`, `cursor`), paging while `pagination.has_more` is true.
5. Set `raw=true` only when the original provider payload is needed; it is returned in `raw`.

## Rules
- Each successful call costs 1 credit; errors cost nothing.
- Errors share one envelope: `{success:false, error:{type,message,status}, request_id}`.

---
name: sociallisteningapi-cross-platform-mention-search
description: Search public mentions of a brand, product or phrase across several social sources with one SocialListeningAPI key and merge the normalized results.
api: sociallisteningapi:sociallisteningapi
operations:
- searchRedditPosts
- searchX
- searchLinkedin
- searchHackernews
- searchYoutube
- searchTiktok
- searchFacebook
- searchGoogle
---

# Cross-platform mention search

Every source returns the same normalized item (`id, platform, url, content, author, engagement, published_at, ext`) under `data.items`, so results can be merged without per-source parsing.

## Steps
1. Pick sources and budget credits first: `searchFacebook` costs 7 credits, `searchLinkedin` costs 2, the others cost 1 per successful call (read `x-credit-cost` in the OpenAPI). Empty results still use credits; failed calls do not.
2. Call each search with `query` (required). `exact` defaults to true and wraps the query in double quotes; set `exact=false` to send it unchanged. Use GET with query parameters or POST with a JSON body.
3. Page with `pagination.next_cursor` while `pagination.has_more` is true, passing it as `cursor`.
4. Merge on `url`, sort by `published_at`, and keep `request_id` per call for support.
5. Watch `credits_remaining` in each response; a 402 `INSUFFICIENT_CREDITS` means the workspace is out.

## Rules
- Auth: `x-api-key` header (keys start `slapi_`); keep it server-side.
- 429 `RATE_LIMITED`: back off using `Retry-After` / `X-RateLimit-Reset` (the contract cites 120 requests per minute per key).
- The API is read-only; there is nothing to undo and no idempotency key.

---
name: Track Kallyope company news and congress activity
description: Poll Kallyope's public content API for new press releases, new congress presentations and changes to site pages, without scraping the rendered site.
api: openapi/kallyope-content-api-openapi.yml
operations:
  - listPosts
  - getPost
  - listDocuments
  - listEvents
  - search
  - listPages
---

# Track Kallyope company news and congress activity

Kallyope has no changelog, no RSS-only constraint and no webhook surface. The only
change signal available to a consumer is the `date` / `modified` fields on records in
the public WordPress REST API, plus the date-window filters the router declares.

**Base URL:** `https://kallyope.com/wp-json`
**Auth:** none.

## Steps

1. **Establish a watermark.** Record the current UTC timestamp before your first poll.
   There is no cursor, no sequence number and no delivery guarantee — the watermark is
   yours to keep.

2. **Poll press releases.** Call `listPosts` with a date window:
   `GET /wp/v2/posts?after=<ISO8601>&orderby=date&order=desc&per_page=100`.
   Use `after` for newly published items and `modified_after` to catch corrections to
   items you have already seen. As of 2026-08-01 the whole corpus is 6 posts, so one
   page is normally enough — still read `X-WP-Total` rather than assuming.

3. **Poll scientific output.** Call `listDocuments` the same way:
   `GET /wp/v2/document?after=<ISO8601>&orderby=date&order=desc`. New congress posters
   and presentations land here, not in `posts`. Resolve the `event` term IDs with
   `listEvents` to see which congress a new document came from.

4. **Fetch the item.** `getPost` (`GET /wp/v2/posts/{id}`) returns the full record.
   `content.rendered` is HTML — strip it. `yoast_head_json.description` is a clean
   one-line summary if you only need the gist.

5. **Detect page changes.** Call `listPages` with
   `?modified_after=<ISO8601>&_fields=id,slug,link,modified` to see whether the pipeline,
   platform or careers page was edited. This is how you notice a pipeline advance
   without diffing rendered HTML.

6. **Ad-hoc lookup.** `search` (`GET /wp/v2/search?search=<term>`) spans posts, pages and
   documents and returns lightweight `{id, title, url, type, subtype}` records — use it
   to resolve a title to an ID before fetching.

## Rules

- **Set the poll interval to a day, not a minute.** Responses are cached
  `public, max-age=86400` at the Fastly edge. Polling more often than daily returns the
  same cached body and buys nothing.
- **Advance the watermark from the response, not the clock.** Because of the edge cache,
  set your next `after` value to the newest `date_gmt` you actually received.
- **No rate limit is published, and none was observed.** Do not treat that as permission —
  keep concurrency at 1 and stay on the daily cadence.
- **`per_page` is bounded 1–100.** Out-of-range values return `400 rest_invalid_param`.
- **Errors carry the status in `data.status`,** not as an RFC 9457 problem document.
- **There is no webhook or event stream.** If you need push, you are building polling —
  say so rather than implying Kallyope offers subscriptions.

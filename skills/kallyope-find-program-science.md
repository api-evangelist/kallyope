---
name: Find the published science behind a Kallyope drug program
description: Resolve a Kallyope drug program (e.g. elismetrep) to its scientific posters, presentations and publications, and pull the source PDF for each.
api: openapi/kallyope-content-api-openapi.yml
operations:
  - listPrograms
  - listDocuments
  - getDocument
  - getMediaItem
  - listContentTypes
  - listEvents
---

# Find the published science behind a Kallyope drug program

Kallyope publishes its congress posters, presentations and publications as a first-party
`document` content type on its public WordPress REST API, classified by a `program`
taxonomy that names the drug program. This is the only structured route from a compound
name to the science behind it.

**Base URL:** `https://kallyope.com/wp-json`
**Auth:** none. Every operation below is anonymously readable — do not send credentials.

## Steps

1. **Resolve the program to a term ID.** Call `listPrograms`
   (`GET /wp/v2/program`). Match the program by `slug` or `name` — as of 2026-08-01 the
   only term is `elismetrep` (id 51). The term's `count` field tells you how many
   documents are attached before you fetch any.

2. **List that program's documents.** Call `listDocuments` with the term ID:
   `GET /wp/v2/document?program=51&per_page=100&orderby=date&order=desc`.
   Read `X-WP-Total` from the response headers to confirm you have the whole set — do not
   assume one page. If `X-WP-TotalPages` is greater than 1, walk `page` until exhausted,
   or follow the `Link` header's `rel="next"`.

3. **Interpret the classification.** Each document carries term-ID arrays for
   `content-type` (poster / presentation / publication / video) and `event` (the
   congress). Resolve those with `listContentTypes` (`GET /wp/v2/content-type`) and
   `listEvents` (`GET /wp/v2/event`) once, then join locally — the API has no expansion
   parameter, so a second lookup per document is wasted work.

4. **Fetch the full record when you need the body.** Call `getDocument`
   (`GET /wp/v2/document/{id}`). `title.rendered`, `content.rendered` and
   `excerpt.rendered` contain **rendered HTML** — strip markup before returning text.

5. **Get the source asset.** `featured_media` is an attachment ID. Call `getMediaItem`
   (`GET /wp/v2/media/{id}`) and read `source_url` for the poster or slide PDF.

## Rules

- **Trim the payload.** Every record carries a large `yoast_head` SEO blob. Use
  `_fields=id,slug,title,excerpt,date,link,program,event,content-type,featured_media`
  to keep responses small.
- **Respect the page bound.** `per_page` is validated at 1–100 inclusive; anything else
  returns `400 rest_invalid_param` with the constraint in `data.details.per_page.message`.
- **Expect stale data.** Responses are served `Cache-Control: public, max-age=86400`
  behind Fastly. A document published today may not appear for up to 24 hours. There is
  no ETag, so conditional GET is not available.
- **Errors are not RFC 9457.** A missing ID returns
  `{"code":"rest_post_invalid_id","message":"Invalid post ID.","data":{"status":404}}`.
  Read `data.status`, not a `status` member, and do not try to dereference a `type` URI —
  there is none. See `errors/kallyope-problem-types.yml`.
- **Do not attempt writes.** POST/PUT/PATCH/DELETE are registered but capability-gated;
  anonymous attempts return `401 rest_forbidden`.
- **Cite the human page.** Every record's `link` field is the canonical kallyope.com URL —
  use it when attributing, not the REST URL.

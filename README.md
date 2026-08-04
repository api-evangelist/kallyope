# Kallyope

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Kallyope, Inc. is a New York City clinical-stage biotechnology company, founded in 2015 by
Columbia University scientists Charles Zuker, Tom Maniatis and Richard Axel, that
translates the biology of the gut-brain axis into medicines. Its proprietary Klarity
platform integrates single-cell sequencing, pathway circuit mapping, optogenetics and
chemogenetics, human genetics, organoid systems and small-molecule and peptide chemistry
to map the neural circuits underlying migraine and metabolism. Lead candidate elismetrep,
a TRPM8 blocker, is in Phase 3 development for acute migraine.

- https://kallyope.com/
- https://www.linkedin.com/company/kallyope/
- https://forgeglobal.com/kallyope_stock/

## API surface

Kallyope publishes **no** developer portal, API reference, SDKs, CLI, sandbox, changelog,
status page or public GitHub organization, and no OpenAPI, AsyncAPI, GraphQL, MCP server
or A2A agent card was found on any host.

Enrichment probing on 2026-08-01 did find one real, anonymous, read-only REST surface:
kallyope.com runs WordPress and exposes the WordPress REST API publicly at
`https://kallyope.com/wp-json` — 326 routes across 14 namespaces. Alongside the core
`posts`, `pages`, `media` and `search` collections, Kallyope has registered a first-party
`document` content type (scientific posters, presentations, publications and video)
classified by four first-party taxonomies: `program` (drug programs, e.g. elismetrep),
`event` (scientific congresses), `content-type` and `document-type`.

This is a content and marketing API, not a Klarity platform, research-data or clinical
API, and it exists as a byproduct of the CMS rather than as a designed public contract.

## Artifacts

| Path | What |
|---|---|
| `openapi/kallyope-content-api-openapi.yml` | OpenAPI 3.1, **derived** by API Evangelist from the live route index and observed responses — not published by Kallyope |
| `overlays/kallyope-content-api-overlay.yaml` | Our enhancements over that description |
| `authentication/kallyope-authentication.yml` | Anonymous reads; application-password and cookie/nonce write paths |
| `conventions/kallyope-conventions.yml` | Pagination, filtering, caching, error envelope; no idempotency, no rate-limit signal |
| `errors/kallyope-problem-types.yml` | Observed 400/401/404 responses; WordPress envelope, not RFC 9457 |
| `data-model/kallyope-data-model.yml` | Entity graph from the registered types and taxonomies |
| `lifecycle/kallyope-lifecycle.yml` | Versioning; no SLA, deprecation policy or status page. Flags a dangling `docs.kallyope.com` DNS record |
| `conformance/kallyope-conformance.yml` | Standards conformance; no published compliance program |
| `security/kallyope-domain-security.yml` | TLS/HSTS/DNSSEC/CAA/SPF/DMARC probe results |
| `well-known/kallyope-well-known.yml` | Every `/.well-known/` path probed — all 404 |
| `json-ld/kallyope-organization.jsonld` | schema.org graph captured verbatim from the homepage |
| `mcp/` | Candidate tool set derived from the OpenAPI, plus the tool crosswalk. Kallyope publishes no MCP server |
| `skills/` | Two generated agent skills grounded in real operationIds |
| `llms/kallyope-llms.txt` | Generated; Kallyope publishes no `llms.txt` |

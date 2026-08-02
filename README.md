# Kallyope

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

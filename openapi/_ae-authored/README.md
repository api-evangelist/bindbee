# Written by API Evangelist, not harvested from the provider

These documents were in `openapi/_original/`, which is the verbatim record of what the PROVIDER
published. They are not that. Each carries an explicit authorship marker — `x-derived-from`,
`x-generated-from: documentation`, or an AE-authored `x-method` — saying we built it.

The Kin Score credits the presence of `_original/` as evidence the provider published a contract,
and grades a marked document as `derived` separately. Leaving these where they were meant the same
file was discounted once and credited once.

Moved, not deleted: they describe real APIs and the pipeline reads them. The only thing wrong was
the claim their location made about who wrote them.

Moved 2026-08-29, roadmap#2 item 4 / roadmap#48.

## 2026-09-04 addition

`bindbee-candidates-api-openapi.yml`, `bindbee-departments-api-openapi.yml`,
`bindbee-employees-api-openapi.yml`, `bindbee-jobs-api-openapi.yml` and
`bindbee-time-off-api-openapi.yml` were moved here by the enrichment pipeline after the REAL
Bindbee contract was found live at `https://api.bindbee.dev/openapi.json` (OpenAPI 3.1.0,
119 paths / 144 operations). Those five carried `x-generated-from: documentation` and described
paths that do not exist (`/ats/candidates` — the real path is `/api/ats/v1/candidates`) with
operationIds the provider never published. They are superseded and must not be scored as the
provider's contract.

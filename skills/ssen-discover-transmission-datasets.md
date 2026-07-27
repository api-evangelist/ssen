---
name: Discover SSEN Transmission datasets
description: Search the SSEN Transmission open data catalog with ODSQL, read a dataset's field schema, and enumerate facets before pulling any data.
api: openapi/ssen-transmission-opendatasoft-explore-v2.1-openapi.json
operations: [getDatasets, getDataset, getDatasetsFacets, getRecordsFacets]
generated: '2026-07-27'
method: generated
---

# Discover SSEN Transmission datasets

Use this before any records call. The SSEN Transmission portal publishes 60 CC BY 4.0
datasets covering Electricity Ten Year Statement circuits and fault levels, ground
investigation points and related transmission network records. Records are **schemaless
in the spec** — you cannot type a row until you have read its parent dataset's `fields`.

**Base URL:** `https://ssentransmission.opendatasoft.com/api/explore/v2.1`
**Auth:** none required. An optional `apikey` query parameter exists for private datasets;
all 60 SSEN Transmission datasets are public.

## Steps

1. **Search the catalog — `getDatasets`**
   `GET /catalog/datasets`
   Filter with ODSQL: `where`, `refine`, `exclude`, `select`, `order_by`, `group_by`,
   plus `limit` / `offset` for paging. Read `total_count` and iterate `results[]`.
   Each result carries `dataset_id` (a slug, never numeric) and a `metas` block with
   title, theme, licence and modified date.

2. **Narrow with facets — `getDatasetsFacets`**
   `GET /catalog/facets`
   Returns facet dimensions and value counts across the whole catalog. Use it to
   discover the theme/publisher/keyword vocabulary instead of guessing `where` clauses.

3. **Read the dataset contract — `getDataset`**
   `GET /catalog/datasets/{dataset_id}`
   This is the mandatory step. Read:
   - `fields[]` — the record schema for this dataset. **Type your records from here.**
   - `has_records` — whether a records query will return anything.
   - `data_visible` — whether the data is readable anonymously.
   - `features[]` — which capabilities (analyze, geo, export …) the dataset supports.
   - `attachments[]` — companion files (PDF, XLSX) shipped with the dataset.

4. **Check value distribution — `getRecordsFacets`**
   `GET /catalog/datasets/{dataset_id}/facets`
   Enumerates facet values and counts *within* one dataset. Cheaper than pulling
   records to find out what values exist.

5. **Follow `_links` rather than building paths.**
   Every response carries `_links[]` with `rel` in
   `self | first | last | next | dataset | catalog`. Page by following `rel: next`.

## Rules

- **Rate limits are real and low.** The anonymous allowance observed on this portal was
  **100 calls per calendar day** (`X-RateLimit-Limit: 100`). Read `X-RateLimit-Remaining`
  on every response and stop before it hits zero; `X-RateLimit-Reset` is an absolute UTC
  midnight timestamp. See `rate-limits/ssen-rate-limits.yml`.
- **Budget your calls.** Because of the daily cap, resolve the dataset with one
  `getDatasets` call and one `getDataset` call, then go straight to exports. Do not poll.
- **Errors are not RFC 9457.** 4xx bodies are `{error_code, message}`. Expect
  `ODSQLSyntaxError` (400) for a bad clause and `NotFoundResource` (404) for an unknown
  `dataset_id` — note that 404 is *not* declared in the spec but the live API returns it.
  See `errors/ssen-problem-types.yml`.
- **Read-only.** Every operation is a `GET`. There is no write surface, so there is no
  idempotency key to send.
- **Attribution is a licence condition.** Data is CC BY 4.0 — credit SSEN Transmission,
  name the dataset and link the source. See <https://data.ssen.co.uk/terms-and-conditions>.

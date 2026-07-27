---
name: Pull SSEN Transmission records in bulk
description: Query a transmission dataset's records with ODSQL, then export the full result set as CSV, Parquet, GPX or DCAT without hitting the per-call record cap.
api: openapi/ssen-transmission-opendatasoft-explore-v2.1-openapi.json
operations: [getRecords, getRecord, listDatasetExportFormats, exportRecords, exportRecordsCSV, exportRecordsParquet, exportRecordsGPX]
generated: '2026-07-27'
method: generated
---

# Pull SSEN Transmission records in bulk

The Explore API caps how many records a `records` call returns, but the spec states
explicitly that the **`exports` endpoints have no such limitation**. The correct pattern
is therefore: shape and validate the query on `records`, then run the identical ODSQL
against `exports`.

**Base URL:** `https://ssentransmission.opendatasoft.com/api/explore/v2.1`
**Prerequisite:** you already know the `dataset_id` and have read its `fields[]` — see
the *Discover SSEN Transmission datasets* skill.

## Steps

1. **Shape the query — `getRecords`**
   `GET /catalog/datasets/{dataset_id}/records`
   Use a small `limit` (e.g. 5) while you iterate on the ODSQL. Available clauses:
   `select`, `where`, `group_by`, `order_by`, `refine`, `exclude`, `limit`, `offset`,
   `lang`, `timezone`. Read `total_count` to size the eventual export.
   System fields on each record: `_id`, `_timestamp`, `_size`, `_links`. Everything
   else is dataset-specific and described by `getDataset`'s `fields[]`.

2. **Spot-check one row — `getRecord`**
   `GET /catalog/datasets/{dataset_id}/records/{record_id}`
   Pass a `_id` returned in step 1. Use this to confirm field names and value shapes
   before committing to a large export.

3. **Check what the dataset can emit — `listDatasetExportFormats`**
   `GET /catalog/datasets/{dataset_id}/exports`
   Not every dataset supports every format — geo formats only appear on geo datasets.

4. **Export the whole result set.** Pick the operation that matches your target:
   - `exportRecords` — `GET /catalog/datasets/{dataset_id}/exports/{format}`
     for any supported format (`csv`, `fgb`, `geojson`, `gpx`, `json`, `jsonl`,
     `jsonld`, `kml`, `n3`, `ov2`, `parquet`, `rdfxml`, `shp`, `turtle`, `xlsx`).
   - `exportRecordsCSV` — `GET /catalog/datasets/{dataset_id}/exports/csv`
   - `exportRecordsParquet` — `GET /catalog/datasets/{dataset_id}/exports/parquet`
     (use this for analytics — columnar, typed, and the cheapest full pull).
   - `exportRecordsGPX` — `GET /catalog/datasets/{dataset_id}/exports/gpx`
     (geo datasets such as ground investigation points).
   Carry the same `where` / `select` / `refine` clauses you validated in step 1.

## Rules

- **One export beats a thousand pages.** With a 100-call daily anonymous allowance,
  paging a large dataset through `getRecords` will exhaust your quota. Export instead.
- **Rate-limit headers are authoritative.** `X-RateLimit-Limit`, `X-RateLimit-Remaining`,
  `X-RateLimit-Reset`, plus per-dataset counters `X-RateLimit-dataset-*`. On 429 the body
  is `{errorcode, error, call_limit, limit_time_unit, reset_time}` — wait for
  `reset_time`, do not retry immediately. See `rate-limits/ssen-rate-limits.yml`.
- **Malformed ODSQL fails fast.** A 400 body names the offending clause and character
  position (`ODSQLSyntaxError`). Fix the clause it names rather than re-sending.
- **Records are open-schema.** Never hard-code a business field name without reading
  `getDataset`'s `fields[]` first — see `data-model/ssen-data-model.yml`.
- **All GET, no writes.** No idempotency contract exists because nothing mutates.
- **CC BY 4.0** — attribute SSEN Transmission on anything derived from an export.

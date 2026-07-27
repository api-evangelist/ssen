---
name: Harvest the SSEN catalog as DCAT
description: Pull the SSEN Transmission catalog as DCAT / DCAT-AP for a data-discovery index, and cross-reference the SSEN Distribution CKAN catalog's DCAT serialisations.
api: openapi/ssen-transmission-opendatasoft-explore-v2.1-openapi.json
operations: [listExportFormats, exportCatalogDCAT, exportCatalogCSV, exportDatasets, getDatasets]
generated: '2026-07-27'
method: generated
---

# Harvest the SSEN catalog as DCAT

SSEN runs two open data portals on two different platforms, and DCAT is the one thing
both speak. Use this skill to build a single cross-portal index of SSEN's published data
without scraping either front end.

**Transmission base URL:** `https://ssentransmission.opendatasoft.com/api/explore/v2.1`
**Distribution base URL:** `https://data-api.ssen.co.uk/api/3/action`

## Steps

1. **Check available catalog formats — `listExportFormats`**
   `GET /catalog/exports`
   Confirms which serialisations the Transmission catalog can emit today.

2. **Export the Transmission catalog as DCAT — `exportCatalogDCAT`**
   `GET /catalog/exports/dcat{dcat_ap_format}`
   The path segment selects the DCAT-AP profile. This is the harvester-friendly form —
   one call for all 60 datasets, and it counts as one call against your daily quota.

3. **Or take the catalog as a table — `exportCatalogCSV` / `exportDatasets`**
   `GET /catalog/exports/csv` for a flat table, or
   `GET /catalog/exports/{format}` (`exportDatasets`) for any other supported format.

4. **Enumerate rather than page — `getDatasets`**
   `GET /catalog/datasets` only when you need ODSQL filtering the export cannot express.
   Read `total_count` first so you know whether a full export is cheaper.

5. **Add the Distribution side.** The SSEN Distribution portal is CKAN 2.10.10 with the
   `dcat` and `dcat_json_interface` extensions loaded. Its DCAT serialisations are served
   at `https://ckan-prod.sse.datopian.com/catalog.jsonld`, `.rdf` and `.ttl`. For the
   native CKAN route call `/api/3/action/package_list` (45 packages) then
   `/api/3/action/package_show?id=<name>`.
   **Send a normal browser `User-Agent`** — `data-api.ssen.co.uk` returns HTTP 403 to a
   default curl agent. That is WAF bot filtering, not authentication.

6. **Reconcile.** The two catalogs do not overlap: Transmission covers the north of
   Scotland high-voltage network; Distribution covers the SHEPD and SEPD licence areas.
   Key on the dataset slug per portal, not on title.

## Rules

- **CKAN envelopes differ from Explore envelopes.** CKAN returns
  `{help, success, result}` on success and `{help, success:false, error:{__type, message}}`
  on failure. Explore returns bare JSON with `{error_code, message}` on 4xx. Handle both.
  See `errors/ssen-problem-types.yml` and `conventions/ssen-conventions.yml`.
- **Budget the Transmission quota** — 100 anonymous calls per calendar day. A DCAT export
  is one call; paging the catalog is many.
- **Licence.** Most datasets on both portals are CC BY 4.0. Carry `license_id` /
  `metas.license` through into your index and attribute on republication.
- **Discovery crosses APIs.** The CKAN Real Time Outage Dataset package carries the Power
  Track outage API endpoint as a resource, and the NeRDA API Guide PDF is a CKAN resource
  too — the open catalog is how SSEN's two bespoke APIs are found at all.

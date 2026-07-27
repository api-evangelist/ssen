# Scottish and Southern Electricity Networks (ssen)

Scottish and Southern Electricity Networks (SSEN) is the SSE plc electricity networks business in the United Kingdom, operating the poles-and-wires layer rather than selling energy. SSEN Distribution is the licensed Distribution Network Operator for two GB licence areas — Scottish Hydro Electric Power Distribution (SHEPD) in the north of Scotland and Southern Electric Power Distribution (SEPD) in central southern England — serving over 3.9 million homes and businesses, while SSEN Transmission owns and operates the high-voltage transmission system for the north of Scotland. Its API posture is the classic network-distributor split — grid and market data are genuinely open, consumer data is not offered at all.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/ssen/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/ssen/refs/heads/main/apis.yml)

## Tags

- Energy
- United Kingdom
- Utilities
- Electricity
- Grid
- Distribution Network Operator
- Transmission
- Smart Metering
- Open Data
- Flexibility
- Renewables
- DER

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### SSEN Distribution Data Portal API

The public CKAN 2.10.10 Action API behind the SSEN Distribution Data Portal, serving 45 open datasets covering substations, LV feeder smart meter half-hourly usage, the Embedded Capacity Register, flexibility market registers, Distribution Future Energy Scenarios, licence area boundaries and Long Term Development Statements. Verified anonymously — no key, no registration. Most datasets carry a CC BY 4.0 licence.

- **Human URL:** [https://data.ssen.co.uk/](https://data.ssen.co.uk/)
- **Base URL:** `https://data-api.ssen.co.uk/api/3/action`

#### Tags

- Open Data
- CKAN
- Catalog
- Electricity
- Grid

#### Properties

- [Documentation](https://data.ssen.co.uk/)
- [API Reference](https://data-api.ssen.co.uk/api/3/action/help_show?name=package_search)
- [Website](https://www.ssen.co.uk/)

### SSEN Power Track Real Time Outage API

Anonymous JSON API behind the SSEN Power Track map, returning planned and unplanned outages on the SSEN Distribution network with fault reference, type, latitude/longitude, estimated restoration time and affected postcodes, plus a per-fault lookup by reference. Published as an API resource on the Real Time Outage Dataset under CC BY 4.0 and verified live with no credentials.

- **Human URL:** [https://data.ssen.co.uk/@ssen-distribution/realtime_outage_dataset](https://data.ssen.co.uk/@ssen-distribution/realtime_outage_dataset)
- **Base URL:** `https://external.distribution.prd.ssen.co.uk/opendataportal-prd/v4/api`

#### Tags

- Outages
- Real Time
- Open Data
- Electricity

#### Properties

- [Documentation](https://data.ssen.co.uk/@ssen-distribution/realtime_outage_dataset)
- [Documentation](https://powertrack.ssen.co.uk/powertrack)

### SSEN NeRDA (Near Real-time Data Access) API

Near real-time power flow data from SSEN Distribution's EHV, HV and LV networks, drawn from SCADA PowerOn, LV monitoring equipment, the load model forecasting tool, the connectivity model and the Long Term Development Statement. The documented endpoints are a static substation endpoint per licence area (`nerdastatic-SEPD`, `nerdastatic-SHEPD`) and time-series endpoints (`nerdart_after`, `nerdart_between`) queried by `nerda_measurement_id` and ISO 8601 timestamps. Access requires a free SSEN account, acceptance of the portal terms and conditions, and an API key generated in the portal API console; anonymous calls to the documented endpoints return 404.

- **Human URL:** [https://www.ssen.co.uk/our-services/tools-and-maps/near-real-time-data-access-nerda-portal/](https://www.ssen.co.uk/our-services/tools-and-maps/near-real-time-data-access-nerda-portal/)
- **Base URL:** `https://nerda.opengrid.com/api`

#### Tags

- Real Time
- Grid
- Power Flow
- Substations
- Flexibility

#### Properties

- [Documentation](https://www.ssen.co.uk/our-services/tools-and-maps/near-real-time-data-access-nerda-portal/)
- [API Reference](https://data-api.ssen.co.uk/dataset/195500da-46ae-4698-939c-f7f4293f7b43/resource/c90db7e1-c666-4092-998c-b71d782b16db/download/nerda-api-guide.pdf)
- [Documentation](https://data.ssen.co.uk/@ssen-distribution/nerda_opengrid_dashboard)
- [Portal](https://nerda.ssen.co.uk/)

### SSEN Transmission Open Data Explore API

The Opendatasoft Explore API v2.1 served from the SSEN Transmission Open Data Portal, exposing 60 CC BY 4.0 transmission datasets — Electricity Ten Year Statement circuits and fault levels, ground investigation points and related network records — through 16 read-only GET endpoints for catalog search, dataset records, facets and exports (CSV, Parquet, GPX, DCAT). Verified anonymously; an optional `apikey` query parameter exists for private datasets.

- **Human URL:** [https://ssentransmission.opendatasoft.com/](https://ssentransmission.opendatasoft.com/)
- **Base URL:** `https://ssentransmission.opendatasoft.com/api/explore/v2.1`

#### Tags

- Open Data
- Transmission
- Catalog
- Scotland

#### Properties

- [OpenAPI](openapi/ssen-transmission-opendatasoft-explore-v2.1-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://ssentransmission.opendatasoft.com/)
- [API Reference](https://ssentransmission.opendatasoft.com/api-console/explore/v2.1/)
- [Website](https://www.ssen-transmission.co.uk/)

## Common Properties

- [Website](https://www.ssen.co.uk/)
- [Documentation](https://data.ssen.co.uk/)
- [Portal](https://data.ssen.co.uk/)
- [Portal](https://ssentransmission.opendatasoft.com/)
- [Portal](https://nerda.ssen.co.uk/)
- [Website](https://www.ssen-transmission.co.uk/)
- [Website](https://ssen-innovation.co.uk/)
- [LinkedIn](https://www.linkedin.com/company/ssen)

## Regulatory Posture

- **Mandate regime:** smart-meter-infrastructure — Great Britain mandated the infrastructure (the licensed Smart DCC monopoly under the Smart Energy Code) and the disclosure (Ofgem Data Best Practice "Presumed Open" licence condition under RIIO-ED2). There is no GB consumer data right for energy.
- **Mandate status:** live-implemented, for those obligations only. Verified through published Open Data Triage record PDFs attached to SSEN datasets, CC BY 4.0 licensing returned by the CKAN API, and the live Smart Meter LV Feeder Usage dataset aggregated to no fewer than five properties.
- **Data standard:** no consumer data standard reference found. No Green Button/ESPI, no CDR Consumer Data Standards, no IEEE 2030.5, OpenADR or OCPP/OCPI. ENA Open Networks Embedded Capacity Register format and DCAT catalog exports are present.
- **Consumer data API:** none published.
- **Market/grid data open:** yes — 45 Distribution datasets and 60 Transmission datasets, anonymously queryable.
- **Access gate:** self-serve. Three of the four APIs need no credential at all; NeRDA needs a free account, T&C acceptance and a self-generated API key.

## Maintainers

- Kin Lane — kin@apievangelist.com

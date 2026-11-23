# Unified Building Permit Schema — v0 (phase 0)

One row per issued building permit, normalized across city open-data
portals. Field names are stable across cities; source-specific quirks
are documented per field.

| Field | Type | Notes |
|---|---|---|
| `id` | string | `{city_key}-{permit_number}`, unique, stable across refreshes |
| `city` | string | City name (Chicago, Austin, Seattle in the sample) |
| `state` | string | 2-letter state |
| `permit_number` | string | The city's own permit identifier, verbatim |
| `status` | string \| null | Source status, lightly normalized (e.g. `Issued`, `Active`) |
| `permit_type_raw` | string | The city's own permit type label, verbatim — never overwritten |
| `trade` | string | Derived classification: `solar`, `roofing`, `hvac`, `electrical`, `plumbing`, `demolition`, `new_construction`, `renovation`, `signage`, `elevator`, `other`. Keyword rules over type + description; see `collect_permits.py` (`TRADE_RULES`). In the phase-0 sample, 92.4% of records classify into a named trade |
| `applied_date` | string \| null | ISO `YYYY-MM-DD` |
| `issued_date` | string | ISO `YYYY-MM-DD`; every record has one (it is the inclusion criterion) |
| `address` | string | Street address as published (no unit-level enrichment) |
| `zip` | string \| null | Not published per-record by Chicago (64% populated overall) |
| `description` | string | Work description as published by the city |
| `estimated_cost_usd` | number \| null | Declared project value where the city publishes it (64.5% populated; Austin publishes it inconsistently) |
| `contractor_business` | string \| null | Business name of permittee where a source publishes a dedicated business-name field. **Personal names and phone numbers are never included** (see policy below) |
| `latitude`, `longitude` | number \| null | As published by the source (87% populated) |
| `source` | string | Full source dataset name + portal, per record |

## Privacy / content policy
- Business and property data only. Where a source record contains an
  individual's name or phone number (common for NYC permittees/owners
  and Chicago contacts), those fields are **dropped at the adapter
  layer** and never reach the dataset.
- No emails, no personal contact data, no data about individuals'
  homes beyond what the permit record itself states (address + work
  description are the public record).

## Known source quirks (phase-0 findings)
- **Chicago** — no per-record ZIP; fees present but declared cost is in
  `reported_cost`; contact names mix individuals and businesses, so
  they are excluded by policy.
- **Austin** — rich descriptions; valuation field inconsistently
  populated; permits can be issued months after application.
- **Seattle** — cleanest source: real timestamps, cost, ZIP, lat/long.
- **NYC (excluded from phase 0)** — DOB issuance `issuance_date` is a
  TEXT field (`MM/DD/YYYY`); server-side recency filtering is
  lexicographic and unreliable, and `dobrundate` is only the extract
  refresh date. NYC requires a bulk-download pipeline — phase-1 item.

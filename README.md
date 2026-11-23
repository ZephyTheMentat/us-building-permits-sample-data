# Fleet Ops Data — U.S. Building Permits, Normalized

**One clean schema for building permits across U.S. cities** — collected
from official open-data portals, normalized, classified by trade, and
refreshed on a schedule. No portal-hopping, no per-city parsers, no
wrestling five different date formats.

This repository is a **free sample** of the dataset that will power the
Fleet Ops Data permits API.

## What's in the sample

`data/permits_sample.json` and `data/permits_sample.csv` — **3,596
issued building permits** from **Chicago, Austin, and Seattle** (the
most recent ~1,200 per city, issued **2026-09-09 → 2026-11-22**),
plus `data/dataset_stats.json` with full field-completeness stats.

Every record carries: permit number, status, the city's raw permit
type, a derived **trade** classification (roofing, solar, HVAC,
electrical, plumbing, demolition, new construction, renovation, …),
applied/issued dates, address, ZIP where published, work description,
declared project value where published, coordinates where published,
and its source dataset. Full field definitions: see the schema summary
below.

### Example records

```json
{
  "id": "seattle-7173168-RR", "city": "Seattle", "state": "WA",
  "permit_number": "7173168-RR", "status": "Issued",
  "permit_type_raw": "Multifamily", "trade": "roofing",
  "issued_date": "2026-11-20", "address": "2815 2ND AVE", "zip": "98121",
  "description": "THE SCOPE OF WORK INCLUDES REPLACEMENT OF THE EXISTING ROOF MEMBRANE SYSTEM AT THE UPPER MECHANICAL ROOF LEVELS.",
  "estimated_cost_usd": 293000.0
}
```

```json
{
  "id": "austin-2026-060070 BP", "city": "Austin", "state": "TX",
  "permit_number": "2026-060070 BP", "status": "Active",
  "permit_type_raw": "Building Permit", "trade": "new_construction",
  "issued_date": "2026-11-21", "address": "1603 CANTERBURY ST BLDG 2",
  "zip": "78702",
  "description": "ePlan: Residential Expedited Review - New TWO Unit Bldg 2 New construction of a 2-Story Single Family Res. [3bed 2.5bath] with Attached Carport"
}
```

## Who uses permit data

- **Contractors & adjacent trades** — a roofing permit filed today is
  tomorrow's dumpster, solar, HVAC, and materials work.
- **Proptech & real-estate platforms** — permit history per property,
  neighborhood construction activity, market metrics.
- **Suppliers & sales teams** — where building is actually happening,
  filtered by trade and project value.
- **Analysts** — clean, comparable permit series across cities without
  maintaining a parser per portal.

## Schema (summary)

| Field | Meaning |
|---|---|
| `id` | Stable unique ID: `{city}-{permit_number}` |
| `city`, `state` | Jurisdiction |
| `permit_number` | City's own permit ID, verbatim |
| `status` | Permit status as published |
| `permit_type_raw` | City's own type label, verbatim |
| `trade` | Derived: solar / roofing / hvac / electrical / plumbing / demolition / new_construction / renovation / signage / elevator / other |
| `applied_date`, `issued_date` | ISO dates |
| `address`, `zip` | Project address (ZIP not published per-record by Chicago) |
| `description` | Work description as published |
| `estimated_cost_usd` | Declared project value where published (~65% of records) |
| `latitude`, `longitude` | Where published (~87% of records) |
| `source` | Source dataset + portal, per record |

Data policy: business/property data only. Personal names and phone
numbers that appear in some source records are excluded by design.

## Update cadence

The sample is a snapshot. The full dataset is designed for **daily
incremental refresh** per city (new and changed permits only), with
coverage expanding city by city — normalization and freshness are the
product.

## Full API access — coming soon

A hosted API is in preparation: search and filter permits by city,
trade, date range, and project value; per-property permit history;
daily change feed. Planned pricing is in the $19–49/mo range for
developer tiers.

**Interested?** Reply by opening an issue in this repository (or use
the contact on the Fleet Ops Data profile) and tell us: which cities,
which trades, and what you'd build. "Would pay for live access" is the
single most useful sentence you can send us — it directly decides what
gets built first.

## License

The sample dataset in `data/` is free for personal and commercial use
with attribution — see `LICENSE.md`. Source data remains the property
of the publishing cities under their open-data terms; this dataset is
a normalized derivative provided as-is.

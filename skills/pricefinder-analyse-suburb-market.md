---
name: Analyse an Australian suburb market
description: Pull suburb-level market intelligence from Pricefinder — summary, demographics, sales and rental time series, price segmentation, peak selling periods and the flyover report.
api: openapi/pricefinder-api-swagger.json
generated: '2026-07-26'
method: generated
source: openapi/pricefinder-api-swagger.json + lifecycle/pricefinder-lifecycle.yml
operations:
  - suggestSuburbs
  - suburb
  - summary
  - demographics
  - flyover
  - flyoverReport
  - streets
  - peakSellingPeriods
  - priceSegmentSales2
  - timeSeriesSales
  - timeSeriesRentals
---

# Analyse an Australian suburb market

The statistical layer above individual comparables — what Pricefinder sells to
advisory firms, developers and lenders as market intelligence.

Authenticate first — see `pricefinder-authenticate-and-check-entitlements`.

## Step 1 — resolve the suburb

`suggestSuburbs` — `GET /suggest/suburbs` — turns a suburb name into a `suburbId`.
Australian suburb names repeat across states (there is a Richmond in VIC, NSW, QLD, SA
and TAS), so check `state` and `postcode` on the `SuburbIdentifier` before proceeding.

## Step 2 — the baseline

- `suburb` — `GET /suburbs/{suburbId}` → `Suburb`: id, `suburbName`, `postcode`,
  `state`.
- `summary` — `GET /suburbs/{suburbId}/summary` → `SuburbSummary` /
  `SuburbStatisticsSummary`: the headline market position.
- `demographics` — `GET /suburbs/{suburbId}/demographics`: population and household
  composition.
- `streets` — `GET /suburbs/{suburbId}/streets` → the street list, each with a
  `streetId` you can take into the street-scoped comparables operations.

## Step 3 — the time series

- `timeSeriesSales` — `GET /suburbs/{suburbId}/stats/timeseries/sales/{propertyType}`
- `timeSeriesRentals` — `GET /suburbs/{suburbId}/stats/timeseries/rentals/{propertyType}`

`{propertyType}` is a path parameter, so you get one series per property type — house,
unit and so on. Run them separately and do not blend the series; a suburb's house
market and unit market frequently move in opposite directions.

**`timeSeriesSales` has two deprecated parameters**, `year_from` and `year_to`, flagged
through the non-standard `pds` vendor object. Use `date_start` / `date_end` instead.

## Step 4 — price segmentation

- `priceSegmentSales2` —
  `GET /suburbs/{suburbId}/stats/timeseries/pricesegmentation/sales/{propertyType}`

**Use `priceSegmentSales2`, not `priceSegmentSales`.** The older
`GET /suburbs/{suburbId}/stats/pricesegmentation/sales/{propertyType}` operation
(`priceSegmentSales`) carries a real Swagger `deprecated: true` flag. No sunset date is
published, but it is the only operation-level deprecation in the statistics surface and
it has a direct time-series replacement.

## Step 5 — seasonality and the report

- `peakSellingPeriods` —
  `GET /suburbs/{suburbId}/stats/peaksellingperiods/{propertyType}`: when this suburb
  actually transacts.
- `flyover` — `GET /suburbs/{suburbId}/flyover` → `SuburbFlyover`: the packaged suburb
  overview as JSON.
- `flyoverReport` — `GET /suburbs/{suburbId}/flyover/pdf`: the same overview rendered
  as a PDF (`application/pdf`) — the client-facing artifact.

## Step 6 — drill into the records

The suburb also scopes the four collection operations. Bind by path, because these
operationIds collide with the postcode, spatial and radial families:

- `GET /suburbs/{suburbId}/sales` — `sales`
- `GET /suburbs/{suburbId}/rentals` — `rentals`
- `GET /suburbs/{suburbId}/listings` — `listings`
- `GET /suburbs/{suburbId}/properties` — `properties`

These are capped by `limit` with **no pagination**. For a whole-suburb sweep, partition
by `date_start`/`date_end` and `property_type` and union client-side, de-duplicating on
record id. See `pricefinder-research-comparable-sales`.

## Reading the results correctly

- **Read `messages[]` on every 200.** Statistical endpoints are where thin-sample and
  suppressed-data notices show up, and they arrive inside a success response.
- A suburb with too few transactions in a window will return a sparse or empty series
  rather than an error. Check sample size before drawing a conclusion.
- `SensitivePrice` and `SensitiveDate` suppression varies by state, so the same
  analysis will have different completeness in NSW than in QLD.
- Carry `disclaimer` into anything you publish.

## Related

- `conventions/pricefinder-conventions.yml` — filters, the pagination gap, `messages[]`
- `lifecycle/pricefinder-lifecycle.yml` — the deprecated operations and parameters

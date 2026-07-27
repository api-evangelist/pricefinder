---
name: Research comparable sales, rentals and listings around a property
description: Assemble a comparables set for an Australian property using Pricefinder's radial, street, suburb, postcode and spatial collection operations — and work around the fact that this API has no pagination.
api: openapi/pricefinder-api-swagger.json
generated: '2026-07-26'
method: generated
source: openapi/pricefinder-api-swagger.json + conventions/pricefinder-conventions.yml
operations:
  - radialSales
  - radialRentals
  - radialListings
  - radialProperties
  - radialPlanningAlerts
  - streetSales
  - streetRentals
  - streetListings
  - streetProperties
  - sale
  - rental
  - listing
---

# Research comparable sales, rentals and listings around a property

Start from a resolved `propertyId` — see `pricefinder-resolve-address-and-value-property`.

## Choose your geography

Pricefinder gives you five parallel ways to scope a comparables query. They return the
same record shapes; only the scoping differs.

| scope | sales | rentals | listings | properties |
|---|---|---|---|---|
| radius around a property | `radialSales` | `radialRentals` | `radialListings` | `radialProperties` |
| a street | `streetSales` | `streetRentals` | `streetListings` | `streetProperties` |
| a suburb | `sales` | `rentals` | `listings` | `properties` |
| a postcode | `sales` | `rentals` | `listings` | `properties` |
| an arbitrary geometry | `sales` | `rentals` | `listings` | `properties` |

**The suburb, postcode and spatial rows share operationIds with each other and with the
radial family.** `properties` alone repeats across 14 paths in this contract. Bind your
client to **METHOD + PATH**, never to `operationId` alone:

- `GET /properties/{propertyId}/radial/sales` — `radialSales`
- `GET /suburbs/{suburbId}/sales` — `sales`
- `GET /postcodes/{postcode}/sales` — `sales`
- `GET /spatial/sales` — `sales`
- `GET /streets/{streetId}/sales` — `streetSales`

`radialPlanningAlerts` (`GET /properties/{propertyId}/radial/developmentapplications`)
adds nearby development applications — useful for a valuation narrative, and unique to
the radial family.

## Filter with the current parameter generation

Use these, on every collection operation:

`min_beds` / `max_beds`, `min_baths` / `max_baths`, `min_car_parks` / `max_car_parks`,
`min_area` / `max_area`, `min_price` / `max_price`, `property_type`,
`date_start` / `date_end`, `matchlevel_min` / `matchlevel_max`, `sort`, `limit`.

**Do not use `beds_gt`, `beds_lt`, `baths_gt`, `baths_lt`, `car_parks_gt`,
`car_parks_lt`, `area_gt`, `area_lt`, `price_gt`, `price_lt`.** That generation is
flagged deprecated on 47 operations — but only through a non-standard `pds` vendor
object, with no Swagger `deprecated` flag and no announced sunset date. It still works.
It will not forever.

## Work around the missing pagination

**This API has no pagination.** There is no cursor, no page number, no offset and no
next-link on any of the 116 operations. `limit` caps the result set on 49 collection
operations and gives you no way to reach anything past the cap. No default and no
maximum for `limit` is documented.

So: never assume a collection response is complete. When a result set may exceed your
`limit`, **partition the query along an orthogonal axis and union client-side**:

1. Split the time window with `date_start` / `date_end` — month by month is usually
   enough for a suburb.
2. Split the price band with `min_price` / `max_price`.
3. Split by `property_type`.
4. For radial queries, shrink the radius and run several overlapping circles.

De-duplicate the union on `saleId` / `rentalId` / `listingId` / `propertyId`. Overlapping
partitions will return the same record more than once.

## Fetch individual records

- `sale` — `GET /sales/{saleId}` → `Sale` (price, `saleDate`, `settlementDate`,
  `contractDate`, `saleType`, agency, agent, `dealingNumber`, `listingHistory`,
  `lowAskPrice`, `highAskPrice`).
- `rental` — `GET /rentals/{rentalId}`.
- `listing` — `GET /listings/{listingId}` → `Listing`. The `rental` boolean on
  `Listing` is what distinguishes a rental listing from a sale listing; there is no
  separate Rental entity in the model.

## Reading the results correctly

- **`SensitivePrice` and `SensitiveDate` may be withheld.** Australian state
  legislation differs on what transaction detail may be republished, and the contract
  encodes that in the type system. A comparable with a suppressed price is still a
  valid record — do not drop it silently, and do not treat a missing value as zero.
- **Read `messages[]` on every 200.** Partial-result and data-quality notices ride
  inside success payloads.
- **Carry `disclaimer` through to whatever you render.**
- No rate limit is published and no 429 is documented. Pace yourself and back off on
  any non-2xx.

## Related

- `conventions/pricefinder-conventions.yml` — the full filter vocabulary and the
  pagination gap
- `lifecycle/pricefinder-lifecycle.yml` — the deprecated `_gt`/`_lt` parameter families
- `pricefinder-analyse-suburb-market` — the statistical layer above comparables

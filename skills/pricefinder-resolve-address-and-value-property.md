---
name: Resolve an Australian address to a property and value it
description: Turn a free-text Australian address into a Pricefinder propertyId, pull the property record and its AVM estimate, and render the PDF valuation report.
api: openapi/pricefinder-api-swagger.json
generated: '2026-07-26'
method: generated
source: openapi/pricefinder-api-swagger.json + conventions/pricefinder-conventions.yml
operations:
  - suggest
  - suggestProperties
  - suggestTypeahead
  - suggestLonLat
  - property
  - propertyExtended
  - propertyAvm
  - propertyAvmReport
  - propertyReport
  - propertySchools
---

# Resolve an Australian address to a property and value it

The marquee flow. Everything Pricefinder sells hangs off `propertyId`, so step one is
always resolution.

Authenticate first — see `pricefinder-authenticate-and-check-entitlements`.

## Step 1 — resolve the address to a propertyId

Pick the resolver that matches your input:

| input | operation | path |
|---|---|---|
| free-text address, mixed entity types | `suggest` | `GET /suggest` |
| free-text address, properties only | `suggestProperties` | `GET /suggest/properties` |
| partial string, as-you-type | `suggestTypeahead` | `GET /suggest/typeahead` |
| longitude / latitude | `suggestLonLat` | `GET /suggest/lonlat` |
| suburb name | `suggestSuburbs` | `GET /suggest/suburbs` |
| street name | `suggestStreets` | `GET /suggest/streets` |

Suggest responses carry `SuggestMatches` / `SuggestMatch`. Do not silently take the
first hit. Read the **`matchlevel`** on the candidate — it scores address-match
confidence, and most collection operations let you constrain it with `matchlevel_min`
and `matchlevel_max`. A low `matchlevel` means you resolved to a neighbouring or
parent property, which will quietly poison every downstream valuation.

Watch for `parent` on the `Property` record: strata and subdivided holdings are
modelled as child properties under a parent, so an apartment address may resolve to
either depending on how it was written.

## Step 2 — pull the property record

- `property` — `GET /properties/{propertyId}` → `Property`: address, features,
  `landDetails`, street, suburb, type, `rpd`, `marketStatus`, `lastSale`,
  `recentListing`, `recentRental`, `owners`, `ownership`, `lastModified`.
- `propertyExtended` — `GET /properties/{propertyId}/extended` → `ExtendedProperty`.

There is **no `expand=` parameter** on this API. Extra detail is a separate operation
and a separate call.

`owners` / `ownership` is the restricted ownership-detail product — available for WA,
QLD, NSW and VIC only, and subject to your subscription. Check `getFeatures` before
depending on it.

## Step 3 — get the valuation

- `propertyAvm` — `GET /properties/{propertyId}/avm` → the AVM estimate as JSON.
- `propertyAvmReport` — `GET /properties/{propertyId}/avm/pdf` → the same valuation
  rendered as a PDF (`application/pdf`).

There is **no batch or portfolio AVM operation**. Valuing a book of properties means
one call per property; pace yourself, because no rate limit is published and no 429 is
documented.

## Step 4 — optional context

- `propertyReport` — `GET /properties/{propertyId}/pdf`: the full property report PDF.
- `propertySchools` — `GET /properties/{propertyId}/schools`: school catchment context.
- `map` — `GET /properties/{propertyId}/map`, `images` / `imagesMain` /
  `imagesFloorplan`, `streetview` — imagery for a report or listing page.

## Reading the response correctly

**Always read `messages[]`.** `Message` (`{code, text}`) is the most-referenced schema
in the entire contract, and it rides inside **200 responses**, not error responses. It
carries data-quality notices and jurisdictional-suppression notices. A 200 with a
non-empty `messages` array is a partial or redacted answer, and branching only on HTTP
status will present it as fact.

**Respect the sensitive types.** `SensitivePrice` and `SensitiveDate` are wrapper types
for values that Australian state legislation may require be withheld. Check for a
withheld value before rendering a price or date.

**Carry the disclaimer.** Every top-level entity has a `disclaimer` field. Licensing
text travels with the payload; render it.

## Related

- `data-model/pricefinder-data-model.yml` — the propertyId-anchored entity graph
- `conventions/pricefinder-conventions.yml` — filters, the absent pagination, `messages[]`
- `pricefinder-research-comparable-sales` — the comparables flow that follows this one

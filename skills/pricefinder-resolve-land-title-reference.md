---
name: Resolve an Australian land title reference to a property
description: Turn a state or territory land-title reference (plan, lot, section, volume, folio, division) into a Pricefinder propertyId across all eight Australian jurisdictions, each of which has its own path grammar.
api: openapi/pricefinder-api-swagger.json
generated: '2026-07-26'
method: generated
source: openapi/pricefinder-api-swagger.json + data-model/pricefinder-data-model.yml
operations:
  - properties
  - planProperties
  - volumeFolioProperties
  - divisionProperties
  - propertyTitle
---

# Resolve an Australian land title reference to a property

This is the land-registry seam expressed as an API: 25 operations under the
`references` tag that map an external title reference onto Pricefinder's internal
`propertyId`. It is the surface valuers, conveyancers and lenders reach for when they
start from a title rather than an address.

Australia has **no national property identifier**. There is no MLS, no RESO regime and
no Universal Property Identifier. What exists instead is eight jurisdictions with eight
different title-reference grammars, and Pricefinder has built a bespoke path set for
each. Expect no uniformity — that is the domain, not a design failure.

Authenticate first — see `pricefinder-authenticate-and-check-entitlements`.

## Bind by path, not by operationId

**Every operation in this tag has a colliding operationId.** `properties` repeats
across 14 paths, `planProperties` across 8, `volumeFolioProperties` across 2. A code
generator or MCP tool-forge that keys on `operationId` will collapse this entire
25-operation surface into three or four tools and silently lose most of the country.
Bind to **METHOD + PATH**.

## Per-jurisdiction grammar

**NSW** — plan type + plan number, optionally section, optionally lot:
- `GET /references/states/nsw/plans/{planType}/{planNumber}/properties` — `planProperties`
- `GET /references/states/nsw/plans/{planType}/{planNumber}/lots/{lot}/properties` — `properties`
- `GET /references/states/nsw/plans/{planType}/{planNumber}/sections/{section}/properties` — `properties`
- `GET /references/states/nsw/plans/{planType}/{planNumber}/sections/{section}/lots/{lot}/properties` — `properties`

**VIC** — same plan grammar as NSW, **plus** volume/folio:
- `GET /references/states/vic/plans/{planType}/{planNumber}/properties` — `planProperties`
- `GET /references/states/vic/plans/{planType}/{planNumber}/lots/{lot}/properties` — `properties`
- `GET /references/states/vic/plans/{planType}/{planNumber}/sections/{section}/properties` — `properties`
- `GET /references/states/vic/plans/{planType}/{planNumber}/sections/{section}/lots/{lot}/properties` — `properties`
- `GET /references/states/vic/volumes/{volume}/folios/{folio}/properties` — `volumeFolioProperties`

**QLD** — plan grammar, and the only jurisdiction with a **reverse** lookup:
- `GET /references/states/qld/plans/{planType}/{planNumber}/properties` — `planProperties`
- `GET /references/states/qld/plans/{planType}/{planNumber}/lots/{lot}/properties` — `properties`
- `GET /references/states/qld/titles/{propertyId}` — `propertyTitle` → `PropertyTitle`.
  This is the inverse direction: propertyId → title. It exists for QLD only.

**SA**:
- `GET /references/states/sa/plans/{planType}/{planNumber}/properties` — `planProperties`
- `GET /references/states/sa/plans/{planType}/{planNumber}/lots/{lot}/properties` — `properties`

**WA** — plan grammar plus volume/folio:
- `GET /references/states/wa/plans/{planType}/{planNumber}/properties` — `planProperties`
- `GET /references/states/wa/plans/{planType}/{planNumber}/lots/{lot}/properties` — `properties`
- `GET /references/states/wa/volumes/{volume}/folios/{folio}/properties` — `volumeFolioProperties`

**TAS** — plan number with **no** planType:
- `GET /references/states/tas/plans/{planNumber}/properties` — `planProperties`
- `GET /references/states/tas/plans/{planNumber}/lots/{lot}/properties` — `properties`

**NT** — plan number with **no** planType:
- `GET /references/states/nt/plans/{planNumber}/properties` — `planProperties`
- `GET /references/states/nt/plans/{planNumber}/lots/{lot}/properties` — `properties`

**ACT** — division / section / lot, not plan / lot:
- `GET /references/states/act/divisions/{division}/properties` — `divisionProperties`
- `GET /references/states/act/divisions/{division}/sections/{section}/properties` — `properties`
- `GET /references/states/act/divisions/{division}/sections/{section}/lots/{lot}/properties` — `properties`
- `GET /references/states/act/divisions/{planNumber}/lots/{lot}/properties` — `properties`
  (note the ACT variant that takes `planNumber` in the division position — a real
  irregularity in the published contract, reproduced here as observed)

## Handling the response

Every one of these operations returns `PropertyIdentifiers` — a wrapper carrying
`properties[]` of `PropertyIdentifier`, plus `disclaimer` and `messages`.

- **A title reference can resolve to many properties.** A plan without a lot returns
  every property on that plan; a strata plan can return dozens. Do not assume one hit.
- **A title reference can resolve to zero.** An empty `properties[]` is a valid answer,
  not an error.
- Take the resulting `propertyId` into `property` / `propertyExtended` / `propertyAvm`
  for detail — see `pricefinder-resolve-address-and-value-property`.
- The `properties` filter parameters (`min_beds`, `max_beds`, `min_area`, `max_area`,
  `property_type`, `limit`, …) apply here too, and so does the deprecated `_gt`/`_lt`
  generation. Use `min_*`/`max_*`.
- **Read `messages[]`.** Title resolution is exactly where data-quality notices appear.

## Error handling

No 4xx is documented on any of these 25 operations. A missing or malformed reference
will not give you a typed error — expect a 200 with an empty `properties[]`, or an
opaque failure. Validate your reference grammar client-side against the jurisdiction
before calling.

## Related

- `data-model/pricefinder-data-model.yml` — the identifier model and why there is no UPI
- `conformance/pricefinder-conformance.yml` — why RESO is absent from this market

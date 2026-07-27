---
name: Retrieve a Pricefinder appraisal, CMA or statement of information
description: Fetch a shared sales CMA, rental CMA or statement of information by appraisal share id, pull agent and office branding, and record a CMA event.
api: openapi/pricefinder-api-swagger.json
generated: '2026-07-26'
method: generated
source: openapi/pricefinder-api-swagger.json + data-model/pricefinder-data-model.yml
operations:
  - appraisals
  - salesCma
  - rentalCma
  - soi
  - getInsights
  - agentImage
  - userLogo
  - image
  - saveEvent
  - propertyPricefinder
  - appraisalsSaleCma
  - appraisalsRentalCma
  - appraisalsSoi
---

# Retrieve a Pricefinder appraisal, CMA or statement of information

The appraisal surface is how a real estate agency's comparative market analysis leaves
Pricefinder and lands in a CRM, a proposal tool or a vendor-facing report. It is the
largest schema family in the contract.

Authenticate first — see `pricefinder-authenticate-and-check-entitlements`.

## The share-id model

Appraisals are addressed by an opaque **`appraisalShareId`**, not by `propertyId`. That
is the point: a CMA is generated inside Pricefinder and then shared, and the holder of
the share id can retrieve it without knowing the underlying property key.

Each artifact has a standard and an **extended** variant, on separate paths:

| artifact | standard | extended |
|---|---|---|
| sales CMA | `GET /appraisals/salescma/{appraisalShareId}` | `GET /appraisals/salescma/{appraisalShareId}/extended` |
| rental CMA | `GET /appraisals/rentalcma/{appraisalShareId}` | `GET /appraisals/rentalcma/{appraisalShareId}/extended` |
| statement of information | `GET /appraisals/soi/{appraisalShareId}` | `GET /appraisals/soi/{appraisalShareId}/extended` |

**Each pair shares one operationId** — `salesCma`, `rentalCma` and `soi` respectively
are declared on both the standard and the extended path. Bind by METHOD + PATH or your
generated client will expose only one of each pair.

Schemas: `SalesCMACommon` / `SalesCMADetailed`, `RentalCMACommon` /
`RentalCMADetailed`, `SOICommon` / `SOIDetailed` — each anchored on `propertyId`.

## Listing and insights

- `appraisals` — `GET /appraisals` → `AppraisalSimpleView`: the appraisal list for the
  authenticated user, carrying `userId`, `agentId` and `propertyId`.
- `getInsights` — `GET /appraisals/insights` → `AppraisalInsightsSampleView`: aggregate
  appraisal insights.

## Branding assets

Reports usually need the agency's marks:

- `agentImage` — `GET /appraisals/agent/image/{agentId}`
- `userLogo` — `GET /appraisals/logo/image/{userId}`
- `image` — `GET /appraisals/office/image/{userId}` (office imagery; note this
  operationId also appears on `GET /images/{id}`)

These return binary image data and declare only a bare `default: successful operation`
response. Handle a missing or empty asset gracefully — there is no documented 404.

## Recording a CMA event

`saveEvent` — `POST /events/cmaEvent/{cmaId}` — records an event against a CMA
(`EventParameters`, keyed by `cmaId`).

This is **one of only five POST operations in the whole API, and it accepts no
idempotency key**. The string "idempoten" appears zero times in the contract. Do not
blind-retry on a timeout — you will risk a duplicate event with no way to collapse it.

## Handing the user into the application

Nine SSO operations return an `SSOLink` deep link that drops an authenticated user
straight into the Pricefinder web app at the right context:

- `appraisalsSaleCma` — `GET /sso/properties/{propertyId}/appraisals/salecma`
- `appraisalsRentalCma` — `GET /sso/properties/{propertyId}/appraisals/rentalcma`
- `appraisalsSoi` — `GET /sso/properties/{propertyId}/appraisals/soi`
- `propertyPricefinder` — `GET /sso/properties/{propertyId}/pricefinder`
- `titles` — `GET /sso/properties/{propertyId}/titles`
- `radialSales` — `GET /sso/properties/{propertyId}/radial/sales`
- `property` — `GET /sso/properties/{propertyId}`
- `home` — `GET /sso`

**Do not use `cma` — `GET /sso/properties/{propertyId}/cma`.** It carries a real Swagger
`deprecated: true` flag. Use the three `/appraisals/*` deep links above instead.

## Reading the results correctly

- Read `messages[]` on every 200.
- Carry `disclaimer` into any rendered report — a CMA is a client-facing document and
  the licensing text is part of the payload.
- `SensitivePrice` / `SensitiveDate` suppression applies to the comparables inside a
  CMA exactly as it does elsewhere.
- No 4xx is documented on any appraisal operation. An expired or unknown
  `appraisalShareId` has no typed error contract — handle non-2xx opaquely.

## Related

- `data-model/pricefinder-data-model.yml` — the appraisal view family
- `lifecycle/pricefinder-lifecycle.yml` — the deprecated `cma` SSO operation

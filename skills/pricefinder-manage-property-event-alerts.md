---
name: Manage Pricefinder property event alerts
description: Subscribe to, list and cancel ForSale / ForRent / Sold / SoldVerified alerts on Australian properties — and understand why these are email notifications, not webhooks.
api: openapi/pricefinder-api-swagger.json
generated: '2026-07-26'
method: generated
source: openapi/pricefinder-api-swagger.json + conventions/pricefinder-conventions.yml
operations:
  - propertiesEventsSubscribe
  - propertyEventsSubscribe
  - propertyEventSubscription
  - propertyEventsUnSubscribe
  - userEventsSubscription
---

# Manage Pricefinder property event alerts

## Read this first: these are not webhooks

The `subscriptions` tag looks like an event surface and is not one. Pricefinder's own
contract states that property event alerts are **"currently only available through
email notifications"**.

There is:

- no callback URL parameter,
- no signing secret,
- no delivery, retry or replay semantics,
- no HTTP push of any kind,
- no AsyncAPI document, no WebSocket, no SSE, no queue.

The string "webhook" appears **zero times** in the 306,784-byte contract.

**If you are building an agent or an integration that needs to react to market events,
this API cannot notify you.** You must poll. Watch `marketStatus` and `lastModified` on
the `Property` record, or re-run a scoped `sales` / `listings` query on a schedule with
a `date_start` window. Budget for that: no rate limit is published and no 429 is
documented, so pace conservatively.

Authenticate first — see `pricefinder-authenticate-and-check-entitlements`.

## Event types

`ForSale`, `ForRent`, `Sold`, `SoldVerified`.

## Use the current operations

| action | operation | path |
|---|---|---|
| subscribe (many properties) | `propertiesEventsSubscribe` | `POST /eventsubscriptions/properties` |
| subscribe (one property) | `propertyEventsSubscribe` | `POST /eventsubscriptions/property/{propertyId}` |
| read one property's subscription | `propertyEventSubscription` | `GET /eventsubscriptions/property/{propertyId}` |
| cancel one property's subscription | `propertyEventsUnSubscribe` | `DELETE /eventsubscriptions/property/{propertyId}` |
| list all subscriptions for the user | `userEventsSubscription` | `GET /eventsubscriptions/currentuser` |

**Do not use the `/properties/{propertyId}/eventsubscription` family** — `eventsSubscription`
(GET), `eventsSubscribe` (POST) and `eventsUnSubscribe` (DELETE) all carry a real
Swagger `deprecated: true` flag. Their replacements are the `/eventsubscriptions/`
operations above. No sunset date is published; migrate anyway.

## Handle the 204 correctly

`propertyEventSubscription` is one of the very few operations in this contract with
real documented response semantics — and it is unusual:

- **`204`** — "The request has succeeded but no events subscribed". An empty
  subscription set is a **204 with no body**, not a 200 with an empty array.
- **`404`** — "Property not found", body `{ "error": string }` (schema `Error`). This
  is the only 404 documented anywhere in the API.
- `200` — the subscription exists, body `PropertyEventSubscription`.

Branch on the status code. A client that only parses the body will treat "no
subscriptions" as a parse failure.

## No idempotency — do not blind-retry

The three subscribe/unsubscribe writes are among only five POST and two DELETE
operations in the entire API, and **none accepts an idempotency key**. There is no
`Idempotency-Key` header, no client request id, no de-duplication window.

On a timeout or an opaque failure from a subscribe call:

1. Do **not** retry immediately.
2. `GET /eventsubscriptions/property/{propertyId}` (`propertyEventSubscription`) to read
   the actual state — remembering the 204.
3. Only then decide whether to re-issue.

The same applies to `propertiesEventsSubscribe` for a batch: read back with
`userEventsSubscription` (`GET /eventsubscriptions/currentuser`) and reconcile against
your intended set rather than replaying the batch.

## Check entitlement first

There are no OAuth scopes on this API. If your subscription does not include alerting,
you will find out through an opaque failure — no 403 is documented on any operation.
Call `getFeatures` (`GET /features` → `UserFeatures`) before wiring an alerting flow.

## Related

- `conventions/pricefinder-conventions.yml` — the absent webhook and idempotency
  contracts
- `errors/pricefinder-problem-types.yml` — the 204 / 404 pair, and the undocumented 401
- `lifecycle/pricefinder-lifecycle.yml` — the deprecated `/properties/{id}/eventsubscription`
  family

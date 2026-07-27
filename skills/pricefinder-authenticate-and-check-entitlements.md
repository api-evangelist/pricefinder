---
name: Authenticate to Pricefinder and check entitlements
description: Obtain an OAuth 2.0 access token from the Pricefinder API and discover what the credential is actually allowed to do, before planning any other work.
api: openapi/pricefinder-api-swagger.json
generated: '2026-07-26'
method: generated
source: openapi/pricefinder-api-swagger.json + authentication/pricefinder-authentication.yml
operations:
  - getToken
  - getFeatures
---

# Authenticate to Pricefinder and check entitlements

Run this first. Every other Pricefinder skill assumes you already hold a valid token
and know your entitlement set.

## Before you start

- Pricefinder has **no self-serve signup, no free tier and no sandbox**. Credentials
  are issued only under a negotiated commercial subscription (minimum one-month term)
  obtained through https://www.pricefinder.com.au/get-started/. If you do not have a
  username and password, stop — there is no key you can mint.
- Base URL: `https://api.pricefinder.com.au/v1`
- The contract declares no `securityDefinitions`. The auth model below is transcribed
  from the provider's own prose in the `getToken` operation description.

## Step 1 — get an access token (`getToken`)

`POST /oauth2/token`, `Content-Type: application/x-www-form-urlencoded`.

Three grant types are supported. Use `client_credentials` for your own access:

| form field | value |
|---|---|
| `grant_type` | `client_credentials` |
| `client_id` | your Pricefinder **username** |
| `client_secret` | your Pricefinder **password** |

HTTP Basic auth is an accepted alternative to sending username and password as form
parameters.

Use `authorization_code` only when acting on **another** Pricefinder user's behalf:
send that user to
`/v1/auth/authorize.html?client_id=<your_username>&state=<random>&redirect_uri=<your_https_callback>`
(HTTPS required). On approval the callback receives `?state=&code=`; on refusal it
receives `?error=access_denied`. Exchange the `code` at `getToken` with
`grant_type=authorization_code` plus `client_id`, `client_secret`, `code` and
`redirect_uri`.

Use `refresh_token` to renew. **Refresh tokens rotate** — the response returns a new
access token *and* a new refresh token, and the old refresh token is consumed. Persist
the new one immediately or you will lose the session.

The 200 response is `ClientAccessToken`: `access_token`, `token_type`, `expires_in`
(typed as a **string**, not an integer — parse defensively), `refresh_token`.

Errors on this operation: `401 Invalid username/password`, `400 Bad Request`. Neither
declares a response body schema.

## Step 2 — present the token

Prefer the header:

```
Authorization: Bearer <access_token>
```

The contract also documents `?access_token=<token>` as a query parameter. **Do not use
it.** Bearer tokens in query strings land in proxy logs, browser history and referrer
headers.

## Step 3 — read your entitlements (`getFeatures`)

`GET /features` → `UserFeatures`.

This matters more here than on most APIs. **Pricefinder defines no OAuth scopes** —
there is no scope vocabulary, no `scope` parameter on the token request, and no
permissions reference page. Authorization is all-or-nothing per credentialed user,
with the actual entitlement set enforced server-side by the commercial subscription.
`getFeatures` is the **only machine-readable way** to discover what your token can do.

Call it once after authenticating and gate your plan on the result. Do not discover
entitlement by firing operations and interpreting failures — the contract documents no
403 anywhere, so an entitlement failure will not be distinguishable from anything else.

## Error handling

- Every data operation returns **401** without a valid token, but **not one of the 115
  non-token operations declares a 401 in the contract**. Handle it anyway.
- No 403, no 429 and no 5xx is documented on any operation. Treat any non-2xx as
  opaque, back off exponentially, and do not retry writes blindly — there is no
  idempotency key anywhere in this API.
- There is no request-id or correlation header. Log your own timestamp, method, path
  and full query string; that is all you will have for a support ticket.

## Related

- `authentication/pricefinder-authentication.yml` — full auth profile
- `conventions/pricefinder-conventions.yml` — cross-cutting runtime semantics
- `errors/pricefinder-problem-types.yml` — what the contract does and does not declare

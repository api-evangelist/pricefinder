# Pricefinder (pricefinder)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Pricefinder is an Australian property intelligence and valuation platform operating at pricefinder.com.au, supplying property records, comparable sales and rental history, suburb statistics, AVM-powered price estimates and CMA/appraisal reports to real estate agencies, mortgage brokers, banks and lenders, valuers, property developers, advisory firms and government. Its home market is Australia and it sits on the data side of the value chain rather than the portal side — it blends Domain and Allhomes first-party listing feeds with state and territory government land valuation and sales data plus third-party mapping, then resells that as reports, a web application and an API. Pricefinder is a brand of Domain Holdings Australia, which CoStar Group completed the acquisition of in August 2025, so it now sits inside the same group as Domain, Allhomes and Commercial Real Estate. Its API posture is unusually honest for this sector — the full machine-readable contract is genuinely public. A complete Swagger 2.0 document (v1.13.1, 112 paths, 188 definitions, 19 tags) and an interactive Swagger UI are served anonymously from api.pricefinder.com.au, and OAuth 2.0 client_credentials, authorization_code and refresh_token flows are documented in full. But every data path returns 401 without credentials, and credentials are not self-serve — there is no developer signup, only industry "Get started" forms that route to sales for a custom, minimum-one-month commercial subscription governed by the Domain Group API Terms and Conditions. Australia has no MLS system and no RESO regime — no RESO Web API or Data Dictionary certification, no OData $metadata document and no Universal Property Identifier appears anywhere in Pricefinder's contract or documentation. The land-registry seam shows up instead as state-specific title reference lookups (NSW/VIC/QLD/SA/WA/TAS/NT/ACT plan, lot, section, volume and folio paths).

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/pricefinder/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/pricefinder/refs/heads/main/apis.yml)

## Tags

- Real Estate
- Australia
- PropTech
- Property Data
- Valuation
- AVM
- Property Listings
- Rentals
- Land Registry
- Title
- Mortgage
- Market Data

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### Pricefinder Property API

Property record retrieval for Australian properties — core and extended property detail, images, floorplans, street view, maps, schools, radial searches for nearby sales, rentals, listings, properties and development applications, and PDF property reports. 22 documented operations under the `properties` tag, plus image, feature and localisation stub endpoints.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Property Records
- Real Estate
- Australia

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/pricefinder-api-swagger.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)
- [Documentation](https://www.pricefinder.com.au/our-data/)
- [Terms of Service](https://www.domain.com.au/group/api-terms-and-conditions/)

### Pricefinder AVM & Valuation API

Automated valuation model output for a property, returned as JSON via GET /properties/{propertyId}/avm against the AVM schema, and as a rendered PDF via /properties/{propertyId}/avm/pdf. Auto-CMA sale and rental PDFs are generated from the same property identifier. This is the valuation surface Pricefinder markets to lenders, valuers and advisory firms.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Valuation
- AVM
- Australia

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)
- [Documentation](https://www.pricefinder.com.au/get-started/)

### Pricefinder Sales, Rentals & Listings API

Retrieval of individual sale, rental and listing records by identifier, and enumeration of sales, rentals and listings scoped by suburb, postcode, street or spatial boundary. Backed by Domain and Allhomes first-party listing feeds and government sale transaction data.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Property Listings
- Rentals
- Transactions

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)

### Pricefinder Suburb & Market Statistics API

Suburb-level market intelligence — suburb detail and summary, demographics, flyover reports and PDFs, street enumeration, peak selling periods, sale price segmentation, and time series for sales, rentals and price segmentation by property type. 15 documented operations under the `suburbs` tag.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Market Data
- Statistics
- Australia

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)

### Pricefinder Title & Land Reference API

State-by-state resolution of Australian land title references to Pricefinder property records — NSW, VIC, QLD, SA, WA, TAS, NT and ACT plan/planType, lot, section, volume, folio and division lookups, plus QLD title retrieval by property identifier. 25 documented operations under the `references` tag; this is the land-registry seam expressed as an API.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Land Registry
- Title
- Australia

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)

### Pricefinder Search & Suggest API

Address, street, suburb and typeahead suggestion endpoints, lon/lat reverse suggestion, owner-name search across properties and sales, and spatial queries returning properties, sales, rentals and listings within a supplied geometry.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Search
- Geospatial

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)

### Pricefinder Appraisals & CMA API

Comparative market analysis and appraisal artifacts — sales CMA, rental CMA and statement-of-information retrieval by appraisal share identifier (standard and extended), appraisal listing and insights, agent, office and logo imagery, and CMA event posting.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Appraisals
- CMA
- Real Estate

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)

### Pricefinder Property Event Subscriptions API

Subscribe, list and delete property event alerts for ForSale, ForRent, Sold and SoldVerified event types, per property or for the current user. The documentation states these are delivered as email notifications only — there is no webhook callback or push delivery documented anywhere in the contract.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- Events
- Subscriptions
- Notifications

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)

### Pricefinder SSO API

Single sign-on deep links that hand an authenticated user into the Pricefinder web application at a specific context — a property, its CMA, sales or rental appraisal, statement of information, radial sales, or titles view. 9 documented operations under the `sso` tag.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- SSO
- Authentication

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)

### Pricefinder OAuth 2.0 Token API

POST /oauth2/token issuing access and refresh tokens for three documented grant types: client_credentials (API user's own username and password, HTTP Basic accepted as an alternative to form parameters), authorization_code (third-party delegated access via the hosted authorize page at /v1/auth/authorize.html) and refresh_token. Tokens are presented as an `Authorization: Bearer` header or an `access_token` query parameter. No OpenID Connect discovery document is served.

- **Human URL:** [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html)
- **Base URL:** `https://api.pricefinder.com.au/v1`

#### Tags

- OAuth
- Authentication
- Security

#### Properties

- [OpenAPI](openapi/pricefinder-api-swagger.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)
- [Authentication](https://api.pricefinder.com.au/v1/auth/authorize.html)

## Common Properties

- [Website](https://www.pricefinder.com.au/)
- [API Reference](https://api.pricefinder.com.au/v1/swagger/index.html)
- [OpenAPI](https://api.pricefinder.com.au/v1/swagger.json)
- [Sign Up](https://www.pricefinder.com.au/get-started/)
- [Support](https://help.pricefinder.com.au/)
- [Documentation](https://www.pricefinder.com.au/our-data/)
- [Integrations](https://www.pricefinder.com.au/integrations/)
- [Terms of Service](https://www.domain.com.au/group/api-terms-and-conditions/)

## Access & RESO Posture

| Question | Answer |
| --- | --- |
| Home market | Australia (national — NSW, VIC, QLD, SA, WA, TAS, NT, ACT) |
| RESO posture | No RESO reference found — Australia has no MLS and no RESO regime |
| RESO certified | No |
| OData `$metadata` | None — `https://api.pricefinder.com.au/$metadata` returns HTTP 404 |
| RESO UPI | Not used — identity is a proprietary integer `propertyId` joined to per-state land-title references |
| Access gate | `application-approval` — industry "Get started" form → sales → custom commercial plan, one-month minimum |
| Membership required | No MLS, board or association membership; no agent/broker licence requirement |
| Open data | No — commercial licence only, even over government-sourced land valuation data |
| Auth model | OAuth 2.0 (`client_credentials`, `authorization_code`, `refresh_token`); Bearer token or `access_token` query param; HTTP Basic accepted at the token endpoint; no OIDC discovery |
| Developer portal | [https://api.pricefinder.com.au/v1/swagger/index.html](https://api.pricefinder.com.au/v1/swagger/index.html) — live public Swagger UI, HTTP 200, but no self-serve key issuance |
| Machine-readable contract | Swagger 2.0 v1.13.1 — 112 paths, 188 definitions, 19 tags, served anonymously at HTTP 200 |
| Webhooks | None — event subscriptions deliver by **email only** |
| SDKs | None official; a third-party Ruby client exists at `realhub/pricefinder-ruby` |
| Postman | None found |

The headline finding is the split between contract and data: anyone can download the complete Pricefinder API contract without authenticating, and nobody can call it without a negotiated commercial subscription. Full probe log, HTTP statuses and provenance are in [review.yml](review.yml).

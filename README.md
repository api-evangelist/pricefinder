# Pricefinder

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
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

**Areas:** Real Estate · Australia · PropTech · Property Data · Valuation · AVM · Property Listings · Rentals · Land Registry · Title · Mortgage · Market Data

## Pricefinder on APIs.io

Everything profiled for Pricefinder is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/pricefinder/](https://apis.io/providers/pricefinder/)** — or go straight to one of the APIs or properties below.

### APIs (19)

- [Pricefinder Appraisals API](https://apis.io/apis/pricefinder/pricefinder-appraisals-api/) — The appraisals API from Pricefinder — 11 operation(s) for appraisals.
- [Pricefinder Events API](https://apis.io/apis/pricefinder/pricefinder-events-api/) — The events API from Pricefinder — 1 operation(s) for events.
- [Pricefinder Features API](https://apis.io/apis/pricefinder/pricefinder-features-api/) — The features API from Pricefinder — 1 operation(s) for features.
- [Pricefinder Images API](https://apis.io/apis/pricefinder/pricefinder-images-api/) — The images API from Pricefinder — 1 operation(s) for images.
- [Pricefinder Listings API](https://apis.io/apis/pricefinder/pricefinder-listings-api/) — The listings API from Pricefinder — 1 operation(s) for listings.
- [Pricefinder Names API](https://apis.io/apis/pricefinder/pricefinder-names-api/) — The names API from Pricefinder — 3 operation(s) for names.
- [Pricefinder Postcodes API](https://apis.io/apis/pricefinder/pricefinder-postcodes-api/) — The postcodes API from Pricefinder — 4 operation(s) for postcodes.
- [Pricefinder Properties API](https://apis.io/apis/pricefinder/pricefinder-properties-api/) — The properties API from Pricefinder — 20 operation(s) for properties.
- [Pricefinder References API](https://apis.io/apis/pricefinder/pricefinder-references-api/) — The references API from Pricefinder — 25 operation(s) for references.
- [Pricefinder Rentals API](https://apis.io/apis/pricefinder/pricefinder-rentals-api/) — The rentals API from Pricefinder — 1 operation(s) for rentals.
- [Pricefinder Sales API](https://apis.io/apis/pricefinder/pricefinder-sales-api/) — The sales API from Pricefinder — 1 operation(s) for sales.
- [Pricefinder Spatial API](https://apis.io/apis/pricefinder/pricefinder-spatial-api/) — The spatial API from Pricefinder — 4 operation(s) for spatial.
- [Pricefinder SSO API](https://apis.io/apis/pricefinder/pricefinder-sso-api/) — The sso API from Pricefinder — 9 operation(s) for sso.
- [Pricefinder Streets API](https://apis.io/apis/pricefinder/pricefinder-streets-api/) — The streets API from Pricefinder — 4 operation(s) for streets.
- [Pricefinder Stubs API](https://apis.io/apis/pricefinder/pricefinder-stubs-api/) — The stubs API from Pricefinder — 1 operation(s) for stubs.
- [Pricefinder Subscriptions API](https://apis.io/apis/pricefinder/pricefinder-subscriptions-api/) — The subscriptions API from Pricefinder — 3 operation(s) for subscriptions.
- [Pricefinder Suburbs API](https://apis.io/apis/pricefinder/pricefinder-suburbs-api/) — The suburbs API from Pricefinder — 15 operation(s) for suburbs.
- [Pricefinder Suggest API](https://apis.io/apis/pricefinder/pricefinder-suggest-api/) — The suggest API from Pricefinder — 6 operation(s) for suggest.
- [Pricefinder Oauth2 API](https://apis.io/apis/pricefinder/pricefinder-oauth2-api/) — The oauth2 API from Pricefinder — 1 operation(s) for oauth2.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [pricefinder](https://apis.io/servers/pricefinder/pricefinder/)

#### Collections (1)

Postman and Open Collection request collections.

- [PriceFinder API v1.13.1](https://apis.io/collections/pricefinder/open-pricefinder-api-swagger/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Pricefinder Api Swagger Overlay](https://apis.io/overlays/pricefinder/pricefinder-api-swagger-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Pricefinder Authentication](https://apis.io/security/pricefinder/pricefinder-authentication/)
- [Pricefinder Domain Security](https://apis.io/security/pricefinder/pricefinder-domain-security/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.pricefinder.com.au/)
- [Documentation](https://www.pricefinder.com.au/our-data/)
- [API reference](https://api.pricefinder.com.au/v1/swagger/index.html)
- [Sign up](https://www.pricefinder.com.au/get-started/)
- [Support](https://help.pricefinder.com.au/)
- [Terms of service](https://www.domain.com.au/group/api-terms-and-conditions/)
- [Privacy policy](https://www.domain.com.au/group/privacy-policy/)

## API Evangelist rating

**Kin Score 34.3** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 35.8 |
| Contract governance | 4.5 |
| Operational transparency | 0.0 |
| Developer ergonomics | 51.8 |
| Access clarity | 34.2 |

**Agent readiness 17.1** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 28.7** — Securities & Market Data

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/pricefinder/](https://apis.io/providers/pricefinder/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Pricefinder: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/pricefinder/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

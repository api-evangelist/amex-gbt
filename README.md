# American Express Global Business Travel

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

American Express Global Business Travel (Amex GBT, NYSE GBTG, headquartered in New York) is the largest business-to-business travel platform in the world, operating in more than 140 countries under the Amex GBT, Egencia and Ovation brands, with CWT acquired in September 2025. Its home market is the United States. In the travel distribution chain Amex GBT is an intermediary rather than a supplier - a travel management company that aggregates air, hotel, rail, car and ground content sourced through the GDS layer, through NDC and through direct supplier connections, then resells it to corporate travel programmes with policy, approval, duty-of-care and reporting wrapped around it. It holds no inventory of its own, but it does hold the booking record, and that is where the switching cost lives. Its API posture is genuinely open at the documentation layer and firmly closed at the runtime layer. The Egencia Developer Center publishes thirteen named APIs and SPIs, and every one of them serves a real, anonymously retrievable OpenAPI 3.1.0 document from apis.egencia.com - no login, no key, no click-through. The identity surface is standards-based (SCIM 2.0 user provisioning over OAuth 2.0 client credentials); the booking, approval, expense, receipt, duty-of-care and reporting surfaces are proprietary Egencia shapes with no OpenTravel, HTNG or IATA NDC schema anywhere in them. Runtime access is customer-account gated - Egencia's own documentation states that "the values for client id and client secret will be provided to the Client after on-boarding to Egencia API platform", and every production endpoint returns 401 to an anonymous caller. So - public contracts, gated runtime, a real documented bulk data export in the BI Transactions API, and API terms Egencia reserves the right to change in its sole discretion.

**Areas:** Travel · United States · Corporate Travel · Travel Management · Business Travel · Distribution · Booking · Aviation · Hotels · Rail · Car Rental · Expenses

## American Express Global Business Travel on APIs.io

Everything profiled for American Express Global Business Travel is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/amex-gbt/](https://apis.io/providers/amex-gbt/)** — or go straight to one of the APIs or properties below.

### APIs (26)

- [American Express Global Business Travel Agent Assist Notes API](https://apis.io/apis/amex-gbt/amex-gbt-agent-assist-notes-api/) — The Agent Assist Notes API from American Express Global Business Travel — 1 operation(s) for agent assist notes.
- [American Express Global Business Travel Approval API](https://apis.io/apis/amex-gbt/amex-gbt-approval-api/) — Approval interface that will get approval custom detail from external client based on the booking data passed in request
- [American Express Global Business Travel Availability Probes Controller API](https://apis.io/apis/amex-gbt/amex-gbt-availability-probes-controller-api/) — The availability-probes-controller API from American Express Global Business Travel — 2 operation(s) for availability-probes-controller.
- [American Express Global Business Travel bookings v1 API](https://apis.io/apis/amex-gbt/amex-gbt-bookings-v1-api/) — The bookings v1 API from American Express Global Business Travel — 12 operation(s) for bookings v1.
- [American Express Global Business Travel bookings v2 API](https://apis.io/apis/amex-gbt/amex-gbt-bookings-v2-api/) — The bookings v2 API from American Express Global Business Travel — 2 operation(s) for bookings v2.
- [American Express Global Business Travel Company Details API](https://apis.io/apis/amex-gbt/amex-gbt-company-details-api/) — The Company Details API from American Express Global Business Travel — 2 operation(s) for company details.
- [American Express Global Business Travel Custom Data Field Cleanup Controller API](https://apis.io/apis/amex-gbt/amex-gbt-custom-data-field-cleanup-controller-api/) — The custom-data-field-cleanup-controller API from American Express Global Business Travel — 1 operation(s) for custom-data-field-cleanup-controller.
- [American Express Global Business Travel Custom Data Fields (CDF) API](https://apis.io/apis/amex-gbt/amex-gbt-custom-data-fields-cdf-api/) — CDF APIs to retrieve Definitions and Manage Values.
- [American Express Global Business Travel Doc Audit Clean Up Controller API](https://apis.io/apis/amex-gbt/amex-gbt-doc-audit-clean-up-controller-api/) — The doc-audit-clean-up-controller API from American Express Global Business Travel — 1 operation(s) for doc-audit-clean-up-controller.
- [American Express Global Business Travel DutyOfCare Data API](https://apis.io/apis/amex-gbt/amex-gbt-dutyofcare-data-api/) — The DutyOfCare Data API from American Express Global Business Travel — 2 operation(s) for dutyofcare data.
- [American Express Global Business Travel Ecommerce Settings Controller API](https://apis.io/apis/amex-gbt/amex-gbt-ecommerce-settings-controller-api/) — The ecommerce-settings-controller API from American Express Global Business Travel — 2 operation(s) for ecommerce-settings-controller.
- [American Express Global Business Travel Expense SPI API](https://apis.io/apis/amex-gbt/amex-gbt-expense-spi-api/) — Push Expense and Subscription Operation. This is only an example. The path and the host are specific for each implementor. However, the model is fixed and…
- [American Express Global Business Travel Gdpr Controller API](https://apis.io/apis/amex-gbt/amex-gbt-gdpr-controller-api/) — The gdpr-controller API from American Express Global Business Travel — 1 operation(s) for gdpr-controller.
- [American Express Global Business Travel Pos Iata Controller API](https://apis.io/apis/amex-gbt/amex-gbt-pos-iata-controller-api/) — The pos-iata-controller API from American Express Global Business Travel — 1 operation(s) for pos-iata-controller.
- [American Express Global Business Travel Receipts API](https://apis.io/apis/amex-gbt/amex-gbt-receipts-api/) — The receipts API from American Express Global Business Travel — 1 operation(s) for receipts.
- [American Express Global Business Travel Redirection Controller API](https://apis.io/apis/amex-gbt/amex-gbt-redirection-controller-api/) — The redirection-controller API from American Express Global Business Travel — 2 operation(s) for redirection-controller.
- [American Express Global Business Travel Resolve Controller API](https://apis.io/apis/amex-gbt/amex-gbt-resolve-controller-api/) — The resolve-controller API from American Express Global Business Travel — 1 operation(s) for resolve-controller.
- [American Express Global Business Travel Schemas Controller API](https://apis.io/apis/amex-gbt/amex-gbt-schemas-controller-api/) — The schemas-controller API from American Express Global Business Travel — 2 operation(s) for schemas-controller.
- [American Express Global Business Travel SCIM User Sync V1 API](https://apis.io/apis/amex-gbt/amex-gbt-scim-user-sync-v1-api/) — SCIM 2.0 compliant user sync APIs
- [American Express Global Business Travel SCIM User Sync V2 API](https://apis.io/apis/amex-gbt/amex-gbt-scim-user-sync-v2-api/) — SCIM 2.0 compliant user sync APIs version 2.
- [American Express Global Business Travel SCIM User Sync V3 API](https://apis.io/apis/amex-gbt/amex-gbt-scim-user-sync-v3-api/) — SCIM 2.0 compliant user sync APIs version 3.
- [American Express Global Business Travel SCIM V1 Admin Users API](https://apis.io/apis/amex-gbt/amex-gbt-scim-v1-admin-users-api/) — SCIM 2.0 compliant admin-users sync APIs
- [American Express Global Business Travel Third Party Acknowledgement API](https://apis.io/apis/amex-gbt/amex-gbt-third-party-acknowledgement-api/) — The Third Party Acknowledgement API from American Express Global Business Travel — 1 operation(s) for third party acknowledgement.
- [American Express Global Business Travel Transaction Service Controller API](https://apis.io/apis/amex-gbt/amex-gbt-transaction-service-controller-api/) — Transaction data operations
- [American Express Global Business Travel Validation API](https://apis.io/apis/amex-gbt/amex-gbt-validation-api/) — Validation interface that will request authorization of booking based on the data provided at checkout time.This is only an example. The path and the host…
- [American Express Global Business Travel Version Controller API](https://apis.io/apis/amex-gbt/amex-gbt-version-controller-api/) — The version-controller API from American Express Global Business Travel — 1 operation(s) for version-controller.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Amex Gbt Webhooks](https://apis.io/asyncapis/amex-gbt/amex-gbt-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [amex-gbt-mcp.yml](https://apis.io/servers/amex-gbt/amex-gbt-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Amex Gbt Agentic Access](https://apis.io/agentic-access/amex-gbt/amex-gbt-agentic-access/)

#### Collections (17)

Postman and Open Collection request collections.

- [Openconnect Approval Service](https://apis.io/collections/amex-gbt/open-amex-gbt-approval-customisation-spi/)
- [Egencia Approval Workflow API](https://apis.io/collections/amex-gbt/open-amex-gbt-approval-workflow-api/)
- [Egencia Get Booking API](https://apis.io/collections/amex-gbt/open-amex-gbt-booking-api/)
- [Egencia Cancellation/Deletion API](https://apis.io/collections/amex-gbt/open-amex-gbt-cancellation-deletion-api/)
- [Egencia Company CDF API](https://apis.io/collections/amex-gbt/open-amex-gbt-company-cdf-api/)
- [Company Details API](https://apis.io/collections/amex-gbt/open-amex-gbt-company-info-api/)
- [Duty Of Care API](https://apis.io/collections/amex-gbt/open-amex-gbt-duty-of-care-api/)
- [Expense SPI](https://apis.io/collections/amex-gbt/open-amex-gbt-expense-spi/)
- [Egencia Receipt API](https://apis.io/collections/amex-gbt/open-amex-gbt-receipt-api/)
- [BI API](https://apis.io/collections/amex-gbt/open-amex-gbt-reporting-api/)
- [BI API](https://apis.io/collections/amex-gbt/open-amex-gbt-service-bi/)
- [OpenAPI definition](https://apis.io/collections/amex-gbt/open-amex-gbt-service-company/)
- [Duty Of Care API](https://apis.io/collections/amex-gbt/open-amex-gbt-service-dutyofcare/)
- [OpenAPI definition](https://apis.io/collections/amex-gbt/open-amex-gbt-service-openconnect/)
- [SSO Context API](https://apis.io/collections/amex-gbt/open-amex-gbt-sso-context-api/)
- [Egencia User Sync API](https://apis.io/collections/amex-gbt/open-amex-gbt-user-sync-api/)
- [Validation SPI](https://apis.io/collections/amex-gbt/open-amex-gbt-validation-spi/)

#### Overlays (17)

OpenAPI overlays, including conversational phrasing for each operation.

- [Amex Gbt Approval Customisation Spi Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-approval-customisation-spi-overlay/)
- [Amex Gbt Approval Workflow Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-approval-workflow-api-overlay/)
- [Amex Gbt Booking Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-booking-api-overlay/)
- [Amex Gbt Cancellation Deletion Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-cancellation-deletion-api-overlay/)
- [Amex Gbt Company Cdf Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-company-cdf-api-overlay/)
- [Amex Gbt Company Info Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-company-info-api-overlay/)
- [Amex Gbt Duty Of Care Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-duty-of-care-api-overlay/)
- [Amex Gbt Expense Spi Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-expense-spi-overlay/)
- [Amex Gbt Receipt Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-receipt-api-overlay/)
- [Amex Gbt Reporting Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-reporting-api-overlay/)
- [Amex Gbt Service Bi Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-service-bi-overlay/)
- [Amex Gbt Service Company Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-service-company-overlay/)
- [Amex Gbt Service Dutyofcare Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-service-dutyofcare-overlay/)
- [Amex Gbt Service Openconnect Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-service-openconnect-overlay/)
- [Amex Gbt Sso Context Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-sso-context-api-overlay/)
- [Amex Gbt User Sync Api Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-user-sync-api-overlay/)
- [Amex Gbt Validation Spi Overlay](https://apis.io/overlays/amex-gbt/amex-gbt-validation-spi-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Amex Gbt Authentication](https://apis.io/security/amex-gbt/amex-gbt-authentication/)
- [Amex Gbt Domain Security](https://apis.io/security/amex-gbt/amex-gbt-domain-security/)
- [Amex Gbt Vulnerability Disclosure](https://apis.io/security/amex-gbt/amex-gbt-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Amex Gbt Scopes](https://apis.io/scopes/amex-gbt/amex-gbt-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.amexglobalbusinesstravel.com/)
- [Documentation](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-overview)
- [Getting started](https://apis.egencia.com/bi/v1/api-info)
- [API reference](https://apis.egencia.com/openconnect/docs/api-docs)
- [Support](https://www.egencia.com/en/contact-questions)
- [Changelog](https://apis.egencia.com/bi/v1/api-info)
- [Blog](https://www.amexglobalbusinesstravel.com/blog/)
- [Security](https://amexgbt.responsibledisclosure.com/hc/en-us)
- [Terms of service](https://www.amexglobalbusinesstravel.com/terms-of-service/)
- [Privacy policy](https://www.amexglobalbusinesstravel.com/egencia/privacy/)

## API Evangelist rating

**Kin Score 49.8** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 56.5 |
| Contract governance | 4.5 |
| Operational transparency | 42.1 |
| Developer ergonomics | 58.9 |
| Access clarity | 21.1 |

**Agent readiness 43.0** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | — |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | mostly |

**Regulatory alignment 40.2** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/amex-gbt/](https://apis.io/providers/amex-gbt/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for American Express Global Business Travel: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/amex-gbt/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

---
name: Authenticate against the Egencia API platform
description: Exchange onboarded client credentials for a one-hour bearer token and use it correctly across every Egencia service.
api: openapi/amex-gbt-service-openconnect-openapi.json
operations: []
generated: '2026-07-28'
method: generated
source: >-
  https://apis.egencia.com/bi/v1/api-info (Developer Guidelines, verbatim),
  securitySchemes across all thirteen OpenAPI documents
---

# Authenticate against the Egencia API platform

Every Egencia inbound API in this estate uses the same OAuth 2.0 client credentials grant against
one token endpoint. Do this once and cache the token; do not re-authenticate per call.

## Prerequisites

You cannot self-serve credentials. Egencia states it plainly: *"The values for client id and
client secret will be provided to the Client after on-boarding to Egencia API platform."* If you
do not have a client id and secret, there is nothing to run — every production endpoint returns
`401` to an anonymous caller. The OpenAPI contracts, however, are fully public, so you can build
and generate against them before onboarding.

## Steps

1. **Get a token.** `POST https://apis.egencia.com/auth/v1/token` with the credentials presented
   as HTTP Basic `base64(client_id:client_secret)`. The endpoint is POST-only — a `GET` returns
   `404` with a JSON error body.
2. **Read `expires_in`.** Egencia's own guidance: *"Authentication tokens, generated with client
   ID and secret, expire after one hour. Renew them by requesting a new token from the
   authentication endpoint if usage exceeds this duration."* Cache the token for its lifetime and
   refresh proactively at ~55 minutes; there is no refresh token in the client-credentials grant.
3. **Present it** as `Authorization: Bearer <token>` on every subsequent request.
4. **Pick the right base URL for the service** — they differ:
   - `https://apis.egencia.com/openconnect/api` — users, bookings, approvals, cancel/delete, CDFs, receipts, company
   - `https://apis.egencia.com/bi/api` — reporting / BI transactions
   - `https://apis.egencia.com/dutyofcare/api` — duty of care
   - `https://apis.egencia.com/openconnect-sso-service` — SSO context
5. **Set the right `Accept` header.** The BI Transactions API documents `application/hal+json`.
   The SCIM User Sync surface uses `application/scim+json`. Everything else is JSON.

## Rules

- **No scopes.** Every `oauth2` securityScheme in every spec declares `clientCredentials` with an
  empty `scopes` map. There is nothing to request and nothing to check — permission is decided by
  what the onboarded account is entitled to, server-side, and surfaces only as `403`.
- **A `401` means the token is empty, invalid or expired** — mint a new one. A `403` means the
  account is not entitled to that operation; retrying will not help.
- **Do not blind-retry writes.** There is no `Idempotency-Key` anywhere in this estate. See
  `conventions/amex-gbt-conventions.yml`.
- **Support routing:** *"Handle any 4xx errors internally. Only reach out to Egencia support for
  5xx error codes."*
- Three contracts — Validation SPI, Expense SPI, Approval Customisation SPI — declare **no**
  security scheme at all. Those are endpoints *you* host and Egencia calls; you define the
  security on your own listener.

## Related

- `authentication/amex-gbt-authentication.yml`
- `scopes/amex-gbt-scopes.yml`
- `conventions/amex-gbt-conventions.yml`

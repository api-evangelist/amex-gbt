---
name: Provision travellers into Egencia over SCIM 2.0
description: Create, retrieve, search, replace, patch and delete traveller profiles across the three concurrent SCIM versions, including the Egencia extension schema.
api: openapi/amex-gbt-user-sync-api-openapi.json
operations: [createUser, getUser, getUsers, updateUser, patchUser, deleteUserUser, createUser_1, getUser_1, getUsers_1, updateUser_1, patchUser_1, deleteUserUser_1, createUser_2, getUser_2, updateUser_2, patchUser_2, deleteUserUser_2, getUserByCondition]
generated: '2026-07-28'
method: generated
source: >-
  openapi/amex-gbt-user-sync-api-openapi.json,
  https://apis.egencia.com/openconnect/v1/api-info?name=User (verbatim SCIM conformance statement)
---

# Provision travellers into Egencia over SCIM 2.0

The User Sync API is the one genuinely portable interface in this estate. Egencia states it
directly: the API *"supports SCIM, or System for Cross-domain Identity Management, an open
standard that allows automating user provisioning using REST API and JSON"* and *"follows version
2.0 of the SCIM protocol."* Work you do here transfers to any other SCIM consumer — except for the
Egencia extension attributes, which do not.

## Pick a version first

Three versions run concurrently. Choose one and stay on it.

- **v3 (`/scim/v3/Users`)** — current. Announced 2026-03-16: *"introduces the ability to manage
  global user profiles with approval access programmatically ... the underlying logic has been
  enhanced to support global entity relationships across different points of sale."* Response
  schema is unchanged from v2. Use this for new integrations.
  Operations: `getUsers`, `createUser`, `getUser`, `updateUser`, `patchUser`, `deleteUserUser`.
- **v2 (`/scim/v2/Users`)** — added 2024-07-26, *"offers a way to retrieve all users of a company
  and addresses SCIM compliant issues existing in v1"*.
  Operations: `getUsers_1`, `createUser_1`, `getUser_1`, `updateUser_1`, `patchUser_1`,
  `deleteUserUser_1`.
- **v1 (`/scim/v1/users`)** — legacy, and shaped differently: it has **no list operation**, and
  search is a POST body (`getUserByCondition` — `POST /scim/v1/users/search`) rather than a
  filtered GET.
  Operations: `createUser_2`, `getUser_2`, `updateUser_2`, `patchUser_2`, `deleteUserUser_2`,
  `getUserByCondition`.

Note the operationId suffixes are inverted relative to the version number — `_1` is v2 and `_2` is
v1. Bind by path, not by guessing from the name.

## Steps

1. **Authenticate** — see `skills/amex-gbt-authenticate.md`. Base URL
   `https://apis.egencia.com/openconnect/api`. Media type `application/scim+json`.
2. **Discover the schemas** if you are generating a mapping:
   `getSchemas` — `GET /v2/Schemas`, and `getSchemaById` — `GET /v2/Schemas/{id}`. Both live on
   the service-level definition `openapi/amex-gbt-service-openconnect-openapi.json`.
3. **Create a traveller** — `createUser` (`POST /scim/v3/Users`). Populate the SCIM core `User`
   schema plus `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User`, and then the Egencia
   extension.
4. **Carry the Egencia extension.** `urn:ietf:params:scim:schemas:extension:egencia:2.0:User`
   carries `companyId`, `singleSignOnId`, `arrangers`, `approvers` and `customDataFields`. This is
   where the travel programme's actual configuration lives — the approval hierarchy, the arranger
   relationships and the cost-allocation coding. Set `singleSignOnId` to *your* identity system's
   key: it is customer-controlled and therefore the one user identifier that travels if you ever
   leave.
5. **Reconcile** with `getUsers` (`GET /scim/v3/Users`) using standard SCIM filter and pagination;
   the response is `urn:ietf:params:scim:api:messages:2.0:ListResponse`.
6. **Update** with `patchUser` for partial changes and `updateUser` for a full replace.
   `deleteUserUser` removes the profile.

## Rules

- **`409 Conflict` means the user already exists** — the SCIM create surface is the only place in
  the estate that declares 409. Reconcile by `userName`/`singleSignOnId` before creating.
- **`400` on SCIM specifically means schema non-compliance**: *"request body cannot be read as it
  does not comply with expected schema"*. `422` is a valid-schema-but-invalid-value error.
- **`503 Service Unavailable`** is declared broadly across the SCIM surface — back off and retry
  reads, but see the idempotency warning below before retrying writes.
- **No idempotency key exists.** A retried `createUser` after an ambiguous timeout can produce a
  duplicate; guard with a `getUsers` filter check rather than a blind retry.
- **Custom data field values must exist before you reference them.** Create them with
  `createCdfValue` (`openapi/amex-gbt-company-cdf-api-openapi.json`) first.
- **Erasure is separate.** `DELETE /gdpr?user_id=` on each service-level definition is a GDPR
  erasure operation, not a SCIM delete, and it is irreversible. Validate first with
  `GET /gdpr?user_id=`.

## Related

- `data-model/amex-gbt-data-model.yml`
- `conformance/amex-gbt-conformance.yml`
- `errors/amex-gbt-error-codes.yml`

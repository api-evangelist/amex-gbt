# American Express Global Business Travel (amex-gbt)

American Express Global Business Travel (Amex GBT, NYSE GBTG, headquartered in New York) is the largest business-to-business travel platform in the world, operating in more than 140 countries under the Amex GBT, Egencia and Ovation brands, with CWT acquired in September 2025. Its home market is the United States. In the travel distribution chain Amex GBT is an intermediary rather than a supplier — a travel management company that aggregates air, hotel, rail, car and ground content sourced through the GDS layer, through NDC and through direct supplier connections, then resells it to corporate travel programmes with policy, approval, duty-of-care and reporting wrapped around it. It holds no inventory of its own, but it does hold the booking record, and that is where the switching cost lives. Its API posture is genuinely open at the documentation layer and firmly closed at the runtime layer. The Egencia Developer Center publishes thirteen named APIs and SPIs, and every one of them serves a real, anonymously retrievable OpenAPI 3.1.0 document from apis.egencia.com — no login, no key, no click-through. The identity surface is standards-based (SCIM 2.0 user provisioning over OAuth 2.0 client credentials); the booking, approval, expense, receipt, duty-of-care and reporting surfaces are proprietary Egencia shapes with no OpenTravel, HTNG or IATA NDC schema anywhere in them. Runtime access is customer-account gated — Egencia's own documentation states that "the values for client id and client secret will be provided to the Client after on-boarding to Egencia API platform", and every production endpoint returns 401 to an anonymous caller. So — public contracts, gated runtime, a real documented bulk data export in the BI Transactions API, and API terms Egencia reserves the right to change in its sole discretion.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/amex-gbt/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/amex-gbt/refs/heads/main/apis.yml)

## Tags

- Travel
- United States
- Corporate Travel
- Travel Management
- Business Travel
- Distribution
- Booking
- Aviation
- Hotels
- Rail
- Car Rental
- Expense
- Duty of Care
- Reporting

## Timestamps

- **Created:** 2026-07-28
- **Modified:** 2026-07-28

## APIs

### Egencia User Sync API

SCIM-based user provisioning for an Egencia corporate travel programme. Egencia's own overview states the API "supports SCIM, or System for Cross-domain Identity Management, an open standard that allows automating user provisioning using REST API and JSON" and that it "follows version 2.0 of the SCIM protocol". Three concurrent versions are published — `/scim/v1/users`, `/scim/v2/Users` and `/scim/v3/Users` — supporting create, retrieve, search, replace, patch and delete of traveller profiles, with an Egencia SCIM extension schema (`urn:ietf:params:scim:schemas:extension:egencia:2.0:User`) carrying companyId, singleSignOnId, arrangers, approvers and custom data fields.

- **Human URL:** [Developer Center — User Sync API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/user-sync-api)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-user-sync-api-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [API Reference](https://apis.egencia.com/openconnect/docs/api-docs/UserSyncAPI)
- [Documentation](https://apis.egencia.com/openconnect/v1/api-info?name=User)

### Egencia Context SSO API

Single sign-on entry point that carries contextual data into the Egencia booking flow at authentication time. Documented endpoints are `GET /v1/newTrip` and `GET /v2/startTrip`, which accept trip context and custom data fields such as TravelRequest, PurchaseOrder and MissionOrder, pre-populate the search form and checkout, and flow that context back out through the Expense SPI.

- **Human URL:** [Developer Center — Context SSO API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/context-sso-api)
- **Base URL:** `https://apis.egencia.com/openconnect-sso-service`

#### Properties

- [OpenAPI](openapi/amex-gbt-sso-context-api-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect-sso/docs/api-docs/sso)
- [Documentation](https://www.egencia.com/openconnect-sso-service/v1/api-info)

### Egencia Company Details API

Retrieves company information for an Egencia corporate account — name, display name and related detail — plus e-commerce settings and an audit view of those settings.

- **Human URL:** [Developer Center — Company Info API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/company-info-api)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-company-info-api-openapi.json)
- [API Reference](https://apis.egencia.com/company/docs/api-docs/company-info-api)
- [Documentation](https://apis.egencia.com/company/v1/api-info?name=company-details)

### Egencia Company CDF API

Manages custom data fields — the client-defined fields Egencia describes as capturing "invoicing, reporting, approval, billing" detail, commonly department, billing unit, reason for travel or project code. A customer's own cost-allocation taxonomy is modelled inside Egencia's schema and keyed by Egencia definition and value IDs.

- **Human URL:** [Developer Center — Company CDF API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/cdf-api)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-company-cdf-api-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect/docs/api-docs/CompanyCDFAPI)
- [Documentation](https://apis.egencia.com/openconnect/v1/api-info?name=CompanyCustomData)

### Egencia Validation SPI

A service provider interface, not a consumable API — Egencia calls the customer. At checkout Egencia posts the booking payload to a web service the customer must build, and the customer's response authorises or blocks the booking. Published contract: `POST /v1/validateFields`.

- **Human URL:** [Developer Center — Validation SPI](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/validation-spi)
- **Base URL:** `https://apis.egencia.com/openconnect-validation-service`

#### Properties

- [OpenAPI](openapi/amex-gbt-validation-spi-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect-validation/docs/api-docs/validation)
- [Documentation](https://www.egencia.com/openconnect-validation-service/v1/api-info)

### Egencia Expense SPI

Near real-time push of booking and expense data out of Egencia into a customer's expense or ERP system, carrying either receipt or invoice information depending on company configuration, with retries if the consumer does not acknowledge.

- **Human URL:** [Developer Center — Expense SPI](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/expense-spi)
- **Base URL:** `https://apis.egencia.com/openconnect-expensestream-service`

#### Properties

- [OpenAPI](openapi/amex-gbt-expense-spi-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect-expense/docs/api-docs/expense)
- [Documentation](https://www.egencia.com/openconnect-expensestream-service/v1/api-info)

### Egencia Get Booking API

Retrieval of a booking and its individual trip items, plus the receipts attached to an item, paired with a Get Booking SPI push notification. Booking and item identifiers are Egencia keys; the supplier PNR and record locator travel with the data.

- **Human URL:** [Developer Center — Booking API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/booking-api)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-booking-api-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect/docs/api-docs/GetBookingAPI)
- [Documentation](https://apis.egencia.com/openconnect/v1/api-info?name=Booking)

### Egencia Expense Cancellation and Deletion API

Trip-level cancellation and deletion of bookings — `POST /v1/bookings/{bookingId}/cancel` and `/delete` — acting on every trip item in the booking.

- **Human URL:** [Developer Center — Cancellation and Deletion API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/cancel-delete-api)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-cancellation-deletion-api-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect/docs/api-docs/CancellationDeletionAPI)
- [Documentation](https://apis.egencia.com/openconnect/v1/api-info?name=CancelAndDelete)

### Egencia Approval Workflow API

Programmatic approval or denial of booking requests, at trip level and at trip-item level.

- **Human URL:** [Developer Center — Approval API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/approval-api)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-approval-workflow-api-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect/docs/api-docs/ApprovalWorkflowAPI)
- [Documentation](https://apis.egencia.com/openconnect/v1/api-info?name=Approval)

### Egencia Approval Customisation SPI

An outbound service provider interface that lets a customer decide, at checkout time, whether level one and level two approval are required and who the approvers are. Published contract: `POST /v1/custom-approval`.

- **Human URL:** [Developer Center — Approval Customisation SPI](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/approval-customisation-spi)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-approval-customisation-spi-openapi.json)
- [API Reference](https://apis.egencia.com/approval/docs/api-docs)
- [Documentation](https://www.egencia.com/openconnect-approval-service/v1/api-info)

### Egencia Receipt API

Retrieval of the receipt for a booked trip item via `GET /v1/receipts/{itemId}`, paired with a Receipt SPI push whose documented payload carries booking_id, item_id, product_type, company_id, traveler_id, organization_parent_id and a HAL-style `_links.receipt` href back into the Egencia API.

- **Human URL:** [Developer Center — Receipts API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/receipts-api)
- **Base URL:** `https://apis.egencia.com/openconnect/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-receipt-api-openapi.json)
- [API Reference](https://apis.egencia.com/openconnect/docs/api-docs/ReceiptAPISPI)
- [Documentation](https://apis.egencia.com/openconnect/v1/api-info?name=Receipt)

### Egencia Duty of Care API

Traveller-tracking data for risk and duty-of-care programmes — `POST /v1/bookings` creates a paginated query of booking data for a partner ID over a date range, then `GET /v1/bookings/{resourceId}` pages the result.

- **Human URL:** [Developer Center — Duty of Care API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/doc-api)
- **Base URL:** `https://apis.egencia.com/dutyofcare/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-duty-of-care-api-openapi.json)
- [API Reference](https://apis.egencia.com/dutyofcare/docs/api-docs/doc-service)
- [Documentation](https://apis.egencia.com/dutyofcare/v1/api-info)

### Egencia Reporting API (BI Transactions)

Consolidated booking transaction data out of Egencia, and the closest thing in the estate to a documented exit path. `POST /v1/transactions` creates a filtered report over a date range and returns pagination links; `GET /v1/transactions/{reportId}` pages the result. Line-of-business variants cover air, hotel, car, train, ground and fees. Response schemas carry `pnr`, `record_locator`, marketing/operating/ticketing carrier codes, `hotel_chain_code`, `ticket_code`, `confirmation_number` and `invoice_number` alongside Egencia's own `itinerary_number`. A separate `GET /v1/egencia_iata` returns the IATA agency code for a point of sale, and an `is_ndc` flag records whether an air booking was made through NDC.

- **Human URL:** [Developer Center — Reporting API](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-details/reporting-api)
- **Base URL:** `https://apis.egencia.com/bi/api`

#### Properties

- [OpenAPI](openapi/amex-gbt-reporting-api-openapi.json)
- [API Reference](https://apis.egencia.com/bi/docs/api-docs/transaction-data-service)
- [Documentation](https://apis.egencia.com/bi/v1/api-info)

## Common Properties

- [Website](https://www.amexglobalbusinesstravel.com/)
- [Developer Portal](https://www.amexglobalbusinesstravel.com/egencia-developer-center/)
- [Documentation](https://www.amexglobalbusinesstravel.com/egencia-developer-center/api-overview)
- [Authentication](https://apis.egencia.com/auth/v1/token) — OAuth 2.0 client credentials
- [OpenConnect service definition](openapi/amex-gbt-service-openconnect-openapi.json)
- [BI service definition](openapi/amex-gbt-service-bi-openapi.json)
- [Duty of Care service definition](openapi/amex-gbt-service-dutyofcare-openapi.json)
- [Company service definition](openapi/amex-gbt-service-company-openapi.json)
- [Postman Collection](https://www.postman.com/egenciaapi/egencia-api/collection/n9n3gk7/egencia-api)
- [Egencia](https://www.amexglobalbusinesstravel.com/egencia/)
- [LinkedIn](https://www.linkedin.com/company/american-express-global-business-travel/)

## Switching Cost

See [review.yml](review.yml) for the full evidence trail. In summary:

- **Interface shape:** standard-plus-proprietary. SCIM 2.0 and OAuth 2.0 are real and load-bearing on the identity surface; everything else is a bespoke Egencia shape with no OpenTravel, HTNG or NDC schema.
- **Second source:** alternatives-with-migration. BCD Travel, CTM, Navan, TravelPerk, SAP Concur Travel and CWT (now owned by Amex GBT) are real substitutes, but none of them speaks Egencia's booking, CDF or reporting shapes.
- **Exit path:** bulk-export-documented — `POST https://apis.egencia.com/bi/api/v1/transactions`, paginated via `GET /v1/transactions/{reportId}`. A `DELETE /gdpr?user_id=` erasure operation exists on every service; it deletes, it does not export.
- **Identifier portability:** PNR / record locator, IATA agency code per point of sale, carrier codes and hotel chain codes travel; Egencia `itinerary_number`, `booking_id`, `item_id`, `company_id`, `traveler_id` and SCIM `userId` do not.
- **Access gate:** customer-account-required. "The values for client id and client secret will be provided to the Client after on-boarding to Egencia API platform."

---
name: Receive the Egencia Expense and Booking SPI push, then fetch receipts
description: Build the customer-hosted listener Egencia calls on every booking event, and use the pushed identifiers to pull the booking detail and its receipt or invoice documents.
api: openapi/amex-gbt-expense-spi-openapi.json
operations: [pushExpense, pushSubscriptionNotification, getBookingProduct, getBookingItem, getReceipt, getReceiptsAsZip]
generated: '2026-07-28'
method: generated
source: >-
  openapi/amex-gbt-expense-spi-openapi.json, openapi/amex-gbt-booking-api-openapi.json,
  openapi/amex-gbt-receipt-api-openapi.json,
  https://www.egencia.com/openconnect-expensestream-service/v1/api-info,
  https://apis.egencia.com/openconnect/v1/api-info?name=Booking
---

# Receive the Egencia Expense and Booking SPI push, then fetch receipts

This is an **SPI**, not an API: *you* build and host the web service, and Egencia calls it. The
published OpenAPI describes the listener you must implement, which is why its `servers[]` entry is
the placeholder `/some.base.path`. Getting this right is the principal outbound data path for a
customer's own travel spend.

## Build the listener

1. **Implement `pushExpense`** — `POST {your-base}/{your-path}`. Egencia calls it *"for each new
   and updated booking as well as for associated service fees"*. The payload mirrors what a human
   sees on the Egencia trip summary: `Expense` with `Flight`, `Hotel`, `Car`, `Train`,
   `GlobalGround`, `Fee`, `Payment`, `ReceiptInfo`, `Person`, `Company`, `CustomDataField`,
   `PolicyCompliance`, `CO2Emission` and `TicketInformation`.
2. **Implement `pushSubscriptionNotification`** — `POST {your-base}/v1/subscriptions`, carrying
   `Subscription`, `SubscriptionEntity` and `SubscriptionEvent`. Egencia's guidance: *"don't forget
   to subscribe to the receipts/invoices, which ensures that only eligible audience can see
   invoices and receipts."*
3. **Read the headers** Egencia sends: `message_timestamp` (*"Time Stamp in format ISO DATE
   TIME"*) and `SGP-Request`.
4. **Acknowledge fast.** *"The web service returns a success message if the message has been taken
   into account. If the consumer web service doesn't reply on due time Expense SPI will retry few
   times."* If you return `429`, `502`, `503` or `504`, Egencia retries **three times** and then
   stops. Return `200` on receipt and process asynchronously.
5. **Secure it yourself.** The SPI contracts declare **no** securityScheme — Egencia defines no
   signature, shared secret or mutual-TLS scheme. Authenticate the caller with whatever your own
   listener enforces (IP allow-listing plus a shared secret in a header is the usual minimum), and
   do not treat an unauthenticated POST as trustworthy.

## Events you will receive

Egencia enumerates them: booking created; item added to an existing trip; approval status change
(`AWAITING APPROVAL` → `BOOKED`); ticket issued; receipt or invoice generated; ticket exchanged and
any resulting extra payment; booking cancelled and its credit note. *"In brief, any modification of
the Egencia Trip summary is conveyed to the client's listener web service by Expense SPI in real
time."*

The separate **Get Booking SPI** push is thinner — it carries `mission_number`, `booking_id`,
`company_id`, `organization_parent_id`, `partner.name` and an `items[]` array of
`id`/`traveler_id`/`product_type`/`status`/`receipt_info`/`booking_date_time`/`is_agent_assisted` —
and expects you to fetch the detail.

## Then pull the detail

1. **Authenticate** — see `skills/amex-gbt-authenticate.md`. Base URL
   `https://apis.egencia.com/openconnect/api`.
2. `getBookingProduct` — `GET /v1/bookings/{bookingId}` for the whole trip, or `getBookingItem` —
   `GET /v1/bookings/{bookingId}/items/{itemId}` for one item.
3. **Receipts and invoices.**
   - `getReceipt` — `GET /v1/receipts/{itemId}` (Receipt API) or
     `GET /v2/bookings/{bookingId}/items/{itemId}/receipts/{receiptId}` (Booking API).
   - `getReceiptsAsZip` — `GET /v2/bookings/{bookingId}/items/{itemId}/receipts` returns an
     **archive**, because *"each booking can have a collection of invoices and credit notes"*.
     Unarchive after download.
   - A **receipt** is one PDF per booking no matter how many payments occur; an **invoice** is one
     document per payment or reimbursement.
   - Check `receipt_provision_available` (added 2026-02-09) *before* expecting a document: when it
     is `false`, Egencia will not issue the receipt and the traveller must get it from the
     supplier.
   - `EGE-ER-OS-4008` (404) means the receipt is genuinely unavailable for that item. Egencia's
     instruction: *"please check online on the Egencia site before asking for support, most likely
     the invoice or receipt you're looking for doesn't exist."*

## Reconciliation

There is **no replay, backfill or dead-letter endpoint**. If your listener is down past the
three-retry budget you will silently miss events. Reconcile with the pull side — the Get Booking
API for individual trips, or the Reporting API (`skills/amex-gbt-export-transactions.md`) for a
date range — rather than assuming the push stream is complete.

## Related

- `asyncapi/amex-gbt-webhooks.yml`
- `errors/amex-gbt-error-codes.yml`
- `changelog/amex-gbt-changelog.yml`

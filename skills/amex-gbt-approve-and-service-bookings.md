---
name: Approve, deny, cancel and delete Egencia bookings
description: Drive the trip and trip-item approval workflow and the cancellation/deletion surface safely, including the per-item partial-failure semantics.
api: openapi/amex-gbt-approval-workflow-api-openapi.json
operations: [approveBooking, denyBooking, approveBookingItem, denyBookingItem, cancelBooking, deleteBooking, cancelBookingItem, deleteBookingItem, getBookingProduct, getBookingItem]
generated: '2026-07-28'
method: generated
source: >-
  openapi/amex-gbt-approval-workflow-api-openapi.json,
  openapi/amex-gbt-cancellation-deletion-api-openapi.json,
  openapi/amex-gbt-booking-api-openapi.json
---

# Approve, deny, cancel and delete Egencia bookings

These are the write operations in the estate and every one of them changes a real corporate travel
booking. There is **no idempotency contract** — treat each call as one-shot and verify by reading
back, never by blind retry.

## Steps

1. **Authenticate** — see `skills/amex-gbt-authenticate.md`. Base URL
   `https://apis.egencia.com/openconnect/api`.
2. **Read the booking first.** `getBookingProduct` — `GET /v1/bookings/{bookingId}` returns the
   booking and its trip items; `getBookingItem` — `GET /v1/bookings/{bookingId}/items/{itemId}`
   returns one item. Item `status` is one of `DRAFT`, `PENDING`, `APPROVED`, `DENIED`, `CANCELLED`.
   Do not act on an item that is not `PENDING`.
3. **Approve or deny.**
   - Whole trip: `approveBooking` — `POST /v1/bookings/{bookingId}/approve`;
     `denyBooking` — `POST /v1/bookings/{bookingId}/deny`.
   - One item: `approveBookingItem` — `POST /v1/bookings/{bookingId}/items/{itemId}/approve`;
     `denyBookingItem` — `POST /v1/bookings/{bookingId}/items/{itemId}/deny`.
   - Both item-level operations accept an optional `level` query parameter with the enum
     `ONE | TWO | SECURITY`. *"Targets a specific hierarchical approver level or security level.
     If not provided, the first approver is used."*
   - A `200` response with `"status": "PENDING"` is **not** a failure — it means the item still
     needs a further approval level. Only `APPROVED` or `DENIED` is terminal.
4. **Cancel or delete.**
   - Whole trip: `cancelBooking` — `POST /v1/bookings/{bookingId}/cancel`;
     `deleteBooking` — `POST /v1/bookings/{bookingId}/delete`. Both act on **every** item in the
     booking.
   - One item: `cancelBookingItem` — `POST /v1/bookings/{bookingId}/items/{itemId}/cancel`;
     `deleteBookingItem` — `POST /v1/bookings/{bookingId}/items/{itemId}/delete`.
   - Cancel applies to confirmed items; delete applies to draft items.

## Partial failure is the normal case

Trip-level cancel and delete return a per-item status list, and a `500` here does **not** mean
nothing happened:

```json
{
  "booking_id": "123",
  "status": "FAILURE",
  "error_code": "EGE-ER-OS-5011",
  "error_message": "There was an error in cancelling the [1234, 9876] items. Please try again.",
  "items": [
    {"id": "1",    "status": "CANCELLED"},
    {"id": "1234", "status": "FAILURE"},
    {"id": "9876", "status": "FAILURE"}
  ]
}
```

Always parse `items[]` and retry **only** the ids that came back `FAILURE`, at item level. Never
re-issue the trip-level call — you will re-attempt items that already succeeded.

## Error codes to branch on

- `EGE-ER-OS-4001` (403) — insufficient permission to approve/deny. Contact the account admin;
  retrying will not help.
- `EGE-ER-OS-4002` (404) — invalid or non-existent booking or item.
- `EGE-ER-OS-5004` (500) — approve/deny failed for the listed item ids; the message names them.
- `EGE-ER-OS-5005` (500) — *"Approval is not required for this trip."* This is a semantic
  condition dressed as a 500. Do not retry — read the booking and stop.
- `EGE-ER-OS-5011` / `EGE-ER-OS-5012` (500) — cancel/delete failed for the listed item ids.
- `EGE-ER-OS-4008` (404) — receipt unavailable for the item.
- `501 Not Implemented` is declared on the approval operations — treat as a permanent capability
  gap for that company configuration, not a transient error.

Full registry: `errors/amex-gbt-error-codes.yml`.

## If you own the approval policy

The Approval Customisation SPI (`getApprovalDetails`, `POST /v1/custom-approval`) is the inverse
direction: Egencia calls **your** web service at checkout with the booking data, and your response
decides whether level one and level two approval are required and who the approvers are. Similarly
the Validation SPI (`validateFields`) can block a booking outright. See
`asyncapi/amex-gbt-webhooks.yml` before building either.

## Related

- `conventions/amex-gbt-conventions.yml`
- `agentic-access/amex-gbt-agentic-access.yml`
- `data-model/amex-gbt-data-model.yml`

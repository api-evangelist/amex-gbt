---
name: Locate travellers with the Duty of Care API
description: Run the two-phase duty-of-care query for a partner ID over a date range and page every air, hotel, car and rail record.
api: openapi/amex-gbt-duty-of-care-api-openapi.json
operations: [getDutyOfCareData, getBookings]
generated: '2026-07-28'
method: generated
source: >-
  openapi/amex-gbt-duty-of-care-api-openapi.json, https://apis.egencia.com/dutyofcare/v1/api-info
---

# Locate travellers with the Duty of Care API

This is the surface a risk management vendor integrates against to find travellers during a
disruption. Like the Reporting API it is two-phase: POST creates a paginated result resource, GET
pages it.

## Steps

1. **Authenticate** — see `skills/amex-gbt-authenticate.md`. Base URL
   `https://apis.egencia.com/dutyofcare/api`. `Content-Type: application/json;charset=UTF-8`,
   `Accept: application/json`.
2. **Create the query.** `getDutyOfCareData` — `POST /v1/bookings` with:
   - `partner_id` (**required**, integer) — *"Unique identifier provided to partner by Egencia."*
   - `company_id` (optional, array of integers) — **maximum 10 per request**; exceeding it returns
     `EGE-ER-DS-EXCEEDED-COUNT-COMPANY-ID`.
   - `start_date_time` / `end_date_time` (optional, ISO 8601 `YYYY-MM-DDTHH:MM:SSZ`, GMT default).

   Date-range behaviour, verbatim: a future date is not allowed as a start date; *"If the only
   start date-time is given, the range will be automatically set to next 24 hours from the start
   date-time. If the only end date-time is given, the range will be automatically set to last 24
   hours from the end date-time. Missing both start and end date-time will be considered as the
   latest 24 hours."*

   The response carries only `metadata` and `_links.next.href` — no records.
3. **Page the records.** `getBookings` — `GET /v1/bookings/{resourceId}`, optionally `?page=N`.
   Follow `_links.next.href` until it is `null`. `metadata` gives `total_pages`, `total_records`,
   `page_limit` and `current_page`.
4. **Stop cleanly.** Two informational codes arrive as `200`, not errors:
   - `EGE-ER-DS-BOOKINGS-NOT-AVAILABLE` — *"No bookings available for this particular request"*
   - `EGE-ER-DS-NO_MORE_BOOKINGS` — *"All records have been fetched. No more bookings available."*
     This is how pagination terminates.

## Reading the records

One record **per line of business**, not per trip: *"Each booking (Air segment booking, Train
segment booking, Car segment booking, Hotel segment booking) is considered as separate records with
unique Record Locators. If there are multiple bookings under one trip id, they will be treated as
separate records."* Join on `trip_id` to reassemble a traveller's itinerary.

Each record carries `record_locator`, `company_id`, `trip_id`, `booking_status`,
`line_of_business` (`FLIGHT`/`AIR`, `HOTEL`, `CAR`, `TRAIN`), `booking_reference`, `gds_reference`,
`traveler_details[]` (name, email, phone, and the company's `custom_data_fields`), and exactly one
of `air_segment_details[]`, `hotel_segment_details[]`, `car_segment_details[]` or
`train_segment_details[]`.

Hotel and rail segments carry latitude and longitude; air and car segments carry IATA codes. That
is what makes geofencing a disruption possible.

## Error codes to branch on

- `EGE-ER-DS-INVALID-PARTNER` — invalid Partner ID.
- `EGE-ER-DS-COMPANY-NOT-CONFIGURED` — a company ID is not configured for that partner.
- `EGE-ER-DS-COMPANY-ID-EMPTY` — `company_id` cannot be empty in the request body.
- `EGE-ER-DS-OVERLAPPED-DATE` — start date later than end date.
- `EGE-ER-DS-INVALID-DATE` — wrong format; use `YYYY-MM-DDTHH:MM:SSZ`.
- `EGE-ER-DS-FUTURE-REQUESTED-DATE` — both bounds must be at or before now.
- `EGE-ER-DS-EXCEEDED-COUNT-COMPANY-ID` — more than 10 company IDs.
- `EGE-ER-DS-REQUESTED_PAGE_NOT_FOUND` — *"Cannot directly access this page."* Follow
  `_links.next` rather than constructing page URLs by hand.

Full registry: `errors/amex-gbt-error-codes.yml`.

## Notes

- The result resource is a snapshot created at POST time. For live tracking, re-run the POST;
  do not re-page a stale `resourceId`.
- `resourceId` is an opaque UUID and is not portable outside Egencia.
- Traveller records contain personal data — names, emails, phone numbers and precise locations.
  Handle under the same controls as any PII feed.

## Related

- `data-model/amex-gbt-data-model.yml`
- `conventions/amex-gbt-conventions.yml`

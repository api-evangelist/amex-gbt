---
name: Export booking transaction history from the Reporting API
description: Run the two-phase BI Transactions export - create a filtered report resource, then page it - across all lines of business, and reconcile incrementally.
api: openapi/amex-gbt-reporting-api-openapi.json
operations: [queryTransactions, queryTransactionsForAir, queryTransactionsForHotel, queryTransactionsForCar, queryTransactionsForTrain, queryTransactionsForGround, queryTransactionsForFees, getTransactions, getIATACode]
generated: '2026-07-28'
method: generated
source: >-
  openapi/amex-gbt-reporting-api-openapi.json, https://apis.egencia.com/bi/v1/api-info
  (Developer Guidelines and api_updates, verbatim)
---

# Export booking transaction history from the Reporting API

The BI Transactions API is the documented bulk export of a customer's own travel transaction
history, and the closest thing in this estate to an exit path. It is a **two-phase** API: you POST
a filtered query to create a server-side report resource, then GET that resource page by page.

## Steps

1. **Authenticate** — see `skills/amex-gbt-authenticate.md`. Base URL
   `https://apis.egencia.com/bi/api`. Send `Accept: application/hal+json` and
   `Content-Type: application/json`.
2. **Create the report.** POST the date range and filters to the endpoint that matches the
   granularity you need:
   - `queryTransactions` — `POST /v1/transactions`, the consolidated all-lines-of-business summary
   - `queryTransactionsForAir` — `POST /v1/transactions/air` (ticket, segment and leg detail)
   - `queryTransactionsForHotel` — `POST /v1/transactions/hotel`
   - `queryTransactionsForCar` — `POST /v1/transactions/car`
   - `queryTransactionsForTrain` — `POST /v1/transactions/train` (ticket, segment and leg detail)
   - `queryTransactionsForGround` — `POST /v1/transactions/ground`
   - `queryTransactionsForFees` — `POST /v1/transactions/fees`

   The response carries pagination `metadata` and a HAL `_links.next.href`.
3. **Page the result.** `getTransactions` — `GET /v1/transactions/{reportId}`. Follow
   `_links.next.href` until `next` is `null`. `metadata` gives you `total_pages`, `total_records`,
   `page_limit` and `current_page`; you may also jump to any valid page number directly.
4. **Resolve the agency code if you need it.** `getIATACode` — `GET /v1/egencia_iata` returns the
   IATA agency accreditation number for a point of sale (`iata` + `pos`).

## Rules that will bite you

- **12-month cap from 1 July 2026.** Egencia announced, and reminded three times, that *"Starting
  July 1, 2026, the reporting API will limit historical data extraction to 12 months per
  request."* Chunk any longer backfill.
- **Pull incrementally, not in big fixed windows.** Egencia's guidance: *"When making repeated API
  calls, retrieve data for consecutive dates rather than fixed periods. For daily updates,
  sequentially pull data for each day and only retrieve historical data as necessary."*
- **Use `last_modified_date` and `record_id`** (added across Air, Hotel, Train, Car and Ground in
  June 2026) to reconcile changed rows rather than re-pulling whole periods.
- **North America GDS air is reconciled in batches** with ARC/BSP after the fact, so a row you
  pulled today may be restated. Re-fetch NA air after the reconciliation date if the numbers
  matter.
- **Treat optional attributes as optional.** *"Treat optional attributes as such; incorporate them
  only when available and ensure your code remains functional in their absence."*
- **`ticket_number` is deprecated** in favour of `ticket_code` (announced 2025-11-04), as is
  `class_of_service_code` on Train. Deprecations are announced only in the dated `api_updates[]`
  feed — there is no `Sunset` header and no `deprecated: true` in the spec. Poll
  `https://apis.egencia.com/bi/v1/api-info` if you need to see them coming.
- **Credentials are revoked on account closure.** If this export is being run as part of leaving
  Egencia, run it *before* the relationship ends.

## What travels and what does not

Portable industry keys in the response: `pnr`, `record_locator`, `confirmation_number`,
`ticket_code`, `invoice_number`, `marketing_airline`, `operating_carrier_segment`,
`ticketing_carrier`, `hotel_chain_code`, and the IATA agency code. Egencia-only keys:
`itinerary_number`, `trip_id`, `record_id`, `company_id`. See
`data-model/amex-gbt-data-model.yml`.

## Errors

`400` invalid input, `401` token empty/invalid/expired, `403` not entitled, `404` not found, `422`
invalid or missing required input, `500` unable to process. Handle 4xx yourself; only escalate
5xx. See `errors/amex-gbt-error-codes.yml`.

## Related

- `conventions/amex-gbt-conventions.yml`
- `changelog/amex-gbt-changelog.yml`
- `lifecycle/amex-gbt-lifecycle.yml`

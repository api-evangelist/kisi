---
name: Export and page through access events
description: Query the Kisi access-event log for a place and page through results using offset/limit and the range header.
api: openapi/kisi-openapi-original.yml
operations: [fetchEventTypes, fetchEvents, createEventSet, fetchEventSet]
---

# Export and page through access events

Pull the door/access event stream for auditing or analytics.

## Preconditions
- Base URL `https://api.kisi.io`, JSON only, `Authorization` header auth.
- `POST /event_sets` (`createEventSet`) has a custom rate limit of 1 request/second — pace exports accordingly.

## Steps
1. `fetchEventTypes` — discover the event type vocabulary you can filter on.
2. `fetchEvents` — list events; filter by `place_id` and page with `limit`/`offset`. Read the `X-Collection-Range` response header to know your position and total.
3. For large/asynchronous exports: `createEventSet` to request an event set, then `fetchEventSet` to retrieve it (respect the 1/s limit on creation).

## Conventions
- Pagination is offset/limit; always advance `offset` by the page size and stop when the range header shows you have all rows.
- On `429`, wait at least one second and continue serially.
- Errors follow the `{code,error}` envelope (`errors/kisi-error-codes.yml`).

Cross-reference: `rate-limits/kisi-rate-limits.yml`, `conventions/kisi-conventions.yml`.

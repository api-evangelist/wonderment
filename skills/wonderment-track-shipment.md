---
name: Track a shipment
description: Look up a customer's shipment status and tracking events by order name or tracking code.
api: openapi/wonderment-openapi.json
operations: [searchShipments]
---

# Track a shipment (Wonderment)

Use this skill to answer "where is my order?" for a shop that uses Wonderment
post-purchase tracking.

## Auth
- Send the shop's API token in the `X-Wonderment-Access-Token` header (format
  `sk_live_<shop>_<secret>`). All requests must be HTTPS.
- The token needs shipment read access.

## Steps
1. Call **searchShipments** — `GET /2022-10/shipments/search/{searchTerm}` where
   `searchTerm` is the order name (e.g. `#1001`) or a tracking code.
2. If the shop has customer search authentication enabled, include the optional
   `t` query parameter (a base64-encoded verification payload containing the
   customer `email` or `phone`). Otherwise omit it.
3. A `null` body means no matching order/tracking code was found.
4. On a match, read `shipments[]`. Use `statusDetails` for the most recent event
   (`status`, `substatus`, `date`, `locationDisplay`, `eta`) and `events[]` for
   the full history. `serviceLevel.name` and `carrierName` describe the carrier.

## Notes
- Read-only endpoint — no idempotency concerns.
- `eta` may be the literal `pending` when no estimate is available yet.

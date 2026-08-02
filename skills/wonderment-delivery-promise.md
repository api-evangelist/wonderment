---
name: Quote a delivery date
description: Get delivery-date predictions for shipping methods to a destination zip.
api: openapi/wonderment-openapi.json
operations: [getDeliveryPredictions]
---

# Quote a delivery date (Wonderment Delivery Promise)

Use this skill to show "arrives by" estimates on a product or cart page.

## Auth
- `X-Wonderment-Access-Token` header, HTTPS only.

## Steps
1. Call **getDeliveryPredictions** — `GET /predictions` (served under the
   `/delivery-promise` base path). Required query param `zip` (destination zip
   code). Optional repeated params `shipping_methods` and `variant_ids`
   (form-style, exploded — repeat the key per value).
2. Read `predictions[]`: each has `shippingMethod`, `estimatedDeliveryDate`
   (median), `minDate` (earliest) and `maxDate` (latest).
3. A `404` (`{"message": ...}`) means no predictions were found for the inputs;
   a `400` means an invalid request.

## Notes
- Read-only; safe to call repeatedly.

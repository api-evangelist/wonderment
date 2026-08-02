---
name: Export and download a shipment report
description: List a shop's shipment report exports and download a finished one.
api: openapi/wonderment-openapi.json
operations: [listReports, downloadReport]
---

# Export and download a shipment report (Wonderment)

## Auth
- `X-Wonderment-Access-Token` header (format `sk_live_<shop>_<secret>`), HTTPS only.

## Steps
1. Call **listReports** — `GET /2022-10/reports`. Optional `limit` query param
   (1-500, default 100). Reports come back newest first.
2. Pick a report whose `status` is `finished` and `expired` is `false`. Note its
   `name` (file name) and, if present, `downloadUrl`.
3. Call **downloadReport** — `GET /api/shipment/report/download/{path}` where
   `path` is the URL-encoded report `name`. The response is an
   `application/zip` file download.
4. A `302` redirect means the file could not be found/loaded for the shop; a
   `401` (`{"error":"Unauthorized"}`) means a bad or missing token.

## Notes
- Reports expire (`expiresAt` / `expired`); don't attempt to download expired ones.
- `scheduledReport` describes recurring exports (`frequencyTimeUnit`/`Amount`).

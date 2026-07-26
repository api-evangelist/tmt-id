---
name: Check for SIM swap and device change with Network Biometrics
description: Call the Network Biometrics API to detect a recent SIM change, device change, number recycle or lost/stolen report on a mobile number before trusting an OTP, a login or a payment.
api: openapi/tmt-id-network-biometrics.yml
operations:
- post-network-biometrics
generated: '2026-07-25'
method: generated
source: openapi/tmt-id-network-biometrics.yml, https://tmtid.com/developer/network_biometrics.html
---

# Check for SIM swap and device change with Network Biometrics

One operation — `post-network-biometrics`, `POST /network-biometrics` at
`https://api.phronesis.tech` (or `https://ea.api.phronesis.tech` for early access) — assembles
whichever mobile-network signals you request about one number.

## Credentials

- `API-Token` header
- `API-Secret` header
- `Content-Type: application/json` (mandatory; anything else is a 400)
- Optional `Correlation-Id` header, max 64 characters, echoed back in the response headers

Some features additionally require use-case pre-approval with the data partners before your
account can call them. A `403` with code `212` (`MNO access not authorised`) means a commercial
permission is missing, not a bad key.

## Step 1 — build the request

`discover` is required in **every** request. `assure` and `protect` are optional and
order-insensitive. Never send `discover.device` and `protect.hasDeviceInfo` in the same request —
use `discover.device` when you know the IMEI, `protect.hasDeviceInfo` when you do not.

```json
{
  "discover": { "number": "447700900501" },
  "protect": {
    "hasSimChanged":    { "inSeconds": 900 },
    "hasDeviceChanged": { "inSeconds": 900 },
    "wasReportedLostOrStolen": {}
  }
}
```

The time window is the decision. Pick it from the risk you are pricing — 900 seconds before
accepting an OTP, days before approving a high-value payment.

## Step 2 — read the transaction envelope first

```json
"transaction": { "status": { "value": 0, "message": "transaction successful" },
                 "id": "dad42be1-fdcd-49ee-bd4c-c153befbff35" }
```

`0` is success, `2` is success with partial data. Codes `300`-`304` (returned under HTTP 202) mean
a specific check failed while the rest of the transaction stands. The full 62-code registry is in
`errors/tmt-id-error-codes.yml`. Throttling arrives as `201`/`202`/`203` under HTTP 503 — there is
no 429.

## Step 3 — read each feature block

Event-window features return `changedInTimePeriod` plus a last-event date, and that date is only
populated when the event fell **inside** the window you asked about. A `null` `lastChange` with
`changedInTimePeriod: false` is a clean answer, not missing data.

`assure` features return `isDataAvailable` and `isMatched`. `isMatched` flips true at
`matchConfidence >= 90` for most fields, but email and date of birth require 100. Some data sources
only ever return 0 or 100.

## Step 4 — test without spending

Use the published test personas in `sandbox/tmt-id-sandbox.yml` — 168 MSISDNs that return canned
scenarios without a chargeable MNO call. Examples straight from the docs:

- `447700900501` — simulates a SIM change in the last 10 minutes
- `447700900502` — simulates a device change in the last 10 minutes
- `447700900500` — simulates lost or stolen (yesterday)
- `447700900504` — simulates number recycle in the last 10 minutes
- `447700900505` — simulates an H3G UK `Rate limit exceeded` error
- `447700600287` — always responds with subscriber not found

## Rules

- No idempotency key exists. Retrying is a second billable query — retry only on 503/408 with
  backoff, never on a 4xx.
- `assure.matchingAccountInfo` may incur an additional MNO charge on some networks.
- These are risk signals about real subscribers. Do not present them as identity, age or
  regulatory determinations.

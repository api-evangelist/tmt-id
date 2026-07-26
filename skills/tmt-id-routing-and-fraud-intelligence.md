---
name: Route and screen traffic with Live, Velocity and TeleShield
description: Use TMT Live, Velocity and TeleShield together to decide whether a number is reachable, which network it sits on today, and whether the number range carries fraud propensity — before sending an SMS or placing a call.
api: openapi/tmt-id-velocity.yml
operations:
- GET
also_uses:
- api: openapi/tmt-id-live.yml
  operations: [GET]
- api: openapi/tmt-id-teleshield.yml
  operations: [POST]
- api: openapi/tmt-id-score.yml
  operations: [GET]
generated: '2026-07-25'
method: generated
source: openapi/tmt-id-live.yml, openapi/tmt-id-velocity.yml, openapi/tmt-id-teleshield.yml,
  openapi/tmt-id-score.yml
---

# Route and screen traffic with Live, Velocity and TeleShield

Three cheap lookups answer three different questions before you spend money on a message or a
call. Run them in this order and stop as soon as one says no.

## Credentials — read this first

These products carry credentials in two incompatible ways:

- **Live** (`GET`, `GET /{format}/{key}/{secret}/{number}` at `https://api.tmtvelocity.com/live`)
  and **Velocity** (`GET`, `GET /standard/{format}/{key}/{secret}/{number}` at
  `https://api.tmtvelocity.com`) put the key and secret **in the URL path**. URLs end up in proxy
  logs, browser history and agent transcripts. Never call these from a browser, never let an agent
  persist the URL, and rotate the credentials if one leaks.
- **TeleShield** (`POST` on `https://api.tmtid.com`) uses `X-API-Key` and `X-API-Secret` headers.
- **Score** (`GET`, `GET /score/{number}` at `https://api.tmtid.com`) declares no credential in the
  published spec at all; use the key/secret pair your onboarding issued.

## Step 1 — is the number alive? (Live)

`GET /{format}/{key}/{secret}/{number}` with `{format}` = `JSON`. Live tells you whether the number
is assigned and in active use. A dead number is a wasted send and, for SMS at scale, the whole
business case for this product. Live is also available over ENUM/NAPTR against
`live.tmtvelocity.com` for carrier-side integration.

## Step 2 — which network serves it today? (Velocity)

`GET /standard/{format}/{key}/{secret}/{number}`, `{format}` = `JSON` or `CSV`. Velocity resolves
the **current** operator including ported numbers, which is what least-cost routing actually needs
— the origin network is frequently not the serving network.

## Step 3 — is the range dangerous? (TeleShield)

Three query types, all `POST`, all taking the number in E.164 in the path:

- `/r-teleshield/{number}` — TeleShield Routing: range validity, allocation, service type
- `/f-teleshield/{number}` — TeleShield Fraud: fraud propensity
- `/e-teleshield/{number v2.0}` — TeleShield Enhanced Fraud, v2.0 data dictionary
- `/e-teleshield/{number v1.3}` — the v1.3 data dictionary, still served
- `/teleshield/{number}` — the original combined query

This is where wangiri, IRSF, CLI spoofing and origin-based rating abuse get caught. Note that
TeleShield versions its data dictionary in the *path parameter label*, so pin which variant you
built against.

## Step 4 — optional: score the number

`GET /score/{number}` returns a 0-100 credibility score derived from the age, stability and
behaviour of the number. Use it as a tiebreaker in a decision you already frame, not as the
decision itself.

## Reading results

All four products answer HTTP 200 and put the outcome in `status` / `status_message` inside the
body — `status: 0` is success. A malformed or unauthorised request also comes back as HTTP 200
with a non-zero status (`{"status": 4, "status_message": "Invalid request. Please check
documentation, thank you"}`). Branch on the numeric status.

## Rules

- Every call is billed per query; the Viteza portal gives 500 free queries across Live, Velocity
  and TeleShield Routing to build against.
- No idempotency, no rate-limit headers, no 429 on these four products.
- Cache Velocity answers only as long as your portability tolerance allows — the whole point of the
  product is that the answer changes.

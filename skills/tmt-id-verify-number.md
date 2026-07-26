---
name: Verify a mobile number with TMT Verify
description: Query TMT Verify for operator, portability, subscriber status, roaming, SIM-swap and KYC-match datapoints about one mobile number, requesting only the datapoints you need.
api: openapi/tmt-id-verify.yml
operations:
- POST
generated: '2026-07-25'
method: generated
source: openapi/tmt-id-verify.yml, https://tmtid.com/developer/tmt_verify_v1.html
---

# Verify a mobile number with TMT Verify

TMT Verify answers questions about one MSISDN. There is exactly one operation — `POST` on
`POST /v3/` at `https://api.tmtverify.com` — and the whole contract lives in what you ask for.

## Credentials

Two headers, both required, both issued by TMT ID (Viteza portal for self-serve, or Support during
commercial onboarding):

- `X-API-Key`
- `X-API-Secret`
- `Content-Type: application/json`

These are declared as header *parameters* in the spec rather than as security schemes, so generic
OpenAPI tooling will not wire them for you. See `authentication/tmt-id-authentication.yml`.

## Step 1 — decide the datapoints before you call

Cost and response shape both follow from `dpoints`. Ask for the minimum. Valid values:

`type`, `etype`, `network`, `originnetwork`, `porteddate`, `porting_history`, `subscriberstatus`,
`roaminginfo`, `deactivation_last`, `deactivation_history`, `simswap`, `portfraud`, `tmt_score`,
`online_presence`, `age_verification`, `kycmatch`, `normalize`, `call_forwarding`, `market_segment`.

`deactivation_last` and `deactivation_history` are USA-only.

## Step 2 — call `POST`

```json
{
  "number": "40721987086",
  "dpoints": "network,originnetwork,porteddate,subscriberstatus,roaminginfo"
}
```

The number carries its country code and no `+`.

## Step 3 — read the result, not the HTTP status

The response is keyed by the number you asked about. Success is `status: 0` with
`status_message: "Success"` **inside** that object — a failed query still returns HTTP 200. Branch
on the numeric `status`, never on the HTTP code. See `errors/tmt-id-problem-types.yml`.

```json
{"40721987086": {"current_network": {"mcc": 226, "mnc": 10, "name": "Orange Romania"},
                 "ported": true, "present": "yes", "status": 0, "status_message": "Success"}}
```

## Reading the answers

- `present` is `yes`, `no` or `n/a` — `n/a` means the operator would not say, not that the number is dead.
- `simswap` returns `last_day`, `risk_indicator`, `date` and threshold fields. Compare against your
  own risk window; TMT does not decide for you.
- `kycmatch` returns `kyc_results` scores 0-100 per field. These are match confidences, not identity
  assertions.
- `age_verification.verified` is `0`, `1`, `-1` or `-2`; `threshold` is only present when `verified` is `1`.

## Rules

- Every call is billed. There is no idempotency key and no free retry — a repeat is a repeat charge.
- Throttling is not signalled on this API at all; a contract defines your limits.
- Do not present `kycmatch`, `age_verification` or `tmt_score` output as a legal, regulatory, KYC,
  AML or age determination. TMT ID's own `llms.txt` explicitly forbids that framing.
- Log the number you queried, not the personal data you sent for matching.

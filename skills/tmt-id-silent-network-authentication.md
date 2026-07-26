---
name: Authenticate a user with silent network authentication
description: Run the TMT Authenticate flow — exchange Basic credentials for a PASETO token, ask the operator how it wants the session opened, run silent network authentication or the OTP fallback, then validate the outcome.
api: openapi/tmt-id-authenticate.yml
operations:
- getAccessToken
- getConfig
- authenticateUserWithMNO
- authenticateUserWithOtp
- validateUserSession
generated: '2026-07-25'
method: generated
source: openapi/tmt-id-authenticate.yml, https://tmtid.com/developer/tmt_authenticate_v1.html
---

# Authenticate a user with silent network authentication

TMT Authenticate replaces the SMS OTP with an operator-verified check that the handset holding the
number is the one making the request. It is the only TMT ID product with a multi-step flow and the
only one with a real token.

Hosts: `https://auth-api.tmtanalysis.com/v1` (production) and
`https://auth-staging-api.tmtid.dev/v1` (staging).

## Step 1 — `getAccessToken`

`POST /oauth/token` with HTTP Basic credentials. Returns a **PASETO** bearer token, not a JWT — do
not try to decode it with JWT tooling, and do not expect a JWKS or an OIDC discovery document
(neither exists). Handle `400`, `401` and `503`.

## Step 2 — `getConfig`

`POST /get_config` with the bearer token. The operator serving the number decides how the session
must be opened, and this call tells you which path to take. **Do not skip it and do not cache its
answer across operators** — the config is the contract for this subscriber's network.

## Step 3a — `authenticateUserWithMNO` (the silent path)

`GET /authenticate`. This is the silent network authentication flow and it involves a `302`
redirect through the operator, so it must run on the handset's mobile data connection, not over
Wi-Fi and not server-to-server. A `302` here is part of the flow, not an error. Handle `400`.

## Step 3b — `authenticateUserWithOtp` (the fallback)

`GET /authenticate/otp` when the silent path is unavailable for that operator or that session.
This sends a real one-time password to a real person — it has a user-visible side effect and a
cost. Never call it speculatively, never call it in a loop, and never call it as a "retry" of the
silent path without a user-initiated trigger. Handle `400` and `503`.

## Step 4 — `validateUserSession`

`POST /validate` to confirm whether the number was actually authenticated. This is the only
authoritative outcome — the redirect completing is not proof. Handle `400`.

## Rules

- Treat the PASETO token as a secret with an unspecified lifetime; re-request on `401` rather than
  assuming a fixed TTL.
- There is no idempotency contract on any step. Steps 3a and 3b are side-effecting; re-running
  them starts a new authentication attempt.
- No scopes exist, so a token is all-or-nothing for your account. Scope the blast radius at your
  own layer.
- Fall back to OTP deliberately, log which path was taken, and record the `validate` outcome — the
  operator config that decided the path can change between sessions.

# TMT ID (tmt-id)

TMT ID (trading name of TMT Analysis Limited, London) is a United Kingdom mobile number intelligence and anti-fraud data provider that sits between the mobile network operators and the businesses that need to trust a phone number. Founded in 2017 as TMT Analysis, it acquired Phronesis Technologies in 2023 and rebranded to TMT ID in 2024. It does not own network infrastructure; it aggregates operator, numbering-plan and ENUM data and resells it as real-time REST lookups — number validity and reachability, current network and portability, SIM-swap and device-change events, subscriber-data matching, risk scoring, telephony-fraud and routing intelligence, and silent network authentication as an alternative to SMS OTP.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/tmt-id/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/tmt-id/refs/heads/main/apis.yml)

## Tags

- Telecommunications
- United Kingdom
- Identity Verification
- Mobile Identity
- SIM Swap
- Anti-Fraud
- Number Intelligence
- Silent Network Authentication
- GSMA Open Gateway
- Network APIs
- ENUM
- KYC

## Timestamps

- **Created:** 2026-07-25
- **Modified:** 2026-07-25

## APIs

### TMT Verify API

Real-time verification of a mobile number against mobile network operator data. A single POST returns the requested datapoints for a number — subscriber name and address match, account type and tenure, market segment, roaming state, portability, and a `simswap` block giving the elapsed time since the last recorded SIM change.

- **Human URL:** [https://tmtid.com/developer/tmt_verify_v1.html](https://tmtid.com/developer/tmt_verify_v1.html)
- **Base URL:** `https://api.tmtverify.com`
- [OpenAPI](openapi/tmt-id-verify.yml) · [Documentation](https://tmtid.com/verify/)

### TMT Velocity API

Global mobile number lookup returning the network operator currently serving a number, including ported numbers. Delivered as a single GET with the API key and secret carried in the URL path.

- **Human URL:** [https://tmtid.com/developer/tmt_velocity_v1.html](https://tmtid.com/developer/tmt_velocity_v1.html)
- **Base URL:** `https://api.tmtvelocity.com`
- [OpenAPI](openapi/tmt-id-velocity.yml) · [Documentation](https://tmtid.com/velocity/)

### TMT Live API

Checks whether a mobile number is currently assigned and in active use on a network, for data cleansing, deliverability and pre-send validation. Offered over HTTPS and over ENUM/NAPTR for carrier-side integrations.

- **Human URL:** [https://tmtid.com/developer/tmt_live_v1.html](https://tmtid.com/developer/tmt_live_v1.html)
- **Base URL:** `https://api.tmtvelocity.com/live`
- [OpenAPI](openapi/tmt-id-live.yml) · [Documentation](https://tmtid.com/live/)

### TMT TeleShield API

Telephony fraud and routing intelligence. Five documented POST operations cover TeleShield Routing, TeleShield Fraud and Enhanced Fraud across the v1.3 and v2.0 data dictionaries, returning number-range validity, allocation, service type and fraud propensity for wangiri, IRSF, CLI spoofing and origin-based rating abuse.

- **Human URL:** [https://tmtid.com/developer/tmt_teleshield_v1.html](https://tmtid.com/developer/tmt_teleshield_v1.html)
- **Base URL:** `https://api.tmtid.com`
- [OpenAPI](openapi/tmt-id-teleshield.yml) · [Documentation](https://tmtid.com/teleshield/)

### TMT Score API

Returns a credibility score for a phone number, derived from the age, stability and behaviour of the number across TMT ID's operator data, for risk decisioning at onboarding, login and transaction time.

- **Human URL:** [https://tmtid.com/developer/tmt_score_v1.html](https://tmtid.com/developer/tmt_score_v1.html)
- **Base URL:** `https://api.tmtid.com`
- [OpenAPI](openapi/tmt-id-score.yml) · [Documentation](https://tmtid.com/score/)

### TMT Authenticate API

Silent Network Authentication with OTP fallback. The client exchanges HTTP Basic credentials at `/oauth/token` for a PASETO bearer token, calls `/get_config` to learn how the relevant mobile network operator wants the session opened, runs `/authenticate` or `/authenticate/otp`, and confirms with `/validate`.

- **Human URL:** [https://tmtid.com/developer/tmt_authenticate_v1.html](https://tmtid.com/developer/tmt_authenticate_v1.html)
- **Base URL:** `https://auth-api.tmtanalysis.com/v1`
- [OpenAPI](openapi/tmt-id-authenticate.yml) · [Documentation](https://tmtid.com/authenticate/)

### Network Biometrics API

The Phronesis-derived flagship: a single POST that assembles a configurable set of mobile network signals about a number and device — SIM swap and device change look-back windows, GSMA device blacklist status, reachability, roaming, account type and tenure, personal-data matching — plus the deprecated v2 Number Assurance operations for assured registration and assured age.

- **Human URL:** [https://tmtid.com/developer/network_biometrics.html](https://tmtid.com/developer/network_biometrics.html)
- **Base URL:** `https://api.phronesis.tech`
- [OpenAPI](openapi/tmt-id-network-biometrics.yml) · [Documentation](https://tmtid.com/developers/)

## API Posture

TMT ID sits on the API-native side of the telecom split. [https://tmtid.com/developers/](https://tmtid.com/developers/) is a real, ungated documentation hub (HTTP 200, no login) linking seven ReDoc-rendered OpenAPI 3.0 references, and the [Viteza](https://viteza.tmtanalysis.com/register) portal offers self-serve signup with 500 free queries. There is no published "Download OpenAPI" link — each specification is carried inline in its ReDoc page and was extracted verbatim into `openapi/`.

Production API keys and secrets are still described as "provided to you during the onboarding process": the docs are open, the credentials are sales-issued.

Authentication is API key and secret — `X-API-Key`/`X-API-Secret` headers on Verify and TeleShield, `API-Token`/`API-Secret` on Network Biometrics, and, notably, key and secret carried as **URL path segments** on Velocity and Live. Authenticate exchanges HTTP Basic credentials for a PASETO bearer token. There is no OIDC discovery document and **no CIBA**.

**CAMARA posture:** TMT ID states on its own site that it is a *"GSMA Open Gateway member"*, but publishes no CAMARA-conformant API. The word CAMARA appears nowhere on tmtid.com or in any of the seven specifications. The functional equivalents are all here — SIM swap detection, silent network authentication, device intelligence — shipped under TMT ID's own proprietary schemas rather than CAMARA contracts. Capability has converged; contracts have not.

No TM Forum conformance, no NEF/SCEF surface, no webhooks or AsyncAPI, no GraphQL or gRPC, no public Postman workspace, and no first-party SDK packages on npm or a GitHub organisation.

## Links

- [Website](https://tmtid.com/)
- [Developers](https://tmtid.com/developers/)
- [Viteza portal](https://viteza.tmtanalysis.com/register)
- [Products](https://tmtid.com/products/)
- [News](https://tmtid.com/news/)
- [Trust Centre](https://tmtid.com/trust-centre/)
- [Vulnerability Disclosure](https://tmtid.com/responsible-vulnerability-disclosure-policy/)
- [LinkedIn](https://www.linkedin.com/company/tmtid/)

# TMT ID (tmt-id)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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

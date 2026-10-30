# MandateShield

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

MandateShield is a fail-closed authority-verification and execution-evidence boundary for autonomous AI-agent purchases, operated from Bern, Switzerland. It binds a signed purchase approval (AP2, TAP, UCP, x402, MPP, ACP or a custom envelope) to a registered mandate and a pinned issuer key, reserves cumulative budget atomically, consumes the reservation into a one-use provider-bound execution permit for an exact Stripe PaymentIntent or x402 request, and then checks the provider outcome without trusting the caller before issuing a signed terminal receipt. The hosted service never moves money or holds payment credentials. It publishes an OpenAPI 3.1 contract with 26 operations, a remote MCP server whose tools/list answers anonymously, a signed A2A agent card, llms.txt, an SPDX SBOM, a JWKS, a 91-code reason registry, a 19-vector conformance suite and self-hosted JavaScript, Python and Go clients.

**Areas:** Company · Payments · Agentic Commerce · AI Agents · Payment Authorization · Fraud Prevention · Cryptographic Verification · MCP · A2A · x402 · AP2 · Stripe

## MandateShield on APIs.io

Everything profiled for MandateShield is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/mandateshield-com/](https://apis.io/providers/mandateshield-com/)** — or go straight to one of the APIs or properties below.

### APIs (22)

- [MandateShield MCP Server](https://apis.io/apis/mandateshield-com/mcp-server/) — Hosted, stateless Streamable HTTP MCP server at https://mandateshield.com/api/mcp. Anonymous initialize and tools/list; three tools…
- [MandateShield Payment Authority Agent (A2A)](https://apis.io/apis/mandateshield-com/a2a-agent/) — JSONRPC A2A interface at https://mandateshield.com/a2a serving A2A 1.0 and 0.3 profiles (selected by the A2A-Version header) with three skills: project…
- [MandateShield Account execution control API](https://apis.io/apis/mandateshield-com/mandateshield-com-account-execution-control-api/) — Hosting-authenticated account-owner emergency control. The interlock governs new MandateShield-mediated strict operations inside one account regardless of…
- [MandateShield Advisory intelligence API](https://apis.io/apis/mandateshield-com/mandateshield-com-advisory-intelligence-api/) — Privacy-thresholded context. Signals can require REVIEW but do not automatically block.
- [MandateShield Analysis only API](https://apis.io/apis/mandateshield-com/mandateshield-com-analysis-only-api/) — Policy diagnostics that always return enforcement_authorized=false.
- [MandateShield Deployment activation proofs API](https://apis.io/apis/mandateshield-com/mandateshield-com-deployment-activation-proofs-api/) — Signed, account-bound records of an externally bound subject reaching one server-observed strict live reservation and one fresh credential-bound permit…
- [MandateShield Deployment Proofs API](https://apis.io/apis/mandateshield-com/mandateshield-com-deployment-proofs-api/) — The Deployment Proofs API from MandateShield — 3 operation(s) for deployment proofs.
- [MandateShield Execution Authorizations API](https://apis.io/apis/mandateshield-com/mandateshield-com-execution-authorizations-api/) — The Execution Authorizations API from MandateShield — 1 operation(s) for execution authorizations.
- [MandateShield Execution evidence API](https://apis.io/apis/mandateshield-com/mandateshield-com-execution-evidence-api/) — Provider-bound permit redemption, caller observation intake, Stripe API or x402 chain reconciliation that does not trust the caller assertion, autonomous…
- [MandateShield Execution Permits API](https://apis.io/apis/mandateshield-com/mandateshield-com-execution-permits-api/) — The Execution Permits API from MandateShield — 2 operation(s) for execution permits.
- [MandateShield Execution Receipts API](https://apis.io/apis/mandateshield-com/mandateshield-com-execution-receipts-api/) — The Execution Receipts API from MandateShield — 1 operation(s) for execution receipts.
- [MandateShield Normalize API](https://apis.io/apis/mandateshield-com/mandateshield-com-normalize-api/) — The Normalize API from MandateShield — 1 operation(s) for normalize.
- [MandateShield Preflight API](https://apis.io/apis/mandateshield-com/mandateshield-com-preflight-api/) — The Preflight API from MandateShield — 1 operation(s) for preflight.
- [MandateShield Production authority API](https://apis.io/apis/mandateshield-com/mandateshield-com-production-authority-api/) — Challenge, strict verification, and processor transitions. The documented eight-field invariant establishes only a RESERVED, consumable authorization; a…
- [MandateShield Proof Network API](https://apis.io/apis/mandateshield-com/mandateshield-com-proof-network-api/) — The Proof Network API from MandateShield — 5 operation(s) for proof network.
- [MandateShield Protocol adapters API](https://apis.io/apis/mandateshield-com/mandateshield-com-protocol-adapters-api/) — Stateless documented-field projection for supported agent-payment inputs. X402 and MPP require a canonical payee identity whose rail-specific fields exactly…
- [MandateShield Provider Submissions API](https://apis.io/apis/mandateshield-com/mandateshield-com-provider-submissions-api/) — The Provider Submissions API from MandateShield — 1 operation(s) for provider submissions.
- [MandateShield Public proof network API](https://apis.io/apis/mandateshield-com/mandateshield-com-public-proof-network-api/) — Signed externally bound self-reports. MandateShield verifies domain or exact GitHub-commit binding and claimant-report integrity; it does not run the…
- [MandateShield Public sandbox API](https://apis.io/apis/mandateshield-com/mandateshield-com-public-sandbox-api/) — Zero-account, test-only and request-local lifecycle simulation. It uses ephemeral keys, contacts no external provider or independent organization, retains…
- [MandateShield Receipts API](https://apis.io/apis/mandateshield-com/mandateshield-com-receipts-api/) — Public-key signed-receipt verification and privacy-safe issuance lookup.
- [MandateShield Sandbox API](https://apis.io/apis/mandateshield-com/mandateshield-com-sandbox-api/) — The Sandbox API from MandateShield — 1 operation(s) for sandbox.
- [MandateShield Verify API](https://apis.io/apis/mandateshield-com/mandateshield-com-verify-api/) — The Verify API from MandateShield — 1 operation(s) for verify.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (3)

Model Context Protocol servers that expose the API to agents.

- [mandateshield-com-mcp.yml](https://apis.io/servers/mandateshield-com/mandateshield-com-mcp-yml/)
- [MCP endpoint, no-auth plugin profile](https://apis.io/servers/mandateshield-com/mcp-endpoint-no-auth-plugin-profile/)
- [MCP registry server.json](https://apis.io/servers/mandateshield-com/mcp-registry-server-json/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Mandateshield Com Agentic Access](https://apis.io/agentic-access/mandateshield-com/mandateshield-com-agentic-access/)

#### Overlays (21)

OpenAPI overlays, including conversational phrasing for each operation.

- [Mandateshield Com Account Execution Control Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-account-execution-control-api-phrasing-overlay/)
- [Mandateshield Com Advisory Intelligence Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-advisory-intelligence-api-phrasing-overlay/)
- [Mandateshield Com Analysis Only Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-analysis-only-api-phrasing-overlay/)
- [Mandateshield Com Deployment Activation Proofs Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-deployment-activation-proofs-api-phrasing-overlay/)
- [Mandateshield Com Deployment Proofs Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-deployment-proofs-api-phrasing-overlay/)
- [Mandateshield Com Execution Authorizations Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-execution-authorizations-api-phrasing-overlay/)
- [Mandateshield Com Execution Evidence Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-execution-evidence-api-phrasing-overlay/)
- [Mandateshield Com Execution Permits Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-execution-permits-api-phrasing-overlay/)
- [Mandateshield Com Execution Receipts Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-execution-receipts-api-phrasing-overlay/)
- [Mandateshield Com Normalize Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-normalize-api-phrasing-overlay/)
- [Mandateshield Com Openapi Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-openapi-overlay/)
- [Mandateshield Com Preflight Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-preflight-api-phrasing-overlay/)
- [Mandateshield Com Production Authority Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-production-authority-api-phrasing-overlay/)
- [Mandateshield Com Proof Network Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-proof-network-api-phrasing-overlay/)
- [Mandateshield Com Protocol Adapters Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-protocol-adapters-api-phrasing-overlay/)
- [Mandateshield Com Provider Submissions Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-provider-submissions-api-phrasing-overlay/)
- [Mandateshield Com Public Proof Network Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-public-proof-network-api-phrasing-overlay/)
- [Mandateshield Com Public Sandbox Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-public-sandbox-api-phrasing-overlay/)
- [Mandateshield Com Receipts Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-receipts-api-phrasing-overlay/)
- [Mandateshield Com Sandbox Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-sandbox-api-phrasing-overlay/)
- [Mandateshield Com Verify Api Phrasing Overlay](https://apis.io/overlays/mandateshield-com/mandateshield-com-verify-api-phrasing-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Mandateshield Com Authentication](https://apis.io/security/mandateshield-com/mandateshield-com-authentication/)
- [Mandateshield Com Domain Security](https://apis.io/security/mandateshield-com/mandateshield-com-domain-security/)
- [Mandateshield Com Trust Center](https://apis.io/security/mandateshield-com/mandateshield-com-trust-center/)
- [Mandateshield Com Vulnerability Disclosure](https://apis.io/security/mandateshield-com/mandateshield-com-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Mandateshield Com Plans Pricing](https://apis.io/plans/mandateshield-com/mandateshield-com-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Mandateshield Com Rate Limits](https://apis.io/rate-limits/mandateshield-com/mandateshield-com-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://mandateshield.com/)
- [Documentation](https://mandateshield.com/docs)
- [Getting started](https://mandateshield.com/connect)
- [API reference](https://mandateshield.com/docs)
- [Pricing](https://mandateshield.com/pricing)
- [Sign up](https://mandateshield.com/dashboard)
- [Support](https://mandateshield.com/legal)
- [Changelog](https://mandateshield.com/current-release.json)
- [Blog](https://mandateshield.com/resources)
- [Security](https://mandateshield.com/responsible-disclosure)
- [Trust centre](https://mandateshield.com/trust)
- [Terms of service](https://mandateshield.com/terms)
- [Privacy policy](https://mandateshield.com/privacy)
- [Status](https://mandateshield.com/status)

## API Evangelist rating

**Kin Score 74.7** — exemplar · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 53.4 |
| Contract governance | 18.2 |
| Operational transparency | 63.2 |
| Developer ergonomics | 80.4 |
| Access clarity | 84.2 |

**Agent readiness 39.5** — agent-native

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | verified |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | flavored (partial credit) |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | — |

**Regulatory alignment 50.0** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/mandateshield-com/](https://apis.io/providers/mandateshield-com/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for MandateShield: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/mandateshield-com/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).

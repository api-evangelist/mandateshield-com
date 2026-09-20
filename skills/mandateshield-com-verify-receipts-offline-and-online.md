---
generated: '2026-09-19'
method: generated
name: Verify decision, permit and execution receipts
description: Independently verify a MandateShield signed decision receipt, execution permit or terminal execution
  receipt — online against the API or offline against the published JWKS — and check its public transparency record.
  Verification preserves evidence; it never authorizes a payment.
api: openapi/mandateshield-com-openapi.yml
operations:
- verifyDecisionReceipt
- getReceiptTransparency
- verifyExecutionPermit
- verifyExecutionReceipt
source: Grounded in openapi/mandateshield-com-openapi.yml (OpenAPI 3.1.0, contract v3.4.0, captured 2026-09-19 from
  https://mandateshield.com/openapi.json; verbatim source in openapi/_original/). Every operationId was verified
  in that spec. Auth per authentication/mandateshield-com-authentication.yml, errors per errors/mandateshield-com-problem-types.yml
  and errors/mandateshield-com-error-codes.yml, idempotency and reversibility per conventions/mandateshield-com-conventions.yml,
  limits per rate-limits/mandateshield-com-rate-limits.yml.
---

# Verify decision, permit and execution receipts

Use this when you hold a MandateShield artifact (an ES256 JWS decision receipt, an `MSP+JWT` execution permit or an `MSE+JWT` execution receipt) and need to establish what it proves — for reconciliation, audit or dispute handling.

## Auth
- None. All four operations are public. Base URL `https://mandateshield.com`.
- Offline: fetch the public keys from `https://mandateshield.com/.well-known/jwks.json` (saved at `well-known/mandateshield-com-jwks.json`) or the release-scoped archive `/evidence/v1.13.0/receipt-verification-jwks.json`; the pinned offline verifiers are `/sdk/v1.13.0/mandateshield-receipt-verifier.mjs` and `/sdk/v1.13.0/mandateshield-execution-evidence-verifier.mjs` (checksums in `packages/mandateshield-com-packages.yml`).

## Steps
1. **Decision receipt** — `verifyDecisionReceipt` (`POST /api/v2/receipts/verify`). Body: `compact`, plus `expected_envelope` and `expected_audience`. Only `enforcement_ready=true` confirms that both match the signed receipt and its transparency record; a valid signature alone is not an execution switch.
2. **Public issuance record** — `getReceiptTransparency` (`GET /api/v2/transparency/{receiptId}`, `msr_...`). Compare its privacy-safe receipt hash and digest bindings with your copy. The record is append-only while retained (30/60/90 days by plan); it is not a Merkle ledger.
3. **Execution permit** — `verifyExecutionPermit` (`POST /api/v2/execution-permits/verify`). Body: `compact`, optional `expected_audience`. Returns `cryptographically_valid`, `claims`, and — always — `online_state=UNKNOWN`, `provider_submission_permitted=false`. It cannot tell you whether the permit was redeemed.
4. **Terminal execution receipt** — `verifyExecutionReceipt` (`POST /api/v2/execution-receipts/verify`). Body: `compact`. Read `evidence_class` (`CALLER_ASSERTED` vs `PROVIDER_API_VERIFIED` / `CHAIN_FINALIZED`) and `independent_verification`; `artifact_purpose=HISTORICAL_EXECUTION_EVIDENCE` and `execution_authorized=false` are fixed.

## Rules an agent must follow
- **Verification is evidence, never authority.** None of these calls can start, repeat or restore a payment.
- **`independent_verification=true` means independent of the caller's report**, not an independent organization or audit — the provider says so explicitly.
- **Retain the exact JWK** named by the receipt with your evidence archive; the live JWKS is a mutable first-party source, not key escrow.
- 404 on step 2 means the mandate or transparency record was not found or has aged out of retention.

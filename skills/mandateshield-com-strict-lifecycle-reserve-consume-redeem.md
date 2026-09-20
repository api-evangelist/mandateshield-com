---
generated: '2026-09-19'
method: generated
name: 'Strict lifecycle: challenge, verify, consume, redeem, reconcile'
description: Run the production authority lifecycle — issue a challenge, verify signed authority into a RESERVED
  reservation, CONSUME it with an exact provider binding, redeem the one-use permit at the executor, submit idempotently
  to the provider, and report for caller-independent reconciliation. Role-separated keys; fail closed throughout.
api: openapi/mandateshield-com-openapi.yml
operations:
- createVerificationChallenge
- verifyCryptographicAuthority
- verifyCryptographicAuthorityBatch
- transitionExecutionAuthorization
- verifyExecutionPermit
- redeemExecutionPermit
- reportProviderSubmission
- verifyExecutionReceipt
source: Grounded in openapi/mandateshield-com-openapi.yml (OpenAPI 3.1.0, contract v3.4.0, captured 2026-09-19 from
  https://mandateshield.com/openapi.json; verbatim source in openapi/_original/). Every operationId was verified
  in that spec. Auth per authentication/mandateshield-com-authentication.yml, errors per errors/mandateshield-com-problem-types.yml
  and errors/mandateshield-com-error-codes.yml, idempotency and reversibility per conventions/mandateshield-com-conventions.yml,
  limits per rate-limits/mandateshield-com-rate-limits.yml.
---

# Strict lifecycle: challenge, verify, consume, redeem, reconcile

This is the production path. It is deliberately split across **three trust zones** and **two key roles**, and an agent or model is allowed in only the first. Read `conventions/mandateshield-com-conventions.yml` (idempotency, reversibility) before running it.

## Auth and roles
- `VERIFY` key (`ms_live_...`, Bearer): steps 1–2 only. Cannot transition state.
- `PROCESSOR` key (`ms_live_...`, Bearer, bound at creation to one exact `processor_audience`): steps 3–6 only. Cannot issue challenges or create authorizations.
- Keep both server-side. The PROCESSOR key used for redemption lives **only** inside the customer-deployed exclusive executor next to the provider credential. Never expose either to the agent, model, browser or merchant page.
- One-time setup in the dashboard: an active registered mandate version and a pinned authority-issuer public JWK (RFC 7638 thumbprint, issuer, audience, protocol). Private keys are rejected.

## Steps
1. **Issue a fresh challenge** — `createVerificationChallenge` (`POST /api/v2/challenges`, VERIFY key). Body: `mandate_id`, `protocol`, `key_thumbprint`. Call it immediately before signing. The nonce expires after **five minutes** and is consumable once. Sign the returned `issuer` and `audience` exactly.
2. **Verify and reserve** — `verifyCryptographicAuthority` (`POST /api/v2/verify`, VERIFY key). Body: `envelope` (the exact final PurchaseEnvelope, including its `idempotency_key`) and `evidence` (compact JWS, AP2 SD-JWT+KB-JWT projection, or TAP-shaped RFC 9421 evidence; iat–exp window ≤ 24 h). For 1–25 attempts use `verifyCryptographicAuthorityBatch` (`POST /api/v2/batch`); every item needs its own key and unconsumed challenge.
   **Require the full invariant** before continuing: `decision=ALLOW`, `enforcement_authorized=true`, `mode=live`, `persisted=true`, `assurance.authority_valid=true`, `assurance.key_trust=ACCOUNT_PINNED`, `execution_authorization.state=RESERVED`, `consumable=true`. Anything else is not a reservation. Even a full pass is **not** permission to call the provider.
3. **CONSUME with an exact provider binding** — `transitionExecutionAuthorization` (`POST /api/v2/execution-authorizations`, PROCESSOR key). Body: `action: CONSUME`, `compact` (the `signed_receipt.compact` from step 2), `idempotency_key` (transition-specific, e.g. `<attempt>:consume`), `expected_envelope`, `expected_audience`, and `provider_binding` (`profile` e.g. `STRIPE_PAYMENT_INTENTS_V1`, `environment`, `account`, `request_id`, `payee_destination`, `method`, `resource`, `body_digest`). Accounts created at or after 2026-07-26T12:01:24Z **must** supply `provider_binding`. Expect `provider_submission_permitted=false`, `provider_redemption_required=true` and an `execution_permit.compact` (ES256, max lifetime 60 s, max_uses 1).
4. **Inspect the permit (advisory)** — `verifyExecutionPermit` (`POST /api/v2/execution-permits/verify`, no auth). Confirms signature and audience; returns `online_state=UNKNOWN` and `provider_submission_permitted=false` by design. Offline verification can never grant execution.
5. **Redeem once at the execution edge** — `redeemExecutionPermit` (`POST /api/v2/execution-permits/redeem`, PROCESSOR key, `Idempotency-Key` header and body `idempotency_key`). Body: `compact`, `expected_request` (`provider`, `payee`, `amount`, `resource` read from the permit claims). Proceed **only** if `provider_submission_permitted=true`, `status=CLAIMED` and `idempotent_replay=false`. Then submit the exact provider operation **idempotently** using the returned `provider_idempotency_key`. MandateShield's claim is single-winner; provider delivery is not exactly-once — provider-native idempotency stays mandatory.
6. **Report and reconcile** — `reportProviderSubmission` (`POST /api/v2/provider-submissions`, PROCESSOR key). Body: `provider_submission_id`, `permit_id`, `claim_id`, `payment_reference`, `outcome`, `occurred_at`, optional `provider_observation`. The report is stored as `CALLER_ASSERTED` evidence; MandateShield then reads the configured Stripe PaymentIntent or x402 chain state itself. Only `PROVIDER_API_VERIFIED` or `CHAIN_FINALIZED` evidence finalizes (`authorization_finalized=true`, `independent_verification=true`) and returns an `execution_receipt`.
7. **Retain and verify the terminal receipt** — `verifyExecutionReceipt` (`POST /api/v2/execution-receipts/verify`). The receipt declares `execution_authorized=false` and can never be reused to authorize another operation.

## Rules an agent must follow
- **Never call the payment provider from an agent, an MCP result or an A2A reply.** Only the exclusive executor holding a fresh `CLAIMED` redemption may.
- **Idempotency keys:** distinct, stable keys for VERIFY, CONSUME and the terminal COMMIT/RELEASE; account-wide boundary; rotating keys does not make a consumed attempt executable again.
- **Reversibility (`conventions/` → `reversibility`):** a RESERVED reservation is released by `transitionExecutionAuthorization` `action: RELEASE` or by expiry (`expires_at`); a CONSUMED one may be RELEASEd only for a *confirmed* non-submission and only before the settlement deadline. If submission may have happened but the result is unknown, **do not RELEASE and do not submit again** — leave it for reconciliation. No numeric reservation TTL or settlement deadline is published; do not assume one.
- **Fail closed on transport errors.** 401 = key missing/invalid/revoked; 402/403 = production access paused or ineligible; 409 = replay, binding, budget or illegal transition; 410 = reservation expired; 503 = persistence unavailable (no new authority). Retry only idempotent MandateShield requests; route prolonged uncertainty to manual review.
- **Emergency stop:** the account owner can PAUSE every MandateShield-mediated path via the control plane (`setGlobalExecutionInterlock`, hosting-session auth — not agent-callable).

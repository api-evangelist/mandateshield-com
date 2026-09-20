---
generated: '2026-09-19'
method: generated
name: Rehearse the full lifecycle in the zero-account sandbox
description: Run the isolated eleven-stage strict-lifecycle simulation with no account, no key and no money movement,
  and read each stage the server actually returned. Use it to learn the state machine before touching production.
api: openapi/mandateshield-com-openapi.yml
operations:
- runStrictLifecycleSandbox
- evaluatePurchase
source: Grounded in openapi/mandateshield-com-openapi.yml (OpenAPI 3.1.0, contract v3.4.0, captured 2026-09-19 from
  https://mandateshield.com/openapi.json; verbatim source in openapi/_original/). Every operationId was verified
  in that spec. Auth per authentication/mandateshield-com-authentication.yml, errors per errors/mandateshield-com-problem-types.yml
  and errors/mandateshield-com-error-codes.yml, idempotency and reversibility per conventions/mandateshield-com-conventions.yml,
  limits per rate-limits/mandateshield-com-rate-limits.yml.
---

# Rehearse the full lifecycle in the zero-account sandbox

Use this before integrating: it exercises challenge, pinned issuer trust, registered mandate, signed decision evidence, atomic reservation, one-use permit redemption, replay suppression, provider attestation and terminal reconciliation with **isolated test fixtures generated inside the run**. Specification: `https://mandateshield.com/specifications/strict-lifecycle-sandbox/v1`; page: `https://mandateshield.com/sandbox`.

## Auth
- None (`security: []` on the operation). No account, no card, no payment credentials. Base URL `https://mandateshield.com`.

## Steps
1. **Run the simulation** — `runStrictLifecycleSandbox` (`POST /api/v2/sandbox/lifecycle`). Body per the `StrictLifecycleSandboxRequest` schema (all optional).
2. **Read the returned stages in order** (`StrictLifecycleSandboxResult`): fresh challenge issued → account-pinned authority verified → active mandate evaluated → decision evidence signed → budget reserved atomically → authorization consumed once → provider-bound permit issued → fresh permit claim granted → permit replay suppressed → sandbox provider attested → terminal reconciliation recorded. A missing, malformed or incomplete stage is a failure, not a partial success.
3. **Confirm the boundary flags** on the response: `money_moved=false`, no external provider contacted, `productionAccepted=false`. Reject any result that does not affirm `money_moved=false`.
4. **Then try the anonymous analysis path** — `evaluatePurchase` (`POST /api/v1/preflight`) with a real-shaped envelope, to see the findings your production envelopes would produce (`mandateshield-com-analysis-only-authority-check.md`).

## Rules an agent must follow
- **Nothing here is production authority.** The sandbox has zero execution power and its results can never pass a `requireAllowed` check in the SDK.
- **Rate limit:** 429 `Public sandbox rate limit exceeded` (numeric ceiling unpublished, no headers) — back off.
- The next step up is the free guided private pilot at `https://mandateshield.com/launch` (account required, $0, no card): one server-observed strict live reservation plus one fresh permit redemption bound to a domain or public repository you control, still with no provider call.

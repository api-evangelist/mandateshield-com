---
generated: '2026-09-19'
method: generated
name: Analysis-only payment authority check (no account)
description: Run a MandateShield v1 policy analysis on a proposed AI-agent purchase before paying — anonymously
  or with a test key — and read the ALLOW/REVIEW/BLOCK findings. Never treat the result as authority to pay.
api: openapi/mandateshield-com-openapi.yml
operations:
- evaluatePurchase
- evaluatePurchaseBatch
- normalizeAgentPaymentProtocol
source: Grounded in openapi/mandateshield-com-openapi.yml (OpenAPI 3.1.0, contract v3.4.0, captured 2026-09-19 from
  https://mandateshield.com/openapi.json; verbatim source in openapi/_original/). Every operationId was verified
  in that spec. Auth per authentication/mandateshield-com-authentication.yml, errors per errors/mandateshield-com-problem-types.yml
  and errors/mandateshield-com-error-codes.yml, idempotency and reversibility per conventions/mandateshield-com-conventions.yml,
  limits per rate-limits/mandateshield-com-rate-limits.yml.
---

# Analysis-only payment authority check (no account)

Use this when an agent is about to buy, subscribe, transfer value, call a metered API or access a paid resource and you want a deterministic policy read on the purchase. This is MandateShield's **v1 analysis-only profile**: it always returns `enforcement_authorized: false`, even with a live key. It is a dry run, not a gate.

## Auth
- None required. Anonymous calls use the rate-limited, non-persisted sandbox (200 mandate validations per day, `rate-limits/mandateshield-com-rate-limits.yml`).
- Optional `Authorization: Bearer ms_test_...` (a test key created after sign-in) persists decisions for repeatable tests — up to 5,000 per month.
- Base URL: `https://mandateshield.com`. Never send card, bank, wallet, provider-secret or private-key material.

## Steps
1. **(If you hold raw protocol syntax)** project it first — `normalizeAgentPaymentProtocol` (`POST /api/v2/normalize`). Body: `adapter` (`AP2_CLOSED_PAYMENT_SD_JWT` | `X402_V2_PAYMENT_REQUIRED` | `MPP_HTTP_PAYMENT_CHALLENGE`), `source` (the raw SD-JWT / PAYMENT-REQUIRED / WWW-Authenticate challenge), `context` (`mandate_id`, `agent_id`, optional `idempotency_key`, `merchant_id`, `resource`, ...). The result is a non-executable purchase envelope; `projection_fields_valid` is not protocol conformance.
2. **Analyze one purchase** — `evaluatePurchase` (`POST /api/v1/preflight`). Required body: `protocol` (`AP2|TAP|UCP|X402|MPP|ACP|CUSTOM`), `mandate_id`, `agent_id`, `merchant_id`, `amount`, `idempotency_key`. Fiat amounts are integer minor units with an ISO 4217 currency; x402/atomic assets use a canonical decimal string `atomic_units` plus `asset_decimals`, `asset_id`, `network`, `resource`.
3. **Or analyze 1–25 at once** — `evaluatePurchaseBatch` (`POST /api/v1/batch`), each item its own envelope with its own `idempotency_key`.
4. **Read the decision**: `decision` is `ALLOW`, `REVIEW` or `BLOCK`; `findings[]` carries `{code, message, severity}` from the 91-code registry in `errors/mandateshield-com-error-codes.yml`; `receipt` is a sha256 hash of the decision. Explain the findings to the user; do not proceed to a provider call on the strength of an ALLOW here.

## Rules an agent must follow
- **`enforcement_authorized` is always false on v1.** The only path that reserves authority is strict v2 (`mandateshield-com-strict-lifecycle-reserve-consume-redeem.md`) and even that never permits a provider call from an agent.
- **One stable `idempotency_key` per intended purchase**, kept across retries; the boundary is account-wide (`conventions/`). An exact-input retry returns the same decision and is not billed twice.
- **429 carries no rate-limit headers.** Read the `{error, code}` body; back off; do not fail open.
- **Do not fabricate protocol fields.** If you do not have a mandate id, agent id or merchant id, send the request anyway and let the findings (`MISSING_MANDATE_ID`, `MISSING_AGENT_ID`, `MISSING_MERCHANT`) tell the user what is missing — that is what a probe with `{}` returned on 2026-09-19.
- The same two tools exist over MCP (`check_ai_payment_authority`, `normalize_agent_payment_protocol` at `https://mandateshield.com/api/mcp`, no auth) — `mcp/mandateshield-com-tool-crosswalk.yml`.

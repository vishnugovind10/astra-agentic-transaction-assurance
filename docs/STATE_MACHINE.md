# Transaction State Machine

## 1. State dimensions are separate

Astra must not collapse commercial status, payment execution, chain finality, provider evidence and accounting reconciliation into one `PAID` field.

## 2. Main lifecycle

1. `RECEIVED` — request received; no trust implied.
2. `NORMALISED` — canonical transaction envelope formed.
3. `IDENTITY_PENDING` — identity/payee evidence is being evaluated.
4. `AUTHORITY_PENDING` — principal mandate and agent scope are being evaluated.
5. `QUOTE_VERIFIED` — quote signature, digest, recipient, amount and expiry match.
6. `EXPOSURE_APPROVED` — available capacity and required reservations pass.
7. `CLAIM_RESERVED` — unique quote/job/payment claim reserved atomically.
8. `AUTHORISED` — exact transaction parameters are authorised.
9. `SUBMITTED` — payment transaction submitted to Aevum.
10. `EXECUTED` — transaction executed in a block; confirmation/finality still assessed.
11. `CONFIRMED` — chain confirmation threshold/finality policy satisfied.
12. `RECEIPT_PENDING` — provider receipt expected or awaited.
13. `EVIDENCE_VERIFIED` — receipt integrity, attribution, reference and policy checks pass.
14. `RECONCILED` — observed transaction matches authorised payment and financial record.
15. `ASSURED` — all required technical assurance conditions pass; service-quality/legal boundaries remain explicit.

## 3. Exception states

- `DENIED` — hard authority, identity, policy, quote, amount or recipient rule violated.
- `CONSTRAINED` — a reduced amount/scope or additional condition could make the request eligible.
- `REQUIRE_HUMAN` — valid but outside autonomous authority.
- `DUPLICATE` — unique transaction claim already consumed or active.
- `EXPIRED` — quote, mandate or credential expired.
- `REVOKED` — mandate or credential revoked.
- `PENDING` — execution or confirmation incomplete.
- `ABSTAIN` — required evidence unavailable, stale, conflicting or unverifiable.
- `EXCEPTION` — positive operational divergence or reconciliation break.
- `NO_GO` — demonstrated post-execution consequence violation.

## 4. Decision precedence

- A demonstrated hard violation overrides other positive checks.
- Otherwise, missing/stale/conflicted required evidence yields ABSTAIN.
- Otherwise, hard policy failure yields DENY.
- Otherwise, an allowed reduction yields CONSTRAIN.
- Otherwise, an out-of-scope but potentially legitimate action yields REQUIRE_HUMAN.
- Only fully supported and passing conditions permit ALLOW/GO.

The implementation should specify exact precedence and not have inconsistent service-specific orderings.

## 5. Atomic claim and payment

The unique claim must be consumed atomically with the payment state transition. A failed token transfer must not leave the transaction falsely settled or permanently consume a claim unless the contract's documented recovery path explicitly handles it. Reentrancy and external token-call failure must be tested.

## 6. Settlement and discharge boundaries

`EXECUTED` is not `CONFIRMED`.
`CONFIRMED` is not `RECONCILED`.
`RECONCILED` is not automatically legal discharge.

Astra can report that on-chain settlement and the relevant enterprise ledger are technically reconciled. Whether a debt or invoice is legally discharged depends on contract terms, applicable law, payment finality and the creditor's authoritative records.

## 7. Recovery

A payment that is submitted but not observed must remain pending/exception; it must not be blindly retried with a new idempotency key. The system should re-query the original transaction and reconcile its state before any replacement transaction is authorised.

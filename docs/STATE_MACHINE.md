# Transaction Lifecycle and Recovery

**Authoritative pre-build lifecycle specification.** This is prose only; the state machine has not been implemented or tested.

## Separate dimensions

Do not use one status for policy, payment execution, service evidence, and reconciliation.

- Pre-execution policy: `ALLOW`, `CONSTRAIN`, `REQUIRE_HUMAN`, `DENY`, `ABSTAIN`.
- Operational lifecycle: states below.
- Post-execution assessment: `GO`, `NO_GO`, `ABSTAIN`; operational exceptions are recorded separately.

`ALLOW` is eligibility to present a transaction for user confirmation, not proof of execution. `GO` means only that defined post-execution checks passed. Payment confirmation does not prove service quality or legal discharge.

## Lifecycle

| State | Meaning | Entry evidence |
|---|---|---|
| `RECEIVED` | Offer/request captured; no trust implied. | Source, received-at time, original evidence reference. |
| `VALIDATING` | Schema, signature, identity/payee, quote, authority and exposure checks running. | Canonicalization version and check run ID. |
| `READY` | Required checks pass; exact transaction awaits user confirmation. | Policy decision, mandate/quote digests, exposure reservation, exact payment summary. |
| `RESERVED` | Scoped job/quote claim and exposure reservation durably held. | Unique key, intent digest, reservation time/expiry. |
| `SUBMITTED` | Wallet/network returned a transaction hash or broadcast observation. | Hash, chain ID, wallet result, submission time. |
| `PENDING` | Outcome unresolved: propagation, receipt, confirmation, or evidence source unavailable. | Same hash or reason no hash exists; last query source/time. |
| `FAILED` | Chain receipt proves transaction reverted, or user explicitly rejected before broadcast. | Revert receipt or wallet rejection; distinguish no-submit from on-chain failure. |
| `CONFIRMED` | Chain observation meets the documented confirmation policy. | Receipt/log, block hash/height, verification sources and policy version. |
| `RECEIPT_PENDING` | Payment confirmed; provider service receipt is awaited. | Payment evidence and due/expected window. |
| `RECEIPT_REJECTED` | Receipt has invalid signature/key or contradicts bound job/quote. | Receipt digest, key/source status, mismatch/reason. |
| `RECONCILED` | Authorized payment, observed chain event, accepted receipt evidence, and ledger entries match within defined scope. | Comparison record, evidence references, reconciliation policy/version. |
| `CANCELLED` | User cancels before broadcast, or authorized workflow cancels before irreversible execution. | Cancellation actor/time and proof no broadcast occurred. |
| `EXPIRED` | Quote/mandate/credential expired before broadcast. | Expiry value, observed time, no-submit evidence. |
| `REVOKED` | Required authority or credential revoked before broadcast. | Issuer/revocation source and observed time. |
| `EXCEPTION` | Positive divergence, suspected duplicate side effect, reconciliation break, or operational case requiring review. | Conflicting evidence and incident/reference ID. |

## Valid transitions

- `RECEIVED → VALIDATING`.
- `VALIDATING → READY` only if required evidence passes; otherwise stop with DENY/ABSTAIN reason and no payment action.
- `READY → RESERVED` after an atomic unique-claim and capacity reservation succeeds.
- `RESERVED → SUBMITTED` only after checking expiry/revocation again and the user authorizes the exact wallet transaction.
- `RESERVED → CANCELLED | EXPIRED | REVOKED` only with evidence that no broadcast occurred; release reservation under the specified policy.
- `SUBMITTED → PENDING | FAILED | CONFIRMED | EXCEPTION` based on chain evidence. A timeout alone never means failure.
- `PENDING → PENDING | FAILED | CONFIRMED | EXCEPTION` by rechecking the same transaction hash and chain. Never create a second payment instruction while unresolved.
- `CONFIRMED → RECEIPT_PENDING`; then `RECEIPT_PENDING → RECONCILED | RECEIPT_REJECTED | EXCEPTION`. Missing receipt remains pending/abstain per policy; it does not undo payment confirmation.
- `RECEIPT_REJECTED → EXCEPTION` after retaining payment facts and opening an operational case.

Invalid: `SUBMITTED → CANCELLED`; `CONFIRMED → FAILED`; `PENDING → new payment hash` without proof of original terminal failure and explicit new authorization; any evidence failure → success/assured state; or mutation of an authorized envelope in place.

## On-chain and off-chain state

For the initial direct-transfer design there is no Astra contract state. The token contract state is the transfer and its receipt/logs. Astra's request, reservation, policy/evidence record, lifecycle and reconciliation are off-chain. The user wallet signs the transfer. Therefore, the Astra UI cannot enforce limits against a key that can independently transfer tokens.

If a future contract is approved, document which transition it owns and how off-chain state is reconciled; never present an off-chain status update as an on-chain enforcement guarantee.

## Recovery rules

- **Submitted but unconfirmed:** retain the reservation/idempotency claim; poll/re-query the original hash. Do not ask for another payment or invent a replacement hash.
- **Confirmed but event not indexed:** verify the original transaction receipt/log through RPC; indexing lag is not failure. Record the source and block. Do not resubmit.
- **Transfer reverted:** require a failure receipt. Releasing the claim/capacity is a controlled transition; the same commercial obligation can be retried only after validating current expiry/revocation and receiving explicit authorization.
- **RPC outage:** preserve `PENDING`, last known observation and retry schedule. Multiple sources can be consulted; disagreement becomes `EXCEPTION`/`ABSTAIN` until resolved.
- **Duplicate request:** return the existing transaction and state; no second wallet handoff.
- **Late service receipt:** attach it to the original transaction, verify against the original quote/job and record lateness. Do not reopen or repeat payment.
- **Provider does not deliver:** payment state remains confirmed; service status stays unresolved/exception. Refund/reversal is a separate commercial process, not implied by Astra.
- **Manual reconciliation:** retain original evidence, operator identity/action/reason and before/after records. Do not overwrite discrepant facts.
- **Reorg/finality uncertainty:** downgrade confidence to pending/exception according to the chosen chain policy, preserve prior observations, and never erase an already displayed transaction hash.

## Reservation accounting

Reserve capacity and scoped idempotency key together before the wallet prompt. Release only with proof of no broadcast or a verified terminal failure. On confirmed payment, move reserved amount to settled exactly once. Repeated event processing must be idempotent. An uncertain transaction cannot be retried under a new idempotency key.

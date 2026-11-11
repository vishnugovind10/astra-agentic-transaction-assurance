# Adversarial Test Plan (Specification Only)

**Status:** scenarios and pass evidence only. No executable tests have been authored.  
**Implementation window:** begin writing tests on or after 16 November 2026, 06:00 UTC.

## Test record contract

Each eventual test records scenario ID, fixture/initial state, attacker action, expected policy decision, expected lifecycle transition, expected durable evidence/reason code, and whether a chain fork/testnet action is required. Live payment attempts use only funded test-only wallets and must be individually approved. Simulation tests must never be labelled as live.

## Required scenarios

| ID | Initial state and attacker action | Decision/state | Evidence required to pass |
|---|---|---|---|
| PAYEE-01 substitution | Valid quote and accepted provider key; replace destination after signing. | `DENY`; remain pre-authorized, no wallet handoff/reservation consumed. | Recomputed digest mismatch and expected/observed address reason. |
| DUP-01 duplicate quote/job | One claim RESERVED or consumed; resubmit same scoped job/quote with same or new request ID. | `DENY`/duplicate response; original state returned; no second side effect. | Unique-key conflict and one reservation/payment hash only. |
| QUOTE-01 tampering | Authorized digest for 20 tUSD8; alter amount, terms, service, chain, token, recipient, expiry or job. | `DENY`; no handoff. | Signed digest and mismatching field list. |
| EXP-01 over authority | Cap 30, settled/reserved commitments leave <20; request 20. | `DENY`, or `CONSTRAIN` only when a reduced amount is valid and newly authorized. | Integer-unit ledger arithmetic and no reservation/payment on denied path. |
| EXP-02 boundary precision | Amount exactly cap, cap+1 atomic unit, over six decimals, exponent or huge input. | Exact boundary passes policy; +1 or malformed precision rejected. | Atomic-unit comparison; no floating-point rounding. |
| TIME-01 expired quote | Quote expires before user confirms or while wallet prompt is open. | `DENY` before submission; reservation released under defined no-submit proof. | Clock boundary at expiry, timestamps, no transaction hash. |
| AUTH-01 revoked mandate | Valid-looking mandate revoked before payment handoff. | `DENY`; no payment. | Revocation source/version/time and rejected request. |
| AUTH-02 unauthorized agent | Signer is not delegated or action/provider/token/chain outside scope. | `DENY`; no payment. | Signer attribution and exact failed scope condition. |
| PAY-02 repeated submission | Same intent receives repeated click/network retry or two concurrent callers. | One payment instruction; later calls return original pending/result. | Concurrency test evidence, one durable claim, at most one tx hash. |
| PAY-03 uncertain submission | Wallet/RPC times out after broadcast, response lost. | `PENDING`; retain claim; re-query original transaction; never create replacement. | Same hash tracked through recovery and no second wallet prompt. |
| PAY-04 transfer revert | User transaction receipt has failure status. | `FAILED`/exception; never `CONFIRMED`; release capacity only per explicit safe rule. | Revert receipt and unchanged token transfer evidence. |
| CHAIN-01 wrong chain/token | Wallet chain ID or token contract differs from mandate. | `DENY`; no signing prompt or no acceptance of returned hash. | Expected/observed chain/token and reason. |
| CHAIN-02 wrong beneficiary/amount | Transaction/event differs from authorized recipient or amount. | `NO_GO` + `EXCEPTION`; financial fact retained; no assured status. | Chain receipt/log showing actual transfer and field comparison. |
| CHAIN-03 RPC unavailable/conflict | RPC timeouts, stale receipt, or sources disagree. | `ABSTAIN`/`PENDING`; do not report confirmed. | Source/time/error details and retry schedule with same hash. |
| RECEIPT-01 missing | Payment confirmed, receipt absent past expected interval. | Payment `CONFIRMED`; service evidence remains `PENDING` or `ABSTAIN`; no full assurance. | UI and record show independent payment/service statuses. |
| RECEIPT-02 invalid signature | Provider receipt signed by unknown/wrong/revoked key or malformed. | Reject evidence; `ABSTAIN` if key status unavailable, `NO_GO`/exception for demonstrated invalid signature. | Signature verification result, key source/status and digest. |
| RECEIPT-03 contradictory service | Valid provider signature but job, quote, scope, result, or terms contradict authorized request. | `EXCEPTION` and post-execution `NO_GO` for bound-field mismatch. | Signed receipt digest and exact mismatch; no service-assured label. |
| RECON-01 ledger divergence | Chain event amount/token/recipient conflicts with authorized intent or internal ledger. | `EXCEPTION`; freeze automatic close; preserve all records. | Reconciliation comparison, source provenance, unresolved case reference. |
| RACE-01 reservation race | Two requests claim same job concurrently. | Exactly one `RESERVED`; other duplicate denied. | Database isolation/unique constraint evidence and event ordering. |
| KEY-01 malicious buyer key | Agent key valid but attempts provider/amount/chain outside authority. | `DENY`; no broadened authorization. | Mandate boundary check. |
| KEY-02 provider key change | Provider rotates/revokes key between quote and execution. | `DENY` if revoked; `ABSTAIN` if authoritative status unavailable. | Key version/time and outcome. |

## Contract-oriented security checks if a contract is later approved

These are design review gates only; the current recommendation is no custom contract in the first slice.

- Unauthorized caller cannot change payee, authority, token, or consumed-claim state.
- Reentrant or malicious token callback cannot create multiple settlement effects.
- Integer boundary values, arithmetic underflow/overflow, zero address, and zero amount are safely rejected/handled.
- ERC-20 false return, no return value, revert, fee-on-transfer, and nonstandard behavior do not create false settlement records. The chosen token's actual behavior must be observed rather than assumed.
- One-time claim and transfer are atomic; revert rolls back claim consumption or uses a specified recoverable state.
- Expiry, revocation, nonce replay, and concurrent execution are tested at exact boundaries.
- Upgrade/admin roles and emergency controls are minimized and explicitly tested; no hidden owner can redirect user funds.

## Decision semantics

`ALLOW`/`CONSTRAIN`/`REQUIRE_HUMAN`/`DENY`/`ABSTAIN` are pre-execution policy outcomes. `PENDING`, `CONFIRMED`, `CANCELLED`, and `EXCEPTION` describe lifecycle/operations. `GO`/`NO_GO`/`ABSTAIN` assess post-execution consequence. A demonstrated harmful mismatch is not the same as missing evidence; missing evidence never becomes a pass through timeout.

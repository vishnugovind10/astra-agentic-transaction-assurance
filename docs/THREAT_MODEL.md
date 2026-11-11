# Threat Model

## 1. Assets to protect

- Principal funds and authorised capacity.
- Agent mandate and revocation state.
- Provider identity/payee binding.
- Quote and commercial terms integrity.
- Job/quote/invoice uniqueness.
- Payment parameters and chain selection.
- Settlement and service evidence.
- Financial ledger accuracy and audit provenance.
- Private keys, credentials and enterprise metadata.

## 2. Actors

- Honest buyer principal.
- Compromised or malicious buyer agent.
- Honest or malicious provider agent.
- Malicious provider/merchant operator.
- Compromised provider signing key.
- Malicious intermediary or API gateway.
- Stale or compromised identity/evidence issuer.
- RPC/explorer/provider returning delayed or inconsistent observations.
- Malicious prompt or content source attempting to alter agent behaviour.

## 3. Threats and controls

| Threat | Example | Required control |
|---|---|---|
| Payee substitution | Provider changes receiving address after quote | Bind recipient to signed quote and trusted payee credential/version; recheck at execution |
| Quote mutation | Price/service/chain altered after approval | Canonical digest, signature, expiry and exact-parameter comparison |
| Duplicate execution | Two agents pay the same job | Atomic unique claim and idempotency key across coordinator/contract boundary |
| Replay | Re-submit previously signed payment request | Nonce, expiry, chain ID, mandate version and consumed-claim tracking |
| Agent compromise | Agent uses valid wallet outside intended scope | Principal-signed mandate, action constraints, revocation and exposure reservation |
| Delegation escalation | Child agent receives wider authority than parent | Verify authority lineage and ensure child scope does not exceed parent |
| Provider key compromise | Attacker signs offer/receipt with stolen provider key | Credential revocation/versioning, anomaly handling and issuer trust status |
| Sybil/reputation manipulation | Provider creates identities or fake ratings | Do not treat reputation as identity or sole authorisation; apply trusted allowlists/issuers |
| Spend amplification | Retries or many jobs create excessive exposure | Cumulative exposure, retry budget, per-provider limits and rate controls |
| Conflicting coordination | Two agents claim exclusive work | Signed task/claim events and independent consistency verification |
| Prompt injection | Invoice/web page tells agent to change payee or increase budget | External content cannot mutate principal mandate; deterministic policy outside LLM control |
| Stale evidence | Expired credential or old quote used | Freshness windows and explicit UNKNOWN/ABSTAIN status |
| RPC divergence | Different RPC providers return inconsistent states | Chain ID check, confirmation policy, independent receipt verification and exception handling |
| Settlement drift | Actual transfer differs from authorised intent | Compare receipt/logs to full authorised envelope; NO_GO on demonstrated mismatch |
| Receipt fabrication | Provider signs delivery assertion without actual service | Treat signature as attribution, use metering/independent validation where required |
| Secret leakage | Private key/API secret committed or exposed in frontend | User wallet signing, server-side secret management, no secrets in public env vars |

## 4. Trust assumptions

- A principal or authorised administrator establishes the mandate and trusted supplier/provider source.
- Cryptographic signatures establish attribution to keys, not truthfulness or legal identity.
- Aevum testnet token and RPC behave according to their documented interfaces; production designs require stronger RPC and monitoring strategy.
- The provider receipt is an assertion unless backed by independent metering or validation.
- The enterprise's legal/accounting system remains authoritative for its own obligation status.

## 5. Required fail-safe behaviour

- Missing identity/payee proof: ABSTAIN or DENY according to mandate policy; never silently allow.
- Changed recipient/amount/token/chain: DENY.
- Duplicate claim: DENY with stable reason code.
- Uncertain transaction submission: inspect the original transaction before retrying.
- Chain event inconsistent with intent: NO_GO/EXCEPTION.
- Missing service evidence: payment may remain financially settled, but service assurance remains ABSTAIN/PENDING.
- Credential revoked after quote but before execution: reject execution.

## 6. Security limitations

A hackathon implementation is not a security audit or production assurance certification. Production use requires independent smart-contract audit, secure key custody, authentication/authorisation hardening, privacy review, monitoring, incident response and tested recovery procedures.

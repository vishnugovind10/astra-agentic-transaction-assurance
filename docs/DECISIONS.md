# Architecture Decision Records

## ADR-001 — Astra is an assurance layer, not a wallet

**Decision:** Astra complements QDay and existing payment protocols rather than replacing wallets, agent identity registries or payment facilitators.

**Reason:** Those layers already exist or are on QDay's roadmap. Astra's focus is the correctness and evidence of the complete commercial transaction.

## ADR-002 — Transaction is the unit of assurance

**Decision:** Use a canonical AgentTransactionEnvelope to bind commercial intent, parties, authority, quote, payment, exposure and evidence.

**Reason:** A transfer can succeed while the commercial transaction is wrong. Transaction context must survive across the lifecycle.

## ADR-003 — Separate agent identity from payee/legal identity

**Decision:** Represent agent registry identity, key control, wallet association, supplier approval and legal entity evidence as separate evidence claims.

**Reason:** No single registry or wallet signature proves all of these things.

## ADR-004 — Preserve uncertainty

**Decision:** Missing, stale or conflicting evidence produces ABSTAIN or EXCEPTION, not success.

**Reason:** False assurance is more harmful than an explicit unresolved state in financial infrastructure.

## ADR-005 — Use actual Aevum tUSD8 for the hackathon flow

**Decision:** The MVP must execute one real testnet transfer using the challenge token.

**Reason:** The challenge requires an end-to-end stablecoin payment flow on Aevum. Simulations are useful for adversarial scenarios but cannot substitute for the live payment.

## ADR-006 — Do not put commercial payloads on-chain

**Decision:** Store minimal identifiers, hashes, constraints and events on-chain. Keep confidential commercial details and evidence off-chain.

**Reason:** Privacy, data minimisation, cost and the public nature of blockchain data.

## ADR-007 — One integrated repository

**Decision:** Publish one Astra repository with one website, shared schemas, contracts, services and docs.

**Reason:** The commercial deliverable is an integrated product, not multiple disconnected prototypes.

## ADR-008 — No competition implementation code before the build window

**Decision:** This pre-build snapshot contains documentation only.

**Reason:** The official challenge page states all competition code must be written during the build period. Re-check the starter kit on opening day.

# System Architecture

## 1. Architectural objective

Astra evaluates a machine-initiated commercial transaction as a lifecycle, not as an isolated token transfer. The assurance boundary begins with the service/payment intent and ends with the strongest state supported by observed execution, provider evidence and reconciliation.

## 2. Logical planes

### Plane A — Commerce ingress

Normalise service offers, invoices, quotes and payment requests from agent frameworks and payment protocols. Inputs are untrusted until validated. Preserve the raw source reference, signature, timestamp and transport context.

### Plane B — Identity and counterparty assurance

Evaluate buyer and seller agent identifiers, principal relationships, wallet associations, supplier records, credential issuer, credential version, expiry and revocation. Distinguish identity registration from legal identity verification and wallet control from beneficial ownership.

### Plane C — Authority and economic consequence

Evaluate principal mandate, action scope, payment type, provider restrictions, maximum amount, expiry, current exposure, reservations and counterparty limits. Map to explicit outcomes: ALLOW, CONSTRAIN, REQUIRE_HUMAN, DENY.

### Plane D — Coordination integrity

Protect transaction and job uniqueness, quote/nonce replay, duplicate side effects, concurrent claims and conflicting workflow transitions. Maintain signed, ordered event references. A coordinator is not the verifier's sole source of truth.

### Plane E — Payment execution

For the hackathon, execute an exact tUSD8 transfer on Aevum Testnet. The execution adapter must validate the chain, token contract, amount units, recipient and mandate. Wallet signing remains user-controlled; no private key belongs in the web application or server configuration.

### Plane F — Outcome and settlement assurance

Observe chain events, confirmations/finality state, provider receipts and financial ledger observations. Compare actual destination, token, amount, chain and transaction reference with authorised intent. Missing, stale or conflicting evidence must remain unresolved.

### Plane G — Financial and evidence ledger

Record authorised amount, reservation, actual transfer, fees, predicted/actual cost, provider reference, assurance decisions and reconciliation evidence. Maintain a correlation ID across services.

## 3. Service boundaries

| Service | Responsibility | Must not claim |
|---|---|---|
| Transaction Gateway | Orchestration and canonical envelope | Independent truth about inputs |
| Identity Assurance | Evaluate identity/payee evidence | Legal identity unless backed by an appropriate source |
| Authority Adapter | Map delegated authority decisions | Create authority or increase mandate |
| Exposure Engine | Calculate reservations and available capacity | Credit underwriting |
| Quote Verifier | Validate signed quote terms and expiry | Proof that service was delivered |
| Coordination Verifier | Detect replay, duplicate and conflicting claims | Distributed consensus or truth of external facts |
| Consequence Verifier | Compare intent and observed execution | Execute payments |
| Settlement Coordinator | Evaluate finality and reconciliation state | Legal discharge by technical assertion |
| FinOps Adapter | Record cost/exposure/variance | Replace enterprise accounting system |

## 4. Trust boundaries

1. Agent-supplied transaction data is untrusted.
2. Provider quotes and receipts are untrusted until signature, issuer, scope and freshness checks pass.
3. Identity registry data is evidence, not a universal legal identity assertion.
4. Off-chain assurance decisions are not automatically enforceable on-chain; relevant constraints must be cryptographically bound into the payment mandate/contract call.
5. Blockchain events establish chain-observed facts under the selected RPC/finality trust model, not off-chain service quality.
6. Enterprise payable discharge is confirmed only by the enterprise system or authorised accounting workflow.

## 5. On-chain/off-chain split

### On-chain

- Minimal payee/provider reference and wallet binding for the demo.
- Principal-authorised transaction mandate or commitment.
- Unique quote/job/payment claim.
- Exact token, amount, chain and destination constraints.
- One-time settlement and emitted event.
- Revocation/version state where required.

### Off-chain

- Full commercial terms and service metadata.
- Agent identity evidence aggregation.
- Enterprise supplier/legal entity mapping.
- Provider quote and receipt verification.
- Economic forecasting and risk analysis.
- Detailed event/evidence storage.
- FinOps and enterprise reconciliation.
- User interface and integrations.

Avoid storing confidential invoice, customer or task data on public chain. Put hashes or opaque references on-chain where useful.

## 6. Integration pattern

Adapters must be narrow and versioned. A provider adapter converts an external identity, mandate, quote or payment protocol object into Astra's canonical domain object while preserving source provenance. An adapter must not turn UNKNOWN into PASS or claim conformance when only a subset of a standard has been implemented.

## 7. Failure handling

- Before execution: use only `ALLOW`, `CONSTRAIN`, `REQUIRE_HUMAN`, `DENY`, or `ABSTAIN` for policy outcomes.
- During execution: use lifecycle states such as `SUBMITTED`, `PENDING`, `FAILED`, and `CONFIRMED`.
- After execution: use `GO`, `NO_GO`, or `ABSTAIN` for consequence assessment, and `EXCEPTION` as an operational case state.
- Do not use `GO` and `ASSURED` as synonyms or let a payment receipt imply service completion. See [Lifecycle and Recovery](STATE_MACHINE.md).

## 8. First-slice implementation boundary

See [Implementation Specification](IMPLEMENTATION_SPEC.md). The initial demo deliberately uses a user-controlled direct tUSD8 transfer and does not deploy custom Astra contracts. The UI/off-chain checks do not constrain an unrestricted key from transferring outside Astra. Do not claim on-chain mandate enforcement. Revisit only if the actual execution model requires autonomous delegated signing with enforceable on-chain scope.

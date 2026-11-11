# Canonical Transaction Model

## 1. Transaction is the unit of assurance

Astra's canonical object is the `AgentTransactionEnvelope`. It binds buyer, seller, principal authority, commercial intent, quote, payment instruction, economic exposure, execution and evidence into one versioned transaction identity.

## 2. Required logical fields

### Identity

- `transaction_id` — globally unique Astra identifier.
- `schema_version` — version of the envelope.
- `transaction_type` — fixed service purchase, invoice payment or metered session.
- `created_at`, `expires_at` — transaction validity window.
- `correlation_id` — cross-service trace identifier.
- `idempotency_key` — unique execution claim key.

### Buyer and principal

- `principal_id` — enterprise or individual principal reference.
- `buyer_agent_id` — buyer agent identity reference.
- `buyer_registry_ref` — registry and chain reference, if available.
- `buyer_wallet` — authorised payment account/wallet.
- `mandate_ref` — principal-authorised delegation reference.

### Seller and payee

- `provider_id` — provider/business reference from trusted source.
- `seller_agent_id` — seller agent registry reference.
- `seller_registry_ref` — registry and chain reference, if available.
- `payee_wallet` — exact receiving address.
- `payee_credential_ref` — issuer, version, digest and expiry of payee evidence.
- `payee_evidence_status` — PASS, FAIL or UNKNOWN, with reason.

### Commercial intent

- `service_id` and `service_description`.
- `job_id` or `invoice_id`.
- `quote_id` and `quote_digest`.
- `terms_digest`.
- `pricing_model` — FIXED, METERED or SUBSCRIPTION.
- `expected_receipt_type` and acceptance/evidence policy.

### Payment instruction

- `chain_id`.
- `token_address`.
- `token_decimals`.
- `amount_atomic`.
- `maximum_amount_atomic`.
- `payment_mode`.
- `recipient`.
- `mandate_expiry`.
- `nonce` and quote/job claim key.

### Financial exposure

- `authorised_capacity_atomic`.
- `settled_exposure_atomic`.
- `reserved_exposure_atomic`.
- `outstanding_commitment_atomic`.
- `available_capacity_atomic`.
- `estimated_fee_atomic` where applicable.
- `predicted_total_cost` and `actual_total_cost`, with currency and precision.

### Evidence and outcome

- source provenance, issuer and signature references;
- timestamps and freshness window;
- chain transaction hash and event reference;
- provider service receipt digest;
- verifier identity and policy version;
- reconciliation result;
- reason codes and final disposition.

## 3. Canonical amount rules

All on-chain token amounts use integer atomic units. For tUSD8, decimals are six. Never use binary floating-point for transfer amounts, balances or exposure invariants. Off-chain currency conversions must use fixed-precision decimals and retain source rate, source timestamp and currency pair.

## 4. Exposure accounting

For a bounded one-off transaction:

`TotalExposure = Settled + Reserved + OutstandingCommitments`

`AvailableCapacity = AuthorisedCapacity - TotalExposure`

A reservation moves to settled after successful settlement; it must not be counted twice. If an authorised commitment is cancelled or expires, release it only when its contractual/operational cancellation condition is satisfied.

For metered services, distinguish:

- maximum authorised session commitment;
- measured consumption to date;
- settled charges;
- forecast remaining consumption;
- contractual minimums or cancellation liabilities.

Forecast cost is not automatically an incurred liability. It may trigger CONSTRAIN or REQUIRE_HUMAN but must not be misreported as settled/committed exposure.

## 5. Quote integrity

A signed provider offer should bind at least:

- provider/seller identity reference;
- receiving wallet or verifiable payee credential reference;
- service/job identifier;
- amount, token and chain;
- pricing model and relevant units;
- terms digest;
- quote nonce;
- issuance and expiry timestamps.

Any change to a bound field requires a new signed quote or explicit authorised amendment. A quote signature proves the signer's assertion, not the truth or quality of the service.

## 6. Unique claims and idempotency

Use distinct identifiers for:

- commercial job;
- provider quote;
- invoice (when applicable);
- payment execution request;
- on-chain nonce.

Define the uniqueness scope explicitly. For the demo, one quote/job claim can be settled once under the buyer mandate. A legitimate re-quote must supersede or cancel the prior claim under an authorised state transition. Do not use a hash of invoice number alone as a universal global key because identifiers can collide across suppliers or buyers.

## 7. Evidence semantics

Every evidence item includes source, issuer, signature/verification method, subject, timestamp, expiry/freshness, digest and verification status. Distinguish:

- cryptographic integrity of the evidence;
- attribution to a key/issuer;
- source authority;
- truth of the asserted external fact;
- acceptance under the transaction's policy.

A signed service receipt can establish attribution to a provider key, but does not independently prove service quality or correctness.

# Astra Vertical-Slice Implementation Specification

**Status:** pre-build specification; not implementation.  
**Primary demo:** one provider, one buyer, one fixed-price service job, a 20 tUSD8 offer under a 30 tUSD8 authority ceiling, one user-confirmed Aevum transfer, one provider-signed receipt.  
**Token units:** tUSD8 has six decimals; the proposed amount is 20,000,000 atomic units and the ceiling is 30,000,000. Reconfirm before coding.

## Product boundary and trust statements

Astra is a transaction assurance and evidence coordinator around an existing wallet, payment asset, and agent/provider workflow. The demo demonstrates exact-intent comparison and transparent evidence states. It does not provide KYC/legal identity proof, custody, credit, service-quality certification, or automatic reversal of an on-chain payment. A provider signature attributes a receipt to a key; it does not prove the service is good or even independently observed.

Keep these assertions separate: agent registration; control of a signing key; legal supplier identity; account/wallet association; approved payment destination; authority from a principal. Evidence for one does not establish the others. Every evidence assertion records issuer, subject, source, digest, method, observed time, validity period, status, and policy/version reference.

## Decision: do not require custom Astra contracts for the first vertical slice

Use one modular monolith with off-chain deterministic checks. Preferred first-slice execution is a user-controlled wallet transaction sending the exact tUSD8 amount to the exact bound payee. Before choosing, resolve the official QDay docs conflict about Aevum ERC-4337/session keys and x402: if QDay supplies a current, reproducible Aevum session-key path and deployed addresses/endpoint before coding, evaluate whether a bounded session materially improves the demo; otherwise use direct EOA transfer. Do not make Astra depend on a QDay SDK whose docs say it is unpublished. The wallet/user confirmation is the payment authorization in the direct-transfer variant. Astra records intent before wallet handoff and verifies the resulting chain transaction against that immutable intent.

For this one-payment testnet demo, a custom registry, mandate, or settlement contract adds implementation, token-approval, deployment, and recovery risks. QDay's current wiki claims testnet ERC-4337/session-key support, but key Aevum endpoints/addresses remain unpublished in the guide and the public product/build pages conflict. Do not build a dependency on those claims until the kickoff check proves a usable path. Therefore:

| Candidate | First slice | If later justified |
|---|---|---|
| `AgentPayeeRegistry` | Do not build. Use explicit demo fixture and show its evidence provenance/limitations. | Consider only with a defined issuer governance, updates/revocation policy, and independently verifiable wallet binding. |
| `TransactionMandate` | Do not build in the direct EOA variant. Present the user-visible transaction summary and wallet confirmation; record the authority decision and mandate reference off-chain. | If a usable native QDay session-key module is available, evaluate it before Astra custom code. Build a custom mandate only if native enforcement is unavailable and delegated signing is necessary. |
| `AgentPaymentSettlement` | Do not build. Direct ERC-20 transfer avoids Astra custody/escrow and allowances. | Consider only if a concrete atomic authorization/idempotency requirement cannot be enforced by the wallet/provider. Require independent contract review and adversarial tests. |

**Security boundary:** the UI/off-chain policy cannot stop an agent that already controls an unrestricted EOA key from making another transfer outside Astra. Do not claim on-chain mandate enforcement or autonomous-agent spend containment in the direct-transfer variant. If the QDay native session mechanism is used, document its precise limits and verified Aevum deployment; if absent and cryptographic containment is required, revise the design before implementation rather than presenting off-chain checks as equivalent.

## Canonical transaction record

`AgentTransactionEnvelope` is a logical, versioned domain record. The complete persisted transaction includes the envelope, evidence references, decision history, execution observations, receipt, and reconciliation outcome. Keep raw evidence in protected off-chain storage; publish no business details on-chain.

### Required groups

| Group | Required facts |
|---|---|
| Identity/lifecycle | Schema version; Astra transaction ID; transaction type; created/expiry timestamps; state revision; correlation ID; idempotency scope/key. |
| Principal and authority | Principal reference; buyer agent/key reference; mandate ID/version/issuer; permitted action and service scope; payee/provider scope; maximum amount/token/chain; validity interval; revocation observation and time. |
| Commercial intent | Service and scope; job ID; buyer/provider namespace; quote ID; terms digest; expected receipt type/policy; price model. For this slice, fixed price only. |
| Counterparty | Provider and seller-agent references; receiving address; evidence binding provider → wallet; issuer/key ID; credential/version/digest; observed time, expiry and revocation status. Legal supplier identity remains a separate optional/unverified assertion. |
| Signed offer | Signer key reference; signature scheme; signed canonical payload digest; issuance/expiry; nonce; service/job; recipient; token/chain; amount and fee ceiling; terms digest. |
| Payment | Chain ID; token address and verified decimals; amount/max amount in atomic units; intended recipient; buyer wallet; nonce; quote digest; user-authorization observation; submitted transaction hash. |
| Exposure | Authorized cap; settled exposure; active reservations; outstanding commitment; fees; remaining capacity; scope and observation time. For the demo, show exact arithmetic and do not persist floating-point token values. |
| Execution/evidence | Receipt source; block number/hash; transaction sender/recipient; token contract; transfer event participants and amount; confirmation policy/version; RPC/source; raw evidence digest; provider receipt digest/signature/key; verification result and reason codes. |
| Reconciliation | Expected versus observed fields; result; discrepancy; accounting/event reference; completion time; reviewer/action if manual. Payment settlement and service assurance remain separate. |

### Canonicalisation and signatures

- V1 signed offers use EIP-712 typed data with domain name `Astra Agent Transaction Quote`, version `1`, and the verified target chain ID. No verifying-contract address is asserted because v1 has no Astra contract. Use Ethereum EOA ECDSA recovery for the demo; contract-wallet signature support is deferred until a real provider requires it.
- The signed quote binds schema version, provider ID digest, provider agent key/address, payee address, service ID digest, job ID digest, quote ID, terms digest, token address, chain ID, amount atomic, fee ceiling atomic, nonce, issued-at and exclusive expiry timestamps. Addresses are typed 20-byte values; amounts/times are unsigned integers; digests and identifiers use fixed-width bytes32 values. Human descriptions are represented by their digest, not rendered text.
- Reject malformed lengths, unknown critical version, unsupported signature scheme, duplicate field encodings, and ambiguous values. Do not sign locale strings or floating-point amounts. Normalize addresses to 20-byte values for comparison; display a checksum form.
- The transaction authorization record binds the verified quote digest to the mandate version, unique scoped claim, wallet address, and exact payment parameters. User wallet confirmation signs the actual token transfer; it is not represented as an EIP-712 autonomous agent mandate.
- A provider receipt uses EIP-712 domain `Astra Service Receipt`, version `1`, chain ID 44005, and binds provider key, transaction/job/quote/terms digests, receipt ID, service result digest, issued-at and expiry. Treat the result as provider assertion only.
- Verify signer attribution, then independently establish that the signing key is an accepted current key for the provider. A valid signature from an unknown or revoked key is not accepted evidence.
- Quote edits require a new signature, digest, expiry and explicit new authorization. Preserve prior revisions as superseded evidence; never mutate an authorized envelope in place.

### Expiry, nonce, and idempotency

- Expiry uses UTC epoch seconds, exclusive at the boundary: valid only while `now < expiresAt`; check again immediately before wallet handoff and after any human wait.
- Nonce prevents replay within the signing authority's defined scope. Idempotency is separately unique by principal + provider + job/quote + payment purpose. A duplicate request returns the original transaction/status; it must not create a new payment instruction.
- Use database uniqueness/transactional reservation for the single modular application. A claim becomes reserved before presenting the wallet request; release only on proven rejection/cancellation or a chain-verified failed transfer. An ambiguous submitted transaction retains the claim until resolved.
- If an authorization is expired/revoked before submission, do not submit. If revoked after submission, do not pretend the chain action can be undone; freeze further action and record the observed transaction.

### Money and exposure

Store raw tUSD8 values as non-negative integer atomic units and decimals as verified token metadata. Parse human input as decimal text with at most six fractional digits; reject excess precision, exponent notation, signs, NaN/infinity, and overflow. Render amounts by exact decimal insertion. Do not use binary float.

For the one-off slice: `available = mandate cap − settled exposure − active reservations − outstanding commitments`. Reserve the 20 tUSD8 amount before payment; after verified transfer, atomically move that amount from reserved to settled. Fees are a separate asset/cost unless the mandate explicitly includes them. If the available amount is uncertain or stale, return unresolved/ABSTAIN.

For the demo, 30 tUSD8 authority is an explicit test fixture/user-approved off-chain ceiling; a direct EOA token transfer does not enforce it outside Astra. State this on the demo screen.

## End-to-end sequence, evidence and user-visible outcome

| Step | Check/evidence | Failure/result and user display |
|---|---|---|
| 1. Receive offer | Preserve original offer, source, arrival time, signature and correlation. | Parse failure → DENY malformed request; show which field cannot be read. |
| 2. Normalize | Build canonical immutable envelope; require unique buyer/provider/job/quote IDs. | Missing/ambiguous field → ABSTAIN if evidence unavailable, DENY if malformed/contradictory. Show missing evidence. |
| 3. Verify quote | Verify signer, key status, digest, terms, amount, token, chain, recipient, nonce, issue/expiry. | Bad signature, changed terms, wrong recipient, expired quote → DENY with reason. |
| 4. Verify counterparty/payee | Separately show provider/agent identity reference and evidence tying this provider to this exact wallet. | Unknown source/key/binding → ABSTAIN; explicit mismatch/revocation → DENY. No single green “identity score.” |
| 5. Evaluate authority | Show principal/agent, mandate version, permitted purchase, token/network, cap and expiry. | Revoked, out-of-scope, or over-cap → DENY; eligible but outside autonomous policy → REQUIRE_HUMAN. |
| 6. Evaluate exposure | Show cap 30, settled/reserved/commitments, proposed 20, remaining after payment (all tUSD8). | Insufficient/stale exposure data → ABSTAIN; deterministic breach → DENY or bounded CONSTRAIN only with explicit user re-authorization. |
| 7. Reserve claim | Persist unique scoped claim and exact intent digest in a database transaction. | Existing active/consumed claim → DENY as duplicate; show original status and no new payment action. |
| 8. Human payment authorization | Display provider, checksum destination, amount, token, chain, job/quote, expiry, exactness and evidence limitations; user confirms in wallet. | Rejection/cancel → CANCELLED and safe reservation release. Wallet chain mismatch → stop. No server key signing. |
| 9. Submit/observe payment | Capture transaction hash; query chain, receipt, token transfer event, sender, recipient, amount and block. | Submission ambiguous → PENDING; keep claim and re-query same hash. Do not retry with a new key. Revert → FAILED only when receipt proves failure. |
| 10. Confirm payment | Apply explicit confirmation/finality rule selected from current chain guidance. Record block hash/height and source. | RPC unavailable or reorg uncertainty → ABSTAIN/PENDING. UI must not label “settled” solely from wallet submission. |
| 11. Verify service receipt | Verify provider key, signature, receipt schema, quote/job/terms digest and receipt timestamp. | Missing → payment may be confirmed but service evidence remains PENDING/ABSTAIN. Invalid signature → reject receipt. Contradiction → EXCEPTION. |
| 12. Reconcile/close | Compare authorized intent, observed token event, receipt claims, exposure ledger. Persist evidence and policy version. | Mismatch → EXCEPTION/NO_GO assessment; preserve payment fact and do not mark service assured. Full supported match → `RECONCILED`; only call transaction `ASSURED` within explicitly stated evidence scope. |

## Off-chain module contracts

Implement these as modules in one application process and one authoritative persistence boundary unless evidence shows a concrete scaling/ownership need.

| Module | Input → output | Evidence and fail-closed behavior |
|---|---|---|
| Offer/quote verifier | Raw offer + source policy → normalized quote + `ALLOW`/`DENY`/`ABSTAIN` | Signature, key status, digest, terms, expiry and freshness. No valid signer or tamper → DENY; unavailable key status → ABSTAIN. |
| Identity/payee verifier | Provider/agent refs + destination + evidence → separate identity and wallet-binding findings | Preserve source/issuer/key, scope, validity and revocation. Never infer legal identity from agent ID or key control. |
| Authority evaluator | Principal, agent, mandate/version, action and transaction → decision + reason codes | Signed/trusted mandate provenance, scope, amount/token/chain, expiry/revocation. Unknown critical mandate state → ABSTAIN. |
| Exposure evaluator | Limit + settled + reserved + obligations + requested amount → capacity arithmetic + decision | Exact integer units, freshness, idempotent reservation accounting. Stale/inconsistent source → ABSTAIN. |
| Claim coordinator | Scoped job/quote/idempotency key + intent digest → RESERVED/duplicate/conflict | Durable unique constraint and compare-and-set state revision. Race produces one reservation only. |
| Payment coordinator | Authorized intent + user wallet result → submitted transaction observation | Verify chain, token and exact params before handoff; no private key custody. Ambiguous result stays pending and uses same transaction hash. |
| Chain verifier | Expected intent + transaction hash + RPC evidence → execution match assessment | Re-read receipt/log and transaction; verify chain ID, token, from/to/value, event and confirmation policy. Multiple sources if available; outage means ABSTAIN. |
| Receipt verifier | Provider receipt + accepted key/evidence policy → valid attribution/scope result | Signature/key status and job/quote/terms binding. Signature alone is not delivery-quality truth. |
| Reconciler/ledger | Intent + chain observation + receipt + exposure entries → reconciled/exception record | Idempotent financial event; no double settlement; immutable evidence references and policy version. |
| Assurance console | Transaction/evidence/decision history → human-readable lifecycle view and explicit user actions | Label SIMULATION or LIVE at every record. Show source/time/reason and stale/unavailable states; no decorative or invented metrics. |

## Small decision vocabulary

- **Pre-execution policy decision:** `ALLOW`, `CONSTRAIN`, `REQUIRE_HUMAN`, `DENY`, `ABSTAIN`.
- **Execution lifecycle:** states in `STATE_MACHINE.md` such as `RESERVED`, `SUBMITTED`, `CONFIRMED`, `PENDING`, `CANCELLED`, `EXCEPTION`.
- **Post-execution consequence assessment:** `GO`, `NO_GO`, `ABSTAIN`; attach any operational `EXCEPTION` separately.

`ALLOW` means eligible to present the exact human-confirmed action; it does not mean payment occurred. `GO` means defined post-execution checks passed; it does not prove service quality or legal discharge. A hard demonstrated contradiction yields DENY pre-execution or NO_GO after execution; missing required evidence yields ABSTAIN. Never map uncertainty to success.

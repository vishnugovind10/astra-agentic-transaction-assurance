# Product Brief — Astra

## 1. Product definition

Astra is a protocol-neutral transaction-assurance layer for autonomous commerce. It binds a commercial request to the identities, authority, counterparty, economic exposure, execution and evidence associated with a machine-initiated transaction.

It complements agent identity and payment rails. It does not replace them.

## 2. Target problem

Agents can initiate purchases faster and across more providers than a human operator can manually review. A payment mandate or wallet cap controls some authority, but does not necessarily bind the entire commercial transaction or establish that the final financial outcome matches the original intent.

The operational problem includes:

- agent identity and principal attribution;
- buyer/seller relationship and receiving-wallet association;
- quote or invoice mutation;
- duplicate job, quote or invoice execution across multiple agents;
- exposure across settled, reserved and outstanding commitments;
- repeated/failed attempts and metered consumption;
- destination, amount, token or network drift;
- missing or conflicting provider receipt;
- payment confirmation versus finality and reconciliation;
- provenance and independent verification of the decision.

## 3. Initial customer/problem wedge

**Agentic procurement of external digital services.** A principal's agent buys a service from a provider agent or service API, such as GPU inference, compute, security analysis, data or API access.

The initial QDay demonstration uses a fixed-price service offer because it keeps commercial terms, payment amount and acceptance of the payment transaction bounded. Metered sessions and invoice workflows are later extensions.

## 4. Actors

- **Principal/buyer organisation:** defines authority and bears the commercial payment obligation.
- **Buyer agent:** initiates a service request under delegated authority.
- **Provider/seller agent:** publishes an offer and supplies the service/receipt.
- **Provider organisation:** may be the legal/commercial supplier behind the provider agent.
- **Identity/credential issuer:** supplies evidence about agent or supplier identity and wallet association.
- **Payment infrastructure:** QDay/Aevum and any supported payment protocol.
- **Independent verifier:** evaluates event/evidence consistency, distinct from the transaction coordinator.

Do not assume that an agent is a legal person or that wallet control proves legal entity ownership.

## 5. Product outcomes

Astra should enable an enterprise or payment platform to answer, with traceable evidence:

1. Who initiated the transaction and under whose authority?
2. What exactly was agreed and quoted?
3. Who was the intended counterparty and payee?
4. What amount and economic exposure were authorised?
5. Was the transaction claim unique and not already executed?
6. What actually happened on-chain?
7. What provider evidence was received?
8. Which checks passed, failed or remain unknown?

## 6. Non-goals

- Replacing Jira, ERP, procurement or accounts-payable systems.
- Replacing QDay's agent identity, wallet control or payment execution roadmap.
- Building a new wallet, token, payment rail, x402 facilitator or MPP clone.
- Issuing credit, guarantees, insurance or custodying customer funds.
- Claiming legal identity or legal discharge from blockchain events alone.
- Guaranteeing service quality based only on provider-signed receipts.
- Building a universal agent reputation score.

## 7. Product boundary with QDay

QDay supplies the execution environment and may supply identity, wallet, payment discovery and payment-session capabilities as those products become available. Astra supplies a complementary commercial transaction-assurance layer: intent binding, counterparty/payee evidence evaluation, exposure/coordination checks, consequence verification and settlement assurance.

The integration must be adapter-based. No future QDay product or API may be treated as available until verified in the starter kit or current developer documentation.

## 8. Success criteria

### Hackathon

- One actual tUSD8 payment on Aevum Testnet.
- One successful end-to-end agent-to-agent service purchase.
- Payee substitution blocked.
- Quote mutation blocked.
- Duplicate claim blocked.
- Exposure breach blocked or constrained.
- Missing service evidence produces an unresolved state, not false success.
- Independently verifiable payment intent, chain event and financial record.

### Product validation

- At least one payment-infrastructure provider or marketplace validates the transaction problem.
- One potential design partner agrees to a scoped technical discovery or pilot discussion.
- Clear integration path to an existing identity and payment rail.

# Astra — Agentic Transaction Assurance

**The assurance layer for autonomous commerce.**

Astra connects commercial intent, agent identity, delegated authority, counterparty/payee evidence, economic exposure, payment execution and settlement assurance.

> **Authorize the intent. Verify the counterparty. Control the exposure. Prove the outcome.**

## Project status

<blockquote>
<strong>PRE-BUILD DOCUMENTATION SNAPSHOT — 11 November 2026</strong><br>
This repository currently contains product and architecture documentation only. The QDay Aevum Builder Challenge requires competition code to be written during the official build window, which opens <strong>16 November 2026 at 06:00 UTC</strong>. No hackathon application, contract or executable test implementation is included in this pre-build snapshot.
</blockquote>

The intended implementation window is 16 November–11 December 2026. The challenge submission deadline is 11 December 2026 at 08:00 UTC.

## The problem

AI agents are beginning to buy compute, inference, data, APIs and services from external providers and other agents. Payment protocols can make a transfer possible; agent identity and wallet controls can bound who may initiate it. But the complete commercial transaction still needs assurance:

- Is the buyer agent acting under the correct principal's authority?
- Is the seller/provider the intended counterparty?
- Is the receiving wallet bound to the intended provider through an acceptable source of evidence?
- Does the signed quote still match the requested service, amount, token, recipient and expiry?
- Has the same job, quote or invoice already been claimed or paid?
- Does the payment create exposure beyond the principal's authorised capacity?
- Did the actual on-chain transfer match the authorised intent?
- Is settlement final and reconciled, and what remains unproven about service delivery?

Astra treats the transaction—not the wallet transfer—as the unit of assurance.

## The initial use case

A buyer agent procures a fixed-price digital service from a provider agent, initially a GPU inference or compute job. The provider returns a signed offer. Astra evaluates identity and payee evidence, authority, quote integrity, exposure and duplicate claims. It then enables an exact tUSD8 payment on Aevum Testnet. The provider supplies a signed service receipt; Astra verifies the available evidence, compares authorised intent with observed execution and records the settlement/reconciliation state.

The hackathon implementation is deliberately narrower than the product architecture: one buyer, one provider, one signed quote, one real tUSD8 transfer, and adversarial cases for payee substitution, quote mutation, duplicate execution, exposure breach and missing evidence.

## Product boundary

Astra is not:

- another agent wallet or payment facilitator;
- a replacement for QDay identity, wallet/session controls or payment discovery;
- a generic x402/MPP implementation;
- a legal identity or KYC provider;
- a credit provider, custodian or lender;
- proof that a provider's work is correct merely because it signed a receipt.

Astra is an assurance and integration layer that consumes trusted identity, enterprise, payment and service evidence from external systems and preserves provenance and uncertainty.

## Architecture at a glance

1. **Agent Commerce Interface** — normalises a service offer, invoice or payment request from an agent/API/protocol.
2. **Identity & Counterparty Assurance** — evaluates agent identity evidence, wallet association, supplier credentials and their freshness/revocation status.
3. **Authority & Economic Consequence** — checks principal delegation, permitted transaction scope and available capacity.
4. **Financial Intelligence & Exposure** — accounts for settled spend, reservations, commitments, fees and bounded future usage.
5. **Coordination & Transaction Integrity** — protects unique job/quote claims, idempotency, replay and conflicting commitments.
6. **Payment Execution** — integrates with Aevum and tUSD8 for the initial flow.
7. **Outcome & Settlement Assurance** — compares intent, chain observations, provider receipts and reconciliation evidence.
8. **Financial & Evidence Ledger** — records decision, event, cost and assurance evidence.

See [Architecture](docs/ARCHITECTURE.md), [Transaction Model](docs/TRANSACTION_MODEL.md), [State Machine](docs/STATE_MACHINE.md), and [Threat Model](docs/THREAT_MODEL.md).

## QDay / Aevum challenge parameters

These values are taken from the official challenge page and must be rechecked against the starter kit before implementation:

| Parameter | Value |
|---|---|
| Network | QDay Aevum Testnet |
| Chain ID | `44005` |
| RPC | `https://rpc-test.qday.info` (verified by read-only chain ID call on 11 Nov 2026; recheck at kickoff) |
| Stablecoin | tUSD8 |
| tUSD8 decimals | 6 |
| tUSD8 contract | `0x456569838f1d43009A91ebA4263147Afd7233483` |
| Build opens | 16 Nov 2026, 06:00 UTC |
| Submission closes | 11 Dec 2026, 08:00 UTC |
| Demo Day | 15 Dec 2026 |

Official challenge: https://qday.io/builder-challenge

Pre-build readiness specifications: [Readiness Audit](docs/READINESS_AUDIT.md), [Aevum Testnet Readiness](docs/QDAY_TESTNET_READINESS.md), [Implementation Specification](docs/IMPLEMENTATION_SPEC.md), [Adversarial Test Plan](docs/ADVERSARIAL_TEST_PLAN.md), [UX Specification](docs/UX_SPEC.md), [Demo Script](docs/DEMO_SCRIPT.md), and [Build-Window Runbook](docs/BUILD_WINDOW_RUNBOOK.md). These are specifications only; competition implementation begins in the official build window.

## Existing architectural lineage

Astra is a new integration product informed by the owner's independent public reference implementations. Their code is not copied into the hackathon project. The relevant capabilities are documented in [Design Lineage](docs/DESIGN_LINEAGE.md):

- [AgencyOS / Agent Trust Probe](https://github.com/vishnugovind10/agent-trust-probe)
- [Agentic FinOps Gateway](https://github.com/vishnugovind10/agentic-finops-ledger)
- [Agentic Financial Consequence Assurance](https://github.com/vishnugovind10/agentic-financial-consequence)
- [Agent Coordination Integrity](https://github.com/vishnugovind10/agent-coordination-integrity)
- [Institutional Settlement State Machine](https://github.com/vishnugovind10/institutional-settlement-state-machine)
- [PayFlow Control Plane](https://github.com/vishnugovind10/payflow-control-plane)

## Website and deployment

The intended website is generated from this repository and deployed to Vercel. It will include the product landing page, interactive assurance demo, transaction explorer, architecture, developer documentation and portfolio lineage.

- **Vercel production URL:** not deployed yet.
- **GitHub repository:** https://github.com/vishnugovind10/astra-agentic-transaction-assurance (public repository; documentation-only pre-build snapshot).

No deployment URL or contract address will be fabricated.

## Build plan

See [Build Plan](docs/BUILD_PLAN.md). The official build window is binding: all competition implementation code must be authored during it. Before the window, this repository is intentionally documentation-only.

## Commercial direction

The intended first product market is agentic procurement of external digital services—compute, inference, APIs, data and specialist agents. Potential customers include agent-payment infrastructure providers, agent/service marketplaces and enterprises using autonomous agents to procure services. See [Commercial Model](docs/COMMERCIAL_MODEL.md).

## Limitations

This document set is an architecture and pre-build specification, not a production product, security audit, legal opinion, financial product or proof of protocol conformance. Production use requires trusted identity sources, secure key management, audited contracts, service evidence policies, reconciliation integrations and applicable legal/payment-provider arrangements.

## License

License selection is intentionally deferred until the owner confirms the intended code and documentation licensing. Do not assume the repository is licensed for reuse before a `LICENSE` file is added.


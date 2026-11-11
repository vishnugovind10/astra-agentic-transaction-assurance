# Design Lineage — Existing Portfolio to Astra

Astra is a new integrated product informed by six independent public reference implementations. They remain separate repositories and retain their own scope, history and limitations. Their source code must not be copied into the QDay hackathon implementation; the challenge requires competition code to be authored within the build period.

## 1. AgencyOS / Agent Trust Probe

Source: https://github.com/vishnugovind10/agent-trust-probe

Existing contributions:
- consequence-bound authority;
- agent identity, mandate and delegation;
- exposure and capacity reservations;
- counterparty relationship and prechecks;
- revocation and explicit `ALLOW`, `CONSTRAIN`, `REQUIRE_HUMAN`, `DENY` decisions;
- lemonade autonomy stress scenario.

Astra mapping: authority and counterparty assurance adapters.

Boundary: Astra must not duplicate AgencyOS identity/governance logic or collapse its decisions into a black-box trust score.

## 2. Agentic FinOps Gateway

Source: https://github.com/vishnugovind10/agentic-finops-ledger

Existing contributions:
- consumption intelligence;
- provider evidence and commercial comparison;
- cost/risk estimation;
- autonomous credit model;
- predicted/actual/variance financial events;
- showback/chargeback and FOCUS/TBM-oriented reporting;
- ledger and reconciliation patterns.

Astra mapping: economic exposure, cost accounting and financial event adapter.

Boundary: forecasts are not automatically incurred liabilities; FinOps does not independently establish chain settlement.

## 3. Agentic Financial Consequence Assurance

Source: https://github.com/vishnugovind10/agentic-financial-consequence

Existing contributions:
- authorised intent versus observed financial consequence;
- destination substitution and transaction drift;
- liquidity, settlement and reconciliation checks;
- stale evidence handling;
- conservative `GO`, `NO_GO`, `ABSTAIN` precedence.

Astra mapping: post-execution consequence verifier.

Boundary: the reference evaluator does not authorise or execute payments.

## 4. Agent Coordination Integrity

Source: https://github.com/vishnugovind10/agent-coordination-integrity

Existing contributions:
- signed participant events;
- event-chain and contract integrity;
- replay and duplicate side-effect checks;
- conflicting exclusive-task commitments;
- independent offline verification;
- `VERIFIED`, `VIOLATION`, `ABSTAIN` outcomes.

Astra mapping: transaction/job claim integrity and independent event verifier.

Boundary: signatures establish attribution to keys, not truth of external facts; existing prototype does not provide production consensus or a persistent witness network.

## 5. Institutional Settlement State Machine

Source: https://github.com/vishnugovind10/institutional-settlement-state-machine

Existing contributions:
- explicit state dimensions;
- finality and liquidity conditions;
- reconciliation and divergence;
- evidence packaging;
- `ALLOW`, `PENDING`, `DENY`, `EXCEPTION`, `ABSTAIN` decisions.

Astra mapping: settlement coordinator and final assurance state.

Boundary: the reference model is synthetic and does not perform real settlement or determine legal finality.

## 6. PayFlow Control Plane

Source: https://github.com/vishnugovind10/payflow-control-plane

Existing contributions:
- payment intent and bounded mandates;
- spend reservations;
- route economics and constraints;
- cost, risk, liquidity, time and counterparty considerations;
- simulated execution and reconciliation states.

Astra mapping: payment economics adapter and future multi-rail seam.

Boundary: the repository uses synthetic route data and does not execute real payment rails.

## 7. Integration principle

Astra does not combine repositories by copying their code or building six dashboards. It defines canonical transaction schemas and narrow adapters around the distinct capabilities. Each source retains its scope and limitations. The hackathon implementation is new code, with design lineage documented for auditability and professional attribution.

## 8. Professional positioning

The portfolio demonstrates a progression from independent mechanisms to an integrated transaction-assurance product:

`Authority → Economic Consequence → Exposure → Coordination → Payment → Settlement → Assurance`

This design lineage should be included in Astra's public website and README, with direct source links and an honest account of what was reused conceptually versus newly implemented.

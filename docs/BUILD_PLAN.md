# QDay Hackathon Build Plan

## 1. Official timeline

Source: https://qday.io/builder-challenge

- Build period opens: 16 November 2026, 06:00 UTC.
- Submission closes: 11 December 2026, 08:00 UTC.
- Demo Day: 15 December 2026.
- Required: one complete stablecoin payment flow on Aevum Testnet using tUSD8.
- All competition code must be written during the build window.

The QDay page states that the starter kit is available and registration opens at the build-window start. Recheck the current starter kit and all environment details at the start.

## 2. Pre-build activities (11–15 November)

Permitted preparation in this repository:
- product and architecture documents;
- standards research and source mapping;
- transaction schema specification in prose;
- threat model and invariants in prose;
- test scenario definitions in prose;
- UI wireframe descriptions;
- demo script and acceptance criteria;
- review QDay starter kit and official developer documentation;
- create the public repository and configure GitHub/Vercel access.

Do not add competition implementation code, executable tests, contract source, frontend source or application configuration before the permitted window.

## 3. Implementation window

### Phase 1 — Foundation (16–18 Nov)
- Inspect official starter kit and verify RPC/chain/token details.
- Create the application and contract implementation from scratch.
- Define typed transaction envelope and domain invariants.
- Deploy a minimal test registry/mandate/settlement contract to Aevum.

### Phase 2 — Assurance (19–23 Nov)
- Implement signed provider offer and payee binding.
- Implement mandate/authority checks.
- Implement exposure reservation and unique claim.
- Implement quote mutation and replay rejection.

### Phase 3 — Payment and evidence (24–29 Nov)
- Execute actual tUSD8 transfer.
- Verify emitted events and recipient balance change.
- Implement provider service receipt and evidence state.
- Implement intent-versus-observed consequence verification.

### Phase 4 — Integrated experience (30 Nov–5 Dec)
- Build assurance console and transaction explorer.
- Build deterministic simulation mode and adversarial cases.
- Integrate FinOps event ledger and settlement assurance.
- Build architecture and design-lineage pages.

### Phase 5 — Hardening and submission (6–11 Dec)
- Run contract, unit, integration and E2E tests.
- Verify deployment and source code.
- Deploy website to Vercel from this repository.
- Record live testnet demonstration.
- Complete README and submission checklist.

## 4. Priority order

P0 — actual Aevum tUSD8 transfer with exact mandate constraints.
P1 — payee binding, quote integrity and one-time claim.
P2 — consequence verification, finality/reconciliation and evidence.
P3 — FinOps record and adversarial UI.
P4 — protocol adapters and broader integration documentation.

Do not expand P0 scope to support every standard or service category.

## 5. Definition of demo-ready

- Valid transaction succeeds with a real testnet transfer.
- Wrong payee is rejected.
- Changed quote is rejected.
- Duplicate claim is rejected.
- Over-limit exposure is rejected or constrained.
- Missing service receipt does not produce an assured service result.
- Actual transfer can be independently compared against the authorised envelope.
- Simulation mode is visibly distinguished from live mode.

## 6. Risk buffer

Keep the final 48 hours for deployment, reproducibility, documentation and demo capture. Do not plan new features after 9 December unless a critical issue requires them.

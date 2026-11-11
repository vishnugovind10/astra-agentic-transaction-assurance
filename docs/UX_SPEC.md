# Astra UX Specification (Pre-Build)

**Audience:** enterprise finance/operations reviewers, payment or agent infrastructure teams, and hackathon judges.  
**Design principle:** show the commercial obligation, its authority/evidence, and the chain-observed outcome in one traceable flow. Avoid implying trust through a generic risk score.

## Global experience rules

- Persistent environment badge: `SIMULATION` or `LIVE · Aevum Testnet (44005)`. Simulation fixtures are visibly synthetic at page and record level.
- Display exact token/network/address/amount and UTC expiry before confirmation. Amounts are formatted from integer units, never estimated.
- Separate evidence dimensions: agent identity reference, provider identity source, wallet binding, mandate, quote integrity, exposure, payment, provider receipt, reconciliation.
- Every status includes reason, source, observed timestamp, freshness and a link to inspect evidence. `UNKNOWN`, `STALE`, `UNAVAILABLE`, `MISMATCH` are visible states.
- Distinguish “wallet prompt opened,” “transaction submitted,” “chain confirmed,” “service receipt verified,” and “reconciled.” Never call a tx hash a completed commercial transaction.
- No fabricated TPV, approval rate, latency, provider counts, charts, or risk scores. Show real project/test status only.

## Routes

| Route | User / purpose | Key information and actions | Empty/loading/failure states and trust cues |
|---|---|---|---|
| `/` | Judge or buyer; understand Astra in one screen. | Proposition; one-sentence problem; four-step intent→authority/payee→payment→evidence flow; clear pre-build/live status; enter demo and architecture. | No vanity metrics. If not deployed, repository/readme status only. Show documentation-only pre-build state until application exists. |
| `/demo` | Judge/operator; walk through one transaction and adversarial cases. | Select simulation or live explicitly; inspect buyer mandate, signed offer, payee binding, exposure; authorize exact transfer; inspect chain evidence and receipt; launch six deterministic attacks. | Simulation has synthetic labels and no wallet button. Live mode shows unsupported wallet, wrong chain, rejected signature, pending RPC, no tUSD8, and evidence-missing states. Reconfirm exact recipient/amount/network in confirmation panel. |
| `/transactions` | Finance/ops; inspect transaction record and exceptions. | Search/filter by lifecycle result; open detail with intent vs actual side-by-side, evidence sources, transaction hash, chain explorer, service evidence, ledger/reconciliation. | Empty state explains no records; loading preserves environment badge; RPC/indexing errors show stale/as-of time and do not fabricate rows. Simulation data separated from live. |
| `/architecture` | Technical reviewer; understand control boundaries. | Lifecycle diagram, trust boundaries, on/off-chain split, modular monolith, no-custom-contract decision for initial slice, unresolved future contract gate. | Mark planned vs implemented. No implied service integrations or deployment. |
| `/docs` | Builder/reviewer; find setup and assurance semantics. | Network config with check date, prerequisites, run commands once implementation exists, canonical model, security limitations, API/contract docs as they become real. | Before start, this is documentation and kickoff checklist. Unknown current configs marked unverified, never silently copied from stale docs. |
| `/lineage` | Judge; see architectural references. | Six independent public reference repos; specific concept → Astra module map; integration boundary; source links. | State explicitly: conceptual lineage does not imply imported source, shared runtime, tested integration, endorsement, or adoption. |

## Transaction detail layout

1. **Header:** transaction ID, environment, current lifecycle state, last updated, top reason.
2. **Intent:** buyer principal/agent, provider and wallet, service/job/quote, exact 20 tUSD8, network, expiry.
3. **Authority and exposure:** mandate source/version/status, 30 tUSD8 cap, settled/reserved/commitment arithmetic, remaining amount.
4. **Evidence:** offer signature/digest/key provenance, payee binding evidence, receipt signature and its limits.
5. **Execution:** wallet result, tx hash, actual token/recipient/amount, block, confirmation policy, official explorer link.
6. **Outcome:** payment status, service evidence status, reconciliation status separately; discrepancies and next operator action.

## Required live and simulation states

- Simulation selected / live selected.
- No wallet available; wrong chain; user rejected wallet prompt; pending wallet response.
- Quote expired while confirmation was open; mandate revoked; payee binding mismatch; duplicate claim.
- Payment pending; RPC unavailable; receipt reorg/confirmation uncertainty; failed transfer.
- Payment confirmed but receipt missing; receipt invalid; service contradiction; reconciliation exception.
- Complete evidence-supported result with limitations stated.

Live mode may only appear after the app is actually connected to a wallet and Aevum; screenshots and demo narration must not turn simulations into chain claims.

# Demo Script and Evidence Plan (Pre-Build)

**Target duration:** 4 minutes. This is a presentation plan only; no Astra UI or payment has been built or executed.

## Story

“An agent can produce a valid-looking payment request, but a successful transfer does not prove it bought the authorized service from the intended provider. Astra binds intent, authority, counterparty, exposure, payment evidence, and service evidence into one reviewable lifecycle.”

## Four-minute sequence

| Time | Screen / action after implementation | Narration and evidence |
|---|---|---|
| 0:00–0:30 | Open product overview; show `LIVE · Aevum Testnet` or `SIMULATION` badge. | State that the pre-build repository was architecture only; describe agent procurement of one fixed-price compute service. No unverified performance metrics. |
| 0:30–1:15 | Open demo offer and transaction details. | Show buyer authority ceiling 30 tUSD8, provider signed quote 20 tUSD8, job/quote/expiry, exact destination, token/chain and provenance. Explain separate agent identity, legal supplier evidence and payee binding. |
| 1:15–1:45 | Show decision and exposure arithmetic. | Demonstrate exact atomic-unit arithmetic, duplicate claim reservation and why the user sees the exact recipient and amount before wallet signing. |
| 1:45–2:30 | Live mode: connect dedicated funded wallet, confirm chain/token/payee/amount, sign exactly once. | Describe wallet prompt as user authorization. Capture transaction hash; wait for receipt. Do not narrate “paid” until independently verified. If using simulation, visibly say “simulated” and do not claim Aevum execution. |
| 2:30–3:10 | Open chain evidence panel and official explorer. | Match transaction hash, chain ID, tUSD8 contract, transfer event sender/recipient/amount and confirmation evidence against authorized envelope. Say what the evidence does not establish. |
| 3:10–3:40 | Show provider service receipt and final lifecycle. | Verify signature/key and quote/job binding. State that signature attests to the provider's claim, not independent service quality. Show payment confirmed separately from service evidence and reconciliation. |
| 3:40–4:00 | Run short adversarial examples: payee change, duplicate, quote mutation, over-limit, missing receipt, reconciliation mismatch. | Show expected deny/duplicate/abstain/exception result. Label all non-chain attacks as simulations. Close: “An agent should not turn an ambiguous commercial instruction into an uncontrolled financial consequence.” |

## Screen-state requirements

- Every screen carries environment and source/as-of state.
- Exact beneficiary, amount, token, chain and quote expiry appear before the wallet handoff.
- Payment, service evidence, and reconciliation each have separate status.
- Failed or missing evidence is visible with a reason; no silent green check.
- Simulated scenarios cannot reuse visual language that implies a real transaction hash or live explorer receipt.

## Evidence checklist for the eventual live transaction

- Screenshot/video shows environment badge and user confirmation summary.
- Wallet address used is dedicated to testnet; no private key or recovery phrase is visible.
- Transaction hash, chain ID 44005, token contract, actual sender, recipient, amount, block and receipt status are captured.
- Independent RPC verification and test-explorer page refer to the same transaction.
- Quote digest and payee binding evidence are retained with issuer/source/version and timestamp.
- Provider receipt signature/key validation result and receipt digest are recorded.
- Reconciliation record compares intent, observed event, receipt and exposure exactly.
- Each adversarial result is explicitly labeled simulated unless it is a real, separately authorized testnet transaction.

## Contingency

If wallet, faucet, RPC, explorer, receipt source, or deployed app is unavailable, switch to the labeled simulation or explain the pending/unavailable state. Never show a fabricated transaction, address, screenshot, metric, or deployment URL. Preserve any real submitted hash and resolve it before considering a retry.

# Build-Window Kickoff Runbook

**Gate:** Do not execute implementation tasks before 16 November 2026, 06:00 UTC (07:00 Europe/Amsterdam). This is a documentation runbook, not code or executable configuration.

## Before kickoff

- Read `READINESS_AUDIT.md`, `QDAY_TESTNET_READINESS.md`, and `IMPLEMENTATION_SPEC.md`.
- Confirm the repository remains clean and public; retain the docs-only history as the pre-build baseline.
- Use a fresh, dedicated test wallet. Keep its seed phrase private/offline. Do not put keys in source, chat, shell history, screenshots, client-side environment variables, or GitHub.
- Ensure a user-selected license decision is recorded before publishing implementation code.
- Review challenge registration and team eligibility.

## 06:00 UTC gate checklist

1. Reopen the official [challenge page](https://qday.io/builder-challenge), [build page](https://qday.io/build), [wallet guide](https://wiki.qday.info/guide/start/add-network), faucet, and official token-address list. Record any changes with UTC timestamp/source.
2. Download and inspect the latest official starter kit and its submission checklist. Resolve discrepancies with event support; do not blindly use stale explorer or token details.
3. Resolve the current QDay documentation conflict: build/home pages mark agent wallets, session keys, paymaster and x402 coming soon, while wiki pages say parts are live but leave Aevum EntryPoint/bundler endpoints and addresses unpublished. Verify actual testnet addresses/endpoints and a reproducible setup. If confirmed usable, evaluate the native session-key path; otherwise proceed with direct user-confirmed EOA tUSD8 transfer. Do not build around unpublished SDK APIs.
4. Register on the official challenge submission platform when available. Capture project/team requirements and deadline in README.
5. Reconfirm read-only RPC chain ID, tUSD8 address/code/decimals/symbol, current explorer, and faucet. Do not start with a payment before chain/token parameters match.
6. Create a fresh application implementation from scratch in this repository. Do not copy or port code from the six lineage repositories. Follow `IMPLEMENTATION_SPEC.md`; keep the first slice small.
7. Write tests alongside implementation for amount precision, quote/recipient/chain binding, mandate/exposure, duplicates, uncertain submission, chain verification and evidence status. Use local simulations/forks where possible before any testnet write.
8. Fund the dedicated wallet with test tokens via the official faucet. Inspect account/chain and balances. Never use mainnet funds.
9. Only after local checks pass, run one controlled live test transaction after explicit wallet review; record hash and independently verify it in RPC and the test explorer.
10. Add the actual user interface and simulation mode; ensure each live/synthetic state is clearly marked.
11. Deploy the real app through the connected Vercel/Git workflow after implementation, verify the production URL/routes, and publish only evidenced claims.

## Vertical-slice order

1. Canonical fixed-price envelope, exact atomic money parsing, and typed evidence records.
2. Signed quote verification and provider-key/payee evidence handling.
3. Authority/exposure evaluation and durable unique claim.
4. Human-controlled exact tUSD8 transfer; uncertain submission recovery without duplicate payment.
5. Independent chain event/receipt verification.
6. Signed provider receipt verification and separate service status.
7. Reconciliation record and operator demo console.
8. Adversarial scenarios and reproducible presentation.

Do not add an Astra contract until the user-controlled direct-transfer boundary is shown to be inadequate for the actual demo. If required, make that design change explicit, test it locally, and re-evaluate token approvals and threat model before deployment.

## Submission evidence checklist

- Public repo commit/branch and clean build instructions.
- Test outputs generated during the permitted window.
- Actual Aevum chain ID, token contract and deployed Astra addresses (if any), all from live evidence.
- One real payment hash plus explorer link; exact token transfer event, sender, recipient, amount and block.
- Provider receipt verification evidence and explicit limits.
- Six adversarial outcomes with screenshots/logs identifying simulation vs live.
- Vercel production URL, timestamped route checks, and deployed commit SHA.
- Final README describes what is implemented, simulated, unavailable and not verified.

## Contingency behavior

- Faucet unavailable: continue local simulation/testing; do not substitute a fake live transfer.
- RPC/explorer unavailable: retain transaction as pending/unverified; do not retry payment blindly. Re-query the original hash later.
- Token interface/address discrepancy: stop live payment and check the official contract list/support.
- Wallet issue: run simulation mode; no seed/private key workaround.
- Provider receipt absent/invalid: show payment outcome separately; never claim service assurance.
- Deployment delayed: present packaged local runnable demo if supported by challenge rules and document limitation; do not publish a fabricated URL.

# Pre-Build Readiness Audit

**Audit date:** 11 November 2026 (Europe/Amsterdam)  
**Repository baseline:** `main`, clean at `77c0b0ec713c925510b1077d3369959ed10985bf` before this sprint.  
**Rule:** This sprint adds documentation and performs read-only verification only. No Astra implementation, executable test, deployment, or test-token transaction is included.

## Current state

### Complete before this sprint

- Public GitHub repository exists and has a documentation-only README, product brief, architecture, transaction model, lifecycle, threat model, standards map, lineage, commercial direction, and coarse build plan.
- The README clearly labels the pre-build status; `SECURITY.md` and `CONTRIBUTING.md` exist.
- Git remote is `origin`, branch is `main`; working tree was clean at audit start.
- The design lineage names six independent public reference repositories and says their source is not copied.

### Gaps corrected in this sprint

- No dated, source-backed QDay environment record; no token/RPC verification result or distinction between a code response and live transaction evidence.
- Starter kit and current QDay pages disagree in a few details: the kit's setup section has the test explorer, but its resources table also lists a non-test explorer; the kit describes ERC-8004 as required for a Confirmation of Payee flow, while the challenge page does not make it a universal submission requirement and ERC-8004 is still a draft. QDay's marketing/build pages mark several agent-payment features as coming soon while current wiki guides say parts of ERC-4337/session-key/paymaster and x402 are live on testnet; critical Aevum endpoints/addresses remain unpublished. Practical availability is unresolved.
- Architecture did not decide whether Astra needs any custom contract for the one-payment demo.
- The envelope lacked explicit field canonicalisation, issuer/key provenance, and transaction submission recovery semantics.
- Decision and lifecycle vocabularies overlapped (`ALLOW`/`GO`, `NO_GO`/`DENY`, `ABSTAIN`/`PENDING`/`EXCEPTION`). The new specs define separate pre-execution decision, execution lifecycle, and post-execution assessment dimensions.
- Demo UX, acceptance evidence, start-day runbook, and local tool inventory were not captured.

## Official environment verification

The challenge page and current QDay build/wiki pages were checked on 11 November 2026. The Aevum RPC returned chain ID `0xabe5` (`44005`). Read-only calls to the listed tUSD8 address returned deployed bytecode, decimals `6`, and ABI-encoded symbol/name `tUSD8`. HTTP HEAD returned 200 for the official test explorer and faucet. This does not prove faucet eligibility, wallet funding, a successful token transfer, confirmation/finality guarantees, or explorer indexing behavior.

QDay's current public surfaces conflict on testnet ERC-4337/session keys/paymaster and x402 readiness: home/build pages label agent-payment features coming soon, while current wiki pages say parts are live but leave Aevum EntryPoint/bundler endpoints and addresses unpublished. Treat them as **unverified for practical integration**, not definitively unavailable. The kickoff decision gate is in `BUILD_WINDOW_RUNBOOK.md`.

The contract returned deployed code with a proxy-like shape; exact implementation/admin semantics and verified source were not established during this read-only probe. Do not treat the address/code observation as an audit or as evidence that all ERC-20 paths work. Recheck against the challenge starter kit and official contract-address list at kickoff.

## Incomplete or decision-ready items

- The minimum demo should use one modular monolith. Prefer a user-confirmed direct tUSD8 transfer unless the kickoff check resolves QDay's claimed native session-key infrastructure into a usable, documented Aevum integration. The first vertical slice does not need custom Astra contracts: custom authorization/settlement contracts add deployment and token-approval risk while a single testnet payment can demonstrate evidence comparison. In the direct-transfer variant, the demo does **not** claim to prevent an agent with unrestricted wallet signing authority from bypassing the UI.
- Simulation fixtures, live transaction records, and evidence provenance need clear separation in the eventual app.
- The exact tUSD8 contract, ERC-20 behavior, token list, starter kit revision, registration/submission URL, and challenge-specific identity requirement must be rechecked on 16 November.
- No signed service receipt format or independent service-quality oracle is defined. The sprint specifies a bounded provider-signed receipt as attribution evidence only.

## User action before or at kickoff

1. Review the proposed one-payment demo and unresolved contract boundary in `IMPLEMENTATION_SPEC.md`.
2. Register on the challenge submission platform when registration opens; team formation/eligibility details are in the starter kit.
3. Choose a fresh, dedicated test-only wallet. Save the recovery phrase offline; never paste it into chat, source, or a website. Fund only from the official faucet and only with test tokens. Keep a separate everyday wallet separate.
4. At kickoff, retrieve the latest starter kit, inspect its exact contract-address list and submission process, and note any revised rules.
5. Decide the documentation/code license before adding source code; the public repository currently grants no reuse license.

## Must wait until 16 November 2026, 06:00 UTC

All Solidity, application source, executable tests, scripts, build configuration, CI, deployment configuration, generated application assets, contract deployment, wallet transactions, and Vercel implementation/deployment. Start with the runbook in `BUILD_WINDOW_RUNBOOK.md`.

## Environment snapshot

See `QDAY_TESTNET_READINESS.md` for exact tool versions. Git, Node.js, npm, pnpm, Foundry (`forge`/`cast`) are present. A standalone `solc`, Vercel CLI, and wallet-named command were not found. Browser availability is supplied by the desktop environment; this command-line inventory does not enumerate installed browser extensions. No tools were installed or changed.

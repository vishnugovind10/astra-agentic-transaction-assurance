# Standards and Ecosystem Status (11 November 2026)

**Status labels are dated observations, not integration claims.** Check every mutable product and draft before implementing. Official-source URLs are listed below.

| Item | Current status | Astra relevance / boundary |
|---|---|---|
| QDay Aevum | **Available now:** public EVM-compatible testnet, chain 44005; ordinary EVM tools/RPC supported. | Initial testnet rail. Native Aevum/tUSD8 transaction is in scope. No claim of mainnet readiness. |
| QDay Trellis Wallet | Product page marks it **coming soon**. Separate wiki claims ERC-4337 agent session-key infrastructure is live on Aevum, but implementation endpoints/addresses are not documented. | Resolve conflicting availability claims at kickoff; no dependency until reproducible public setup exists. |
| QDay Signet | Product page marks the hosted agent payment product **coming soon**; wiki separately claims an x402 facilitator is live on testnet. | Conflicting status; hosted Signet and direct facilitator may be different components. Do not conflate them. Re-evaluate only if a public, documented testnet path is confirmed. |
| QPass | Mentioned as future agent credential functionality within Trellis/Signet pages; **no usable public integration established**. | Do not claim credentials, identity validation or integration. Use explicit demo evidence references. |
| QDay x402, SDKs/APIs, smart accounts, stablecoin paymaster | **Conflicting official status:** build/home pages say coming soon; wiki says ERC-4337/session keys/paymaster and x402 facilitator are live on Aevum, while also saying EntryPoint/bundler details will be published and QDay SDK is unpublished. | Treat as `UNVERIFIED` integration readiness until working endpoints, addresses, credentials, and a reproducible setup are available. Ordinary EVM/ERC-20 direct transfer remains a fallback. |
| ERC-20 | **Available standard/interface; tUSD8 is identified by QDay as an ERC-20.** | Use exact official token address; verify decimals and transfer behavior at kickoff. Token contract behavior/source still requires inspection. |
| ERC-8004 | **Draft** EIP as of source review; standard proposes on-chain agent registration/reputation/validation. No Aevum registry deployment verified. | Design reference only; registration is not legal identity or service-quality proof. Do not claim compliance or require it unless official challenge/starter kit confirms a specific requirement and a usable deployment exists. |
| AP2 (Agent Payments Protocol) | Public protocol repository/specification available; its document describes mandate, receipt, and role responsibilities. | Map selected evidence concepts only; do not implement or claim whole-protocol compatibility. Its commerce/payment roles differ from this direct ERC-20 demo. Pin exact version and field mapping if used. |
| x402 | Open protocol/specification and implementations are available; QDay's official pages conflict on whether its testnet facilitator is live. | Generic concepts may inform a provider request/receipt adapter, but no Aevum integration dependency until a documented, reproducible path is confirmed. |
| Machine Payments Protocol (MPP) | Publicly announced as an open standard co-authored by Tempo and Stripe; current deployment/rail support must be checked at implementation. | Design reference only; unrelated to the single fixed-price tUSD8 transfer unless later mapped. Do not build another payment handshake. |
| Confirmation of Payee (CoP) | A general payment-risk concept; no QDay-specific CoP service or deployment verified. | Astra demonstrates exact provider-to-wallet evidence binding. This is a bounded application check, not bank-grade name-checking or legal identity verification. |
| A2A / MCP | Public communication/tool protocols. | Discovery/context adapters may be future edges; a tool transcript is not commercial authority or payment/service evidence. Not needed for first slice. |

## Starter-kit versus current product status

The challenge starter kit recommends ERC-8004 for its Confirmation of Payee track and names a general explorer URL in its resource table. The challenge page allows other tracks and requires QDay integration, but does not state that every submission must use ERC-8004. The official current testnet setup/build page identifies `explorer-test.qday.info`. Until event staff clarify otherwise, treat ERC-8004 as an optional challenge-specific reference, and use the testnet explorer for Aevum evidence.

## Mapping labels for any future adapter

- `NATIVE_IMPLEMENTATION`: exact protocol functionality, with version and conformance evidence.
- `COMPATIBLE_MAPPING`: a limited translation of selected fields/semantics; not full conformance.
- `SIMULATED`: deterministic synthetic fixture only.
- `PLANNED`: no implementation.

Never describe field mapping or mock data as protocol compliance or live integration.

## Primary sources reviewed

- [QDay Builder Challenge](https://qday.io/builder-challenge)
- [QDay Build on QDay](https://qday.io/build)
- [QDay Getting Started](https://wiki.qday.info/Knowledge/technical/developer-guide/getting-started/)
- [QDay ERC-4337 Development](https://wiki.qday.info/Knowledge/technical/developer-guide/erc-4337-development/)
- [QDay AI Agent Development](https://wiki.qday.info/Knowledge/technical/developer-guide/ai-agent-development/)
- [QDay Trellis](https://qday.io/trellis)
- [QDay Signet](https://qday.io/signet)
- [ERC-8004 EIP](https://eips.ethereum.org/EIPS/eip-8004)
- [AP2 specification](https://github.com/google-agentic-commerce/AP2/blob/main/docs/ap2/specification.md)
- [x402 protocol repository](https://github.com/coinbase/x402)
- [MPP announcement by Stripe](https://stripe.com/blog/machine-payments-protocol)
- [Aevum token list / official links to confirm at kickoff](https://wiki.qday.info/guide/start/faucet)

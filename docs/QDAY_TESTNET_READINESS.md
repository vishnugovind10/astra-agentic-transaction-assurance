# QDay Aevum Testnet Readiness

**Checked:** 11 November 2026  
**Scope:** official-source review plus read-only RPC/HTTP calls. No wallet was connected and no transaction was sent.

Statuses: `VERIFIED` means confirmed from an official source or read-only endpoint on the date shown; it does not promise future availability. `UNVERIFIED` means not confirmed. `REQUIRES_USER_ACTION` means the user must perform a wallet/account action. `UNAVAILABLE` means the referenced service/feature is not currently presented as usable.

## Network and token facts

| Item | Value / status | Evidence and implication |
|---|---|---|
| Network name | `QDay Aevum Testnet` — `VERIFIED` | [QDay wallet guide](https://wiki.qday.info/guide/start/add-network), checked 11 Nov 2026. Use the testnet only. |
| Chain ID | `44005` (`0xabe5`) — `VERIFIED` | Official [build page](https://qday.io/build) and wallet guide; read-only `eth_chainId` returned `0xabe5`. App must reject a mismatched chain. |
| HTTPS RPC | `https://rpc-test.qday.info` — `VERIFIED` | Official build page and wallet guide; read-only JSON-RPC responded. Availability is a point-in-time observation, not an SLA. |
| WebSocket RPC | `wss://rpc-test.qday.info` — `VERIFIED` as published | Official [build page](https://qday.io/build). A WebSocket session was not probed; kickoff should test it only if needed. |
| Explorer | `https://explorer-test.qday.info` — `VERIFIED` as official link | Published by build page and wallet guide; HTTP HEAD returned 200. A rendered transaction detail page was not tested. Do not use `https://explorer.qday.info` from the starter kit's resource table for testnet evidence without confirming it is the current test explorer. |
| Native gas asset | `tQDAY`, 18 decimals — `VERIFIED` as documented | Official build page. The faucet can provide gas for a fresh wallet. |
| Faucet | `https://faucet.qday.info/en` — `VERIFIED` as official endpoint; wallet funding `REQUIRES_USER_ACTION` | Challenge page says faucet is working and provides test gas and tUSD8; endpoint returned HTTP 200. The starter kit says request tokens by entering a wallet address; actual claim/cooldown is controlled by faucet. Do not submit someone else's address. |
| tUSD8 address | `0x456569838f1d43009A91ebA4263147Afd7233483` — `VERIFIED` on official challenge page | [Challenge page](https://qday.io/builder-challenge), checked 11 Nov. Recheck official contract list before interacting. |
| tUSD8 decimals | `6` — `VERIFIED` | Challenge page states six decimals; read-only `decimals()` returned 6. Demo 20 tUSD8 = 20,000,000 atomic units; this is a proposed parameter, not an executed payment. |
| tUSD8 code/metadata | `VERIFIED` for bytecode + read methods only | `eth_getCode` returned non-empty code; read-only `decimals()`, `symbol()`, `name()` returned six, `tUSD8`, `tUSD8`. Observed code has a proxy-like shape; implementation/source verification and admin semantics remain `UNVERIFIED`. |
| ERC-20 transfer/balance behavior | `UNVERIFIED` end-to-end | Method reads and official ERC-20 description are not a successful transfer. Verify from the challenge wallet during the build window using test tokens only. |
| EVM compatibility/tooling | `VERIFIED` as officially supported | [Build page](https://qday.io/build) lists Foundry, Hardhat, Remix, ethers.js, viem, wagmi; no QDay-specific SDK is needed for ordinary EVM interaction. The smart-contract guide demonstrates Solidity `^0.8.20`, but no single required compiler release/EVM target is established; choose after reviewing the kickoff kit and toolchain. |
| ERC-4337/session keys/paymaster | `UNVERIFIED` for practical Astra integration because official QDay sources conflict | The current wiki's [ERC-4337 page](https://wiki.qday.info/Knowledge/technical/developer-guide/erc-4337-development/) says testnet smart accounts, agent session keys and stablecoin paymaster are live, but also says Aevum EntryPoint and bundler endpoints will be published in docs. Its [wallet page](https://wiki.qday.info/Knowledge/technical/developer-guide/wallet-integration/) says Aevum smart accounts are live while example SDK is planned/unpublished. The public [Build page](https://qday.io/build) still labels these capabilities coming soon. No usable Aevum endpoint, addresses or reproducible setup were confirmed in this audit. Do not assume usable or unavailable; resolve with the starter kit/current developer docs/QDay support at kickoff. |
| x402 facilitator | `UNVERIFIED` for practical Astra integration because official sources conflict | The [AI Agent Development page](https://wiki.qday.info/Knowledge/technical/developer-guide/ai-agent-development/) says the facilitator is live on testnet; current QDay [home/build](https://qday.io/build) labels x402 agent support coming soon. No endpoint, contract, credential or starter-kit instruction was established. Treat as optional until a working documented testnet path is demonstrated. |
| Contract deployment and explorer verification | Supported process documented; exact verifier UI/API `UNVERIFIED` | Build page instructs deploy and verify on explorer; verify current flow after login/registration if required. |
| Starter kit | `VERIFIED` available; inspected 8-page PDF downloaded from challenge page | The kit confirms timeline, setup, faucet, deliverables, judging and eligibility. Its resource table has a test/mainnet explorer ambiguity; use the test endpoint above and verify at kickoff. |
| Competition account/registration | `REQUIRES_USER_ACTION` | The challenge page says registration opens 16 Nov 06:00 UTC. Register on the linked submission platform; do not assume pre-registration is full event registration. |

## Point-in-time RPC checks

On 11 November 2026, direct read-only JSON-RPC calls to `https://rpc-test.qday.info` returned:

- `eth_chainId` → `0xabe5`.
- `eth_getCode` at the tUSD8 address → non-empty deployed bytecode.
- ERC-20 `decimals()`, `symbol()`, `name()` → `6`, `tUSD8`, `tUSD8`.
- `explorer-test.qday.info` and `faucet.qday.info/en` returned HTTP 200 to HEAD requests.

No wallet was connected; no balance was queried for a personal address; no faucet claim, allowance, transfer, contract deployment, or other write occurred.

## Starter-kit notes

The official eight-page starter kit was inspected on 11 November. It gives the same chain/RPC/WebSocket, tQDAY symbol, explorer-test URL, faucet URL, and tUSD8 address/decimals. It lists the Dorahacks submission page and says all code must be developed during the build window. It calls ERC-8004 the specific identity-to-address standard for its Confirmation of Payee flow; the general challenge page permits multiple tracks and does not make that a universal technical requirement. ERC-8004 itself is still a draft and any Aevum deployment must be checked before depending on it. Current QDay marketing/build pages and the wiki's more detailed AA/x402 pages conflict on whether agent-payment capabilities are available on testnet; the wiki also leaves key deployment endpoints/addresses for later publication. Resolve with event staff and actual documentation before building a dependency. Treat the starter kit as event guidance, recheck mutable details and contact event staff for any discrepancy.

The kit suggests mainstream EVM tools. Its deployment examples show a private key environment variable as a command-line parameter; do not place any key in source, shell history, screenshots, logs, frontend variables, or this repository. Prefer the dedicated wallet with local wallet signing and never use a wallet holding valuable assets.

## Wallet and funding steps

1. Create a new wallet dedicated to the hackathon, preferably in a separate browser profile/device. Record the recovery phrase offline in a secure place; never share it.
2. Add Aevum Testnet using the official [wallet guide](https://wiki.qday.info/guide/start/add-network) and verify chain ID 44005, RPC, test explorer, and currency before signing.
3. Use the official [faucet](https://faucet.qday.info/en) to request only test tokens. The challenge site states faucet access is open; the kit describes per-address/cooldown limits. Funding is a user action and was not performed in this audit.
4. Confirm the wallet is on Aevum and inspect its tQDAY/tUSD8 balances. Do not authorize an unlimited token allowance for the demo; avoid approvals entirely if the selected flow is a direct transfer.
5. At implementation time, verify token address from the current official contract list and inspect wallet transaction details before signing.

## Official sources

- [Builder Challenge](https://qday.io/builder-challenge)
- [Build on QDay](https://qday.io/build)
- [Add QDay to your wallet](https://wiki.qday.info/guide/start/add-network)
- [Get testnet tokens](https://wiki.qday.info/guide/start/faucet)
- [Aevum Testnet explorer](https://explorer-test.qday.info/)
- [Aevum faucet](https://faucet.qday.info/en)
- [Challenge starter kit](https://drive.google.com/file/d/1QvkDX_lgbnKh4wZHc-gWMpWsn8YdHCOu/view?usp=sharing)
- [QDay Smart Contract Development](https://wiki.qday.info/Knowledge/technical/developer-guide/smart-contract-development/)
- [QDay Contract Verification](https://wiki.qday.info/Knowledge/technical/developer-guide/contract-verification/)
- [QDay ERC-4337 Development](https://wiki.qday.info/Knowledge/technical/developer-guide/erc-4337-development/)
- [QDay AI Agent Development](https://wiki.qday.info/Knowledge/technical/developer-guide/ai-agent-development/)
- [QDay Wallet Integration](https://wiki.qday.info/Knowledge/technical/developer-guide/wallet-integration/)

## Local development environment (11 November 2026)

Read-only command inventory; no package installation or environment change was performed.

| Tool | Availability/version | Note |
|---|---|---|
| Git | `2.56.1.windows.1` | Available. |
| Node.js | `v26.13.0` | Available. Recheck supported framework requirements at kickoff. |
| npm | `11.20.0` | Available. |
| pnpm | `11.25.0` from a Codex WindowsApps fallback shim | Command resolves to Codex runtime shim, not independently verified standalone pnpm installation. |
| Foundry `forge` / `cast` | `1.7.1` | Available; `cast` read-only RPC query used for chain/token checks. |
| `solc` | Not found as standalone command | Foundry may manage compiler versions, but this was not tested; do not install before build start. |
| Vercel CLI | Not found | No deployment configuration was created. Use the authorized connected Vercel/GitHub workflow during build. |
| Browser / wallet extension | Google Chrome `157.0.8103.39` found; browser access also available through Codex desktop. No MetaMask or other wallet executable was found by command inventory; extensions were not inspected. | Confirm a dedicated test-only profile and wallet at kickoff. |

These are machine-local observations and may change. Do not install or change tooling during this sprint; confirm team/tool requirements and versions on 16 November.

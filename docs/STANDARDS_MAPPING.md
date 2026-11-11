# Standards and Ecosystem Mapping

This document defines intended integration boundaries. It is not a claim of full conformance.

## QDay / Aevum

- Aevum Testnet is EVM-compatible.
- Chain ID: 44005.
- Official challenge tUSD8 is an ERC-20 with six decimals at `0x456569838f1d43009A91ebA4263147Afd7233483`.
- RPC currently shown on QDay's website: `https://rpc-test.qday.info`.
- Recheck all values and starter-kit instructions on 16 November before implementation.

Astra's first execution adapter is Aevum/tUSD8. No dependency on unreleased smart-wallet or x402 facilitator features is allowed for the MVP.

## ERC-8004

Official specification: https://eips.ethereum.org/EIPS/eip-8004

Potential use:
- agent registry reference;
- registration metadata;
- agent wallet association;
- reputation/validation references.

Boundary:
- registration is not legal entity identity verification;
- wallet proof is not beneficial ownership verification;
- reputation is not proof of service quality;
- deployment and registry availability on Aevum must be verified before claiming live integration.

## AP2

Repository/specification: https://github.com/google-agentic-commerce/AP2

Potential use:
- payment mandate concepts;
- checkout/payment intent binding;
- signed transaction evidence and receipts.

Boundary: Astra should map to AP2-compatible evidence where appropriate, not create a competing general payment mandate protocol. Specify the exact version and fields implemented.

## x402

Repository/specification: https://github.com/coinbase/x402

Potential use:
- machine payment request/response;
- payment requirements and settlement receipt;
- adapter for paid API/service calls.

Boundary: x402 payment success does not itself establish principal-level business purpose, aggregate exposure, provider service quality or enterprise reconciliation.

## Machine Payments Protocol (MPP)

Repository/specification: https://github.com/google-agentic-commerce/protocol

Potential use:
- charge/session/subscription models;
- metered usage integration;
- payment receipt mapping.

Boundary: do not implement a second payment handshake. Map MPP requests and session usage into Astra's transaction envelope and economic controls.

## A2A and MCP

A2A: https://github.com/a2aproject/A2A
MCP: https://modelcontextprotocol.io/

Potential use:
- service discovery and agent communication;
- tool/resource invocation context;
- correlation of job IDs and provider responses.

Boundary: tool traces are not proof of commercial authority or settlement.

## Verification labels

Every adapter must state one of:
- `NATIVE_IMPLEMENTATION` — directly implements the relevant spec functionality.
- `COMPATIBLE_MAPPING` — maps fields/semantics but is not a complete conformance implementation.
- `SIMULATED` — uses deterministic test fixtures.
- `PLANNED` — not implemented.

Never describe a compatible mapping or simulation as full protocol compliance.

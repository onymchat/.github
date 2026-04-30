<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.png">
    <img src="assets/logo-light.png" alt="Onym Chat" width="112">
  </picture>
</p>

<p align="center">
  <em>Private group messaging, anchored on Stellar.</em>
</p>

<p align="center">
  <sub>ZK-VERIFIED GOVERNANCE&nbsp;&nbsp;·&nbsp;&nbsp;CONSTANT ON-CHAIN COST</sub>
</p>

<br>

Group membership lives inside a zero-knowledge proof. The Stellar ledger holds 32 bytes per group — a Poseidon commitment, opaque, constant in size regardless of membership. Nobody learns who's in the group: not a server, not a federation, not a validator.

## Architecture

```
                           user
                             │
                             │  propose · vote · evict
                             ▼
            ┌─────────────────────────────────┐
            │      iOS  ·  Android  apps      │   01   clients
            └────────────────┬────────────────┘
                             │
                             ▼
            ┌─────────────────────────────────┐
            │     Swift  ·  Kotlin   SDKs     │   02   bindings
            └────────────────┬────────────────┘
                             │
                             ▼
            ┌─────────────────────────────────┐
            │            Rust  core           │
            │    Groth16 prover · Poseidon    │   03   proving
            │            BLS12-381            │
            └────────────────┬────────────────┘
                             │
                             │   192-byte proof
                             │   +  32-byte next commitment
                             │
               ┌─────────────┴─────────────┐
               │                           │
               ▼                           ▼
    ┌─────────────────────┐     ┌─────────────────────┐
    │   Nostr · Blossom   │     │       Relayer       │
    │                     │     │        (Axum)       │   04   gas
    │   E2E msgs · blobs  │     │     signs · pays    │
    └─────────────────────┘     └──────────┬──────────┘
       transport layer
       off-chain · side channel            ▼
                                ┌─────────────────────┐
                                │   Soroban contract  │
                                │      3 pairings     │   05   on-chain
                                │   ~7M instructions  │
                                └──────────┬──────────┘
                                           │
                                           ▼   emits event
                                ┌─────────────────────┐
                                │    Stellar ledger   │
                                │   32 bytes / group  │       ← only public state
                                │  opaque · constant  │
                                └─────────────────────┘


           governance — enforced in-circuit, not by policy

           1v1   ·   anarchy   ·   democracy   ·   tyranny   ·   oligarchy
```

## Governance

Five shapes of trust. All enforced inside the Groth16 proof — never by policy, never by a moderator.

**1v1** — Two-party thread. Strongest privacy guarantee, simplest commitment.

**Anarchy** — Open-add for trusted circles. No moderator, no ceremony.

**Democracy** — ≥50% of current members co-open the next commitment in-circuit. Quorum is a circuit constraint.

**Tyranny** — Single root authority. Fast updates, no quorum, no recourse.

**Oligarchy** — Separate admin Merkle tree. Editors gate commitment updates; editors stay distinct from members.

## Why Stellar

Pairing-friendly host calls (BLS12-381, CAP-0059) and Poseidon as a host function (CAP-0075) make Groth16 verification practical at production cost. The relayer-pays model means users don't need a funded account, a wallet, or a native asset to send a message.

## Repos

The org is laid out in layers, top-to-bottom:

`onym-clients` (iOS + Android)  ·  `onym-sdk-swift`  ·  `onym-sdk-kotlin`  ·  `onym-relayer`

Per-governance modules (Rust circuit + Soroban contract):

`onym-sep-1v1`  ·  `onym-sep-anarchy`  ·  `onym-sep-democracy`  ·  `onym-sep-tyranny`  ·  `onym-sep-oligarchy`

Reference:

`onym-papers`  ·  `onym-docs`

## Links

[onym.chat](https://onym.chat)  ·  Telegram [@onymchat](https://t.me/onymchat)  ·  Twitter [@onymchat](https://x.com/onymchat)

<br>

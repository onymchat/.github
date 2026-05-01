<p align="center">
  <em>Private group messaging with governance, anchored on Stellar.</em>
</p>

<p align="center">
  <sub>ZK-VERIFIED GOVERNANCE&nbsp;&nbsp;·&nbsp;&nbsp;CONSTANT ON-CHAIN COST</sub>
</p>

<br>

Group membership lives inside a zero-knowledge proof. The Stellar ledger holds 32 bytes per group — a Poseidon commitment, opaque, constant in size regardless of membership. Nobody learns who's in the group: not a server, not a federation, not a validator.

## Architecture

<pre>
                           user
                             │
                             │  propose · vote · evict
                             │
               ┌─────────────┴─────────────┐
               │                           │
               ▼                           ▼
    ┌─────────────────────┐     ┌─────────────────────┐
    │       <a href="https://github.com/onymchat/onym-ios">iOS</a> app       │     │     <a href="https://github.com/onymchat/onym-android">Android</a> app     │   01   clients
    └──────────┬──────────┘     └──────────┬──────────┘
               │                           │
               ▼                           ▼
    ┌─────────────────────┐     ┌─────────────────────┐
    │      <a href="https://github.com/onymchat/onym-sdk-swift">Swift SDK</a>      │     │     <a href="https://github.com/onymchat/onym-sdk-kotlin">Kotlin SDK</a>      │   02   bindings
    └──────────┬──────────┘     └──────────┬──────────┘
               │                           │
               └─────────────┬─────────────┘
                             │
                             ▼
            ┌─────────────────────────────────┐
            │            Rust  core           │
            │     <a href="https://github.com/onymchat/onym-contracts/tree/main/plonk/prover">PLONK prover</a> · Poseidon     │   03   proving
            │            BLS12-381            │
            └────────────────┬────────────────┘
                             │
                             │   1601-byte proof
                             │   +  32-byte next commitment
                             │
               ┌─────────────┴─────────────┐
               │                           │
               ▼                           ▼
    ┌─────────────────────┐     ┌─────────────────────┐
    │   Nostr · Blossom   │     │       <a href="https://github.com/onymchat/onym-relayer">Relayer</a>       │
    │                     │     │        (Axum)       │   04   gas
    │   E2E msgs · blobs  │     │     signs · pays    │
    └─────────────────────┘     └──────────┬──────────┘
       transport layer
       off-chain · side channel            ▼
                                ┌─────────────────────┐
                                │   <a href="https://github.com/onymchat/onym-contracts/tree/main/plonk">Soroban contract</a>  │
                                │      2 pairings     │   05   on-chain
                                │  ~12M instructions  │
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
</pre>

## Governance

Five shapes of trust. All enforced inside the PLONK proof — never by policy, never by a moderator.

**1v1** — Two-party thread. Strongest privacy guarantee, simplest commitment.

**Anarchy** — Open-add for trusted circles. No moderator, no ceremony.

**Democracy** — ≥50% of current members co-open the next commitment in-circuit. **Not shipping today** — the current circuit accepts simplified proofs that do not enforce the K-of-N member quorum; the threshold gate is deferred until the K_MAX > 2 prover work lands. Don't pick this flavor for production until that ships.

**Tyranny** — Single root authority. Fast updates, no quorum, no recourse.

**Oligarchy** — Separate admin Merkle tree, K-of-N admin quorum gates commitment updates (K up to 2 today, K_MAX raises planned). At threshold = 2, two co-signing admins are required — it is not a "any single admin can update the tree" model. Editors stay distinct from members.

## Why Stellar

Pairing-friendly host calls (BLS12-381, CAP-0059) and Poseidon as a host function (CAP-0075) make PLONK verification practical at production cost. The relayer-pays model means users don't need a funded account, a wallet, or a native asset to send a message.

## Repos

The org is laid out in layers, top-to-bottom:

`onym-ios`  ·  `onym-android`  ·  `onym-sdk-swift`  ·  `onym-sdk-kotlin`  ·  `onym-relayer`  ·  `onym-contracts`

`onym-contracts` ships all five Soroban contracts — one per governance shape — in a single Cargo workspace.

Reference:

`onym-papers`  ·  `onym-docs`

## Design-space topology — where we are, where we're heading

```
                          DESIGN-SPACE TOPOLOGY
                     (★ = where we are now, ▷ = aimed-at)


              PAST                   PRESENT                  FUTURE
            ─────────               ─────────                ─────────

         ┌─────────────┐        ┌─────────────┐          ┌─────────────┐
         │   C1-old    │        │    C1 ★     │          │     C7      │
         │             │        │             │          │             │
         │  Groth16    │ ──Δ──► │   PLONK     │ ──╳──►   │  Stellar +  │
         │  + per-     │        │   + KZG     │          │  Plonky3 +  │
         │  circuit    │ SNARK  │   + EF SRS  │ blocked  │     FRI     │
         │  MPC × 30   │ family │             │ on FRI   │             │
         │             │ swap + │  pairing-   │ host     │  hash-based │
         │ pairing-    │ SRS    │  based      │ functions│  PQ ★★      │
         │ based       │ reuse  │             │          │             │
         │             │        │             │          │  transparent│
         │ trapdoor:   │        │ trapdoor:   │          │  setup      │
         │ 1-of-N ×30  │        │ 1-of-141k×1 │          │             │
         └─────────────┘        └─────────────┘          └─────────────┘
            (deprecated)                                  (post-quantum,
                                                           host-blocked,
                                                            Class b)

```

## Links

[onym.chat](https://onym.chat)  ·  Telegram [@onymchat](https://t.me/onymchat)  ·  Twitter [@onymchat](https://x.com/onymchat)

<br>

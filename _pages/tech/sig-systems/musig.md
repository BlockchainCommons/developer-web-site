---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Future Development: MuSig 2"
hide_description: true
classes:
  - wide
permalink: /musig/
sidebar:
  nav: musig
---

## Overview

<a href="/crypto-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-crypto-musig.png" style="margin-left: 20px; float: right" width="25%"></a>

MuSig2 is a two-round Schnorr-based multi-signature (multisig) scheme that combines efficiency with security for Bitcoin and other applications. It allows multiple participants to aggregate their public keys into a single group key and produce a joint signature that is indistinguishable from an individual signature. This compact format reduces on-chain data requirements and conceals participant identities, avoiding additional rounds of communication required in earlier versions.

Initially introduced in 2018, the original MuSig protocol required three rounds to securely address rogue-key attacks. MuSig2, introduced in 2020, reduced this to two rounds, enhancing real-world practicality. Another variant, MuSig-DN, introduced in 2021, leverages deterministic nonces to further secure nonce generation by mitigating the risk of nonce reuse attacks. MuSig2 has more recently been integrated into the release version of [`libsecp256k1` library](https://github.com/bitcoin-core/secp256k1/), used in both Bitcoin, Ethereum, and many other blockchains and applications.

## Why is MuSig Important?

MuSig is one of two major methods to implement Schnorr-based multi-signatures, alongside [FROST](/frost/), offering efficient, aggregatable signatures. As blockchains increasingly adopt Schnorr signatures, MuSig2’s compactness and simplicity make it a favorable choice for multisig.

Schnorr signatures generally offer:
* Compactness
* Faster Verification
* Signature Aggregation
* Blind Signatures
* Adapter Signatures

### Applications and Use Cases

MuSig2 supports use cases where efficient, compact multisigs are essential, including:

- **Bitcoin Multi-Signature Wallets**: Allows joint control of wallets with reduced on-chain data.
- **Privacy-Focused Payments**: Provides indistinguishable multi-signatures, maintaining participant privacy.
- **Smart Contracts and Layer 2 Protocols**: Enables atomic swaps, payment channels, and cross-chain transactions with improved efficiency.
- **Edge Identifiers and Clique Security**: Facilitates secure, trusted group formation and supports network-level identification, useful for distributed consensus and secure communication.

## How Does MuSig Work?

MuSig2 operates in two rounds:

1. **Commitment Round**: Participants generate a nonce and broadcast a commitment to it, ensuring integrity and preventing rogue key attacks.
2. **Nonce Disclosure and Signature Round**:  Participants disclose their nonces and calculate partial signatures, which are aggregated to form a single Schnorr signature.

This two-round structure reduces interaction overhead compared to MuSig1, making MuSig2 more suitable for real-world applications where communication is costly.

## MuSig2 vs. FROST

MuSig2 focuses on simplicity and compactness but does not support native threshold signatures (e.g., m-of-n). Threshold signing can still be implemented using Taproot trees on Bitcoin (or similar structures elsewhere). While MuSig2 ensures accountability by making each participant’s contribution verifiable, FROST prioritizes privacy by concealing individual contributions within an quorum m-of-n signature.

## Links

* [MuSig Sequence Diagrams](/musig/sequence/)
* [MuSig2: Simple Two-Round Schnorr Multisignatures](https://medium.com/blockstream/musig2-simple-two-round-schnorr-multisignatures-bf9582e99295) (Medium article)
  * [MuSig2: Simple Two-Round Schnorr Multi-Signatures](https://eprint.iacr.org/2020/1261.pdf) (eprint PDF)
* [MuSig-DN: Schnorr Multisignatures with Verifiably Deterministic Nonces](https://medium.com/blockstream/musig-dn-schnorr-multisignatures-with-verifiably-deterministic-nonces-27424b5df9d6) (Medium article)
  * [MuSig-DN: Schnorr Multi-Signatures with Verifiably Deterministic Nonces](https://eprint.iacr.org/2020/1057) (eprint PDF)
* [Docs: secp256k1_musig.h](https://github.com/bitcoin-core/secp256k1/blob/master/include/secp256k1_musig.h) (GitHub)
  * [Examples: secp256ka music.c](https://github.com/bitcoin-core/secp256k1/blob/master/examples/musig.c) (GitHub)

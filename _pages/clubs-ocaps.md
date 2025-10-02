---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Clubs Delegation & Cryptographic ocaps
hide_description: true
classes:
  - wide
permalink: /clubs/ocaps/
sidebar:
  nav: clubs
---

## Traditional & Cryptographic ocaps

Object capabilities (ocaps) are a traditional computer security model. Gordian Clubs use cryptographic ocaps as a new ocaps methodology:

* **Traditional Ocaps**: Software enforces capability delegation.
* **Cryptographic Ocaps**: Mathematics enforces capability delegation.

It's the same principle, but with cryptographic enforcement instead of code. Capabilities now become mathemtical objects, which avoids the confused deputy problem, because math doesn't lie.

Among the capabilities enforced mathematically in Gordian Clubs are:

* Read Capabilities: enforced by permits, including well-understood symmetric key and private key usage as well as SSKR shares.
* Update Capabilities: alternatively enforced by FROST threshold, which allow group decisions.
* Delegation Capabilities: a more bleeding-edge methodology supported by adaptor signatures.

## Delegation

Gordian Clubs use Schnorr signatures as a core technology, with their foundational use being as a threshold signing system that can be used to update Editions. However, Schnorr technologies are like LEGO blocks: different Schnorr options can be stacked together to create towering edifices. One of these additional technologies is the "adaptor signature", which can be combined with other Schnorr technologies to allow delegation, where access can be provided to Gordian Clubs without revealing extant private keys.

> :warning: These delegation protocols are "naive" in the cryptographic sense. Like naive Schnorr aggregation (which could leak keys, leading to MuSig2/MuSig-DN), these examples demonstrate the core pattern but need cryptographic proofs. Schnorr adaptor signatures are a mature cryptographic primitive already deployed in production systems, but their use for capability-based access control with single-holder keys is a novel (but reasonable) application.

### Naive Read Delegation via Adaptor Signature

**Goal:** Alice (with read access) delegates to Bob without sharing keys OR updating current edition
  1. **Alice creates an incomplete signature**
     - Generates random secret `t` and commits to it: `T = t·G`
     - Encrypts symmetric key: `k_enc = k ⊕ hash(t, B, edition_id)`
     - Creates adaptor signature hiding `t` that only Bob can complete
  2. **Only Bob can complete it**
     - Bob uses his private key `b` to complete the signature
     - Completion reveals the secret `t` to Bob (and only Bob)
  3. **Result: Delegated read authority**
     - Bob can decrypt this edition using revealed `t`
     - Computes: `k = k_enc ⊕ hash(t, B, edition_id)`
     - No key sharing required
     - Alice retains her original access

### Naive Write Delegation via Adaptor Signatures

* **Goal:** Alice (with write access) delegates to Bob without sharing keys OR updating current edition
  1. **Alice creates an incomplete signature**
     - Standard Schnorr: `s = r + H(m)·a`
     - Adaptor version: `s' = r + tweak + H(m)·a`
     - The "tweak" is locked to Bob's public key
  2. **Only Bob can complete it**
     - Bob uses his private key `b` to derive the tweak to produces a valid signature from Alice
     - Proves both: Alice's authorization + Bob's identity
  3. **Result: Delegated write authority**
     - Bob can create a new edition
     - Each shows Alice's valid signature
     - No key sharing required
     - Alice retains her original authority

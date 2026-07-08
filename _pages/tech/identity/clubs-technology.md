---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Clubs Technology
hide_description: true
classes:
  - wide
permalink: /clubs/technology/
redirect_from:
  - /club/technology/
sidebar:
  nav: clubs
---

Gordian Clubs are built atop the [Gordian stack](/). Following is a listing of the major building blocks.

## Data Encoding & Storage

* [**🔟 dCBOR (Deterministic CBOR)**](/dcbor/)
  * Binary encoding which ensures that identical semantics always produces identical bytes
* [**🔗 Uniform Resources**](/ur/)
   * Text encoding for binary dCBOR data.
   * Uses [**💬 Bytewords**](/bytewords/).
* [**📦 Gordian Envelope (Smart Document Architecture)**](/envelope/)
  * Subject-predicate-object semantic structure.
  * Selective disclosure through *elision* or *encryption*
    * Reveal only what's needed
  * Radically recursive: everything is an Envelope.

## Identity, Verification & Authorization

* [**🅧 XIDs (eXtensible IDentifiers)**](/xid/)
  * 32-byte cryptographic identifiers derived from keys (🅧 7e1e25d7...)
  * Resolve to XID Documents with communication keys, endpoints, permissions, etc.
  * Support key rotation while maintaining stable identity
* [**🎫 Permits (Multiple Ways to Access Same Content)**](/envelope/features/#encryption-support)
  * Each permit type decrypts to the same base symmetric key
  * XID permits, public-key permits, SSKR threshold shares, password permits
* [**🅟 Provenance Marks (Cryptographic Edition Ordering)**](/provemark/)
  * Sequential, tamper-evident chains of document versions
  * Permit Editions within a Club
  * Enable write access validation and update authorization
  * Human-readable names using [**💬 Bytewords**](/bytewords/) (🅟 KNOB BETA AQUA NOON) for easy verification

## Communication

* [**🔒 GSTP: Secure Multi-Party Coordination**](/envelope/gstp/)
  * Transport-agnostic protocol (works over Bluetooth, QR, NFC)
  * Encrypted message exchange with state preservation
  * Enables secure coordination without infrastructure
  * Can support multiparty signing of new Editions
  * Can release Edition updates (but this infrastructure is *not* required)

## Schnorr

* [**🤝 FROST (Flexible Round-Optimized Schnorr Threshold)**](/frost/)
  * M-of-N threshold signatures indistinguishable from single signatures for **online** group signing
  * Privacy-preserving: Can't tell which specific members participated
  * Enables group decision-making without revealing individual votes
  * Support release of updated Editions
* **🖋️ Adapter Signatures**
  * Conditions embedded in signatures
  * Allows for delegation of read and write access
  * See ["Clubs Delegation and Cryptographic ocaps"](https://developer.blockchaincommons.com/clubs/ocaps/) for more.
 
## Cryptographic Maturity Levels

The cryptography within Gordian Clubs is at variable levels of maturity. Audits are needed for some systems, while others are in a research phase that might enable future capabilities.

**Production Ready**
* [SSKR](/sskr/) for permits (formal security proofs, audited implementations)
* [FROST](/frost/) and [MuSig2](/musig/) for threshold signatures (formal security proofs, audited implementations)
* ChaCha20-Poly1305 as Gordian Envelope's core cryptography (proven implementations)

**Requires Formal Security Audit**
* Gordian Envelope's unique elision and permit primitives
* Schnorr adaptor signatures for single-key capabilities (proven primitive, novel application to access control)
* Integration of adaptor signatures with Gordian Clubs for read/write delegation
* Combinations of adaptor signatures with FROST/MuSig2

**Research Phase**
* Threshold-based capability delegation (plausible but unproven extensions)
* Key agreement protocols leveraging FROST/MuSig2 shared material
* Scriptless scripts for complex authorization logic

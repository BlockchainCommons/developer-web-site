---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Meeting: Post Quantum Cryptography (2025) Rough Summary"
hide_description: true
classes:
  - wide
permalink: /meetings/2025-03-pqc/summary/
redirect_from: /meeting/2025-pqc/summary/
sidebar:
  nav: architecture
---

The following is a _rough_ summary of the **March 5, 2025** Gordian Developers Meeting, focused on post-quantum cryptography.

## Quantum Computing

Takes advantage of superposition & entanglement
* Can do some computing tasks quicker
* Discussion of RSA being broken as far back as 1994

Not _everything_ is vulnerable, some cryptography just has its security level reduced

Quantum computers really exist,
* But Willow (Google) just has a few logical qubits
* And they're just laboratory experiments
* Crypto attacks still likely 5-10 years off
* Nonetheless We need PQC to protect the future

## Foundation & Quantum Link

QuantumLink is available in Passport Prime
* Airgapped Bitcoin wallets
* Personal Security Assistant
* with QuantumLink as Post Quantum Cryptography (PQC) communication

### What's the Problem?

Scanning QRs is main communication for airgapping and it can have user disadvantages
* Might need 100s of QR codes for large transactions!
* So MicroSD is the alternative, but that's tough too!
* Doesn't support up-to-date blockchain info
* Anti-Exfil would require more back and forth

Need more than QR+MicroSD!

### QuantumLink is the Solution

Creates a secure local wireless communication method
- creating a better user experience

Uses out-of-band key exchange
- Create an encrypted tunnel
- everything is encrypted & signed

Works over Bluetooth, NFC, other transports

### How Does It Work?

Key exchange: between Passport (hardware wallet and Envoy (mobile app) using QR
- Then Envoy returns using Bluetooth

Now have active secure tunnel
- every message has an encryption key
- which is encrypted with recipient's public key

(Main MCU & Bluetooth MUCH are separated! Connected by SPI bus. All data between them is encrypted. so Bluetooth chip can't intercept data!)

Advantages:

The Out-of-band key exchange secures communication
- All messages are encrypted + signed
- Bandwidth allows firmware updates!
- Can keep Prime up-to-date with blockchain info

### What is Quantum Resistance?

Quantum computing vulnerability affects most classic crypto (RSA, ECDSA, Diffie-Huelman)

Quantum Resistance is crypto that isn't vulnerable to quantum computers!
- lattice-based crypto
- hash-based signatures
- code-based crypto
- multivariate crypto

Lattice-based crypto is what Quantum uses
* ML-KEM used to encrypt a symmetric encryption key (ChaChaPoly, etc)
* ML-DSA for digital signatures

### Quantum Link Under the Hood

* Passport shows static QR code with its Bluetooth device address
* Phone scans this and opens Envoy app
* Passport sees the connection and switches to an Animated QR code
   * This is a UR code with a XID document and public keys for ML-KEM and ML-DSA
   * Plus additional metadata
* Envoy saves data & builds response, which it sends over Bluetooth
   * Because it can now encrypt & sign
* This creates encrypted tunnel
* Messages use Gordian Sealed Transport Protocol (GSTP)
   * Request/Response using Envelopes
   * expanded with Post-Quantum Computing

For more on these protocols, see [Envelope](/envelope/), [GSTP](/envelope/gstp/), and the [Understanding Envelope video](https://www.youtube.com/watch?v=-vcLCFKQvik).

* Payload in messages is encrypted with ChaCha20Poly1305 Key
   * ChaCha key encrypted with ML-KEM private key
   * The public keys are looked up using XIDs
   * Allowing connection with multiple devices!

QunatumLink payloads synchronize onboarding, sign Bitcoin transactions, update price & date, install firmware updates, and install new Passport Prime apps

### Key Storage

Passport Prime stores keys with AES-256 encryption in secure element

Envoy stores keys with iOS Key Manager or Android Keystore

Currently, there's a single public key exchange at time of initial pairing, but no formal key rotation
- User could perform pairing again
- Might have auto key rotation in future

_Everything is open source! Blockchain Commons protocols are BSD-2-Clause-Plus, Passport Prime apps are GPL3+ or Affero GPL3+. QUantum Link will be documented! And third parties will be able to add messages._

For more on Passport Prime, see [Foundation Devices](https://foundation.xyz/passport-prime).

## PQC at Blockchain Commons

Why is Blockchain Commons advacing work on Post-Quantum Cryptography (PQC)?

Symmetric encryption using ChaCha20-Poly1305 is secure under Quantum Computing, but weakened.

Public-key crypto is totally broken!
- RSA, ECC, DH
- That is, if you have a sophisticated Quantum Computer (which doesn't exist yet)

So symmetric crypto is OK with larger keys, but public-key crypto must be replaced
- That's where ML-KEM & ML-DSA come in

ML-DSA
- Module Lattice Digital Signature Algorithm
- Non-deterministic
- Not linearly composable (no PQC DSA is!)

ML-KEM
- Module Lattice Key Encapsulation Mechanism

Blockchain Commons abstracts crypto use
- Not crypto-agile
- But crypto-agnostic

No SLH-DSA Yet
- Hash-based instead of lattice-based signatures
- Very large & somewhat redundant
- Would be easy to add due to abstraction

Because of challenges with large, slow quantum signatures
- Blockchain Commons uses a two-step process
- PQC for key exchange & rotation
- Classic crypto for ongoing usage

![](/assets/images/pqc-size-1.jpg)

![](/assets/images/pqc-size-2.jpg)

Available in bc-components in Blockchain Commons stack
- has primitives used by higher level things
- such as keypairs & encryption!

Signature schemes abstracted! SSH, ML-DSA, ed25519, ecdsa, and schnorr!
- Similarly encryption is abstracted: X25519 or ML-KEM
- Just change part of a line of code to change crypto scheme used!

## ZeWIF Project

Blockchain Commons has long been supported by Bitcoin community, but wants to protect *everyone* doing self-sovereign digital wallets. We want everyone to use our "layer 0" specifications to help protect their wallet holders!

That includes Zcash, who came to Blockchain Commons to talk about how they were deprecating their software wallet, zcashd
- Allowing Blockchain Commons to create interchange format

This is important because
- we don't want people to be locked into a single wallet

We want to encourage
- Cooperation

We want users to have
- Freedom
- Not to lose their funds

Gordian Principles
- [Openness, independence, resilience](https://developer.blockchaincommons.com/principles/)

We want to support COMPATIBILITY and USER CHOICE
- on Bitcoin & elsewhere!

Uses [Envelope](/envelope/), just like QuantumLink

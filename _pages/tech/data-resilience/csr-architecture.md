---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "CSR Architectural Overview"
hide_description: true
classes:
  - wide
permalink: /csr/architecture/
sidebar:
  nav: csr
---

## Overview

_CSR allows for the recovery of seeds and other secrets by dividing
responsibility for recovery up between multiple devices ..._

## Key Principles

The key principles of CSR are:

1. To allow for the resilient and secure recovery of data.
1. To place rational limits on what can be stored.
1. To give users the choice of how to divide their shares and who to trust them with.
1. To allow recovery from a variety of sources.
1. To support a variety of methods for recovery.
   * This by default includes recovery from online services.
   * This could include self-sovereign recovery (online or offline).
   * This could include social key recovery (friends/family/colleagues or trusted services).
   * This could include variations in-between.
1. To support a variety of methods for authenticating recovery.
   * Authentication should be appropriate and diverse.
   * Authentication should leverage existing authentication methods and processes.
   * Authentication can include physical possession.
   * Authentication can include other non-digital processes.
1. To make the security of the reconstruction a core goal.
1. To do all of this in a simple way that can hide complex details from the user if they don't choose to engage with them.
1. To do all of this in a standardardized way that allows for shared infrastructure.

## What CSR Will Do

### CSR Secret Storage

CSR will:

* Support a default self-sovereign scenario where the user is set up with robust, self-sovereign storage of their seed (and other secrets) using 2-of-3 SSKR.
* Automate this baseline storage case without user intervention.

### CSR Secret Updates

CSR will:

* Allow a user to personally choose key-recovery services for storing their shares in more advanced storage cases.
* Allow a user to increase the size of their SSKR, to 3-of-5 or 4-of-9 or 2-of-3 of 2-of-3.
   * This will be done in a progressive way that does not require revisiting current key-recovery services.
* Properly rotate secrets when an older, smaller set of SSKR shares is deprecated.
* Automatically rotate secrets and rebuild shares if a key-recovery service is compromised or disappears.

### CSR Secret Recovery

CSR will:

* Automate recovery in the baseline storage cases, with information of additional shares stored with the foundational share.
* Ensure the security of seeds (and other secrets) as they are recovered.
* Require authentication for the recovery of shares.
* Support a variety of authentication options.
   * Federated Login
   * In-Person Verification
   * Password
   * Phone Call
   * Physical Possession
   * Biometric
* Enable progressive revelation of recovery location.
   * In baseline scenario, first recovery location reveals all other recovery locations.
   * In more advanced scenarios, each recovery location may breadcrumb to next recovery location.
* Progressively improve security for more complex scenarios by requiring a variety of authentications to restore various shares.

### What the Key Choice Points Are

CSR is designed such that a user has to make _no_ decisions in the baseline case. Instead, the process is fully automated. All the necessary information is stored on the user's platform cloud for contacting and authenticating the return of additional shares.

Choice points then occur when a user decides to expand or modify their usage of the CSR system, either on their own or at the prompting of the system. These choice points include:

* User adds offline or social-recovery to their system.
* User chooses third parties who holds shares.
* User updates secrets.
* Users rotates up amount of shares, from 2-of-3 to 3-of-5 or 4-of-9 or 2-of-3 of 2-of-3.
* User revokes shares.

## CSR Technical Underpinnings

CSR is built on the following technical underpinnings:

**SSKR.** A secret-sharing scheme that currently focuses on Shamir's Secret Sharing but is expected to expand to also support VSS when it's sufficiently mature. It is used by CSR to shard secrets. The baseline scenario uses 2-of-3 SSKR, but more advanced scenarios will support 3-of-5, 4-of-9, and two-layer 2-of-3 of 2-of-3. The architecture will be designed to allow progressive expansion of sharding scenarios. See [SSKR](/sskr/).

**Gordian Envelope.** Smart Document system. An Envelope consists of two parts: the payload data, which typically consists of one or more encrypted objects (originally focused on seeds, but possibly also containing web tokens and/or other secret digital data); and the permit, which decrypts the payload data if the proper key or other data is applied and which may include hints about how to enable the decryption. See  [Gordian Envelope](/envelope/).

**SSKR Permits.** A Gordian Envelope permit that allows for the opening of an envelope by combining the SSKR shares found in multiple envelopes, each of which contains the encrypted data and one of the SSKR shares. The SSKR shares are used to reconstruct a symmetric secret that was used to lock the crypto-envelope.

**ChaChaPoly.** The ChaCha20-Poly1305 cipher. An encryption methodology, and the standard suggested for the first generation of crypto-envelope encryption. See [RFC 8439](https://datatracker.ietf.org/doc/html/rfc8439).

**Gordian Sealed Transaction Protocol (GSTP).** A communication methodology that is secure, transit-agnostic, and distributed. It was designed to allow for a digital-asset wallet to communicate with a share server, for the storage or recovery of shares. See [GSTP](/envelope/gstp/).

### CSR Technical Process

The overall technical process looks like this:

1. Seed (or other secret data) is revealed from where it's stored.
2. Metadata is collected including recovery metadata (such as Bitcoin descriptors or Lightning payment channels) or other metadata (such as seed creation data).
3. A payload is created by encrypting the secret data and metadata using ChaChaPoly with a unique, random, symmetric key.
4. The symmetric key is sharded using SSKR.
5. Three crypto-envelopes are constructed, each containing the encrypted payload plus one share of the symemtric key.
6. A second payload is added to the first crypto-envelope, containing hints about where the other two envelopes will be stored. This is not encrypted, but will be protected by the authentication implicit in the first envelope's storage.
7. Optionally, other metadata is added to that second, unencrypted payload of the first envelope, or even to other envelopes.
8. A second unique, random symmetric key is created and sharded into five 3-of-5 shares. The original payload data is reencrypted with the second key. Both a unique 3-of-5 share and the second encrypted copy of the payload are also placed in the three envelopes.
9. First envelope is placed in Platform Cloud.
10. Second and third envelope are placed at locales specified by the first envelope, likely share servers, with communication managed by GSTP.
11. Two more envelopes are created using the fourth and fifth copies of the 3-of-5 share and the second copy of the payload. They will be distributed if the user decides to upgrade from 2-of-3 to 3-of-5 sharding.

Afterward, the envelopes look as follows:

* **Envelope #1** (Platform Cloud)
   * PERMIT: Share #1 (Key #1: 2 of 3)
   * PERMIT: Share #1 (KEY #2: 3 of 5)
   * ENCRYPTED PAYLOAD (with KEY #1)
   * ENCRYPTED PAYLOAD (with KEY #2)
   * UNCRYPTED PAYLOAD (locale hints)
* **Envelope #2** (Service)
   * PERMIT: Share #2 (Key #1: 2 of 3)
   * PERMIT: Share #2 (KEY #2: 3 of 5)
   * ENCRYPTED PAYLOAD (with KEY #1)
   * ENCRYPTED PAYLOAD (with KEY #2)
* **Envelope #3** (Service)
   * PERMIT: Share #3 (Key #1: 2 of 3)
   * PERMIT: Share #3 (KEY #2: 3 of 5)
   * ENCRYPTED PAYLOAD (with KEY #1)
   * ENCRYPTED PAYLOAD (with KEY #2)
* **Envelope #4** (Unused)
   * PERMIT: Share #4 (KEY #2: 3 of 5)
   * ENCRYPTED PAYLOAD (with KEY #2)
* **Envelope #5** (Unused)
   * PERMIT: Share #5 (KEY #2: 3 of 5)
   * ENCRYPTED PAYLOAD (with KEY #2)

Assuming the destruction or loss (but not compromise) of the device holding the main seed, the user can then recover as follows:

1. User restores access to Platform Cloud on a new device.
1. CSR authenticates with Platform Cloud.
2. CSR retrieves first envelope from Platform Cloud.
3. CSR examines unencrypted payload to see where other envelopes are.
4. CSR authenticates with second server using GSTP and retrieves second envelope.
5. If there is a problem, CSR authenticates with third server using GSTP and retrieves third envelope.
6. CSR combines SSKR shares from two envelopes.
7. CSR unlocks symmetric key.
8. CSR uses symmetric key to unlock first payload in either envelope (they should be identical), which is the secret.

## What CSR Not Do In Its Initial Deployment

This is just the first iteration of the CSR system, let alone the larger, more complex [Collective Key Management (CKM)](/ckm/) system. It needs to be carefully constrained to ensure the ability to release in a reasonable mount of time.

It does *not*:
* Support collaborative key generation.
* Support collaborative key usage.
* Support VSS.
* Protect your key before it's split.
* Protect your key once it's reconstructed.
* Support the usage of multisigs.
* Support Envelope Permits other than SSKR.
* Support Envelope Encryption other than ChaChaPoly.
* Cryptographically verify existence of shares.

## Appendix I: Defining CSR

The name "Collaborative Seed Recovery" was carefully selected for this project:

* **Collaborative**
   * We have not used the language of "social" seed recovery because the recovery could be entirely self-sovereign or the parties supporting the recovery might be entities outside of the holder's social network, including businesses.
   * The collaboration remains trustless because no individual can be a Single Point of Failure (SPOF) or Single Point of Compromise (SPOC). They are nonetheless parties that a key holder has confidence in.
* **Seed**
   * The CSR architecture is designed & focused on the recovery of secrets, in particular SEEDS, not KEYS (though keys may also be recovered along with metadata).
   * This is to support a larger ecosystem, to include scenarios beyond cryptocurrencies such as Bitcoin and Ethereum seeds. It also provides resilience in the reconstruction of seeds for any app that uses persistent keys, such as U2F or Signal, and is future proofed to support reconstruction of seeds that can generate future curves or Zero-knowledge proofs, such as Chia's unique keys, BBS+ keys, and much more.
* **Recovery**
   * The goal is the resilience and reconstruction of a seed that the holder has lost, along with metadata, which then allows them to restore wallet functionality. It is NOT intended as a mechanism for the recovery of a seed or key that is compromised. Instead, the architecture is designed as the foundation of future Collaborative Key Management multi-party cryptography techniques and features, which also allow for no single points of compromise.

## Appendix II: Detailing SSKR, Gordian Envelope, and GSTP

**SSKR** is fundamental to CSR, as it allows for the sharding of secrets that are then given out in the CSR process. For more on SSKR, see the [SSKR Docs](/sskr/) and especially the [SSKR Lexicon](/sskr/lexicon/), which has terms of use for describing CSR sharding and shares.

**Envelope** is the mechanism by which SSKR shares are encoded in CSR. It's required primarily for its ability to shard payloads larger than SSKR can handle. For more on Envelope, see the [Envelope Docs](/envelope/).

**GSTP** is the mechanism by with SSKR shares are distributed to remote servers and recovered from them. For more on GSTP, see the [GSTP Docs](/envelope/gstp/).

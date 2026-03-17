---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Reference Apps
hide_description: true
classes:
  - wide
permalink: /architecture/apps/
sidebar:
  nav: architecture
---

The Gordian Reference Apps demonstrate the usage of Blockchain
Commons' [Reference Libraries](/libraries/) to fulfill the [Gordian
Principles](/principles/). They may also be combined into an ecosystem
that demonstrates the Gordian Marco-Architecture and its theory of
functional partition:

![](/assets/images/appmap-black.png)

The Gordian Reference Apps are all linked by airgaps and torgaps, to
demonstrate how privacy and security can be maximized while still
maintaining a very usable ecosystem. However, transport specifics
become less important when using the [Gordian
Architecture](/architecture/) as a whole, and so in a real-life
ecosystem, apps may be normally Networked.

Note that some of these apps are deprecated: they still serve their
purpose as references for specific architectural roles, but are not
updated and supported. The most updated apps are listed on the [main
architectural page](/architecture/), where this listing is more
complete.

_See [Gordian Architecture Roles](/architecture/roles/) for
definitions of roles that apps can taken within this architecture._

## Networked Services

_Gordian Reference Apps can be fully networked if services are correctly partitioned:_

**[Fully Noded Server](https://fullynoded.app/)
**Roles:** Cosigner, Network Server, Seed Generator, Seed Vault (partial)

Fonta1n3's Fully Noded Server demonstrates how to separate the functionality of a server from its connected Fully Noded Wallet. It's available as a Bitcoin server for MacOS machines. (This grew out of Fonta1n3's work on the Gordian Server, which we've deprecated because his work on Fully Noded is more up-to-date.)

## Airgapped Apps

_Closely held devices allow users to protect their confidential data
with Airgaps. Gordian Seed Tool is our prime Reference App for
demonstrating this functionality, while LetheKit offers another style
of secure seed generation:_

**[Gordian Seed Tool](https://github.com/BlockchainCommons/GordianSeedTool-iOS)**<br>
**Roles:** Cosigner, Seed Generator, Seed Vault

A cryptographic seed manager for your iOS device. Allows the
maintenance of seeds in a closely held device and the easy use of
those seeds through [Envelope Requests](/envelope/request/) for either specific keys or for
the signature of PSBTs. This is our most developed app, and the one
that best displays a variety of ways to achieve the Gordian
Principles.

**[LetheKit](https://github.com/BlockchainCommons/bc-lethekit)**<br>
**Roles:** Seed Generator

A do-it-yourself hardware kit that can be used to generate secure
seeds for Bitcoin. It shares functionality with Seed Tool, but
functions as fully airgapped hardware rather than software on a
closely held but potentially connected device. It's also more tightly
partitioned, focusing exclusively on seed generation.

**[Gordian Cosigner](https://github.com/BlockchainCommons/GordianSigner-Catalyst).** (DEPRECATED) <br>
**Roles:** Cosigner, Seed Vault

An offline PSBT and multi-sig signer. You can import keys and read
PSBTs via QR code. After you sign the PSBT, you can then transfer it
back to your Wallet across an airgap, which will coordinate the
broadcast of the transaction. The current iteration supports Seed
Generation, but the plan was to remove it to better partition the
services. This app has never advanced to a full release, in large part
because its functionality, and thus its reference examples, have been
superceded by Seed Tool.

**[Gordian QRTool](https://github.com/BlockchainCommons/GordianQRTool-iOS)** (DEPRECATED) <br>
**Roles:** Seed Vault

A more general data storage tool that displays how the Gordian
Principles can apply to the protection of a variety of digital data
that has been encoded as QRs. It also shows the tightest partitioning,
allowing secure seed storage and nothing else. This is also a fully
released app.


**[Gordian Wallet](https://github.com/BlockchainCommons/GordianWallet-iOS)** (DEPRECATED) <br>
**Roles:** Cosigner, Policy Coordinator, Seed Vault (partial), Transaction Coordinator

Gordian Wallet is a mobile Bitcoin wallet that supports sophisticated
[#SmartCustody](https://www.smartcustody.com/) features such as
multi-sigs and PSBTs. It acts as: a policy coordinator, determining
how you set up your accounts; a transaction coordinator, creating
transactions based on your policies; and a broadcast coordinator,
determining how to send our your transactions. Note that the Seed role
of Gordian Wallet is limited because its multisig design shares Seeds
with the linked [Gordian
Server](https://github.com/BlockchainCommons/GordianServer-macOS).

## Microservices

_Blockchain Commons microservices demonstrate how services can be
built for very specific functionality within the Gordian
Architecture._

**[SpotBit](https://github.com/BlockchainCommons/spotbit)** <br>
**Roles:** Pricing Calculator

A price-info microservice, used by Gordian Wallet (and potentially
other paritioned services) through a _torgap_. Spotbit can be used to
aggregate Bitcoin pricing information from a variety of exchanges and
to store that data.

## CLI Apps

_Many of our CLI apps were built to exercise our [Gordian Reference
Libraries](https://github.com/BlockchainCommons/crypto-commons) but
they also serve as references for the Gordian Principles, the
UNIX/command-line equivalent of some of our mobile apps and network
services._

* **[Bytewords](https://github.com/BlockchainCommons/bc-bytewords-cli) \(CLI\).** A tool for testing bytewords.
* **[dCBOR](https://github.com/BlockchainCommons/dcbor-cli) \(CLI\).** A tool for parsing & validating deterministic CBOR.
* **[Envelope](https://github.com/BlockchainCommons/envelope-cli-swift) \(CLI\).** A tool for creating and parsing [Gordian Envelopes](/envelope/).
* **[Keytool](https://github.com/BlockchainCommons/bc-keytool-cli) \(CLI\).** A tool for deriving keys and addresses from seeds. 
* **[LifeHashTool](https://github.com/BlockchainCommons/LifeHashTool) \(Swift CLI\).** A tool for generating Lifehash PNGs from the command line.
* **[Seedtool](https://github.com/BlockchainCommons/bc-seedtool-cli) \(CLI\).** A tool for generating seeds from a variety of random inputs and for translating seeds among formats like BIP39, [SSKR](/sskr/), hex, and [Bytewords](/bytewords/).
* **[URDemo](https://github.com/BlockchainCommons/URDemo) \(iOS Demo\).** A demonstration of the [URKit](https://github.com/BlockchainCommons/URKit) that can be compiled and run in Xcode using Swift. It demonstrates multi-part animated QRs.

### Research Apps

_Additional CLI Apps were written for research purposes and are not as complete or polished._

* **[Mori](https://github.com/BlockchainCommons/mori-cli) \(CLI\).** A timelock tool to make sure your assets aren't lost.
* **[Musign](https://github.com/BlockchainCommons/musign-cli) \(CLI\).** A research project for signing and verifying messages with ECDSA and Schnorr.
* **[Sweeptool](https://github.com/BlockchainCommons/sweeptool-cli) \(Rust CLI\).**  A tool for moving funds out of a descriptor wallet.

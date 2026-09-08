---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Architecture Roles
hide_description: true
classes:
  - wide
permalink: /architecture/roles/
sidebar:
  nav: architecture
---

_A variety of roles can be funtionally partitioned within the Gordian
Architecture. What follows is a bare listing of some of them. Also see
[Gordian Architecture Lexicon](/architecture/lexicon/) and [Gordian
Architecture Apps](/architure/apps).._

***Cosigner (aka Signing Device).*** Accepts partially signed or
unsigned transactions (typically PSBTs on the Bitcoin network) and
signs them, either as part of a single-sig or multisig
transaction. This typically means that the _Cosigner_ is also a _Seed
Vault_, but this is not required: seeds could be hand-entered or
scanned from another source whenever a signature is requested. This
role was first demonstrated at Blockchain Commons in [Gordian
Cosigner](https://github.com/BlockchainCommons/GordianCosigner-iOS)
and is now part of the more fully feature [Gordian Seed
Tool](https://github.com/BlockchainCommons/GordianSeedTool-iOS). Second-generation
wallets, which are airgapped, such as the [Foundation Devices
Passport](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Case-Study-Passport.md)
and the Keystone Pro also support the functionality.

***Fee Calculator.*** Determines appropriate fees for uses of a
blockchain network. There is no Blockchain Commons reference for fee
calculation services. Sources such as `bitcoinfees.earn.com` and
`mempool.space` provide the service, which is also often supported by
a _Network Server_.

***Network Server.*** Talks directly to a cryptocurrency network,
accepting inputs of blocks and enabling transmission of new
transactions. Typically a full node. De facto, also a _Broadcast
Coordinator_, and so that role is not separated out. At Blockchain
Commons, this was originally [Gordian
Wallet](https://github.com/BlockchainCommons/GordianWallet-iOS), but
[Gordian
Coordinator](https://github.com/BlockchainCommons/iOS-GordianCoordinator)
is the second-generation server. In a self-sovereign scenario, a
Bitcoin Core or Electrum server could also fulfill the role.

***Policy Coordinator.*** Manages the creation of
multisigs. Blockchain Commons' [Gordian
Wallet](https://github.com/BlockchainCommons/GordianWallet-iOS) was an
early example, but at this point we lean toward
[Sparrow](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Case-Study-Sparrow.md),
or again [Gordian
Coordinator](https://github.com/BlockchainCommons/iOS-GordianCoordinator)
as a more fully featured piece of software.

***Pricing Calculator.*** Determines current values of
cryptocurrencies, usually in fiat currencies. Blockchain offers the
[Spotbit](https://github.com/BlockchainCommons/spotbit) microservice
as a reference app for this role.

***Seed Generator.*** Creates new seeds. This typically means that the
_Seed Generator_ is also a _Seed Vault_, but this is not required: a
highly partitioned architecture could separate out the functions for
increased security. The prime examples that Blockchain Commons offers
for partitioned seed generation are
[LetheKit](https://github.com/BlockchainCommons/lethekit) and [Gordian
Seed
Tool](https://github.com/BlockchainCommons/GordianSeedTool-iOS). Most
hardware and software wallets also support seed generation, but in a
non-partitioned way.

***Seed Vault.*** Stores seeds securely. [Gordian Seed
Tool](https://github.com/BlockchainCommons/GordianSeedTool-iOS) is
Blockchain Commons' reference for seed storage. Hardware wallets, both
first-generation connected wallets such as Ledger and Trezor and
second-generation airgapped wallets such as Keystone and
[Passport](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Case-Study-Passport.md),
also provide the service.

***Share Server.*** Stores one share of a sharded secret
securely. This is a core element of [CSR](/csr/).

***Transaction Coordinator.*** Creates transactions. Though Blockchain
Commons' [Gordian
Wallet](https://github.com/BlockchainCommons/GordianWallet-iOS)
creates transactions, its role as a transaction coordinator is a bit
dated. Instead, we suggest the [Sparrow
wallet](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Case-Study-Sparrow.md)
or our [Gordian
Coordinator](https://github.com/BlockchainCommons/iOS-GordianCoordinator)
as a released software wallet that can act as a transaction
coordinator, interacting with a variety of Cosigners and Seed
Vaults. (The difference between a "software wallet" and a pure
"transaction coordinator", is that a "software wallet" can also hold
keys, though [proper partitioning](/architecture/) moves keys to other devices.)


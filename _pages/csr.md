---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "CSR: Collaborative Seed Recovery"
hide_description: true
classes:
  - wide
permalink: /csr/
sidebar:
  nav: csr
redirect_from:
  - /csr/overview/
---

## Overview

<a href="/core-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-core-csr.png" style="float: right; margin-left: 20px;" width="25%"></a>

CSR allows for the recovery of seeds and other secrets by dividing
responsibility for recovery up between multiple devices, some (but not
all) of which will be necessary for recovery. Its baseline recovery
mechanism uses self-sovereign recovery (controlled entirely by the
user), while more advanced scenarios allow for social key recovery
(supported by friends or family). Backup is meant to be largely
automated, especially in the baseline scenario, while recovery may
require some user intervention.

One of the advantages of CSR over traditional social key recovery is
that you don't have to choose friends or family that you trust. Though
you can do so in advanced scenarios, you can also entrust fragments of
keys to companies running share servers. You don't have to worry about
them stealing keys, because you're only giving them individual
shares, but you can trust that they'll likely still be around when
you need to reconstruct your key.

CSR can be operated in different ways by different servers, provided that they all agree on a format for communication, which is Blockchain Commons [Gordian Sealed Transaction Protocol (GSTP)](/envelope/gstp/).

## Why is CSR Important?

The most dangerous [adversary](https://www.smartcustody.com/) facing self-sovereign digital-asset holders is not theft but loss. Losing a seed typically means the lost of all the funds locked with its keys, and history has already proven that this is a very real threat.

CSR is built to address this threat by increasing the [Resilience](/principles/) of seeds. It does so by using Shamir's Secret Sharing, which shards a seed, and then only requires some of those shares to reconstruct the seed. That means that one of more shares can usually be lost (depending on [how they're constructed](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/SSKR-Sharing.md), and the seed is not, which is a huge improvement over the all-or-nothing approach when you're just holding a seed.

Other similar approaches have appeared, most notably Ledger Recover, but they undercut the very self-sovereign nature of self-sovereign holding of digital assets by taking choices away from users. CSR instead ensures that **You Decide.** You choose who holds your shares, whether they be corporations with strict KYC requirements (as with Ledger Recover), companies with more privacy-preserving principles, other users who you swap shares with, or your own self-sovereign storage of shares on paper, steel, or something else.

## How Does CSR Work?

CSR is a two-part process. Sharding of a seed allows storage, then reconstruction of a seed allows recovery.

Storage occurs in a wallet of seed storage app. When a user
chooses to backup a seed, it will automatically shard the seed and
send those shares off to selected Share Servers. When a user wishes to assert
more agency, they can choose where those shares are stored, or even
take them offline.


Recovery requires the user to authenticate using
different, predetermined methods with each Share Server. They can
include everything from passwords and email responses to personal
identification. Each authentication retrieves a share, and when
sufficient shares have been retrieved, the seed is reconstructed.

CSR is an apex applications that uses a variety of Blockchain Commons specifications.

* [**SSKR.**](/sskr/) Seeds are sharded with SSKR, which is a seed-sharding library that currently supports an expanded version of Shamir's Secret Sharing that allows for grouping of shares.
* [**Envelope.**](/envelope/) SSKR shares of seeds are stored in Envelopes, which is a Smart Document format that allows for the storage of both the seed and related metadata and later allows the elision of some of that data if preferred by the holder.
* [**GSTP.**](/envelope/gstp/) The Gordian Sealed Transaction Protocol uses Envelopes to allow secure, distributed, transport-agnostic communication between a user and a share holder. This could be an online server, but alternatively an offline storage mechanism such as a Java Card.
* [**Gordian Depository.**](https://github.com/BlockchainCommons/bc-depo-rust) The Gordian Depository is Blockchain Commons' own Share Server, which uses the above specifications and a [specific API](https://github.com/BlockchainCommons/bc-depo-api-rust).
 
The following example depicts the recovery of a seed complete with
note and name, something possible thanks to the use of Envelope.

![](/assets/images/csr/example.jpg)

_See the [CSR Architectural Overview](csr-architecture.md) for more
information._

### What are the Phases of CSR Deployment?

There are six broad phases to CSR deployment.

**Phase 1:** Release of [SSKR](/sskr/) to allow sharding of seeds and [Envelope](/envelope/) to support encoding of metadata. [☑️]

**Phase 2:** [Gordian Depository](https://github.com/BlockchainCommons/bc-depo-rust) as a reference Share Server along with [Gordian Depository API](https://github.com/BlockchainCommons/bc-depo-api-rust). [☑️]

**Phase 3:** Release of Gordian Companion as a reference app for sharding a seed and communicating with Gordian Depositories. [ ]

**Phase 4:** Work with third parties to support the creation of additional Share Servers in order to create an ecosystem truly supporting user choice, where a user of an application can meaningfully choose where to send the shares of his seed. [ ]

**Phase 5:** Expansion of SSKR to support VSS in the Trusted Dealer Key Generation mode, so that a user app can regularly test for the existence of seed shares without creating a danger of compromise by actually reconstructing the seed. Currently it seems most likely this will be accomplished with the recently audited [ZF FROST libraries](https://frost.zfnd.org/index.html). [ ]

**Phase 6:** Leverage work with VSS and FROST into a full [CKM](/ckm/) deployment. [ ]

## Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>CSR Kickoff Meeting:</b>

{% include video id="FVzTRsqcyMU" provider="youtube" %}

    </td>
    <td width="640px">
      <b>CSR Example API:</b>

{% include video id="mpCEGUiCwFg" provider="youtube" %}

    </td>
    <td width="640px">
      <b>CSR Demo:</b>
{% include video id="9fyICk0lwL0?start=3255" provider="youtube" %}
    </td>

  </tr>
</table>

_See the [Gordian Developer Meetings: CSR &
Envelope](https://www.youtube.com/playlist?list=PLCkrqxOY1Fbp-P1Yv-7gmu75i2QS2Z6vk)
playlist for even more videos on the development of CSR and Gordian
Envelope._

## Libraries

{% include lib-envelope.md %}

## Links

**Intro:**

* [Architectural Overview](/csr/architecture/)

**Components Intro:**

* [Envelope](/envelope/)
   * [Attachments](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-006-envelope-attachment.md)
* [SSKR](/sskr/)
* [Gordian Sealed Transaction Protocol (GSTP)](/envelope/gstp/)
   * [Encrypted State Continuation (ESC)](/envelope/esc/)
* [Schnorr](https://www.blockchaincommons.com/musings/Schnorr-Intro/) (blog post)
* [ZF FROST](https://frost.zfnd.org/frost.html) (ZF FROST Docs)

**Next Step Intro:**

* [CKM](/ckm/)

**Developer Resources:**

* [Sequence Diagram](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/csr-sequence-diagram.md) (GitHub repo)

**Developer Reference Apps:**

* [Gordian Depository](https://github.com/BlockchainCommons/bc-depo-rust)
   * [Gordian Depository API](https://github.com/BlockchainCommons/bc-depo-api-rust)
* [Gordian SeedTool](https://github.com/BlockchainCommons/GordianSeedTool-iOS) (app implementation)

**Use Cases:**

* [Progressive Use Cases](/csr/use-cases/)

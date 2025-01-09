---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "SSKR: Sharded Secret Key Reconstruction"
hide_description: true
classes:
  - wide
permalink: /sskr/
sidebar:
  nav:
    - sskr
redirect_from:
  - /sskr/overview/
---

## Overview

<a href="/crypto-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-crypto-sskr.png" style="margin-left: 20px; float: right" width="25%"></a>

SSKR is Sharded Secret Key Reconstruction. It's a way that you can
divide ("shard") the master seed underlying a Bitcoin HD wallet into
"shares", which a user can then distribute to friends, family, or
fiduciaries. If the user ever loses their seed, they can then
"reconstruct" it by collecting sufficient of the shares (the
"threshold").

## Why is SSKR Important?

One of the biggest challenges in the control of digital assets
(particularly the self-sovereign control of digital assets) is
[Resilience](/principles/)
It's too easy for a user to lose the seed or key that control their
assets, and thus the assets.

SSKR resolves the Single Point of Failure (SPOF) represented by a seed
by allowing a backup of the seed to be created in a way that doesn't
introduce a Single Point of Compromise (SPOC) to the system. It does
by using Shamir's Secret Sharing to shard a secret, with the intent
being that the shares are then placed in discrete, remote
locations. The seed can only be recovered if a sufficient threshold of
shares are then combined.

## How Does SSKR Work?

The basic level of SSKR allows you to create a single group of shares,
with a threshold for how many of those must be collected to
reconstruct the seed. The following shows an example from [Gordian
SeedTool](https://github.com/BlockchainCommons/GordianSeedTool-iOS) of
creating three shares, of which two must be recovered.

<figure class="third">
  <a href="/assets/images/sskr/export-1.jpg"><img src="/assets/images/sskr/export-1.jpeg"></a>
  <a href="/assets/images/sskr/export-2.jpg"><img src="/assets/images/sskr/export-2.jpeg"></a>
  <a href="/assets/images/sskr/export-3.jpg"><img src="/assets/images/sskr/export-3.jpeg"></a>  
</figure>

The user would take these shares and give one each to three different
trusted people (or places, such as a safe or bank vault). (Or, this
could be automated using [CSR](/csr/).)

_See the [SSKR FAQ](/sskr/faq/) for a more complete explanation of these words._

### How Does Advanced SSKR Work?

SSKR supports a more advanced methodology where you can define
multiple groups, and then require a certain number of shares to come
back from each group for a certain number of groups.

The following shows an example from [Gordian
SeedTool](https://github.com/BlockchainCommons/GordianSeedTool-iOS)
where 2 of 3 shares must come back from 2 of 3 groups.

<figure class="third">
  <a href="/assets/images/sskr/export-4.jpg"><img src="/assets/images/sskr/export-4.jpeg"></a>
  <a href="/assets/images/sskr/export-5.jpg"><img src="/assets/images/sskr/export-5.jpeg"></a>
  <a href="/assets/images/sskr/export-6.jpg"><img src="/assets/images/sskr/export-6.jpeg"></a>  
</figure>

This can allow for more complex scenarios, such as a business that
hands off one set of shares to Chief Officers, and then backs that up
with a set of shares held by their accountants or some other
fiduciary.

(Though this example is symmetrical, it's not required. You could
require 2 of 3 from group #1 and 3 of 5 from group #2, and then only
require 1 of 2 groups, which means either threshold would fulfill the
requirement.)

Once you generate advanced SSKR shares, the user would distribute them
just like basic SSKR shares, but here being very careful to understand
the roles of everyone they'ree giving shares to, since they're
creating a more complex procedure.

## What are SSKR URs?

SSKR shares of seeds or keys can be encoded using [Uniform Resources](/ur/). As described in [BCR 2020-011](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-011-sskr.md), each share is converted to CBOR and prefixed with a `ur:sskr` tag. This supports the storage of shares in a self-identifying manner.

However, the methodology has generally been deprecated to instead prefer SSKR Envelopes.

## What are SSKR Envelopes?

[Envelope](/envelope/) is Blockchain Commons' privacy-focused format for the storage and transmission of data. Though the UR format allowed for the self-identifying storage of singular bits of data, Envelope goes further, allowing for the storage of multiple types of data, including metadata, to provide even more information on what's inside. (There are also many other advantages, as discussed on the [Envelope](/envelope/) page.)

However, SSKR Envelopes take a somewhat different tack from SSKR URs. You _don't_ shard your seed or key. Instead, you place your seed or key in an Envelope, generate a new symmetric key, use the key to encrypt the Envelope, shard the key, and distribute copies of the Envelope that each contain the encrypted sub-Envelope and one of the shares. By this methodology, you can store many seeds and many keys in a single Envelope, but still only have one share to manage. It's much more in tune with the way that secrets work in the real world, where you may have many seeds for many different devices, and even old master keys generated _without_ seeds.

SSKR Envelopes are our preferred way to encode and share secrets, but you might use SSKR URs if you had a very constrained device or a very simple use case. The [test vectors](/sskr/vector/) page includes examples of both.

## Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>SSKR in Overview:</b>

{% include video id="RYgOFSdUqWY?start=1612" provider="youtube" %}

    </td>
    <td width="640px">
      <b>Early Demo Video:</b>

{% include video id="PIND7J096U8" provider="youtube" %}

    </td>
  </tr>
</table>

## Libraries

{% include lib-shamir.md %}

{% include lib-sskr.md %}

## Links

**Intro:**

* [Shamir's Secret Sharing: An Overview](https://docs.google.com/document/d/1rZJlFZcftrCM_KaxFnHUIskJKlSQzF0zFn4WIRQGDLU/edit#heading=h.imy5xgr88lxa) (#RWOT9)
* [BCR-011: UR Type Definition for SSKR](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-011-sskr.md) (Blockchain Commons Research)
* [SSKR Lexicon](/sskr/lexicon/)

**Developer Resources:**

* [Developer FAQ](/sskr/faq/)
* [Test Vectors](/sskr/vectors/)
* [Using URs for SSKR](/ur/sskr/)

**Developer Reference Apps:**

* [Gordian SeedTool](https://github.com/BlockchainCommons/GordianSeedTool-iOS) (app implementation)
* [SeedTool CLI](https://github.com/BlockchainCommons/bc-seedtool-cli) (CLI implementation)
* [Ledger Seed Tool App](https://github.com/aido/app-seed-tool) (app implementation by [Aido](https://github.com/aido))

**Use Cases:**
* [Cold Storage](/sskr/use-cases/cold/)

**Other Resources:**

* [Designing SSKR Share Scenarios](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/SSKR-Sharing.md) (#SmartCustody)
* [SSKR Dangers](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/SSKR-Dangers.md) (#SmartCustody)
* [Evaluating Social Schemes for Recovery](https://github.com/WebOfTrustInfo/rwot8-barcelona/blob/master/final-documents/evaluating-social-recovery.md) (#RWOT8)

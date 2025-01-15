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
divide ("shard") a seed (which is to say a secret, often used as the foundation for Hierarchical Deterministic, "HD", wallet)
into
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

The above examples show SSKR being encoded with [Uniform Resources](/ur/). As described in [BCR 2020-011](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-011-sskr.md), each share is converted to CBOR and prefixed with a `ur:sskr` tag. This supports the storage of shares in a self-identifying manner.

 This is the simplest way to use SSKR, though not the preferred one: it's generally been deprecated to instead prefer SSKR Envelopes.

## What are SSKR Envelopes?

[Envelope](/envelope/) is Blockchain Commons' privacy-focused format for the storage and transmission of data. Though the UR format allowed for the self-identifying storage of singular bits of data, Envelope goes further, allowing for the storage of multiple types of data, including metadata, allowing the preservation of multiple secrets and of descriptions of those secrets. (There are also many other advantages, as discussed on the [Envelope](/envelope/) page.)

To allow this, SSKR Envelopes takes a somewhat different tack from SSKR URs. You _don't_ shard your seed, key, or other secret. Instead you:

1. Place your seed or key in an Envelope.
2. Generate a new symmetric key.
3. Use the new symmetric key to encrypt the Envelope containing your data.
4. Shard the symmetric key with SSKR.
5. Distribute copies of the Envelope that each contain the encrypted sub-Envelope and one of the shares of the symmetric key.

By this methodology, you can store many seeds and many keys in a single Envelope, but still only have one share to manage. You can also include metadata explaining what the seed or key is for, either inside _or_ outside the encrypted content. (Encrypted data might remind you what a key is for; unecrypted data might help you or your heirs to recover sufficient key shares.) This method is much more in tune with the way that secrets work in the real world, where you may have many seeds for many different devices, and even old master keys generated _without_ seeds.

Though SSKR Envelopes are our preferred way to encode and share secrets, you might use SSKR URs if you had a very constrained device or a very simple use case. The [test vectors](/sskr/vector/) page includes examples of both.

## How to Get Started with SSKR Envelopes

You can easily incorporate SSKR into your project by using [an appropriate SSKR library](https://developer.blockchaincommons.com/sskr/#libraries) to shard your seed or other secret. However, we now instead suggest the use of SSKR Envelope to support the storage of metadata and multiple secrets. To fully support this, we suggest the following:

1. Add support for [LifeHash](https://developer.blockchaincommons.com/lifehash/) for seeds in your UX, so that you can visually recognize seeds, making it easy to see when they have been reconstructed (or just transferred) correctly.
2. Create a basic [`ur:envelope`](https://developer.blockchaincommons.com/envelope/) containing your seed, adding notes and date as metadata. You can use our [128-bit](https://developer.blockchaincommons.com/seed-128/) or [256-bit](https://developer.blockchaincommons.com/seed-256/) test seed for this purpose.
3. Read your seed back. Throw out data that you don't need when you do so.
4. Next, output your Envelope as a QR. Read that back in.
5. Next, output your Envelope as an [Animated QR](https://developer.blockchaincommons.com/animated-qrs/). Read that back in.

At this point you should have seeds that you know are interoperable thanks to the ability to check with LifeHash and the successful export and import in a variety of formats. You can now integrate SSKR as well:

6. Integrate SSKR and export an SSKR Envelope of your seed as a 2-of-3 sharding.
7. Test import with each of three combinations of two shares: AB, BC, and AC.
8. Test a two-of-three group, 2-of-3 share sharding, and import with a few different combination of two shares from two different groups, such as: A1A2B1B2, A2A3C2C3, and A1A3B2B3.
9. Figure out what other metadata you'd like to include, such as seed name, seed creation date, and notes. Export your seed with this metadata, shard, and reconstruct.

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

{% include lib-envelope.md %}

## Links

**Intro:**

* [Shamir's Secret Sharing: An Overview](https://docs.google.com/document/d/1rZJlFZcftrCM_KaxFnHUIskJKlSQzF0zFn4WIRQGDLU/edit#heading=h.imy5xgr88lxa) (#RWOT9)
* [BCR-011: UR Type Definition for SSKR](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-011-sskr.md) (Blockchain Commons Research)
* [SSKR Lexicon](/sskr/lexicon/)

**Developer Resources:**

* [Developer FAQ](/sskr/faq/)
* [Test Vectors](/sskr/vectors/)
* [Using URs for SSKR](/ur/sskr/)
* [Using Gordian Envelope](/envelope/)

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

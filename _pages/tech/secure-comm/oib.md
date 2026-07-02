---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-ux-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Object Identity Block (OIB)"
hide_description: true
classes:
  - wide
permalink: /oib/
sidebar:
  nav:
    - oib
redirect_from:
  - /oib/overview/
---

## Overview

<a href="/ux-stack"><img src="/assets/images/bc-stack-ux-oib.png" width="25%" style="float: right; margin-left: 20px;"></a>

The Object Identity Block (OIB) is a methodology for making a digital object
immediately recognizable to users (or at least as recognizable as
possible given the challenges of representing large digital numbers).

## Why is OIB Important?

User Error is a prime cause of asset loss. Fraud, including phishing,
is another. Both of these threats tend to result from the inability of
the average user to distinguish digital assets from each other, since
the private key (or seed) used to secure one asset can look much like
the private key (or seed) used to secure another.

The Blockchain Commons Object Identity Block (OIB) seeks to resolve
that problem by providing several hints, such as a
[LifeHash](/lifehash/), an abbreviated digest, and a human-readable
name that a user can put together to better identify a digital object.

## How Does OIB Work?

As discussed in ["BCR-2021-002: Digests for Digital
Object"](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2021-002-digest.md#object-identity-block),
an OIB can consist of up to six elements:

![](/assets/images/oib/example.jpg)

* *LifeHash.* This is a visual hash for the object, providing it with
sight-based identification. Our [LifeHash](/lifehash/) pages provide
more details on their creation and usage.
* *Icon.* This is an icon for the type of object. We have standard
icons that we use for seeds, public keys, private keys, and
addresses. Though interoperable icons would be great, the most
important thing currently is consistency within an application.
* *Abbreviated Digest.* We
[specify](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2021-002-digest.md#the-digest-source-specification)
ways to create digests for specific objects. (Also see our
["Creating Digests"](https://developer.blockchaincommons.com/lifehash/creation/)
page.) We then suggest using a seven-character abbreviation of that
digest when shown to users.
* *Human-Readable Name (optional).* This could be a name created by
the user, but if a user hasn't yet created a name, we suggest
auto-assigning one. Gordian SeedTool does so by identifying the color
of the LifeHash, and then adding on two [Bytewords](/bytewords).
* *Additional Icons.* These can reference object subtypes. In
particular, for seeds and keys, we suggest including the master
fingerprint for the seed and its Lifehash, as those can remain
consistent even when another app doesn't have complete private
information on a seed. See [LifeHash Creation](/lifehash/creation/)
for some notes on creating them and
[LifeHash test vectors](/lifehash/vectors/) for some examples of what they
look like.
* *Additional Metadata (optional).* Other information could include
data on what an asset contains or protects, notes left by the user, or
anything else.

## Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>LifeHash in Technology Overview:</b>

{% include video id="RYgOFSdUqWY?start=1495" provider="youtube" %}

    </td>
    <td width="640px">
      <b>OIB in Technology Overview:</b>

{% include video id="RYgOFSdUqWY?start=1579" provider="youtube" %}

    </td>
  </tr>
</table>

## Links

**Intro:**

* [LifeHash](/lifehash/)

**Developer Resources:**

* ["BCR-2021-002: Digests for Digital Objects"](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2021-002-digest.md) (Blockchain Commons Research)
* [Best Practices](/oib/best-practices/)
* [Test Vectors](/oib/vectors)
* [LifeHash Developer Docs](/lifehash/)

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-securecomm.jpg
  og_image: /assets/images/bc-card.jpg
title: "LifeHash"
tagline: Using Visual Hashes
hide_description: true
classes:
  - wide
permalink: /lifehash/
sidebar:
  nav:
    - lifehash
    - securecomm
    - technology
---

<div class="hexline hexgrid71">
  <div class="hex11 opaqued">
    <a href="/airgap/">
      <img src="/assets/badges/airgap.png">
    </a>
  </div>
  <div class="hex12top highlighted">
    <a href="/secure-comm/">
      <img src="/assets/badges/cat-comm-half.png">
    </a>
  </div>
  <div class="hex21 opaqued">
    <a href="/torgap/">
      <img src="/assets/badges/torgap.png">
    </a>
  </div>
  <div class="hex31 highlighted">
    <a href="/lifehash/">
      <img src="/assets/badges/lifehash.png">
    </a>
  </div>
  <div class="hex41">
    <a href="/oib/">
      <img src="/assets/badges/oib.png">
    </a>
  </div>  
  <div class="hex51 opaqued">
    <a href="/animated-qrs/">
      <img src="/assets/badges/animated-qr.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/garner/">
      <img src="/assets/badges/garner.png">
    </a>
  </div>
 <div class="hex71 opaqued">
    <a href="/hubert/">
      <img src="/assets/badges/hubert.png">
    </a>
  </div>
</div>


_LifeHash is a method of hash visualization based on Conway's Game of
Life that creates beautiful icons that are deterministic, yet distinct
and unique given the input data. It is part of the [OIB](/oib/)._

## Why is LifeHash Important?

Users need to know what seeds or private keys they are using when
signing. They also need to know what addresses they are sending assets
to. Unfortunately, the actual seed, key, or address, which is just a
big number, isn't very recognizable by the human brain.

That's where LifeHashes come in: they create a visual representation
for that seed, key, or address that a human will be able to easily
recognize. This is particularly important as LifeHash develops as an
interoperable standard: a user can see that their digital object has
successfully transferred from one location to another. LifeHashes can
also be used to prevent spoofing as an out-of-band confirmation, and
will become inherently recognizable to users with repetition.

Lifehash can also be a crucial element in an [Object Identity Block (OIB)](/oib/), which uses multiple cues to help a user identify digital data.

## How Does LifeHash Work?

The basic concept is to take a SHA-256 hash of the input data (which
can be any data including another hash) and then use the 256-bit
digest as a 16x16 pixel "seed" for running the cellular automata known
as [Conway’s Game of
Life](https://en.wikipedia.org/wiki/Conway's_Game_of_Life).

After the pattern becomes stable (or begins repeating) the resulting
history is used to compile a grayscale image of all the states from
the first to last generation. Using Game of Life provides visual
structure to the resulting image, even though it was seeded with
entropy.

Some bits of the initial hash are then used to deterministically apply
symmetry and color to the icon to add beauty and quick
recognizability.

_Please note that LifeHash is not a cryptographic hash. More research
and analysis would be needed before anything could be said about its
cryptographic characteristics. Please see the [FAQ](/lifehash/faq/)
for more information._

## Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>LifeHash Overview:</b>

{% include video id="cu0K__KLxKo" provider="youtube" %}

    </td>
    <td width="640px">
      <b>LifeHash in Technology Overview:</b>

{% include video id="RYgOFSdUqWY?start=1495" provider="youtube" %}

    </td>
  </tr>
</table>

## Libraries

{% include lib-lifehash.md %}

## LifeHash Links

**Intro:**

* [Object Identity Block (OIB)](/oib/)

**Developer Resources:**

* [LifeHash Versions](/lifehash/versions/)
* [Creating Digests for LifeHashes](/lifehash/creation/)
* [Best Practices](/lifehash/best-practices/)
* [Test Vectors](/lifehash/vectors)

**Developer Reference Apps:**

* [lifehash-cli](https://github.com/BlockchainCommons/lifehash-cli) (CLI implementation)
* [lifehash.info](https://lifehash.info/) (web implementation)
* [LifeHashTool](https://github.com/BlockchainCommons/LifeHashTool) (Swift CLI implementation)

**Use Cases:**

* [General Use Cases](/lifehash/use-cases/)

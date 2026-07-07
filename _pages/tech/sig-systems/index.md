---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-sigsystem.jpg
  og_image: /assets/images/bc-card.jpg
title: Signatures System Technologies
tagline: "FROST, MuSig, and Provenance Marks"
hide_description: true
classes:
  - wide
permalink: /signatures/
sidebar:
  nav:
    - signatures
    - technology
---


<div class="hexline hexgrid71">
  <div class="hex31">
    <a href="/frost/">
      <img src="/assets/badges/frost.png">
    </a>
  </div>
  <div class="hex32top highlighted">
    <a href="/signatures/">
      <img src="/assets/badges/cat-sig-half.png">
    </a>
  </div>
  <div class="hex41">
    <a href="/musig/">
      <img src="/assets/badges/musig.png">
    </a>
  </div>  
  <div class="hex51">
    <a href="/provemark/">
      <img src="/assets/badges/provenance-marks.png">
    </a>
  </div>
</div>

_Signature technologies allow users to prove control of a private key
and therefore prove that the holder of a key agreed to
something. They've traditionally been used for identity authentication
and contract signing. But new threshold-based signature schemes such
as FROST (and sometimes MuSig) go further and allow for thresholds of
groups to make decisions, while Provenance Marks use sigantures to
prove ordering._

***Why?*** _Obviously, being able to prove control of keys and
identity is important. MuSig and FROST also offer much stronger
security because they can sign for keys that don't currently exist in
a single form (making them less prone to compromise, while the use of
thresholds also makes them less prone to loss)._

## ![](/assets/badges/frost.png) FROST

**Threshold Signature.** FROST is a threshold signature system whose
keys can be created by Distributed Key Generation. It supports
increased privacy due to its signature aggregation power.

For more see:

* [**FROST**](/frost/)
* [**White Paper**](https://eprint.iacr.org/2020/852.pdf)

## ![](/assets/badges/musig.png) MuSig2

**Multi-Signature.** Musig is a multisig system that can be also used for
threshold signing with certain additional tools. Like FROST, its
signatures are aggregatable, but it offers stronger ability to prove
who signed (at the cost of privacy).

For more see:

* [**MuSig 2**](/musig/)
* [**White Paper**](https://eprint.iacr.org/2020/1261.pdf)

## ![](/assets/badges/provenance-marks.png) Provenance Mark

**Hash Chain.** Provenance marks are a cryptographic system used to
link digital objects, so that it can be proven that one followed
another and (if desired) that there are no gaps.

For more see:

* [**Provenance Mark**](/provemark/)


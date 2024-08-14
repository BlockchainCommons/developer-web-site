---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Future Development: Flexible Round-Optimized Schnorr Threshold Signatures (FROST)"
hide_description: true
classes:
  - wide
permalink: /frost/
sidebar:
  nav: frost
---

## Overview

<a href="/crypto-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-crypto-frost.png" style="margin-left: 20px; float: right" width="25%"></a>

FROST (Flexible Round-Optimized Schnorr Threshold Signatures) is a threshold signature scheme built on Schnorr signatures that allows for
"T of N" signers, each of whom hold a share of a private key, to produce a valid signature for that private key. Because of its
[foundation in Schnorr](https://www.blockchaincommons.com/musings/Schnorr-Intro/), it has several interesting characteristics, such as signature aggregation,
where a set of signatures look exactly the same as a single signature. Beyond that, FROST improves on the fundamentals
of Schnorr through improved network efficiency and protection against forgery attacks.

## Upcoming Events

* **SEPTEMBER 18th, 2024. FROST Implementer's Meeting.** This is a meeting for FROST implementers to discuss the status of their FROST work with fellows. If you're a FROST implementer, [Sign up to our FROST Implenter's announcement list](https://www.blockchaincommons.com/subscribe/#frost-implementers) to receive notices & links for the meeting.
* **DECEMBER 4th, 2024. FROST Implementer & Developer Meeting.** This is a joint meeting where we and FROST implementers will talk about real-world development of FROST for _your_ wallet. If you're a wallet or app developer, [Sign up to our Gordian Developer's announcement list](https://www.blockchaincommons.com/subscribe/#gordian-developers) or Signal group to stay up-to-date on this and other meetings of interest.

For more info, see our [2023 FROST Implementer's Round Table](https://developer.blockchaincommons.com/frost/meeting1/) and our [early 2024 Developer's meeting](https://developer.blockchaincommons.com/frost/developers1/).

## Why is FROST Important?

Using a Schnorr-enabled signature scheme allows for [many advantages](https://www.blockchaincommons.com/musings/Schnorr-Intro/) including compactness
and faster verification as well as the use of signature aggregation, adapter signatures, blind signatures, and threshold signatures. Each of
these elements adds vital tools to the toolkit of a signature designer.

Some of the biggest benefits come from the threshold signatures at the heart of FROST, which improve
the [resilience](/principles/) of digital assets. Traditionally, multisigs or Shamir's Secret Sharing were used to create this type of
resilience, but they each had limitations. Multisigs could become quite large as more signatures were added, while Shamir's Secret
Sharing created vulnerabilities on reconstruction (and also had a history of problematic implementations). FROST resolves size issues
with its aggregate signatures (which are always the same size), while the Distributed Key Generation (DKG) methodology creates keys that are _never_ in
one place!

### How is FROST Better than Extant Technologies?

* **Advantages over Bitcoin Multisigs.** 
   * **Better privacy:** on-chain footprint is always a single key and a single signature, regardless of configuration
   * **Lower fees:** redeem scripts are much smaller than script-based multisig
   * **Off-chain resharing:** repair, refresh, enroll, disenroll, and modify the threshold without moving funds, incurring fees, or exposing private information
* **Advantages over Shamir Secret Sharing.**
   * No trusted dealer
   * No secret reconstruction

## How Does FROST Work?

FROST is fully described in [a paper](https://eprint.iacr.org/2020/852.pdf) by Chelsea Komlo and Ian Goldberg. Some of the fundamental elements
of FROST are:

**Schnorr Signatures.** A signature scheme based on finite fields rather than prime numbers. Besides being very elegant, Schnorr signatures have numerous other advantages, as discussed in ["Musings of a Trust
Architect: A Layperson's Intro to Schnorr"](https://www.blockchaincommons.com/musings/Schnorr-Intro/).

**Signature Aggregation.** One of the advantages of Schnorr signatures it that they can easily be added to or subtracted
from each other as part of their finite field operations, creating signature aggregation where all signatures are the same size, no matter how
many signatures they contain. 

**Distributed Key Generation (DKG).** One of the core features of FROST is Distributed Key Generation: rather than a key being created,
then broken apart to allow for threshold signing with the shares, the shares are instead each created individually. This means that the full key never exists in a single place.

**Trusted Dealer Generation.** This is the traditional methodology for generating shares, where a key is created then broken apart by
a "trusted dealer". Some FROST implementations such as [ZF FROST](https://frost.zfnd.org/index.html) also support this methodology, creating
a bridge between traditional use of Shamir's Secret Sharing and the use of DKG in FROST.

**Verifiable Secret Sharing (VSS).** A methodology for verifying the existence of shares without reconstruting a secret. FROST uses a
VSS protocol invented by Paul Feldmann. 

**Emerging Features.** At our first [FROST Meeting for Developers & Implementers](https://www.youtube.com/watch?v=uCM8dDql6oo&t=2762s), Jesse Posner talked about some of the emerging features of FROST. This included being able to proactively refresh or repair secret shares or even change the threshold up or down, all _without changing the underlying secret_. This is the sort of content that we hope to share at future Implementer Round Tables, to ensure that all FROST implementers have the opportunity to use FROST to its fullest capability.

## Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>FROST/Gordian Overview:</b>

{% include video id="uCM8dDql6oo" provider="youtube" %}

    </td>
  </tr>
  <tr>
    <td width="640px">
      <b>FROST Round Table I:</b>

{% include video id="U9MvNuyCpE4" provider="youtube" %}

    </td>
  </tr>
</table>

## Sponsors

Blockchain Commons' 2024 work on FROST has been sponsored by the [Human Rights Foundation](https://hrf.org/).

<center><a href="https://hrf.org/"><img src="https://www.blockchaincommons.com/images/sponsors/hrf-white.png"></a></center>

## Links

**Intro:**

* [Understanding FROST](https://frost.zfnd.org/frost.html) (ZF FROST Book)
* [A Layperson's Intro to Schnorr](https://www.blockchaincommons.com/musings/Schnorr-Intro/) (Blockchain Commons Blog)
* [Paper by Chelsea Komlo and Ian Goldberg](https://eprint.iacr.org/2020/852.pdf) (Cryptology ePrint Archive)

**Developer Resources:**

* [FROST Implementer's Round Table I](/frost/meeting1/)
* [FROST Developer's Meeting](/frost/developers1/)

**Developer Reference Libraries:**

* [ZF FROST](https://frost.zfnd.org/index.html) (ZF FROST Book)


---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Future Development: Flexible Round-Optimized Schnorr Threshold Signatures (FROST)"
hide_description: true
classes:
  - wide
permalink: /frost/
sidebar:
  nav:
    - frost
    - meetings
---

## Overview

{% include news-frost.md %}

<a href="/crypto-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-crypto-frost.png" style="margin-left: 20px; float: right" width="25%"></a>

FROST (Flexible Round-Optimized Schnorr Threshold Signatures) is a threshold signature scheme built on Schnorr signatures that allows for
"T of N" signers, each of whom hold a share of a private key, to produce a valid signature for that private key. Because of its
[foundation in Schnorr](https://www.blockchaincommons.com/musings/Schnorr-Intro/), it has several interesting characteristics, such as signature aggregation,
where a set of signatures look exactly the same as a single signature. Beyond that, FROST improves on the fundamentals
of Schnorr through improved network efficiency and protection against forgery attacks.

The aggregation used by Schnorr eliminates multisig bloat and Shamir reconstruction risks and enables threshold authorization of [Gordian Envelope](/envelope/), [XID-based credentials](/xid/), and [Gordian Clubs](/clubs/).

## Upcoming Events

_Our last FROST meeting for the year will be December 3, 2025. Sign up to our [Gordian Developers mailing list or Signal channel](https://www.blockchaincommons.com/subscribe/) to receive notification._

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

FROST is fully described in [a paper](https://eprint.iacr.org/2020/852.pdf) by Chelsea Komlo and Ian Goldberg. We also have a [hands-on tutorial](https://learningfrost.blockchaincommons.com/). Some of the fundamental elements
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

## Events

_The Blockchain Commons FROST events bring together principals in the space to advance the technology. Each event is fully documented with videos, slidedecks, summaries, and key quotes._

<table width="100%">
  <tr>
    <td width="640px">
      <b>Implementers RT 1 (2023):</b>
      <a href="meeting1">
        <img src="/assets/images/frost-implementers-2023.png">
      </a>
      <i>Our <a href="meeting1">first Round Table</a> allowed implementers to discuss a variety of issues related to FROST.</i>
    </td>
    <td width="640px">
      <b>Implementers RT 2 (2024):</b>
      <a href="meeting2">
        <img src="/assets/images/frost-implementers-2024.png">
      </a>
      <i>Our <a href="meeting2">second Round Table</a> includes presentations from six FROST implementers on their projects.</i>
    </td>
  </tr>
  <tr>
    <td width="640px">
      <b>Developers Meeting 1 (2024):</b>
      <a href="developers1">
        <img src="/assets/images/frost-developers-2024-1.png">
      </a>
      <i>Jesse Posner & Christopher Allen overviewed FROST in our <a href="developers1">first meeting for wallet developers</a>.</i>
    </td>
    <td width="640px">
      <b>Developers Meeting 2 (2024):</b>
      <a href="developers2">
        <img src="/assets/images/frost-developers-2024-2.png">
      </a>
      <i>An introduction, UX challenges, the UNiFFI SDK, and a FROST Federation were at our <a href="developers2">second meeting for wallet developers</a>.</i>
    </td>
    <td width="640px">
      <b>Signing CLI Demo (2025):</b>
      <a href="/meetings/2025-08-frost-cli/">
        <img src="/assets/images/frost-cli-2025.png">
      </a>
      <i>A <a href="/meetings/2025-08-frost-cli/">demo of signing a Bitcoin PSBT with a FROST quorum</a>, using BDK, ZF FROST, and our own tools.</i>
    </td>
  </tr>
</table>

## Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>FROST/Gordian Overview:</b>

{% include video id="uCM8dDql6oo" provider="youtube" %}

    </td>
    <td width="640px">
      <b>FROST Round Table 1:</b>

{% include video id="U9MvNuyCpE4" provider="youtube" %}

    </td>
    <td width="640px">
      <b>FROST Developers 1:</b>

{% include video id="uCM8dDql6oo" provider="youtube" %}

    </td>
  </tr>
  <tr>
    <td width="640px">
      <b>FROST Round Table 2:</b>

{% include video id="VxLTJ_OxGT4" provider="youtube" %}

    </td>    
    <td width="640px">
      <b>FROST Developers 2:</b>

{% include video id="FbrB1SCXCNc" provider="youtube" %}

    </td>    
</tr>
<tr>
    <td width="640px">
      <b>TD Signing CLI Demo:</b>

{% include video id="8csdApREJIs" provider="youtube" %}

    </td>    
    <td width="640px">
      <b>DKG Signing CLI Demo:</b>

{% include video id="13skzOvWklk" provider="youtube" %}

    </td>    
  </tr>
</table>

## Sponsors

Blockchain Commons' 2024 & 2025 work on FROST has been sponsored by the [Human Rights Foundation](https://hrf.org/).

<center><a href="https://hrf.org/"><img src="https://www.blockchaincommons.com/images/sponsors/hrf-white.png"></a></center>

## Links

**Intro:**

* [Understanding FROST](https://frost.zfnd.org/frost.html) (ZF FROST Book)
* [A Layperson's Intro to Schnorr](https://www.blockchaincommons.com/musings/Schnorr-Intro/) (Blockchain Commons Blog)
* [Learning FROST from the Command Line](https://learningfrost.blockchaincommons.com/)
* [Paper by Chelsea Komlo and Ian Goldberg](https://eprint.iacr.org/2020/852.pdf) (Cryptology ePrint Archive)

**Meetings & Presentations:**

* [FROST Implementer's Round Table 1](/frost/meeting1/)
* [FROST Developer's Meeting 1](/frost/developers1/)
* [FROST Implementer's Round Table 2](/frost/meeting2/)
* [FROST Developer's Meeting 2](/frost/developers2/)
* [FROST Bitcoin Signing Demo](/meetings/2025-08-frost-cli/)

**Developer Reference Libraries:**

* [ZF FROST](https://frost.zfnd.org/index.html) (ZF FROST Book)

**Secure MPC Tools:**

* [Hubert Dead-Drop Hub](/hubert/)

**Bitcoin Signing:**

* [Trusted Dealer Signing Demo](https://www.youtube.com/watch?v=8csdApREJIs)
* [Distributed Key Generation Signing Demo](https://www.youtube.com/watch?v=13skzOvWklk)
* [Learning FROST Bitcoin Signing Tutorial](https://github.com/BlockchainCommons/Learning-FROST-from-the-Command-Line/blob/main/04_0_FROST_and_Bitcoin.md)
* [FROST Tools Branch](https://github.com/BlockchainCommons/zcash-frost-tools)

**Other Technology Use Cases:**

* [Gordian Clubs](/clubs/)
* [Gordian Envelope](/envelope/)
* [XIDs](/xid/)

<hr>

<i>Snowflake icons courtesy of <a href="https://freedesignfile.com/?cat=20205&s=snowflake">free design file</a>.</I>

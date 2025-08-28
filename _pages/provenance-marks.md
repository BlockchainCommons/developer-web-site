---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Provenance Marks"
hide_description: true
classes:
  - wide
permalink: /provemark/
redirect_from:
  - /provenance-marks/
  - /provenance-mark/
sidebar:
  nav:
    - stack-crypto
---

## Overview

<a href="/crypto-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-crypto-pm.png" style="margin-left: 20px; float: right" width="25%"></a>

Provenance Marks provide a cryptographically-secured system for establishing and verifying the authenticity of works. By combining cryptography, pseudorandom number generation, and linguistic representation, this system generates unique, sequential marks that commit to the content of preceding and subsequent works.

## Why are Provenance Marks Important?

In an age of rampant AI-powered manipulation and plagiarism, determining provenance is more important than ever.  These marks ensure public and easy verification of provenance, offering robust security and intuitive usability. Provenance Marks are particularly valuable for securing artistic, intellectual, and commercial works against fraud and deep fakes, protecting creators’ reputations and the integrity of their creations.

## How Do Provenance Marks Work?

Each Provenance Mark is a collection of structured binary date that contains 5-6 data elements. A linked set of Provenance marks together form a chain of provenance, which proves that a set of objects are linked (and in what order).

![](https://developer.blockchaincommons.com/assets/images/pm-chain.png)

The elements in a Provenance Mark are:

* **key.** A random number, generated from a seed for the chain of provenance.
* **hash.** A SHA-256 that commits to the next key in the chain.
* **id.** The unique identifier for a chain of provenance, which is the `key` of the genesis mark.
* **seq.** The sequence number for the Provenance Mark in the chain, starting from `0` for the genesis mark and incrementing from there.
* **date.** The date of the Provenance Mark's creation.
* **info (optional).** CBOR metadata for the mark.

It's the hash that actually creates the chain because it commits to the content of the next Provenance Mark in the chain. (In fact, it commits to everything in the next Provenance Mark in the chain, but the next `key` is the random element that couldn't otherwise be known.)
```
hash=trunc(H(key||nextKey||id||seq||date||info),linkLen)
```

Provenance Marks come on four sizes: `low`, `medium`, `quartile`, and `high`. They vary in size from 16 bytes (`low`) to 106 bytes (`high`), and the precise construction of the various fields depends on the size, as described in [BCR-2025-001](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2025-001-provenance-mark.md).

Provenance Marks are often represented as [byte words](https://developer.blockchaincommons.com/bytewords/), which allows their clear depiction using a number of simple words. A low-resolution mark, consisting of 16 bytes + 4 bytes of checksum, might be represented as follows:
```
taco kite buzz nail arch
fact bias nail apex plus
deli wave cats webs ruin
legs quiz draw work onyx
```
## How to Get Started with Provenance Marks

<img src="https://developer.blockchaincommons.com/images/assets/pm-symbol.svg" width="90em" style="float: right">

1. Read [BCR-2025-001: Provenance Marks: An Innovative Approach for Authenticity Verification](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2025-001-provenance-mark.md)
2. Choose a chain (either a sequence of objects or an object that changes over time) to mark with a chain of provenance
3. Test Provenance Marks with the [Provenance Mark CLI](https://github.com/BlockchainCommons/provenance-mark-cli-rust).
4. Use the [rust](https://github.com/BlockchainCommons/provenance-mark-rust/tree/master/src) or [swift](https://github.com/BlockchainCommons/Provenance) library to implement Provenance Marks.

## Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>June 2025 Meeting (Provenance Marks):</b>

{% include video id="vKAK6j4mqgE" provider="youtube" %}

    </td>
  </tr>
</table>

## Libraries

{% include lib-pm.md %}

## Links

**Intro:**

*[BCR-2025-001: Provenance Marks: An Innovative Approach for Authenticity Verification](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2025-001-provenance-mark.md)

**Developer Reference Apps:**

* [Provenance Mark CLI](https://github.com/BlockchainCommons/provenance-mark-cli-rust)

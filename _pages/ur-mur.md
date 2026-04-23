---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Multipart UR (MUR)
hide_description: true
classes:
  - wide
permalink: /ur/mur/
sidebar:
  nav:
    - ur
redirect_from:
  - /mur/
---

## Overview

<a href="/ux-stack"><img src="/assets/images/bc-stack-ux-urs.png" width="25%" style="float: right; margin-left: 20px;"></a>

The [UR specification](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md) is typically used for singular messages. However, it also supports the fragmentation of longer messages that can then be sequenced into a series of URs: this is a multipart UR (MUR). It allows for the creation of [Animated QRs](/animated-qrs/).

## Why are MURs Important?

Though MURs are not exclusively for use with Animated QRs, the creation of Animated QRs has been their biggest use case so far.

* **They allow for the encoding of larger data as QRs.** Static QRs have fairly small limits as to what they can contain. They're sufficient for an address, a seed, or an SSKR share, but they're not big enough for a larger PSBT or [Gordian Envelope](/envelope/). MURs allow larger data to be fragmented and then broadcast in an animatation containing multiple QR frames. This ensures that larger data can be transmitted reliable across airgaps or stored as a sequence of QRs.
* **They support unreliable communication.** MURs use a combination of fixed-rate and rateless fountain codes to achieve robustness against transmission errors. If a single frame is missed, it can be calculated by XORing other frames rather than waiting for the whole sequence to repeat.

## How Do MURs Work?

1. A message is divided into `seqLen` fragments.
2. Each fragment is encoded as a ur of the form `ur:<type>/<seq>/<fragment>`, where sequence is the current sequence Number (`seqNum`), a dash, and the `SeqLen`, e.g., `ur:seed/1-3/lpadaxcsencylobemohsgmoyadhdeynteelblrcygldwvarflojtcywyjydmylgdsa`.
3. The first `seqLen` fragments are fixed-rate fragments: if you put them together, they form the complete UR.
4. The MUR continues past that, but the further fragments are rateless. An RNG mixes together the fragments for each frame such that an XOR can be used to extract missing fragments, e.g., if a user had previously received fragment `1` and now received a rateless fragment of `1 ⊕ 2`, they could then extract fragment `2`.
5. The random rateless fragments continue transmitting until the recipient has put together the complete UR. (Clearly, if you were printing frames of a QR, you'd only print a limited number, possibly just the fixed-rate fragments0

The [Multipart UR (MUR) Implementation Guide](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md) offers more precise information on the entire MUR creation and reading methodology. This builds on the [UR specification](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md).

## Links

* [UR Developers Page](/ur/)
* [**BCR-2020-005: UR Specification**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md) (Blockchain Commons Research)
* [**Multipart UR (MUR) Implementation Guide**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md)





---
cover: true
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/blueprint.jpg
  og_image: /assets/images/bc-card.jpg
title: "Touchpoints - Airgapping PSBTs (Animated QRs)"
hide_description: false
classes:
  - wide
permalink: /touchpoints/psbt/
redirect_from:
  - /touchpoints/psbts/
  - /touchpoints/animated-qrs/
  - /touchpoints/animatedqrs/
  - /touchpoints/animatedqr/
  - /touchpoints/animated-qr/
sidebar:
  nav:
    - touchpoints
    - technology
    - resources
---

**Target Technology:** Bitcoin<br>
**Target Audience:** Bitcoin Developers

_Blockchain Commons' Animated QRs technology answers one simple
question: how can I send a large data file, such as a PSBT, securely
over an Airgap?_

## ![](/assets/badges/animated-qr.png) Animated QRs

**Communication QR.** The simple answer is Animated QRs. A standard QR
is limited in size to 7089 numeric characters, 4296 alphanumeric
characters, or 2953 bytes of binary data. Animated QRs allow infinite
expansion beyond that size limit by fragmenting the data and then
presenting the fragments as individual frames of an animated QR code.

Animated QRs are built using fountain codes, which allow for more
efficient transmission and avoid the problem of missing a single frame
and then having to cycle all the way back to that same frame again.

Though Animated QRs are built to support transmission across airgaps,
innovative uses in other mediums are possible. For example, you could
print out all the frames of an Animated QR on a piece of paper, and
then read them back in from that, one at a time.

* For more, see [**Animated QRs**](/animated-qrs/)

## ![](/assets/badges/ur.png) Uniform Resources (UR)

**Communication Text.** Animated QRs are built on Uniform
Resources. These are a plan-text encoding method for binary data
that's self-describing and that can be stored efficiently in QR
codes. The UR is also what supports the ability to fragment data.

* For more, see [**URs**](/ur/)

## ![](/assets/badges/mur.png) Multipart Uniform Resources (MUR)

**Multipart Communication Text.** When a UR is fragmented it becomes a
MUR. They allow for the encoding of larger data. Because of their use
of fountain codes, they're also great for transmitting over unreliable
communication networks.

Our [implementation
guide](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md)
for MURs is what you need to turn your data into UR fragments that you
can then qr-encode. **START HERE!**

* For more, see [**MURs**](/ur/mur/)
* For more, see [**MUR Implementation Guide**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md)

## ![](/assets/badges/airgap.png) Airgaps

**Communication Gap.** Animated QRs were built to allow transmission
of PSBTs across airgaps. These are communication gaps built to protect
your secrets. When you keep your seeds and private keys on a hardware
wallet that doesn't connect to the internet, that's an airgap. QRs
have become a preferred method for bridging that gap in part because
they only allow transmission in one direction at a time (making
attacks very difficult). But PSBTs were a problem: they're an ideal
use for airgaps because you can create your transaction on the
internet, pass it through an airgap to the device holding your keys,
sign it, and then pass it back through the airgap for
transmission. The problem is that they're usually too big to go in QR
codes. Animated QRs offered the answer.

* For more, see [**Airgaps**](/airgap/)

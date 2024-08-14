---
cover: true
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/qr-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Blockchain Commons Developer Pages
hide_description: false
classes:
  - wide
permalink: /
sidebar:
  nav: mainside
---

_**Latest News:** Added [Achitectural Design Patterns & Auth Patterns](/architecture/patterns/auth) pages (5/8/24). Added [Envelope Executive Summary](/envelope/summary/) and [Feature List](/envelope/features/) pages (4/24/24)._
{: .notice--info}

The Gordian Stack is a layered set of specifications founded on the CBOR data format and built up from that, step by step, to support digital-asset management in a way that fulfills [the Gordian Principles](/principles/). These developer pages contain developer resources that
explain the elements in the Stack, and their importance, and make it easy
for you to adopt them.

![bc-stack](https://developer.blockchaincommons.com/assets/images/bc-stack.png)

The heart of the Gordian Stack is the [<font color="#ffac1c">Gordian Envelope</font>](/envelope/), a simple data-storage mechanism that organizes content into semantic triples. You don't need to know about the underlying elements to use Envelopes, so as a developer you can jump straight in and be confident that everything under Envelope is correctly abstracted to maintain layer divisions.

The [<font color="#17c3ff">Animated QR specification</font>](/animated-qrs), built on Multipart Uniform Resources (MURs), is the other major entry point to the Gordian Stack, since it allows for the transmission of large data sets, such as Bitcoin's PSBTs, across Airgaps.

Following is a description of the major layers of the Gordian Stack, in descending order.

If you have any questions or want more resources for any specific
element in the Stack, please [let us know via
email](mailto:team@blockchaincommons.com) or [file an
Issue](https://github.com/BlockchainCommons/developer-web-site/issues)
at the repo for this website.

## The Core Stack

<img src="https://developer.blockchaincommons.com/assets/images/bc-stack-core-0.png" style="float: right; margin-left: 20px;" width="25%">

_Blockchain Commons' Core Stack includes its two major user-facing innovations, Collaborative Seed Recovery (CSR) and Envelope, plus the binary encoding scheme that enables them both (dCBOR)._

### Core: <font color="#221dff">CSR</font> Layer

{% include stack-csr.md %}

### Core: <font color="#ffac1c">Envelope</font> Layer

{% include stack-envelope.md %}

### Core: <font color="#8f1402">CBOR</font> Layer

{% include stack-cbor.md %}

## The UX Stack

_The UX Stack includes graphical and text encodings that further empower the Core Stack._

### UX: <font color="#2df775">OIB</font> Layer

<img src="https://developer.blockchaincommons.com/assets/images/bc-stack-ux-0.png" style="margin-left: 20px; float: right" width="25%%">

{% include stack-oib.md %}

### UX: <font color="#c96055">UR</font> Layer

{% include stack-ur.md %}

## The Crypto Stack

<img src="https://developer.blockchaincommons.com/assets/images/bc-stack-crypto-0.png" style="float: right" width="20%">

_The Crypto Stack features the cryptographic elements in Blockchain Commons' stack, including seeds, sharding, and DKG systems._

### Crypto: <font color="#888888">CKM</font> Layer

{% include stack-ckm.md %}

### Crypto: <font color="#038e3e">Sharding</font> Layer

{% include stack-sharding.md %}

### Crypto: <font color="#038e3e">Seed</font> Layer

{% include stack-seed.md %}

## Architectural Overview

_The Gordian Stack is built atop a carefully considered architecture that works to uphold human dignity and choice on the internet._

* **Architecture.** The Gordian architecture is built on specific [principles](/principles/), using specific [design patterns](/architecture/patterns/auth/), and with a general philosophy of functional partition.
   * See [our architecture page](/architecture/)
   * See [Musings of a Trust Architect](https://www.blockchaincommons.com/musings/)

## Other <font color="#888888">Future</font> Development

_A few other technologies are currently in early stages of development or integration._

* **The Discovery Layer.** A discovery layer is planned that will allow end-users to find and identify Gordian Depositories or other online services.
  
## For More Info

Our Gordian Developer community is actively working with this resources! Join us in Discussions or at our monthly meetings!

* [Gordian Developer Community](https://github.com/BlockchainCommons/Gordian-Developer-Community/discussions)
   * [Meetings Records](https://github.com/BlockchainCommons/Gordian-Developer-Community/blob/master/meetings/README.md)
   * [Join Us!](https://www.blockchaincommons.com/subscribe/)

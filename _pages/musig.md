---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Future Development: MuSig 2"
hide_description: true
classes:
  - wide
permalink: /musig/
sidebar:
  nav: musig
---

## Overview

<a href="/crypto-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-crypto-musig.png" style="margin-left: 20px; float: right" width="25%"></a>

MuSig 2 is a simple two-round Schnorr multisig system. It's a signature scheme that is a notable improvement over Bitcoin's classic ECDSA-based multisign primarily due to its use of aggregation: multiple keys are "added" together to create a multisig, and the sum key is not just the same size as an individual key, but in fact indistiguishable from an individual key.

MuSig was originally released in 2018, then updated in 2020 to reduce the required communication rounds from three to two, hence the name: MuSig 2.

## Why is MuSig Important?

MuSig is one of two major methods to take advantage of Schnorr-based signatures, allowing users to take advantage of a fast, scalable, and aggregatable signature system (the other being [FROST](/frost/)). 

The general advantages of Schnorr signatures, as described in ["A Layperson's Introduction to Schnorr"](https://www.blockchaincommons.com/musings/Schnorr-Intro/) are:

* Compactness
* Signature Aggregation
* Faster Verification
* Blind Signatures
* Adapter Signatures 

Unlike FROST, MuSig 2 does not support threshold signatures, except through the use of a supplement such as a Taproot tree. However, it can support full accountability: an advantage of FROST is that you can't prove who signed in an m-of-n signature, but in MuSig you  know exactly who signed if the public keys are available.

## Links

* [MuSig2: Simple Two-Round Schnorr Multisignatures](https://medium.com/blockstream/musig2-simple-two-round-schnorr-multisignatures-bf9582e99295) (Medium article)
* [MuSig2: Simple Two-Round Schnorr Multi-Signatures](https://eprint.iacr.org/2020/1261.pdf) (eprint PDF)

* [Docs: secp256k1_musig.h](https://github.com/bitcoin-core/secp256k1/blob/master/include/secp256k1_musig.h) (GitHub)
* [Examples](https://github.com/bitcoin-core/secp256k1/blob/master/examples/musig.c) (GitHub)

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

MuSig 2 is a [simple two-round Schnorr multisig system]

## Why is MuSig Important?

MuSig is one of two major methods to take advantage of Schnorr-based signatures, allowing users to take advantage of a fast, scalable, and aggregatable signature system (the other being [FROST](/frost/)). 

The general advantages of Schnorr signatures, as described in ["A Layperson's Introduction to Schnorr"]((https://www.blockchaincommons.com/musings/Schnorr-Intro/)) are:

* Compactness
* Signature Aggregation
* Faster Verification
* Blind Signatures
* Adapter Signatures 

Unlike FROST, MuSig 2 does not support threshold signatures, except through the use of a supplement such as a Taproot tree. However, it does allow full accountability: an advantage of FROST is that you can keep private who signed in an m-of-n signature, in MuSig you always know exactly who signed.

## Links

* [MuSig2: Simple Two-Round Schnorr Multi-Signatures](https://eprint.iacr.org/2020/1261.pdf) (eprint PDF)
* [Docs: secp256k1_musig.h](https://github.com/bitcoin-core/secp256k1/blob/master/include/secp256k1_musig.h) (GitHub)
* [Examples](https://github.com/bitcoin-core/secp256k1/blob/master/examples/musig.c) (GitHub)

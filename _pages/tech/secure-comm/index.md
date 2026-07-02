---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-securecomm.jpg
  og_image: /assets/images/bc-card.jpg
title: Secure Communication Technologies
tagline: From Gaps to Garner
hide_description: true
classes:
  - wide
permalink: /secure-comm/
redirect_from:
    - /securecomm/
    - /secure-communication/
    - /securecommunication/
sidebar:
  nav:
    - securecomm
    - technology
---


<div class="hexline hexgrid71">
  <div class="hex11">
    <a href="/airgap/">
      <img src="/assets/badges/airgap.png">
    </a>
  </div>
  <div class="hex12top highlighted">
    <a href="/data-formats/">
      <img src="/assets/badges/cat-comm-half.png">
    </a>
  </div>
  <div class="hex21">
    <a href="/animated-qrs/">
      <img src="/assets/badges/animated-qr.png">
    </a>
  </div>
  <div class="hex31">
    <a href="/garner/">
      <img src="/assets/badges/garner.png">
    </a>
  </div>
 <div class="hex41">
    <a href="/hubert/">
      <img src="/assets/badges/hubert.png">
    </a>
  </div>
  <div class="hex51">
    <a href="/lifehash/">
      <img src="/assets/badges/lifehash.png">
    </a>
  </div>
  <div class="hex61">
    <a href="/oib/">
      <img src="/assets/badges/oib.png">
    </a>
  </div>  
  <div class="hex71">
    <a href="/torgap/">
      <img src="/assets/badges/torgap.png">
    </a>
  </div>
</div>

_The Blockchain Commons technology stack includes a stack of data formats. CBOR (and variant dCBOR) are fundamental binary data serialization formats. Bytewords encodes binary objects as four-letter English words. URs and MURs turn Bytewords into self-describing data objects. Known values encode ontological concepts as unsigned integers. Finally, Gordian Envelope builds on all of that to offer a self-describing, recursive, smart-document storage format._

***Why?*** _Data formats are more than just wants to encode data. They also encode specific principles. Blockchain Commons' data formats embody the [Gordian principles](/principles/) in large part by ensuring that open (and interoperable) as well as resilient (and harder to confuse). They also strive to be deterministic on a variety of formats (which supports openness and interoperability) and efficient._


## ![](/assets/badges/bytewords.png) Bytewords

Bytewords translates binary objects into a series of four-letter English words. They can be used to reliably record digital secrets, but they're also a building block that transforms CBOR into URs and MURs.

For more see:

* [**Bytewords**](/bytewords/)
* [**BCR-2020-012**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-012-bytewords.md) (Research Repo)

## ![](/assets/badges/cbor.png) CBOR

CBOR is an IETF data format (RFC 8949) that we have adopted as the fundamental data representation for higher-level Blockchain Commons data formats such as Gordian Envelope and URs.

For more see:

* [**CBOR**](/cbor/)
* [**RFC8949**](https://www.rfc-editor.org/info/rfc8949/) (RFC Editor)

## ![](/assets/badges/dcbor.png) dCBOR

dCBOR stands for Deterministic CBOR. It's our own variant of CBOR that always encodes the same on any platform. It's a necessary building block for using CBOR as the foundation of a deterministic storage system like Gordian Envelope.

For more see:

* [**dCBOR**](/dcbor/)
* [**draft-mcnally-deterministic-cbor Internet-Draft**](https://datatracker.ietf.org/doc/draft-mcnally-deterministic-cbor/) (Data Tracker)

## ![](/assets/badges/envelope.png) Gordian Envelope

Envelope is the culmination of many of Blockchain Commons' other data formats. It's build on dCBOR and can be encoded as a UR. It's goal is to provide "smart document" storage. Arbitrary, recursive data storage is permitted, and anything can be encrypted or elided without damaging signatures.

For more see:

* [**Envelope**](/envelope/)
* [**draft-mcnally-envelope Internet-Draft**](https://datatracker.ietf.org/doc/draft-mcnally-envelope/) (Data Tracker)

## ![](/assets/badges/known-values.png) Known Values

Known Values correlate ontological concepts to integer values, allowing for efficient and standardized recording of common, repeated concepts.

For more see:

* [**Known Values**](/known-value/)
* [**BCR-2023-002**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-002-known-value.md) (Research Repo)

## ![](/assets/badges/ur.png) UR: Uniform Resources

URs utilize a minimalistic form of Bytewords to efficiently record binary data as text in a way that's interoperable and self-describing.

For more see:

* [**URs**](/ur/)
* [**BCR-2020-005**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md) (Research Repo)

## ![](/assets/badges/mur.png) MUR: Multipart UR

Multipart URs break URs into multiple pieces to allow for the transmission of smaller data sets, which is important when URs are used for communications as QR codes.

* [**MURs**](/ur/mur/)
* [**BCR-2024-001**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md) (Research Repo)


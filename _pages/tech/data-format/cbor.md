---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataformat.jpg
  og_image: /assets/images/bc-card.jpg
title: Concise Binary Object Representation (CBOR)
hide_description: true
classes:
  - wide
permalink: /cbor/
sidebar:
  nav:
    - dataformat
    - technology
redirect_from:
  - /cbor/overview/
---

<div class="hexline hexgrid71">
  <div class="hex11 highlighted">
    <a href="/bytewords/">
      <img src="/assets/badges/bytewords.png">
    </a>
  </div>
  <div class="hex12top">
    <a href="/data-formats/">
      <img src="/assets/badges/cat-dataformat-half.png">
    </a>
  </div>
  <div class="hex21 opaqued">
    <a href="/cbor/">
      <img src="/assets/badges/cbor.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/dcbor/">
      <img src="/assets/badges/dcbor.png">
    </a>
  </div>
 <div class="hex41 opaqued">
    <a href="/envelope/">
      <img src="/assets/badges/envelope.png">
    </a>
  </div>
  <div class="hex51 opaqued">
    <a href="/known-values/">
      <img src="/assets/badges/known-values.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/ur/">
      <img src="/assets/badges/ur.png">
    </a>
  </div>  
  <div class="hex71 opaqued">
    <a href="/mur/">
      <img src="/assets/badges/mur.png">
    </a>
  </div>
</div>

_CBOR is a binary data format defined in [RFC 8949](https://datatracker.ietf.org/doc/html/rfc8949). It is the foundation of most of Blockchain Commons' data formats. [Envelope](/envelope/) is built using CBOR, while [URs](/ur/) and [MURs](/ur/mur/) can be used to encode CBOR as four-letter English words (or in minimalistic form, as two letters)._

## Why is CBOR Important?

We wrote [Why CBOR?](https://www.blockchaincommons.com/introduction/Why-CBOR/) to explain why we chose CBOR. 

There we said:

We chose CBOR as our serialization format choice for several key reasons:
<!--more-->

* **Structured Binary.** We required a structured, binary format. Many of the solutions we are concerned with involve cryptographic keys, signatures, and other forms of data best represented as binary. Text formats like JSON require additional encoding layers like Base-64, adding bulk and complexity, especially when you want to continue parsing down inside that data.
* **Conciseness.** We wanted the serialized structured data to be as concise as possible. Small structures should result in messages of no more bytes than reasonably necessary. This makes CBOR much better for us than formats such as BSON, for example, which has a surprisingly large serialization footprint, as it trades off conciseness for the ability to easily update it in place in a database.
* **Self-Describing.** We prefer a self-describing format. This means that the serialized data contains the associated metadata that describes its semantics. Self-describing formats can be schemaless, which makes sense in a world where both ends of a communication relationship are evolving rapidly. Like JSON, which is fundamentally schemaless, we also wanted the option to support formal schemas as needed while avoiding tying developers to specific schema processors or toolchains.
* **Fully Extensible.** We needed a system that is entirely extensible, with any data types desired, which ensures self-description even when you're working with a variety of data types, such as our own [listing of UR tags](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md).
* **Constraint Friendly.** We needed a format that works well in constrained environments, like special-purpose embedded systems and the Internet of Things. This means the codec implementations should be straightforward and efficiently implementable in a minimum number of lines of code.
* **Streaming Friendly.** We were pleased to also have a system that works well with streaming, without requirements for extra memory; CBOR's tagging system allows for data to be skipped if it is irrelevent or unknown.
* **Independent.** We required a format that is not closely tied to any particular hardware, software platform, or programming language.
* **Standardized.** We wanted a format that has had many experienced eyes on it, which means a format that has been through the standards process. The CBOR standard offers us a precise, exemplary specification and multiple reference implementations with test vectors. The fact that CBOR is an international standard through IETF also reduces resistance to adoption, increasing the likelihood of an active community of developers and projects relying on the code and tools that support the standard. CBOR is further standardized by the registration of common data types [through IANA](https://www.iana.org/assignments/cbor-tags/cbor-tags.xhtml), which multiplies these benefits.
* **Easy Adoption.** We wanted a format that is easy to implement and is platform- and language-agnostic. CBOR offers a mature tool chain so developers can quickly adopt it regardless of their hardware, software platform, or programming language. Implementation are available [in a variety of languages](http://cbor.io/impls.html).
* **Deterministic.** Of particular importance for our Gordian Envelope specification is that the CBOR standard defines a "deterministic encoding": a set of rules that ensure the encoded CBOR output is unequivocally unique. These rules mean that for a given semantic input, exactly one coding will always be output. This is essential for cryptographic techniques that rely on encoding the same data repeatably. A deterministic encoding avoids the need for post-processing to "canonicalize" data before it can be used in cryptographic constructs.

In summary, the choice of CBOR as the serialization format for Blockchain Commons allows for the development of technical specifications and tooling that are easily adopted by developers and can help solve common problems with decentralized, secure, private, and independent hardware and software. We've found that CBOR is the best choice for our needs, and we are confident that it will continue to serve our community of developers and supporters well into the future.

Also see [a comparison to Protocol Buffers](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md#qa), [a comparison to Flatbuffers](https://stackoverflow.com/questions/47799396/flatbuffers-vs-cbor), and [a comparison to other binary formats](https://www.rfc-editor.org/rfc/rfc8949#name-comparison-of-other-binary-) and of course [our video on this topic](https://www.youtube.com/watch?v=uoD5_Vr6qzw "Why CBOR?").

## How Does dCBOR Work?

[RFC 8949](https://datatracker.ietf.org/doc/html/rfc8949) fully details the methodology for encoding data in CBOR. In short: it's a data format that tags all of the data it contains.

## dCBOR Videos


<table width="100%">
  <tr>
    <td width="640px">
      <b>Why CBOR?</b>

{% include video id="uoD5_Vr6qzw" provider="youtube" %}

    </td>
  </tr>
</table>

## Links

**Intro:**

* [**dCBOR**](/dcbor/)
* [**RFC 8949**](https://datatracker.ietf.org/doc/html/rfc8949

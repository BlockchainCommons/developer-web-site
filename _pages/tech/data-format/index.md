---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataformat.jpg
  og_image: /assets/images/bc-card.jpg
title: Data Format Technologies
tagline: CBOR to Envelope
hide_description: true
classes:
  - wide
permalink: /data-format/
redirect_from:
    - /dataformat/
    - /dataformats/
    - /data-formats/
sidebar:
  nav:
    - technology
    - dataformat
---


<div class="hexline hexgrid71">
  <div class="hex11">
    <a href="/bytewords/">
      <img src="/assets/badges/bytewords.png">
    </a>
  </div>
  <div class="hex12top highlighted">
    <a href="/data-formats/">
      <img src="/assets/badges/cat-dataformat-half.png">
    </a>
  </div>
  <div class="hex21">
    <a href="/cbor/">
      <img src="/assets/badges/cbor.png">
    </a>
  </div>
  <div class="hex31">
    <a href="/dcbor/">
      <img src="/assets/badges/dcbor.png">
    </a>
  </div>
 <div class="hex41">
    <a href="/envelope/">
      <img src="/assets/badges/envelope.png">
    </a>
  </div>
  <div class="hex51">
    <a href="/known-values/">
      <img src="/assets/badges/known-values.png">
    </a>
  </div>
  <div class="hex61">
    <a href="/ur/">
      <img src="/assets/badges/ur.png">
    </a>
  </div>  
  <div class="hex71">
    <a href="/mur/">
      <img src="/assets/badges/mur.png">
    </a>
  </div>
</div>

_The Blockchain Commons technology stack includes a stack of data formats. CBOR (and variant dCBOR) are fundamental binary data serialization formats. Bytewords encodes binary objects as four-letter English words. URs and MURs turn Bytewords into self-describing data objects. Known values encode ontological concepts as unsigned integers. Finally, Gordian Envelope builds on all of that to offer a self-describing, recursive, smart-document storage format._

***Why?*** _Data formats are more than just wants to encode data. They also encode specific principles. Blockchain Commons' data formats embody the [Gordian principles](/principles/) in large part by ensuring that _open_ (and interoperable) as well as _resilient_ (and harder to confuse). They also strive to be deterministic on a variety of formats (which supports openness and interoperability) and efficient._

## ![](/assets/badges/cbor.png) CBOR

CBOR is an IETF data format ([RFC 8949](https://www.rfc-editor.org/info/rfc8949/)) that we have adopted as the fundamental data representation for higher-level Blockchain Commons data formats such as Gordian Envelope and URs.


For more see:

* [**CBOR**](/cbor/)



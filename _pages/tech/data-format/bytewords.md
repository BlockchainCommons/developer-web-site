---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataformat.jpg
  og_image: /assets/images/bc-card.jpg
title: Bytewords
tagline: Binary Encoded as Text
hide_description: true
classes:
  - wide
permalink: /bytewords/
sidebar:
  nav:
    - dataformat
    - technology
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

_Bytewords is a method for encoding binary objects as a sequence of
four-letter English words._

## Why is Bytewords Important?

Encoding binary strings as human-readable words allows for better
human interaction. Blockchain Commons adopted this approach not only
for improved human interactivity, but also so that CBOR data
structures could be encoded in a self-describing way, increasing their
resilience.

[BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)
and
[SLIP39](https://github.com/satoshilabs/slips/blob/master/slip-0039.md)
offered two existing methods for textual encoding of binary data, but
were ultimately found insufficient. Bytewords offers advantages over
existing schemes, including: use of words with a uniform length of
four letters; ability to minimally encode words with two letters;
addition of a checksum;and specific selection of "interesting" words
for improved memory.

Bytewords is used by the [Uniform Resources](/ur/) specification.

## How Does Bytewords Work?

[CBOR](/dcbor/) data is converted to a hex string and a CRC32 checksum
is appended to it. Each byte of the hex string is then converted to
one of the 256 words on Bytewords word list. For "minimal" encoding,
each byte is instead encoded as the first and last character of the
appropriate entry on the Bytewords list, which have been designed to
be unique.

["BCR-2020-012:
Bytewords"](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-012-bytewords.md)
lists the complete set of words and provides examples.

### How Does Bytemoji Work?

Words aren't the only ways to identify things! Blockchain Commons has also released "Bytemoji", a set of 256 emojis that can be used to generate visual strings. As with Bytewords, the Bytemojis were carefully curated to focus on emojis that are distinct, don't rely on color differences, read well at small sizes, and otherwise are easy to distinguish and remember. They were also selected to serialize as 3 or 4 UTF-8 bytes and to render as a single glyph 

Bytemojis are intended to be released as "clusters" of four or more bytemojis, which together can produce a strong recognition of a digital object:
```
🌊 😹 🌽 🐞
JUGS DELI GIFT WHEN
71 27 4d f1
```
["BCR-2024-008:
Bytemoji"](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-008-bytemoji.md)
lists the complete set of emojis and provides examples.

## Bytewords Videos


<table width="100%">
  <tr>
    <td width="640px">
      <b>Bytewords in Technical Overview</b>

{% include video id="RYgOFSdUqWY?start=973" provider="youtube" %}

    </td>
  </tr>
</table>

## Libraries

{% include lib-bytewords.md %}

## Links

**Developer Resources:**

* [**BCR-2020-012:
Bytewords**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-012-bytewords.md) (Blockchain Commons Research)
* [**BCR-2024-008:
Bytemoji**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-008-bytemoji.md) (Blockchain Commons Research)

**Playground:**

* [**BC-UR Playground**](https://irfan798.github.io/bcur.me/)

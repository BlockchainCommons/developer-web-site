---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Uniform Resources (UR)
hide_description: true
classes:
  - wide
permalink: /ur/
sidebar:
  nav: ur
  nav: urlayer
---

## Overview

The [Uniform
Resources](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md)
specification is a method for encoding structured CBOR binary data in
plain-text strings that are also well-formed URIs. It's usable with
any binary data, but was developed with Bitcoin and other
cryptocurrencies in mind.

To be precise, Uniform Resources (URs) include:

1. A standard way to wrap [CBOR-encoded data
structures](https://cbor.io/) in a URI.
2. A standard way to type the data in the URI so that it is self-describing.
3. A standard way split and sequence longer messages.
4. Optimizations for efficiency when URs are presented as QR codes.

URs allow for the self-identified encoding of a variety of
cryptographic data. This has been [widely
adopted](/ur/implementations/)
for use in PSBTs and is also available for many other types of data
such as seeds, keys, and shards, [all listed in a registry of data
types](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md). It's
optimized for airgapped usage and allows for standardized
interoperability for Bitcoin apps released by different companies.

## Why are URs Important?

URs are important because:

* **They're self-identifying.** Different methodologies for
transferring keys such as `xpub`, `ypub`, and `zpub` have proliferated
and caused confusion. Worse, they've created layer violations by
mixing encoding and policy. UR is a specification with more clearly
defined layers that could be expandable, yet still self-identify its
contents.
* **They provide strong layering.** Fundamentally, URs are a textual
(URI) encoding of a tagged CBOR structure.  Anything else is optional.
* **They're transport independent.** Though URs offer powerful support
for certain forms of transport, they're not directly linked to those
other layers: they can be used uniformly for URIs, QRs, NFCs, or other
transport methodologies, offering strong interoperability.
* **They allow easy conversion between binary and text.** URs establish a
correspondence between a CBOR tag, which is numeric, and a UR type string,
which is textual. Developers can then move the same CBOR structure back
and further between binary encoding (with the tag) and text encoding
(without the tag, but with the type string).

Depending on how URs are used, they can offer even more advantages.

* **They Can Offer Support for QRs.** While QR codes themselves are
standardized, the data encoded within QR codes is not, resulting in
inconsistent usage among developers. When used with QRs, URs resolve
these interoperability issues by creating a specified method for
encoding binary data using CBOR and by specifying [how to
sequence](/animated-qrs/) larger binary encoding (as version 40 QR
codes max out at 2,953 bytes), and they do so in a more compact way
than base64. See the [Multipart UR (MUR) Implementation Guide](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md) for more on multipart sequencing.
* **They Can Improve Human Factors.** If the [Bytewords](/bytewords/)
encoding is used with QRs, they become easy to visual and verify,
thanks to the careful selection of the Bytewords to ensure that
they're both unique and easy to remember and distinguish.
* **They Can Offer Support for Security.** Similarly, URs can offer
improved security if they are used with a transport mechanism such
as QR or NFCs, which enable airgaps. Since the transfer of key
material between devices is a prime point of vulnerability, this can
be a big gain.
* **They Can Make Multisigs Easy.** Multisig is the future of Bitcoin,
allowing for the creation of independent and resilient cryptocurrency
addresses. Previous specifications are locked into the single-sig
paradigm, while URs include specifications for a variety of data types
crucial to multisig use, and can also allow for the transport of the
larger PSBTs required by multisigs, thanks to its options for
[Animated QRs](/animated-qrs/).

> :bulb: _URs are used widely in the Gordian reference apps, but
community members have focused most on UR's sequencing feature to
create [animated QRs](/animated-qrs) that support PSBTs. URs can do a
lot more: they can support any airgapped Bitcoin function and more
than that, can support data encoding and transmission for a large
number of decentralized technologies whether they're airgapped or
not._

## How Does URs Work?

As detailed in the [UR
specification](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md),
URs are binary data that is represented with CBOR using a minimal
canonical and deterministic representation, converted to [minimal
bytewords](/bytewords/)
and prefaced with the UR type.

Thus the process of encoding a UR, which is largely automated by
Blockchain Commons libraries, is:

1. Refer to the [Registry of Uniform Resource
Types](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md)
for how to represent the desired data.
2. Refer to the [CBOR RFC](https://tools.ietf.org/html/rfc7049) for
how to encode the data.  In particular, be aware of how to [encode
major types and byte strings](https://tools.ietf.org/html/rfc7049#section-2.1).
Also, refer to [dCBOR](/dcbor/), as all URs must match the dCBOR profile.
   * The CBOR reference is the _best_ place to read about CBOR
     encoding, but be aware that whenever you encode something, you
     will typically preface data with one or more bytes showing data
     type and length; and as required you may also tag data.
   * Note that you will _not_ be prefixing your overall CBOR with a CBOR tag if
     you are using UR format.
3. Convert your complete CBOR binary representation to
[Bytewords](/bytewords/) using the minimal encoding. This is the first
and last letters of the Byteword, and will be done automatically if
you are using the Blockchain Commons [bytewords
libraries](/bytewords/#libraries) and
request `minimal` encoding
4. Prefix your UR with `ur:type/`, or in the case of a part of a
sequence `ur:type:sequence/`. Again, this will be done automatically
if you use a Blockchain Commons [UR
Library](https://github.com/BlockchainCommons/bc-ur).
5. If you will be placing your UR in a QR code, be sure to shift it to
ALL CAPS for best efficiency.

For example:

* **Seed:** 59F2293A5BCE7D4DE59E71B4207AC5D2
* **CBOR:** A1015059F2293A5BCE7D4DE59E71B4207AC5D2
   * `ur:crypto-seed` is defined as a map which must include the seed and which may include other data such as creation date.
   * The CBOR breaks down into `A1-01-50-59F2293A5BCE7D4DE59E71B4207AC5D2`.
   * `A1`represents a map of length 1.
      * That's major type 5 (for a map), which is represented as `101` in the most significant three bits, plus a length of 1, which is represented as `00001` in the least significant three bits, or overall `0b10100001`, which is `0xA1`.
   * `01` represents item 1 in the map.
   * `50` represents a 16-byte byte-string payload.
      * That's major type 2 (for a byte string), which is represented as `010`, plus a payload of 16 bytes, or `10000`, or overall `0b01010000`, which is `0x50`.
   * `59F2293A5BCE7D4DE59E71B4207AC5D2` represents the byte payload.
   * The CBOR prefix for a `crypto-seed` is not used since this will be used in UR format.
* **Bytewords:** obey acid good hawk whiz diet fact help taco kiwi gift view noon jugs quiz crux kiln silk tied help yell jade data
   * `obey` (`0xA1`) through `tied` (`0xd2`) represent the CBOR data, while `help yell jade data` are checksums.
* **Bytewords Minimal:** oyadgdhkwzdtfthptokigtvwnnjsqzcxknsktdhpyljeda
* **UR:** ur:crypto-seed/oyadgdhkwzdtfthptokigtvwnnjsqzcxknsktdhpyljeda
* **UR for QR:** UR:CRYPTO-SEED/OYADGDHKWZDTFTHPTOKIGTVWNNJSQZCXKNSKTDHPYLJEDA

Note that simplistic encoding of seeds as `ur:crypto-seed` is largely
being deprecated in favor of [Gordian Envelope](/envelope/). This
example is being maintained as a simplest-possible example of a use of
URs, but a corresponding Envelope would instead look as follows:

* **UR for QR:** UR:ENVELOPE/LPTPCSGDHKWZDTFTHPTOKIGTVWNNJSQZCXKNSKTDOYADCSSPOYBETPCSSECYHNENCYAHOYBDTPCSKPFYHSJPJECXGDKPJPJOJZIHCXFPJSKPHSCXGSJLKOIHOYAATPCSJSGHISINJKCXINJKCXJYISIHCXJTJLJYIHDMOLBWJOSO

```
Bytes(16) [
    isA: Seed
    date: 2021-02-24T09:19:01Z
    hasName: "Dark Purple Aqua Love"
    note: "This is the note."
]
```

## Bytewords Videos


<table width="100%">
  <tr>
    <td width="640px">
      <b>URs in Overview</b>

{% include video id="RYgOFSdUqWY?start=1198" provider="youtube" %}

    </td>
    <td width="640px">
      <b>Multi-part URs in Overview</b>

{% include video id="RYgOFSdUqWY?start=1373" provider="youtube" %}

    </td>
    <td width="640px">
      <b>Multipart UR Implementation Guide</b>

{% include video id="z9fmgCqMZvw" provider="youtube" %}

    </td>
  </tr>
</table>

## Libraries

{% include lib-ur.md %}

{% include lib-bytewords.md %}

## Links

**Intro:**

* [List of UR Implementations](/ur/implementations/)
* [**Multipart UR (MUR) Implementation Guide**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md)

**Developer Resources:**

* [**BCR-2020-005: UR Specification**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md) (Blockchain Commons Research)
* [**BCR-2020-006: Registry of UR Types**](
https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md) (Blockchain Commons Research)
* [**Other UR Research Papers**](https://github.com/BlockchainCommons/Research/tree/master) (Blockchain Commons Research)

* [**UR FAQ**](ur-faq.md)
* [**A Guide to Using URs for PSBTs**](/ur/psbts/) [our biggest success story]
* [**A Guide to Using URs for Key Material**](/ur/keys/)
* [**A Guide to Using URs for SSKR**](/ur/sskr/)
* A Guide to Using URs for Request & Response (new version pending)

* [**crypto-seed Test Vectors**](/ur/vectors/seeds/)
* Request & Response Test Vectors (new version pending)

**Developer Reference Apps:**

* [**Gordian Seed Tool**](https://github.com/BlockchainCommons/GordianSeedTool-iOS) (iOS)
* [**seedtool-cli**](https://github.com/BlockchainCommons/seedtool-cli) (CLI)
* [**URDemo**](https://github.com/BlockchainCommons/URDemo)






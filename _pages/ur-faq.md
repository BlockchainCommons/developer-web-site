---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: UR FAQ
hide_description: true
permalink: /ur/faq/
sidebar:
  nav:
    - ur
toc: true
toc_label: UR FAQ  
---

## General Questions

### What is an Airgap?

You can't ever be certain that a network or serial interface between
two devices won't lead to one of those devices corrupting the
other. Nor can you be certain that a networked connection is proof
from man-in-the-middle attacks. That leads to the need for airgaps,
where devices don't physically connect except "through a gap of
air". In recent years, QR codes have been a prime method of bridging
airgaps: they leverage the cameras and displays in the devices to
communicate. Airgaps can also be bridged by NFCs, MicroSDs, and other
methods that remove the real-time interactivity of a networked
connection.

URs are generally important for any type of interoperability between
devices, because their self-identifications makes it easy for a
receiving device to know what they're getting. However, they're
particularly important when QRs are being used to communicate through
an airgap, because of UR's support for QRs and particularly for
Animated QRs.

### Who Supports the Use of Airgaps?

Blockchain Commons' development of airgap specifications is not just
the product of our work, but also cooperation with other Bitcoin
wallet companies to create digital formats, specifications, and
reference apps that support new ways to protect your digital
assets. This discussion happens primarily in the [Gordian Developer
Community](https://github.com/BlockchainCommons/Gordian-Developer-Community/discussions).

### What Can Be Encoded in URs?

*Any* data can be encoded as URs as long as it has a CBOR encoding and
a user-defined UR type. The [Registry of Uniform Resource
types](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md)
lists data types that Blockchain Commons specifies, maintains, and
promotes. You can also define proprietary [user-defined
types](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md#user-defined-types-x-).

To date, the major uses of URs have fallen into three categories:

* **PSBT Signing.** URs can transfer PSBTs as they are being signed.
* **Key Transfer.** URs can encode seeds and HD keys.
* **SSKR Shares.** URs can encode shards of a key or seed sharded by SSKR.
 
When data is being transferred between airgapped apps, it is usually
done with an [animated QR](/animated-qrs/) or else a [Gordian Envelope](/envelope/), possibly part of a
[request-response interaction](/envelope/request/).

## Encoding Questions

### Why Not Use Base64?

URs were specifically designed to support efficient usage in QRs,
and that's one of their prime advantages over base64. When a UR is
properly stored as upper-case letters, it can be encoded in a QR using
the "alphanumeric" encoding mode, while base64 requires "binary"
encoding mode. base64 thus wastes at least two bits per character when
used in QRs.

### What Does CBOR Have to Do with URs?

URs are constructed by encoding the data in CBOR using specific methodologies described in the [UR registry](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md#registry), then converting that to [ByteWords](/bytewords/), as described in ["How Do URs Work"](https://developer.blockchaincommons.com/ur/#how-do-urs-work).

### What Tools Can I Use to Understand CBOR?

Obviously, the most important tool is the [CBOR
reference](https://tools.ietf.org/html/rfc7049), with our [dCBOR
profile](https://datatracker.ietf.org/doc/draft-mcnally-deterministic-cbor/)
trailing close behind.

CBOR has a more human-readable text [diagnostic
notation](https://datatracker.ietf.org/doc/html/rfc7049#page-33) you
should become familiar with. When you are testing your understanding
of how CBOR encoding works or debugging, you can use the [CBOR
Playground](http://cbor.me/) to transform CBOR between hex and
diagnostic notation, or if you prefer a command-line implementation,
use the [CBOR-cli](https://www.npmjs.com/package/cbor-cli), which can
be installed with `npm` if you have Node.js installed.

Specifications for CBOR structures are written in the [Concise Data
Definition Language
(CDDL)](https://datatracker.ietf.org/doc/html/rfc8610).

The [bytewords
CLI](https://github.com/BlockchainCommons/bytewords-cli) can also be
of use, since CBOR is converted to Bytewords for text encoding when
constructing URs.

## Multipart URs

### What is a Multipart UR (MUR)?

A Multipart UR (MUR) is a UR that has been broken into multiple parts and sequenced. Each one includes a sequence number and a sequence length, as described in the [UR specification](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md#ur-encoding). For example:
```
ur:seed/1-3/lpadaxcsencylobemohsgmoyadhdeynteelblrcygldwvarflojtcywyjydmylgdsa
```
### How Do MURs Relate to Animated QRs?

MURs are primarily intended for usage in Animated QRs, since QR codes have a low limit on how much data they can contain. But, you don't want to just keep repeating the same sequence of QRs when you're displaying an Animated QR because if the recipient misses a single element in the sequence you have to repeat every single one. Instead, Blockchain Commons uses fountain codes, which allow for more efficient reading of the animated QRs. How to do so is described in the [Multipart UR (MUR) Implementation Guide](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md).

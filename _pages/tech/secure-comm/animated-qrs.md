---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Animated QRs
hide_description: true
classes:
  - wide
permalink: /animated-qrs/
sidebar:
  nav: animated-qrs
---

## Overview

<a href="/ux-stack"><img src="/assets/images/bc-stack-ux-animated-qrs.png" width="25%" style="float: right; margin-left: 20px;"></a>

**Animated QRs** are an interoperable specification for transmitting
data that allows for the transmission of relatively large amounts of
data (compared to standard QRs).

## Why are Animated QRs Important?

Animated QRs were created with the following goals:

* To allow for the transmission of data across airgaps.
* To support the conveyance of more data than allowed by a static QR.
* To especially enable PSBT signing that is done on an unconnected device.
* To ensure interoperability among many vendors by the use of [Uniform Resources](/ur/).
* To resolve [The NASCAR Problem](https://indieweb.org/NASCAR_problem) in wallets.

See the [UR docs](/ur/) for more information on the foundational data
format that allows for the creation of animated QRs.

### Airgap Usage

<img src="https://developer.blockchaincommons.com/assets/images/airgap.png" align="right" width=100px>

QRs came into use in the digital-asset community because they could be
utilized in airgap architectures. This allowed for the storage of
sensitive data (such as seeds and keys) in a device not directly
connected to the internet, such as a hardware wallet or (to a lesser
extent) a phone. A coordinator linked to the internet could then
communicate with the disconnected device by QRs, without any sort of
direct connection.

One of the earliest use cases for airgaps was Partially Signed Bitcoin
Transactions (PSBTs). A networked coordinator could create a PSBT and
then send it to the disconnected device for signing. After a PSBT was
totally signed it could be returned to the networked coordinator for
transmission to the Bitcoin network.

_Why Airgaps?_ Airgaps allow users to keep critical data in a location
that's not connected to the internet, protecting them from most sorts
of theft and hacking, which tend to rely on internet-connected
devices. By usings QRs to constrain communication with the airgapped
device, security is greatly improved, and even if some overflow attack
were possible, the requirement for a user to carry data back and forth
across the airgap would greatly reduce the choices of an exploit. This
means that even if a device coordinating a transaction were corrupted,
the keys wouldn't be, and even if a transaction were produced
incorrectly, a user could see this on their signing device.

_Why PSBTs?_ PSBTs are a perfect complement to airgaps because they
allow transactions to be created on an internet-connected device
(which knows about UTXOs) but signed on an airgapped device (which
knows about keys). They also become more important as multisigs are
increasingly used to protect digital assets, because PSBTs allow for
individual key holders to each sign on their own device, at their own
convenience.

### Size Problems

Unfortunately, QRs can only contain limited amounts of
information. Technically, they can hold up to 7089 numeric characters,
4296 alphanumeric characters, or 2953 bytes of binary data. The
reality is often lower than that, especially because large QRs pack
their data so tightly that they can easily become unreadable on
consumer devices such as phones or computer cameras.

This proved problematic for PSBTs because they often require the
transmission of larger amounts of information, particularly as more
signers were added. Solving this problem required a new type of QR:
the animated QR, which transmits several QR frames that together
comprise the entirety of a large set of digital data, such as a PSBT.

### Adoption Problems

Prior to Blockchain Commons' release of Animated QRs, PSBTs could not
be reliably transmitted using QR codes. As a result of this need,
Blockchain Commons' Animated QRs have seen [wide
adoption](/ur/implementations/) in the wallet developer community.

The use of an interoperable specification for encoding, fragmenting,
sequencing, transmitting, recombining, and decoding data is what
allows Animated QRs to defeat the NASCAR problem. Already, software
wallets that don't use Blockchain Commons' specifications are building
up huge menus of hardware devices, with different interactions
required for each. Wallets using the Uniform Resources standard can
instead present a unified communication methodology, making
interaction with other devices effortless.

## How Do Animated QRs Work?

Blockchain Commons' Animated QRs are built atop the [Uniform Resource
(UR) specification](/ur/) to ensure their interoperability among many
wallets. URs are fundamentally self-describing URIs that package CBOR
data and that can also be fragmented and sequenced. This allows a much
larger data set to be broken into smaller segments and later to be
recombined, all in a carefully specified way. To create Animated QRs,
fragments are created that are each small enough to be encoded in a QR
that will be readable by average consumer devices.

### Fountain Code Tech

<img src="https://developer.blockchaincommons.com/assets/images/animated-qr.gif" align="right" width=100px>

Blockchain Commons' multipart Animated QRs are built from URs using
[Luby transform
codes](https://en.wikipedia.org/wiki/Luby_transform_code) (fountain
codes). This rateless encoding method allows for the receipt of an
animated QR to begin at any frame and for the transmission to overcome
issues with missing frames.

Our [**Multipart UR (MUR) Implementation Guide**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md) described how to implement the fountain codes underlying Animated QRs.
This process is also detailed in our references:
[URKit](https://github.com/BlockchainCommons/URKit), a reference
framework that allows for the encoding and decoding of URs in Swift;
and [URDemo](https://github.com/BlockchainCommons/URDemo), an iOS demo
that shows how Animated QRs built on URs can be sent and received.

## Animated QR Video

<table width="100%">
  <tr>
    <td width="640px">
      <b>Animated QRs Overview:</b>

{% include video id="HsFF5HPKQIk" provider="youtube" %}

    </td>
    <td width="640px">
      <b>Multipart UR Implementation Guide:</b>

{% include video id="z9fmgCqMZvw" provider="youtube" %}

    </td>    
  </tr>
</table>  

## Libraries

{% include lib-ur.md %}

## Links

**Intro:**

* [**UR Overview**](/ur/)
* [**Multipart UR (MUR) Implementation Guide**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md)

**Research Papers:**

* [**BCR-2020-005: UR Specification**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md) (Blockchain Commons Research)
* [**BCR-2020-006: Registry of UR Types**](
https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md) (Blockchain Commons Research)
* [**Other UR Research Papers**](https://github.com/BlockchainCommons/Research/tree/master) (Blockchain Commons Research)

**Playground:**

* [**BC-UR Playground**](https://irfan798.github.io/bcur.me/)
* [**Multi-UR & Animated QR Generator**](https://irfan798.github.io/bcur.me/#multi-ur)

**Developer Reference Apps:**

* [**Gordian Seed Tool**](https://github.com/BlockchainCommons/GordianSeedTool-iOS) (iOS)
* [**seedtool-cli**](https://github.com/BlockchainCommons/seedtool-cli) (CLI)
* [**URDemo**](https://github.com/BlockchainCommons/URDemo)

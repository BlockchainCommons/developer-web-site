---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-ux-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "LifeHash Use Cases"
hide_description: true
classes:
  - wide
permalink: /lifehash/use-cases/
sidebar:
  nav:
    - lifehash
---

LifeHash provides a beautiful visual representation of any digital
hash. It was designed as an adjunct method for recognizing
cryptocurrency keys and seeds, supplementing current computer
algorithms as well as visual methods for examining digital hashes, such
as looking at the first and last characters.

LifeHash is particularly useful for airgapped architectures, where
data is being passed from one device to another using QR codes (or
even by hand data entry). A LifeHash can help a user to quickly assess
that the data was the same on either side of the gap.

However, LifeHashes have many potential uses, as outlined here.

## LifeHashing Data

A LifeHash can be generated for *any* data, provided that it's first
SHA-256 hashed, then converted to a LifeHash. Possibilities include:

1. **Deep Links.**
1. **DIDs.**
1. **Private Keys.**
1. **Public Keys.**
1. **QRs.**
1. **Seeds.**
1. **URLs.** 

In all these situations, the goal is to show a LifeHash alongside a
non-human-readable piece of data, whether it be an identifier, a URL,
or a signature, and then to show it again at some future time: when
the data is displayed again, when it transferred to another device, or
when it used again. By verifying that the LifeHashes look the same,
perhaps matching a printed copy of the LifeHash, and by using that to
supplement other methods of comparing the data, the user can be
assured that the data is the one they intend to use and that it has
not changed.

***Example.*** A user receives a digital identifier (DID) for a
user. They can generate a LifeHash and check it against a trusted
public catalog of DIDs. After they put it in their address book, they
can later verify that LifeHash still looks the same when they interact
with the DID, to ensure that no one has changed the contents of their
address book since its entry.

***Example.*** A web site offers a QR code for sending it Bitcoins and
backs that up with a LifeHash. After reading the QR code, the user can
check the LifeHash on their own device to make sure it matches.

## LifeHashing Computations

LifeHashes may also be used to verify computations that might be
occurring on different devices:

1. **Calculations.**
1. **Checksums.**
1. **Signatures.**

In these situations, the goal is to check that the results on two
different devices match, without having to entirely depend on a
digital object such as a checksum or the applications that check those
objects. These calculations could have been made asynchronously (such
as checking a checksum that was created when a code distribution was
created) or synchronously (such as double-checking that two computers
have made the same calculation of a multi-sig address or even a bill).

***Example.*** After receiving a software distribution, a user uses
`sha256sum` to verify its provenance, but also visually compares the
LifeHash of the checksum with one presented on the website himself.

## Demonstrations

Remember that you can quickly LifeHash a UTF-8 string or a SHA-256
hash on [LifeHash.info](https://lifehash.info/).


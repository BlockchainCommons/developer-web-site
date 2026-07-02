---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-ux-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "LifeHash Best Practices"
hide_description: true
classes:
  - wide
permalink: /lifehash/best-practices/
sidebar:
  nav:
    - lifehash
---

LifeHashes can improve the security of digital data in conjunection
with other methods of inspection and hashing, but they must be
presented well, as discussed in the following best practices.

_Also see the [OIB Best Practices](/oib/best-practices/)._

## LifeHash Presentation

### Present LifeHash as a Square

**Don't** vignette or round the corners of a LifeHash image. Every
pixel contributes to the security of the image, so show the image as a
square. If you *really* want to round the corners, make the radius
small enough to still show the corner pixels.

### Present LifeHash Crisply

**Don't** interpolate or blur a LifeHash image: show every pixel
crisply. On iOS UIKit this is accomplished by setting
`layer.magnificationFilter = .nearest` on a `UIImageView`. Under
SwiftUI you call `myImage.interpolation(.none)`. The [Swift LifeHash
library](https://github.com/BlockchainCommons/LifeHash) already does
this for you.

## LifeHash Inclusion

### Use LifeHash as a Complementary Tool

The biggest challenge in using LifeHash is in determining how to
ensure the LifeHash that's being tested against is valid.

First, you need ways to ensure that a LifeHash wasn't created by a
hacker. If a hacker fakes a bit of data, and then makes a valid
LifeHash based on that data, then nothing is gained.

Second, you need to be aware that there will be ways to throw CPUs at
LifeHashes to produce near-matches, just as is the case with normal
hashes.

The best solution for these challenges is to consider LifeHashes one
tool in a larger arsenal. If you make sure you know the identity of
senders, through closely held devices, certs, or peer-to-peer
connections; and if you check other data such as the digital hash
itself; then you can increase our security through the examination of a
LifeHash as well.

## Library Usage

### Optimize Your Use of the Library

The [Swift LifeHash
library](https://github.com/BlockchainCommons/LifeHash) renders
LifeHash images asynchronously and caches the result, so if you pass
in the same fingerprint you'll get the same image back right away. If
LifeHash rendering seems slow, be sure you're compiling the Release
configuration of your target: LifeHash is *really fast* when compiled
for Release.


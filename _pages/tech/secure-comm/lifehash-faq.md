---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-ux-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Lifehash FAQ"
hide_description: true
permalink: /lifehash/faq/
sidebar:
  nav:
    - lifehash
toc: true
toc_label: Lifehash FAQ
---

## General Questions

### Is LifeHash Secure?

LifeHash is not a cryptographic hash. Anecdotally, it proves very
different visual images for even one bit's different in its
input. However, to prove its security to a cryptographic security
level, studies would have to be done to prove that its visual hashing
method were truly random and that visually similar hashes didn't
dramatically increase the collision rate.

LifeHash is instead a human-factor/UX hack: something meant to make it
easier for us, as humans, to deal with the security of cryptographic
systems. We know that humans aren't actually any good at reading the
long numbers that are the actual identifiers for cryptographic seeds
and keys. LifeHash offers a complementary identification method.

Practically, that means that LifeHash should always be used
_alongside_ other identification methods, such as digests. Our
[OIB](/oib/) system is meant to provide that additional
identification?

### How Many Colors Are Needed for Lifehash Display?

A special [Grayscale-Fiducial](/lifehash/versions/) format is
supported to allow more contrast on grayscale displays. Though color
displays are obviously preferable, LifeHash will offer meaningful
differences with grayscale.

### What Minimum Size is Required for LifeHash Display?

We have tested LifeHash on displays as small as 200x200 and gotten
good results. Some displays of LifeHash by some users have been even
smaller.

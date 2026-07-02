---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-ux-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "OIB Best Practices"
hide_description: true
classes:
  - wide
permalink: /oib/best-practices/
sidebar:
  nav:
    - oib
---

_Also see the [LifeHash Best Practices](/lifehash/best-practices/)._

## OIB Presentation

### Clearly Type Your Object

The OIB suggests that you use an icon to type an object, but it can be
beneficial to go beyond that. Consider if you want to use different
background colors, different formats, or other hints to show different
types of objects.

If a user is looking at a seed on one screen and a key on another, you
don't want them to think they might be the same thing!

## OIB Hashes

### Be Aware of What You're Hashing

When possible, use the cleanest, most original data that you can. Use
the [digest source
specifications](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2021-002-digest.md#the-digest-source-specification)
for seeds, keys, and anything else that has been specified, for
interoperability. Try to use original data over constructed data for
other representations.

### Differentiate Your Hashes When Possible

The default Blockchain Commons suggestions for OIB include two types
of hashes, a seven-digit "abbreviated digest" for the object and an
eight-digit master fingerprint (when appropriate).

This differential is appropriate and a good rule of thumb: if you have
different types of hashes as part of your OIB try to differentiate
them rather than displaying identical looking data that means two
different things.

### Be Aware of How You Abbreviate Hashes

We abbreviate our OIB hash to seven characters. Abbreviating to just
the first and last characters is another common technique. However, be
aware of what those characters represent. In some cases (such as
Bitcoin addresses) a leading character is always the same. Too much of
that, and your hash isn't meaningful!

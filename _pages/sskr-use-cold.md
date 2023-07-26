---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "SSKR Use Case: Cold Storage"
hide_description: true
classes:
  - wide
permalink: /sskr/use-case/cold/
sidebar:
  nav: sskr
---

[Smart Custody](https://www.smartcustody.com/) suggests maintaining
copies of keys etched in metal. This is an excellent use case for
SSKR, as it allows for the storage of key shares that don't allow for
the compromise of a key if an individual set of etchings is
discovered.  Ken Sedgwick demonstrated one way to do so using dogtags
and the [test vector](/sskr/vectors/).

{% include seed-128.md %}

Ken's complete method requires the following shopping list (with two
different options for the military embossing machine):
[https://gist.github.com/ksedgwic/d4d34cab504ac453eaf2a3656f86e2f6#file-dogtag-parts-md](https://gist.github.com/ksedgwic/d4d34cab504ac453eaf2a3656f86e2f6#file-dogtag-parts-md)

The overall procedure is simple:

1. Print out your SSKR as a guide.
2. Optionally, print a [new sheet](https://gist.github.com/ksedgwic/ce28cedd58b40fdb6522c905c07655e4) to divide each SSKR into two parts.
3. Emboss each share onto a pair of tags using the machine.

![](https://lh3.googleusercontent.com/pw/ACtC-3falQ_VKCz_s0-DWW9rUEX0jAWcMvFXfC4A7fHuNEIEsXN4ymbDctdeEMmm53GQPUgKaWSyCijbtQjnTbXEJN1P6UwWnZoGHXfjxVEpRSqt7qbPxXNA5wCDg_EEhfi51G_w5aYTmewlRsmINTFltkm2kw=w644-h1144-no?authuser=0)

You could stop there, but Ken suggests additional security:

4. Bolt each pair of tags against a blank tag to make them unreadable.
5. Dip each set of bolted tags in black plasti dip to bind them together.
6. Cover the edges of the dip with glitter to form a unique pattern that can't be replicated.
7. Photograph the patterns so that you can later test if the tags have been opened up.

![](https://raw.githubusercontent.com/BlockchainCommons/bc-sskr/master/images/dogtags.jpg)

Ken [has more photographs](https://photos.google.com/share/AF1QipMdrblk43sLJQeeMAcrT6AH9r9Peb9kENyAF6aflZbQD5Bqbyi8zSLegHg5akUYYg?key=WlJVd3ZLakI2Y0NoSnFiRkE1RjFCWks5Zjl0U1BB) of the entire procedure.

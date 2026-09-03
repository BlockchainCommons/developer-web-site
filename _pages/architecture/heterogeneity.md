---
cover: false
header:
  onnverlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Design Principles: Heterogeneity"
tagline: "Variation to Improve Security"
hide_description: true
classes:
  - wide
permalink: /architecture/heterogeneity/
redirect_from:
  - /heterogeneity/
sidebar:
  nav:
    - archdesign
    - architecture
---

_Heterogeneity is the design principle that security is improved by
things being different. This is true across many different
disciplines._

It might also be called _partitioning_. It encompasses both _variety_
(when things vary because they're different) and _separation_ (when
things vary because they're apart). This pattern has been used across
a wide number of architectural designs from Blockchain Commons. It's
helpful because it removes honeypots and single points of compromise,
reduces the danger of zero-day attacks, and otherwise improves the
security of whatever you're protecting.

## Heterogeneity Examples

* **Hardware Heterogeneity.** Separation is particularly useful for hardware: if it's in different places, it often can't be attacked simultaneously.
* **Software Heterogeneity.** Variety of software ensures that not everything is exposed to the same failure or attack. If one piece of software didn't generate secrets randomly, but your secret is based upon several different pieces of software, you're still somewhat safe.
* **Geographical Hetereogeneity.** Disasters often affects specific areas. If a war, earthquake, or fire occurs, and you've practiced geographical heterogeneity for computing resources, secret storage, or whatever, then you're more likely to be able to recover.
* **Secret Heterogeneity.** Secrets can be split up by means such as [SSKR](/sskr/), [CSR](/csr/), and [CKM](/ckm/). Doing so protects them from singular attacks or loss.

## Heterogeneity Links

* [**Authentication Design Patterns**](/architecture/patterns/auth/)

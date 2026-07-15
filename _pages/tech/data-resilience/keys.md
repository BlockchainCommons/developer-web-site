---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/images/tech-dataresilience.jpg
  og_image: /assets/images/bc-card.jpg
title: "Key Management"
tagline: "Keeping Keys Safe with Best Practices"
hide_description: true
classes:
  - wide
permalink: /keys/
sidebar:
  nav:
    - keymanagement
    - dataresilience
    - technology
---

<div class="hexline hexgrid71">
  <div class="hex21 opaqued">
    <a href="/ckm/">
      <img src="/assets/badges/ckm.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/csr/">
      <img src="/assets/badges/csr.png">
    </a>
  </div>
  <div class="hex32top">
    <a href="/data-resilience/">
      <img src="/assets/badges/cat-dataresilience-half.png">
    </a>
  </div>
  <div class="hex41 opaqued">
    <a href="/sskr/">
      <img src="/assets/badges/sskr.png">
    </a>
  </div>
  <div class="hex51 highlighted">
    <a href="/keys/">
      <img src="/assets/badges/key-management.png">
    </a>
  </div>
  <div class="hex61">
    <a href="https://www.smartcustody.com/">
      <img src="/assets/badges/smart-custody.png">
    </a>
  </div>  
  <div class="hex71 opaqued">
    <a href="/envelope/">
      <img src="/assets/badges/envelope.png">
    </a>
  </div>
</div>

_Key Management is a system for keeping your keys safe. It may be
supported by technologies such as [CKM](/ckm/), [CSR](/csr/), or
[SSKR](/sskr/), but it's also supported by the risk modeling found in
[Smart Custody](https://www.smartcustody.com/) and other best
practices, some of which are discussed herein._

## Why is Key Management Important?

There are no backups in the world of cryptographically secured digital
assets. Your key (or your seed) is the only way you can claim your
assets or manage your identity. If its gone, then the assets or
identity are effectively gone as well.

We have a whole [list of
adversaries](https://github.com/BlockchainCommons/SmartCustodyBook/blob/master/manuscript/03-adversaries.md)
as part of our [Smart Custody
book](https://www.smartcustody.com/#the-book), but many of them come
down to two fundamental security flaws:

* **Single Point of Failure (SPOF).** You have a single copy of the single key controlling your assets, and you lose it.
* **Single Point of Compromise (SPOC).** You have a single key controlling your assets, and someone steals it.

## How Does Key Management Work?

The fundamental solution to SPOFs and SPOCs is to ensure that the loss
or compromise of a single key doesn't cause the loss or compromise of
your assets. This is done by backing up keys in secure ways and by
creating scenarios where more than a single key is needed to control
an asset.

You can:

* Back up your key as shards that don't form SPOCs and that can be put
together with a smaller threshold to avoid SPOF. ([**SSKR**](/sskr/))
* Back up your shards to remote servers in an automated way. ([**CSR**](/csr/))
* Create a key as shards and use those shards for signatures without ever bringing the key together. ([**CKM**](/ckm/)

Even without these technologies, you can lean on [best
practices](/key-management/best-practices/) to help keep your keys
safe.

Some of these key management techniques can be burdensome, which is
why our [Smart Custody book](https://www.smartcustody.com/#the-book)
talks about risk analysis: this allows you to figure out which assets
require higher levels of key management and which do not.

## Links

**Key Management:**

* [**Best Practices**](/key-management/best-practices/)
* [**Learning Key Hierarchies (CLI)**](/key-management/cli/)

**Key Management Technologies:**

* [**Collaborative Key Management (CKM)**](/ckm/)
* [**Collaborative Seed Recovery (CSR)**](/csr/)
* [**SSKR**](/sskr/)

**Related Topics:**

* [**Smart Custody**](https://www.smartcustody.com/)

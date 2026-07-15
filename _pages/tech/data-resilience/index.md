---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataresilience.jpg
  og_image: /assets/images/bc-card.jpg
title: Data Resilience Technologies
tagline: Keeping Digital Assets Safe
hide_description: true
classes:
  - wide
permalink: /data-resilience/
redirect_from:
    - /dataresilience/
sidebar:
  nav:
    - dataresilience
    - technology
---


<div class="hexline hexgrid71">
  <div class="hex21">
    <a href="/ckm/">
      <img src="/assets/badges/ckm.png">
    </a>
  </div>
  <div class="hex31">
    <a href="/csr/">
      <img src="/assets/badges/csr.png">
    </a>
  </div>
  <div class="hex32top highlighted">
    <a href="/data-resilience/">
      <img src="/assets/badges/cat-dataresilience-half.png">
    </a>
  </div>
  <div class="hex41">
    <a href="/sskr/">
      <img src="/assets/badges/sskr.png">
    </a>
  </div>
  <div class="hex51">
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

_Data resilience has been a core of Blockchain Commons from the
beginning. Resilience is one of the [Gordian
Principles](/principles/): users need to have secure control of their
digital assets, including cryptocurrencies, identities, and registered
assets.  Data resilience generally falls under the rubric of [key
management](/key-management/) and our course [Smart
Custody](https://www.smartcustody.com/). Our stack meant to help
protect keys, seeds, and other secrets is built on [SSKR](/sskr/),
[CSR](/csr/), and [CKM](/ckm/)._

***Why?*** _Self-sovereign technologies depend on cryptography and
other trustless technologies to avoid centralization and to ensure
that you stay in control of your identity and assets. However, that
also means that there's no customer-service fallback: no one will help
you recover your identity or assets if you lose the seeds or keys that
control them. These data resilience technologies are about creating
technological fallbacks (and offering best practices and other advice
as well)._

## ![](/assets/badges/key-management.png) Key Management

**Best Practices.** How do you keep your keys safe? These best
practices discuss the topic, with examples using XID keys.

For more see:

* [**Key Management**](/key-management/)

## ![](/assets/badges/smart-custody.png) Smart Custody

**Course.** The Smart Custody course include the Smart Custody book,
which takes you step-by-step through the planning and mechanics of
securing cryptocurrency, and additional smart custody papers focusing
on SSKR, multisig, and other data resilience technologies.

For more see:

* [**Smart Custody Overview**](https://www.smartcustody.com/)
* [**Smart Custody Book**](https://www.smartcustody.com/#the-book)

## ![](/assets/badges/ckm.png) Collaborative Key Management (CKM)

**Resilient Key Storage.** CKM is a technology that will allow you to
create, maintain, and use a sharded key. It is available for exemplar
usage with [FROST](/frost/) signatures.

For more see:

* [**CKM**](/ckm/)

## ![](/assets/badges/csr.png) Collaborative Seed Recovery

**Resilience Seed Storage.** CSR is a methodology for storing and
recovering key shards from public shard servers.

For more see:

* [**CSR**](/csr/)

## ![](/assets/badges/sskr.png) SSKR

**Key Sharding.** SSKR is an expansion of Shamir's Secret Sharing,
that allows you to shard a secret and recover it from a subset of the
shares. (Our expansion also supports groups.)

For more see:

* [**SSKR**](/sskr/)

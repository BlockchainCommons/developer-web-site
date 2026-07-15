---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/images/tech-dataresilience.jpg
  og_image: /assets/images/bc-card.jpg
title: "Collaborative Key Management (CKM)"
tagline: "Keeping Keys Safe with SMPC"
hide_description: true
classes:
  - wide
permalink: /ckm/
sidebar:
  nav:
    - ckm
    - dataresilience
    - technology
redirect_from:
  - /ckm/overview/
---

<div class="hexline hexgrid71">
  <div class="hex21 highlighted">
    <a href="/ckm/">
      <img src="/assets/badges/ckm.png">
    </a>
  </div>
  <div class="hex31">
    <a href="/csr/">
      <img src="/assets/badges/csr.png">
    </a>
  </div>
  <div class="hex32top">
    <a href="/data-formats/">
      <img src="/assets/badges/cat-dataresilience-half.png">
    </a>
  </div>
  <div class="hex41 opaqued">
    <a href="/sskr/">
      <img src="/assets/badges/sskr.png">
    </a>
  </div>
  <div class="hex51">
    <a href="/keys/">
      <img src="/assets/badges/key-management.png">
    </a>
  </div>
  <div class="hex61 opaqued">
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

_Collaborative Key Management (CKM) is a Blockchain Commons
architecture for generating, storing, and using keys in a
collaborative way, where several different devices or entities come
together and use their individual secrets to generate and regenerate
keys solely for the brief instant that they're needed. Keys do not
exist except when they're being used!

_CKM expands upon [Collaborative Seed Recovery (CSR)](/csr/), our
existing architecture (begun in 2019 with [SSKR](/sskr/), then
expanded to the wider CSR architecture in 2022), which supports the
resilience and recovery of seeds (not keys) using well-reviewed
cryptographic code. Whereas CSR used sharding techniques to allow for
the recovery of techniques, CKM uses similar fragmenting of keys
during their usage!_

_CKM is still a future project, but these current pages are intended
to create a touchstone for the current CSR work, so that it's
appropriately future-proofed against current CKM development._

## Why Is CKM Important?

Digital assets are an emerging asset class in the 21st century
economy. Unlike physical assets, they require a secure digital custody
solution. Unfortunately, current solutions either tend to depend on
the support of a centralized exchange, which has the danger of
creating a Single Point of Compromise, or depend on a user to secure
their own keys, which has the danger of creating a Single Point of
Failure.

CKM resolves the current problems of centralization and of
self-custody solutions by providing a new methodology for sharing out
the secrets necessary to create a key. The result is not endangered by
the lossage possibilities of self-custody or the censorship and
compromise dangers of third-party custody. It also improves on CSR by
creating a system much less prone to compromise and incrementally less
vulnerable to censorship.

_See [CKM Architectural Overview](/ckm/architecture) for more
information on both general and specific problems solved by CKM._

## How Does CKM Work?

CKM depends on SMPC (Secure Multi Party Computation). Its ultimate
goal is to use secrets held by Secret Servers on the internet to
generate keys to be used for a variety of services. No Secret Server
individually has enough key material to access digital assets; instead
those keys are only generated just as they're needed by the Secret
Servers working together.

[ZF Frost](/frost/) became the first example of a production-ready
public implementation of CKM, following its security review in
2023. Its [Distributed Key
Generation](https://frost.zfnd.org/tutorial/dkg.html) demonstrates how
keys are generated in this manner.

## Links

**Intro:**

* [Architectural Overview](/ckm/architecture/)

**Components Intro:**

* [Envelope](/envelope/)
* [SSKR](/sskr/)
* [CSR](/csr/)
   * [Attachments](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-006-envelope-attachment.md)
* [Schnorr](https://www.blockchaincommons.com/musings/Schnorr-Intro/) (blog post)
* [ZF FROST](https://frost.zfnd.org/frost.html) (ZF FROST Docs)
  
**Use Cases:**

* [CKM Comparative Use Cases](/ckm/use-cases/)
* [CSR-Focused Progressive Use Cases](/csr/use-cases/)

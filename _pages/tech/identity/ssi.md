---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-identity.jpg
  og_image: /assets/images/bc-card.jpg
title: Self-Sovereign Identity
tagline: Identity You Control
hide_description: true
classes:
  - wide
permalink: /ssi/
sidebar:
  nav:
    - identity
    - technology
---

<div class="hexline hexgrid72">
  <div class="hex11 highlighted">
    <a href="/ssi/">
      <img src="/assets/badges/ssi.png">
    </a>
  </div>
  <div class="hex12top">
    <a href="/identity/">
      <img src="/assets/badges/cat-identity-half.png">
    </a>
  </div>
  <div class="hex21">
    <a href="/xid/">
      <img src="/assets/badges/xid.png">
    </a>
  </div>
  <div class="hex31">
    <a href="/identity/attestations/">
      <img src="/assets/badges/attestations.png">
    </a>
  </div>
  <div class="hex32 opaqued">
    <a href="https://learningxids.blockchaincommons.com/">
      <img src="/assets/badges/learning-xids.png">
    </a>
  </div>
  <div class="hex41 opaqued">
    <a href="/identity/fair-witness/">
      <img src="/assets/badges/fair-witness.png">
    </a>
  </div>
  <div class="hex51 opaqued">
    <a href="/cliques/">
      <img src="/assets/badges/cliques.png">
    </a>
  </div>
  <div class="hex52 opaqued">
    <a href="/garner/">
      <img src="/assets/badges/garner.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/clubs/">
      <img src="/assets/badges/clubs.png">
    </a>
  </div>  
  <div class="hex71">
    <a href="/identity/ppp/">
      <img src="/assets/badges/ppp.png">
    </a>
  </div>
</div>

_Self-sovereign identity (SSI) is identity that preserves your
autonomy and your control.  You hold your identity, and you can move
your identity among services. You interact with other entities as a
peer. You can make claims about your identity and about other
entities, and they can make claims about you. Ultimately, though, you
get to decide which claims are tightly attached to your identity, and
which are not._

_W3C's [DID v1.0](https://www.w3.org/TR/did-1.0/) was an attempt to
create a specification for SSI that we contributed to. [XIDs](/xid/)
are a newer take on SSI that we think conforms better to the [original
intent](https://www.lifewithalacrity.com/article/the-path-to-self-soverereign-identity/)
of the self-sovereign identity movement._

## Why is Self-Sovereign Identity Important?

If your digital identity is controlled by a corporation (like Meta) or
by a government (like the USA), then you are ultimately a digital
serf. You don't control the persona that you create online, and it
could be taken from you in a moment. All your social networking, all
your reputation building, and all your permissions on systems, in
repos, and in communities could be gone in an instant. This matters,
because the digital world is becoming the public square of the 21st
century. If it's taken from you, many opportunities are snatched away
as well.

Self-sovereign identity ensures that can't happen by advocating for an
identity that you can create, hold, and modify without external
control. You decide how your identity is presented, who gets to see
it, and what it contains. This grants you the same rights and ability
to represent yourself online that you have in the physical world, and
so ensures that you don't lose rights as the reach of the digital
world increases.

## How Does Self-Sovereign Identity Work

The [2016 introduction of
SSI](https://www.lifewithalacrity.com/article/the-path-to-self-soverereign-identity/)
introduced ten principles to define self-sovereign identity:

1. **Existence.** Users must have an independent existence.
2. **Control.** Users must control their identities.
3. **Access.** Users must have access to their own data.
4. **Transparency.** Systems and algorithms must be transparent.
5. **Persistence.** Identities must be long-lived.
6. **Portability.** Information and services about identity must be transportable.
7. **Interoperability.** Identities should be as widely usable as possible.
8. **Consent.** Users must agree to the use of their identity.
9. **Minimalization.*8 Disclosure of claims must be minimized.
10. **Protection.** The rights of users must be protected.

(These principles are being updated for 2026 as part of the
[Revisiting SSI](https://revisitingssi.com/) project.)

The technical specifics of SSI are less important than meeting the
needs of these principles. However, these principles can also be seen
in different ways, through different lenses. [Principal
Authority](https://www.blockchaincommons.com/dispatches/Principal-Authority/)
is another way to look at SSI: it says that a Principal has the
ultimate authority over their identity. At Blockchain Commons, we also
define SSI through architectural principles such as [The Gordian
Principles](/principles/) and the needs for [Data Minimization],
[Elision Cryptography], [Progressive Trust], and [Pseudonymous Trust
Building]. 

### The Blockchain Stack

A lot of Blockchain Commons' early work supported the safe and
resilient control of digital assets. However, that was done to
ultimately support the creation of a new generation of self-sovereign
identity. Here's how many of our technologies stack up to SSI (and
build atop it).

**Data Format Technologies:**

* [**CBOR**](/cbor/) - A concise, structured, self-identifying binary format.
* [**dCBOR**](/dcbor/) - A deterministic version of CBOR.
* [**UR**](/ur/) - A text encoding of CBOR.
* [**Envelope**](/envelope/) - A capstone technology built on dCBOR and storable as a UR. This data storage format provides elision and encryption abilities needed for robust SSI.

**Signature System Technologies:**

* [**Provenance Marks**](/provemark/) - A hash chain, which allows you
to have multiple versions of an identity and show which is the most
up-to-date.

**Data Resilience Technologies:**

* [**SSKR**](/sskr/) - A way to back up your identity's keys.
* [**CSR**](/csr/) - A way to collaboratively back up your identity's keys.
* [**SKM**](/ckm/) - A way to collaborative create an identity's keys.

**Identity Technologies:**

* [**XID**](/xid/) - An SSI model that is truly self-sovereign, built on Envelope & Provenance Marks, storable as URs.
* [**Cliques**](/cliques/) - Another model for SSI, this one built on relationships.
* [**Attestations & Endorsements**](/attestations/) - A way to add claims to an SSI.
* [**Fair Witness Methodology**](/fair-witness/) - A way to make claims more meaingful.
* [**Public Participation Profile**](/ppp/) - A model for pseudonymous identity, built with attestations and endorsements.

**Identity Communication:**

* [**Clubs**](/clubs/) - A self-sovereign way to share news or other information.
* [**Garner**](/garner/) - A self-sovereign way to share identity documents.

## Self-Sovereign Identity Links

**Introduction:**

* [**"The Path to Self Sovereign Identity" (2016)**](https://www.lifewithalacrity.com/article/the-path-to-self-soverereign-identity/) (Life with Alacrity)
* [**"Self-Sovereign Identity: 5 Years On" (2021)**](https://www.lifewithalacrity.com/article/SSI-5-Years-On/) (Life with Alacrity)
* [**"Principal Authority: A New Perspective on SSI" (2021)**](https://www.lifewithalacrity.com/article/Principal-Authority/) (Life with Alacrity)
* [**"The Origins of SSI" (2023)**](https://www.lifewithalacrity.com/article/origins-SSI/) (Life with Alacrity)
* [**Has Our SSI Ecosystem Become Morally Bankrupt?** (2024)](https://www.lifewithalacrity.com/article/ssi-bankruptcy/) (Life with Alacrity)
* [**Revisiting SSI (2026)**](https://revisitingssi.com/) (Website)

**The Dangers of Overidentification:**

* [**"Echoes from History: Designing Self-Sovereign History with Care" (2023)**](https://www.lifewithalacrity.com/article/echoes-history/) (Life with Alacrity)
* [**"Echoes from History II: The Dangers of eIDAS" (2023)**](https://www.lifewithalacrity.com/article/eidas/)

**Self-Sovereign Computing:**

* [**"Self-Sovereign Computing" (2023)**](https://www.lifewithalacrity.com/article/self-sovereign-computing/) (Life with Alacrity)

**SSI Models:**

* [**XIDs**](/xid/)
* [**Cliques**](/cliques/)
* [**DID v1.0 (2022)**](https://www.w3.org/TR/did-1.0/) (W3C)

**Self-Sovereign Use Case:**

* [**Amira 1.0.0 (2019)**](https://w3c-ccg.github.io/amira/) (W3C)
* [**"Amira Progress Report" (2026)**](https://www.blockchaincommons.com/articles/amira-update/) (blog)
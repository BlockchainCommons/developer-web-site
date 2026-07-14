---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-identity.jpg
  og_image: /assets/images/bc-card.jpg
title: Public Participation Profile
tagline: Building Pseudonymous Identity
hide_description: true
classes:
  - wide
permalink: /identity/ppp/
sidebar:
  nav:
    - ppp
    - identity
    - technology
---

<div class="hexline hexgrid72">
  <div class="hex11">
    <a href="/ssi/">
      <img src="/assets/badges/ssi.png">
    </a>
  </div>
  <div class="hex12top">
    <a href="/identity/">
      <img src="/assets/badges/cat-identity-half.png">
    </a>
  </div>
  <div class="hex21 opaqued">
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
  <div class="hex71 highlighted">
    <a href="/identity/ppp/">
      <img src="/assets/badges/ppp.png">
    </a>
  </div>
</div>

_Public participation profiles enable contributors to engage in
projects aligned with their values while maintaining control over
their personal information._

## Why are Public Participation Profiles Important?

For many individuals, participating in public projects creates a
fundamental tension. They want to make meaningful contributions to
projects aligned with their values and skills, but they desire
protection from surveillance, discrimination, retaliation, or unwanted
exposure that might come if their real-world identity were revealed.

Public participation profiles resolve this tension by creating
structured, verifiable, pseudonymous digital representations that
allow contributors to share what's necessary for trust and to improve
trust in the profile over time through peer endorsements while
protecting what needs to remain private.

## How Do Public Participation Profiles Work?

Public participation profiles require stable pseudonymous identifiers
that can be linked to [self-attestations and peer
endorsements](/identity/attestations/). [XIDs](/xid/) are built to
support public participation of this type, especially as defined in
the [Amira use case](https://w3c-ccg.github.io/amira/).

### 1. Stable Pseudonymous Identifier

The foundation of a public participation profile is a stable,
cryptographically-verifiable identifier, such as a XID, that doesn't
reveal real-world identity. It must provide consistent identity across
interactions and cryptographic verification of control of that identity
(via private key) without an inherent connection to real-world identity.

### 2. Self-Attestations

A public participation profile must have the ability for the holder to
make [self-attestations](/identity/attestations/) about themselves (or
at least about how they represent themselves in the identity).

This can include valuee statements that establish alignment without
revealing personal background and claims of baseline skills and
experience, again without specific identity disclosure.

Effective privacy-respecting value statements:

- Focus on universal principles rather than specific circumstances.
- Connect to project goals rather than personal background.
- Demonstrate alignment without revealing motivations.

Effective privacy-respecting self-attestations:

- Highlight distinctive capabilities without uniquely identifying details.
- Focus on relevant skills, not chronological history.
- Provide specific technical domains rather than job titles.
- Express experience in years rather than dates.

### 3. Self-Attestation Commitments

Some self-attestations might need to be hidden from the general public
while the identity still commits to them. This can be done with
evidence commitments, which allow contributors to log capabilities
without revealing sensitive information and to later reveal them
through selective disclosure to specific parties and/or through the
development of progressive trust.

A commitment is usually created by logging a claim than hashing that
claim and publishing the hash. If the claim is later revealed (either
publicly or privately), it gains weight because it can be dated back
to the publication date of the hash.

### 4. Peer Endorsements

A public participation profile must also support peer attestations,
which provide independent verification of claims and improve trust for
the profile while preserving pseudonymity. They will often come from
peers who have only interacted with the pseudonymous public
participation profile.

Effective peer attestations:

- Come from other trusted community members.
- Provide context about collaborative relationship.
- Verify specific skills or contributions.
- Include cryptographic signatures for verification.

## Public Participation Profile Links

**Introduction:**

* [**Amira Use Case**](https://w3c-ccg.github.io/amira/) (W3C)

**Public Participation Profiles:**

* [**PPP Risk/Reward**](/identity/ppp/riskreward/)
* [**PPP Life Cycle**](/identity/ppp/lifecycle/)

**Learning Public Participation Profiles from the Command Line:**

* [**Learning PPP**](/identity/ppp/cli/)
* [**Learning XIDs**](https://learningxids.blockchaincommons.com/) (Course)

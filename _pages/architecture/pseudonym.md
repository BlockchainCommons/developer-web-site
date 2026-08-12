---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Design Principles: Pseudonymous Trust Building"
tagline: "Building Confidence in an Identifier"
hide_description: true
classes:
  - wide
permalink: /architecture/pseudonym/
redirect_from:
  - /pseudonym/
  - /pseudonymity/
  - /architecture/pseudonymity/
sidebar:
  nav:
    - pseudonym
    - archdesign
    - architecture
---

_A pseudonym is a "fictitious name." On the internet, it's typically a
screen name or forum name or identity name that doesn't match your
real name._

_But how do you trust that pseudonym? Building trust traditionally
relies on real-world identity, credentials, and reputation. When
operating pseudonymously (using an identity that's not linked to your
real-world self), these traditional trust signals are unavailable._

_The design pricipal of Pseudonymous Trust Building says **Give
Identities the Tools to Bootstrap Trust**. It's based on our long-time
work to "Rebuild the Web of Trust," particularly as made concrete in
the [Amira use
case](https://www.blockchaincommons.com/articles/amira-update/), which
we created specifically to highlight the requirements of pseudonymous
identity._

## Core Principles of Pseudonymous Trust

1. **Work Quality Over Identity**: Let the quality of your work speak rather than your credentials.
2. **Verifiable Contributions**: Provide work that can be independently verified.
3. **Contextual Transparency**: Be open about methods, limitations, and biases.
4. **Progressive Evidence**: Build a consistent track record over time.
5. **Peer Validation**: Gather attestations from others in the community.

### Core Principles of Pseudonymous Verification

The key to pseudonymous trust is maintaining verification capabilities while preserving privacy:

1. **Evidence Without Identity**: Provide verifiable evidence without connecting to real identity.
2. **Minimal Disclosure**: Only reveal what's necessary for the specific context.
3. **Separate Contexts**: Use different pseudonyms for different contexts if needed.
4. **Cryptographic Verification**: Use signatures to prove consistent identity.
5. **Progressive Trust**: Reveal more information as relationships develop.

## Core Systems of Pseudonymous Trust & Verification

Pseudonymous Trust Building is accomplished through four major systems.

1. **Verifiable Attestations.** These are
[self-attestations](https://developer.blockchaincommons.com/identity/attestations/)
that are provable in some way.
2. **Peer Endorsements.** This is another part of the [attestation &
endorsement model](/identity/attestations/), where someone else makes a claim about you.
3. **Evidence Commitment.** This is a type of attestation that uses [elision
cryptography](/architecture/elision-cryptography.md).
4. **The Progressive Trust Life Cycle.** This [life
cycle](/architecture/progressive-trust.md) can be expanded through
constant iteration.

The philosophy of Pseudonymous Trust Building requires architecting
digital identity systems that support all four of these systems.

### Verifiable Attestations: Proving What You Say

The key to a verifiable attestation is making a claim that can be
proven (or at the least that can be supported). This is accomplished
in part through [fair witness
methodology](https://developer.blockchaincommons.com/identity/fair-witness/)
where you improve trust in your attestations by being clear about the
context and your biases. But it's also accomplished through careful
creation of claims.

In short, self-attestations shouldn't be vague and general ("I'm good
at security programming"), they should be specific and linked to
evidence ("I wrote the Padlock Security Program", with a link to
GitHub and a signature from a GitHub-recognized key). This lays the
foundation for Pseudonymous Trust Building.

### Peer Endorsements: Building a Network of Trust

As discussed in [Attestation & Endorsement
Model](/identity/attestations/), peers can make their own attestations
of you, to support your self-attestations. This is equally true for a
pseudonymous identity and a real-world identity.

In fact, the peer endorsements you get might be pseudonymous too: they
then are supported by their own self-attestations, by their own
evidence commitments, and by their own network of trust.

These attestations build a web of trust around your pseudonymous
identity without requiring you to reveal who you are.

### Evidence Commitments: Proving Without Revealing

Some claims may be too personal or too sensitive to publish.  Evidence
commitments use elision cryptography to commit to evidence without
revealing it prematurely. This is done by hashing data that contains
evidence.

Evidence commitments can be used to improve the trustworthiness of a
pseudonymous identity: you can state what evidence you've committed,
in case it's required at some future point without revealing it. This
approach lets you prove you made a claim at a certain time when you
released the hash (so it's not coming out of thin air when it's
convenient). Later, you can reveal that evidence as trust
[progressively](/architecture/progressive-trust/) develops.

### The Progressive Trust Model: Expanding the Life Cycle

Progressive Trust as described in [The Progressive Trust Life
Cycle](progressive-trust.md) may also be more important for a
pseudonymous identity than a real-world one, because peers of a
pseudonymous identity have nothing to go on except the trust that
gradually extends over time.

This goes further than a single life cycle. A single progressive trust
life cycle increases the trust between two entities as they
interact. That in turn can increase the trust between an entity and
the larger community as the life cycle repeats again and again for
individual interactions, because those interactions create the record
of a longer-term history.

## Pseudonymity in XIDs 

[XIDs](/developer/xid/) were built to support the core systems
required to support Pseudonymous Trust Building.

* **Verifiable Attestations.** Attestations of all sorts can be
created as "edges" in XIDs: a self-attestation is a link between the
XID and itself. As with all elements of Gordian Envelope, the edges in
XIDs can be recursively defined, allowing for the inclusion of
verifying data, such as URLs.
* **Peer Endorsement.** A peer endorsement is simply an edge
connecting two XIDs. This allows a verifier to step through to determine
a endorser's trustworthiness, through their own endorsements and
self-attestations.
* **Evidence Commitments.** An edge (or other data in a XID) can be
elided before the XID is published. That's because [Elision
Cryptography](/architecture/elision-cryptography/) is another
fundamental element of Gordian Envelope. Alternatively, an entire
envelope could be elided and its hash published on a web site as part
of a list of commitments.
* **Progressive Trust Model.** The elision in a Gordian Envelope can
be removed one datum at a time, allowing a perfect model for increasing
trust over time.

Specific examples of XID usage can be found in the [Learning
Pseudonyms from the Command Line](/architecture/pseudonym/cli/)
article.

## Pseudonymous Trust Building Links

* [**Learning Pseudonyms from the Command Line**](/architecture/pseudonym/cli/)

* [**Attestations & Endorsements**](/identity/attestations/)
* [**Fair Witness Methodology**](/identity/fair-witness/)


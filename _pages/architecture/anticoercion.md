---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Design Principles: Coercion Resistance"
tagline: "Protecting from Force"
hide_description: true
classes:
  - wide
permalink: /architecture/coercion-resistance/
redirect_from:
  - /coercion-resistance/
  - /coercionresistance/
  - /architecture/anti-coercion/
sidebar:
  nav:
    - archdesign
    - architecture
---

_It's hard to gain traction with "Privacy" features in the modern
world because most people think that it's not important if other
people can see their data. But the disconnect comes because Privacy is
just a tool, one intended to serve a greater good: coercion
resistance._

_Coercion Resistance is built on the precept that **people should be
able to make their own choices and take their own actions without
control imposed by another entity.** Privacy largely helps to support
this end._

_Most of Blockchain Commons' technologies are built to support
protection against coercion in a variety of ways._

## Coercion, Censorship, and Privacy

The concepts of coercion resistance, censorship resistance, and
privacy are somewhat closely related architectural philosophies, but
they can all three be explained under the rubric of the first.

* **Coercion Resistance** is the big tent. It says what's desired
(protecting individuals from force by other entities) and it
encompasses the other two.
* **Censorship Reistance** is a subset of coercion resistance: it
protects individuals from entities who might use force to disallow
them from communicating. But it doesn't speak to many other ways that
force can be leveraged against individuals.
* **Privacy** is a set of design patterns whose main goal is Coercion
Resistance. By protecting who you are and what you're doing, by using
pseudonyms and encrypting conversations, by minimizing data and
disclosing selectively, you're making it harder for other entities to
coerce and censor you, but it's ultimately a tool, not a goal:
coercion resistance is the goal.

## Censorship Resistance in Action

The philosophy of Censorship Resistance is threaded throughout
Blockchain Commons' technology.

### Censorship-Resistant Identity

When we first proposed [self-sovereign identity](/ssi/) in 2016, the
goal was to give users real control of their identity, so that it
couldn't be revoked by a centralized entity like Facebook or
Google. It [didn't work out that
way](https://www.blockchaincommons.com/dispatches/ssi-bankruptcy/), as
the [DID specification](https://www.w3.org/TR/did-1.0/) that emerged
left too much power in the hands of Issuers, who can coerce a user to
only share in certain ways or even to act in certain ways so that
their phone-home identity doesn't get revoked.

Our [XIDs](/xid/) offer a new solution that is truly
self-sovereign—but by self-sovereign, we really mean "designed to be
proof against coercion."

* For more, see [XIDs](/xid/)

### Coercion Resistant Identity Publication

Having an identity may not be enough. You need to be able to broadcast
that identity so that people can look it up and consider its
validating information. You also need to be able to publish updates to
your identity as it evolves over time. If your publication methods are
controlled by a third party, then they can coerce you even if your
identity is your own.

Unfortunately, this is a very common situation, as you may be lodging
an identity you own at a third-party site (e.g., a Mastodon server or
Bluesky) or you may be publishing it through an open web site (e.g.,
GitHub). Thosse resources may seem good now, but there's no guarantee
that they will continue to be available in the future.

Blockchain Commons' answer is Garner, which publishes identity files
over Tor using a public key as its address.

* For more see [Garner](/garner/)

### Coercion Resistant Assets

Blockchain Commons' first published works were [Learning Bitcoin from
the Command Line](https://learningbitcoin.blockchaincommons.com/) and
[Smart Custody](https://www.smartcustody.com/), both of which focused
on the use of Bitcoins. This wasn't due to allure of Bitcoin (or
blockchains generally): it was because we recognized blockchain-based
digital currencies as coercion resistant.

[Bitmark](https://bitmark.com/), one of our earliest patrons, offered
blockchain control of more types of digital (and even real-world)
assets—an area that has unfortunately been neglected since they were
forced to shut down. We've worked to offer some of our own
possibilities for recording control of digital assets with [provenance
marks](/provemark/).

Generally, pseudonymous digital assets combined with provenance chains
of control, offer two major sorts of coercion resistance:

* **Coercion-Resistance Assets.** With the exception of the 'ole
"wrench attack" (which is why privacy is an important
coercion-resistance tool), your assets can't be taken from you.
* **Coercion-Resistance Commerce.** In addition, unless the whole
community decided to block the usage of specific bitcoins, your
ability to purchase can't be blocked.

* For more, see [Learning Bitcoin](https://learningbitcoin.blockchaincommons.com/)
* For more, see [Smart Custody](https://www.smartcustody.com/).

### Coercion Resistant Communication

Though [Garner](/garner/) offers a specific solution for the
coercion-resistant communication of identity files, the general
concept of communication remains prone to coercion. Not only could
your email or IP traffic be blocked, but you could also be punished
for what you say (ranging from losing your job to being killed,
depending on what you said and where you live).

This demonstrates the strong need for coercion resistant
communication, where you can protect what you say and the fact that
it's you saying it. [Gordian Clubs](/clubs/) is Blockchain Commons'
most general method of protecting communication, because it wraps it
in an autonomous cryptographic object. [Hubert](/hubert/) can also be
used for communication, though its particular focus is algorithmic
communication (e.g., censorship resistant access to protocols like
FROST).

* For more, see [Gordian Clubs](/clubs/).
* For more, see [Hubert](/hubert/).

### Coercion Resistant Data

The question of communication can be viewed through another lens:
data. Can you move your data (including identity, assets, and
everything else) from one place to another? Or, is it locked in by a
proprietary format, or just a refusal to open software up to export in
standardized formats?

This is why some of Blockchain Commons' earliest specification work
was on developing standard ways to exchange information, including
[URs](/ur/), [animated QRs](/animated-qrs/), [dCBOR](/dCBOR/), and
ultimately [Gordian Envelope](/envelope/). These are ways to store and
transmit data in standardized, self-identifying ways that make
intetoperability a snap.

* For more, see [dCBOR](/dcbor/).
* For more, see [Uniform Resources](/ur/).
* For more, see [Animated QRs](/animated-qrs/).
* For more, see [Gordian Envelope](/envelope/).

### Coercion Resistant Apps

One of the hardest methods of coercion to deal with is baked into
applications you use. They're usually written to force you to do
certain things in certain ways. We're increasingly familiar with "dark
patterns," where for example social media companies use infinite
scroll to encourage addictive usage and where they juice their
algorithms to encourage conflict to keep people coming back. But the
mere fact of what's on a home page and what's buried three levels deep
on a Settings page will also force users to use applications in
certain ways.

The answer to this is likely not a specification and not a
particularly technology, but rather training users to recognize
coercive app designs and encouraging developers not to create them.

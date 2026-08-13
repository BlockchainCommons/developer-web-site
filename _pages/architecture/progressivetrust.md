---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Design Principles: Progressive Trust"
tagline: "Releasing Data over Time"
hide_description: true
classes:
  - wide
permalink: /architecture/progressive-trust/
redirect_from:
  - /progressive-trust/
  - /progressivetrust/
  - /architecture/progressivetrust/
sidebar:
  nav:
    - progressivetrust
    - archdesign
    - architecture
---

_Traditional digital systems often rely on centralized authorities and
binary trust decisions. Progressive Trust instead restores human
choice and agency by enabling nuanced, contextual trust evaluations
that can be adjusted based on new evidence and interactions. Its goal
is **to model how trust works in the real-world**, acknowledging that
trust develops gradually, evolves over time, and exists in shades of
gray rather than black and white._

## Real-world vs Online Trust

Traditionally, people came together in a medium where their [personal
data was innately minimized](/architecture/data-minimization/) and
gradually got to knew each other until they'd developed sufficient
trust to engage in some interaction for mutual benefit, which could be
anything from hiring someone as a contractor to marrying them. As
social psychologist James W. Pennebaker said, "Conversations are like
dances." It's a mature process that evolved over thousands of years.

Perhaps the internet was like that in its early days, when you met
someone on a MUD or engaged in a `talk` and learned more about them
over time. But when the internet became commercialized in the '90s,
powerful institutions brought new models for interactivity. They
gained economic benefit from _limiting_ what you could see. This
allowed them to simplify usability and to create their own controlled
communities, all of which increased their commercial viability, so
limit they did. That's the internet that exists today. Centralized
entities offer you restricted views of your fellows and provide binary
choices on silver platters: trust or not. There are no shades of gray,
there is no progression. Decisions are made by them, not us.

Web browsers offer an example. They tell us who to trust on the
internet, but they do so without nuance. They tell us sites that have
been able to acquire certificates, legimately or illegimately. They
don't tell us sites with a record of stability, they don't tell us
sites with a reputation for truth, and they definitely don't tell us
who to trust on those sites. This is just one of the places that
progressive trust can benefit the internet, for trust isn't black and
white: it exists in a world of gray.

Fortunately, the wheel is turning again. The modern internet offers
distributed and decentralized systems that put choice back into the
hands of individuals, allowing them to make decisions without the
coercion, censorship, and unbalanced power of centralized
systems. This allows the creation of systems where a user isn't served
up binary choices for trust based on limited information. Technologies
such as [data minimization and selective
disclosure](/architecture/data-minimization/) improve the situation
even more. They can further protect our human dignity and choice by
letting us choose to slowly (progressively) reveal our own information
over time. We just need to normalize these technologies and apply them
to human interactions that have been warped by the 21st century
corpocratic control of the internet. We need to apply them fully in
order to fully _trust_.

## Creating a Life Cycle

<a
href="/architecture/progressive-trust/life-cycle/"><img
src="https://www.blockchaincommons.com/assets/mermaid/progressivetrust.png"
width=200 style="float: right; border: 2px solid white; margin-left: 10px; margin-bottom: 5px;"></a>

One way to understand Progressive Trust is as a Life Cycle. Two
parties come into contact with each other, introduce themselves, and
then slowly learn about each other through what they say
(self-attestations) and what the community says (peer endorsements).

It they ultimately agree to interact in some meaningful way, they come
to an agreement and it's fulfilled, which could either lead to
feedback or else escalation and dispute.

### Creating Trust Networks

Another way to understand Progressive Trust is as a network. That's
because progressive trust doesn't exist in isolation. It forms a web
of trust relationships across different contexts:

```text
Person A -- trusts --> Person B   (in context X with trust level 0.8)
                    -- trusts --> Person C   (in context Y with trust level 0.6)
                    
Person B -- trusts --> Person D   (in context X with trust level 0.7)

Person C -- trusts --> Person D   (in context Y with trust level 0.9)
```

This means Person A might derive indirect trust of Person D through
different paths with different trust levels in different contexts.

## Building Assertions

Any Progressive Trust is ultimately based upon having a bucket of
assertions and revealing them over time. These might be
self-attestations, peer endorsements, or other sorts of claims. They
might be very structured, like attestations revealed in [XIDs](/xid/)
(including fair witness assertions, observation context, and
endorsements) or they might be freeform, like things revealed in
conversation.

### Implementing Selective Disclosure for Progressive Trust

Any structured set of claims can support [elision
cryptography](/architecture/elision-cryptography/) to enable
progressive trust: individual datums are elided, then revealed as
trust increases.

However, this requires careful design of the structure. Information
that is to be revealed at different stages of progressive trust must
be placed in atomic data holders that can be individually elided, and
they should be grouped into topics that could be elided as a
whole. For example, it's not very helpful to include a whole home
address in a single datum when the user might only want to reveal a
country, state, or zip code. This data should be divided up, but it
should continue to be categorized as a group so that either an
individual datum or the group could be redacted or revealed.

Selective disclosure is most often required when some of the
progressive trust information is private, but the overall information
packet, which might include agreement sign-offs or other signatures,
must be disclosed.

Examples of selective disclosure might include:

* Elision of initial assertions about an identity that might break privacy.
* Elision of collected references that might be prejudicial.
* Elision of details of approval or agreement that might be prejudicial to the counterparty.

In all of these cases, the data can be elided while maintaining
signatures, such as those offered for approval.

## Progressive Trust in XIDs

[Gordian Envelope](/envelope/) and [XIDs](/xid/) offer tools for
implementing progressive trust in digital systems. XIDs provide stable
identifiers for entities across trust networks while Gordian Envelope
enables context-specific trust assertions and selective disclosure.

To implement progressive trust using XIDs:

1. **Create XIDs for all entities** in your trust network.
2. **Define trust contexts** as specific domains of interaction (e.g., "code review," "document attestation").
3. **Use the life cycle phases** to track progression of trust in each context.
4. **Document trust assertions** using envelopes with appropriate attestations and endorsements.
5. **Implement selective disclosure** to reveal appropriate information based on trust level.
6. **Build trust scores** that adapt based on successful progression through the life cycle phases.
7. **Track trust history** to enable evolution of trust over time.

For more, see [Learning Progressive Trust from the Command
Line](/architecture/progressive-trust/cli/).

## Progressive Trust Links

* [**Progressive Trust Life Cycle**](/architecture/progressive-trust/life-cycle/)
* [**Learning Progressive Trust from the Command Line](/architecture/progressive-trust/cli/)

* [**Gordian Envelope**](/envelope/)
* [**XIDs](/xid/)

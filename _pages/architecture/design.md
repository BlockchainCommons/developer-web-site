---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: Architectural Design Principles
hide_description: true
classes:
  - wide
permalink: /architecture/design/
sidebar:
  nav:
    - archdesign
    - architecture
---

_Any architecture is built upon design principles. These are foundational ideas baked into the design of systems to have specific effects. Corporate-controlled software is often built on "dark patterns," which are meant to trick users into doing things that aren't to their benefit. The design principles used at Blockchain Commons are instead meant to be positive, improving security, resilience, and other [principles](/principles/) that we focus upon. Following are some of the design philosophies built into our software._ 

## General Architectural Principles

### Heterogeneity: Separation

Heterogeneity is a design philosophy that says: **Security is improved when things are different from each other.** It might also be called partitioning. It includes separation (when things **vary because they’re apart**). It’s helpful because it removes honeypots and single points of compromise: if your multisig keys are on separate, offline devices that are kept in discrete places, it becomes pretty hard to compromise your signature.

* For examples, see [**Authentication Design Patterns**](/architecture/patterns/auth/)

### Heterogeneity: Variety

Variety is another way to look at Heterogeneity. Instead of varying things by separating them, it instead **varies things by ensuring they're different**. This can reduce the danger of zero-day attacks and other exploits: if you have a multisig account, and one of the keys was created by a hardware device that used insufficient entropy, having another key built by another device becomes a big advantage.

* For examples, again see [**Authentication Design Patterns**](/architecture/patterns/auth/)

### Least & Necessary

Least Privilege is a security philosophy that says that **a person or program should have the minimum amount of access** that's needed for it to accomplish its task. Least Authority looks at that from a larger, ecosystem point of view, while Least Access says that the ability to read data should be minimized to what's needed. The flip side of these, Necessary Privilege, Necessary Authority, and Necessary Access instead view this philosophy from the bottom-up: what's needed to do a task? If you only sign things on a day to day basis, then you don't need to regularly use a key that also includes permission to change your identity: the result of a theft is then much less.

* For more, see [**"Musings of a Trust Architect: Least & Necessary Design Patterns"**](https://www.blockchaincommons.com/musings/Least-Necessary/)

## Identity Design Principles

### Data Minimization

Data Minimization is closely linked to the general principle of "Least & Necessary." It says that you should always release the minimum data necessary to fulfill a need. The classic example is purchasing an age-restricted item such as alcohol. You should never have to show a full identity credential (such as a driver's license, which is what in-person stores usually check). You shouldn't even have to reveal your age. All you should need to do is release a credential that says that you meet the age requirement.

* For more see the [**Data Minimization Page**](/architecture/design/data-minimization/)

### Elision Cryptography

Elision Cryptography is a design that links to the philosophy of Data Minimization. It says **Data Minimization can be accomplished by removing everything that's not needed in a way that maintains cryptographic signatures.** [Gordian Envelope](/envelope/) was built to accomplish this goal. As an example, you could have an mDL, and if it supported Elision Cryptography, you could display it with everything elided but the age and the state's signature. (In this particular example, it'd still be better to just prove you're of an appropriate age without having to reveal it.)

* For more see the [**Elision Cryptography Page**](/architecture/design/elision-cryptography/)

### Progressive Trust

In real life, you slowly reveal things to people over time. That's very different from the digital world, where revelation is often all-or-nothing thanks to a lack of Data Minimization. Progressive Trust says **you should be able to increase what you've revealed to someone as you get to know them better.** It's an ongoing process.

* For more see the [**Progressive Trust Page**](/architecture/design/progressive-trust/)

### Pseudonymous Trust Building

Digital identities need not link to a real-world identity. Pseudonymous Trust Building says that you should be able to create an unlinked pseudonym and **build credentials and trust for that pseudonym over time** through proven work and/or connections to a web of trust.

* For more see the [**Pseudonymous Trust Building Page**](/architecture/design/pseudonym/)

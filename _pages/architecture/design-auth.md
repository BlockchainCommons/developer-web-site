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

_Any architecture is built upon design principles. These are foundational ideas baked into the design of systems to have specific effects. Corporate-controlled software is often built on "dark patterns," which are meant to trick users into doing things that aren't to their benefit. These design principles are instead meant to be positive, improving security, resilience, and other [principles](/principles/) that Blockchain Commons focuses upon. Following are some of the design philosophies built into our software._ 

## General Architectural Principles

### Heterogeneity: Separation

Heterogeneity is a design philosophy that says: **Security is improved when things are different from each other.** It might also be called partitioning. It includes separation (when things **vary because they’re apart**). It’s helpful because it removes honeypots and single points of compromise: if your multisig keys are on separate, offline devices that are kept in discrete places, it becomes pretty hard to compromise your signature.

* For examples, see [**Authentication Design Patterns**](/architecture/patterns/auth/)

### Heterogeneity: Variety

Variety is another way to look at Heterogeneity. Instead of varying thing by separating them, it instead **varies things by ensuring they're different**. This can reduce the danger of zero-day attacks and other exploits: if you have a multisig account, and one of the keys was created by a hardware device that used insufficient entropy, having another key built by another device becomes a big advantage.

* For examples, again see [**Authentication Design Patterns**](/architecture/patterns/auth/)

### Least & Necessary

Least Privilege is a security philosophy that says that **a person or program should have the minimum amount of access** that's needed in a system for it to accomplish its task. Least Authority looks at that from a larger, ecosystem point of view, while Least Access says that ability to read data should be minimized to what's needed. The flipside of these, Necessary Privilege, Necessary Authority, and Necessary Access instead view this philosophy from the bottom-up: what's needed to do a task? If you only sign things on a day to day basis, then you don't need to regularly use a key that also includes permission to change your identity: the result of a theft is much less in the first case than the second.

* For more, see [**"Musings of a Trust Architect: Least & Necessary Design Patterns"**](https://www.blockchaincommons.com/musings/Least-Necessary/)

## Identity Design Principles




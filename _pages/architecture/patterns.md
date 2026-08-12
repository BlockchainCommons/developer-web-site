---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: Architecture Design Patterns
hide_description: true
classes:
  - wide
permalink: /architecture/patterns/
sidebar:
  nav:
    - archdesign
    - architecture
---

Any architecture requires the building blocks of design patterns. A
house isn't built from scratch as revealed by Christopher Alexander in
his book, _A Pattern Language_ (1977). Instead it's made up of
repeatable patterns such as "a place to wait", "a sitting circle", and
"open stairs". Technical systems are similarly made up of design
patterns: the
[architectures](https://developer.blockchaincommons.com/architecture/)
espoused by Blockchain Commons are full of them.

## Picking Your Design Patterns

Choosing design patterns is an art, not a science. Each design pattern
lays out specific _problems_ that you might face and _solutions_ to
resolve those problems, but you must ultimately decide which problems
are important enough to require solutions (and whether the solutions
are worth their trouble or not).

Two things in particular must be considered:

1. __Solutions Create New Problems.__ Any solution introduces new
problems to a system, some of which are listed in the _Disadvantages_
section of each pattern. Even beyond those, any pattern may introduce
more complexity to a system, reducing user ease of use. Using design
patterns means finding the appropriate balance for your system.
2. __No System is Perfect.__ No system will ever provide perfect
security. In fact, it could be that no system will provide even very
good security. When determining your balance, you thus shouldn't seek
to balance against perfection.

## Patterns vs Principles

These architectural pages broadly define architectural goals in two ways:

* [**Principles**](/principles/) tend to be strategic. They're
big-picture methodologies that accomplish general goals. They're
broadly stated.
* **Patterns** tend to be tactical. They're very specific gears that
one might place within the larger clockwork of of a design principle. For example, the [authentication](/patterns/auth/) patters that we use as examples are largely in support of the Heterogeneity principles.

## Design Pattern Examples

At current, we have one set of design pattern examples:

* [**Authentication Design Patterns**](/patterns/auth/)

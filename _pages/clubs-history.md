---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Clubs History
hide_description: true
classes:
  - wide
permalink: /clubs/history/
sidebar:
  nav: clubs
---

Gordian Clubs were inspired by [Project Xanadu](https://www.xanadu.net/) and their Clubs system.

> In <font color="orange">Xanadu</font> did Kubla Khan<br>
> A stately pleasure-dome decree:<br>
> Where Alph, the sacred river, ran<br>
> Through caverns measureless to man<br>
>   Down to a sunless sea.<br>
> &mdash;Samuel Taylor Coleridge

## The Original Vision: Xanadu's Club System

Ted Nelson's [Xanadu](https://www.xanadu.net/), often described as the hypertext system that predated (and arguably inspired) the World Wide Web, faced a fundamental challenge: how do you manage permissions in a global information network? The Xanadu team's solution came from Mark S. Miller, who invented the Club System. It addressed the issue of global permissions by incorporating both users and clubs (groups) within its permission structure, which spawned several innovations:

- **Clubs as first-class permission holders**: Both users and groups could hold and grant access rights.
- **Users as Clubs of One**: In fact, there was little differentiation: users were effectively groups of one.
- **Self-controlled membership**: Clubs (and users) could manage their own membership, offering read & write permissions.
- **Recursive hierarchy**: Clubs could contain other clubs, creating natural hierarchies without requiring delegation chains.
- **Inverted hierarchy**: Starting with a "public club" at the root made openness the default, with privacy the exception.

This was profound thinking for its time. It modeled how human organizations actually work, through groups, delegation, and collective decision-making. But it had a fatal flaw: control of access rights was managed administratively, meaning that it required centralization of servers and administrators as well as unconditional trust.

## The Missing Pieces Fall Into Place

In 1993, Blockchain Commons Principal Architect Christopher Allen suggested the idea of cryptographic clubs, where access rights were instead controlled by cryptographic objects such as private keys. Xanadu passed on the vision because of the lack of crucial technologies at the time, including:

* **Fast Public Key Operations**: RSA was too computationally expensive for widespread use. We needed efficient public key schemes.
* **Signature Aggregation**: Multiple signatures meant growing data structures. We needed compact proofs of group authorization.
* **Threshold Cryptography**: Clubs required group decision making that was not prone to single points of failure. We needed threshold signatures.

These technologies finally emerged in the 21st century, with mature encryption (such as IETF ChaCha20-Poly1305) and Schnorr signatures (such as [FROST](/frost/)) being two of the most critical components.The Blockchain Commons stack provides the rest of the [technologies](/clubs/technology/) required to develop cryptographic clubs, or as we call them: Gordian Clubs.

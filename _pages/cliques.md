---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Cryptographic Cliques & Edge Identifiers
hide_description: true
classes:
  - wide
permalink: /cliques/
sidebar:
  nav: cliques
---

## Overview

<a href="/core-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-core-id.png" style="float: right; margin-left: 20px;" width="25%"></a>

Traditionally, self-sovereign identity has focused on the Single Signature Paradigm: a unique private key is controlled by a unique individual (or entity), and that key is used to define the entity and to verify decision and commitments made by the entity. The cliques model instead recoqnizes identification as a relationship between two or more entities.

* **Relational Edges** are created between any two entities, with a private key jointly created using a [Schnorr-based signature scheme](https://www.blockchaincommons.com/musings/Schnorr-Intro/), and with the corresponding public key identifying the edge. Decisions and commitments can then be made jointly by the two members, using the key.
* **Cryptographic Cliques** (or closed cliques) are created after forming relational edges between *all* members of a group. The relational edges then jointly create a key pair that represents the clique as a whole.

The following example demonstrates a three-person (triadic) clique:

<center>
  <img src="https://www.blockchaincommons.com/images/cliques/cliques-3.png" width="40%" height="40%">
</center>
<br>

Three variations of clique can increase their power:

* **Open Cliques** don't require a connection between every member of the clique, allowing for organic evolution and expansion of a clique in a way that more closely matches the way that groups evolve.
* **Fuzzy Cliques** require the use of a threshold-based Schnorr signature scheme, such as [FROST](/frost/). They allow decisions and commitments to be made privately by subsets of a group.
* **Device Cliques** can include software or hardware elements such as oracles, wallets, fact checkers, and biometric verifiers. They can increase the power of cliques as the foundation for either groups or individuals.
 
The _Musings_ articles, ["Edge Identifiers & Cliques"](https://www.blockchaincommons.com/musings/musings-cliques-1/) and ["Open & Fuzzy Cliques"](https://www.blockchaincommons.com/musings/musings-cliques-2/) give far more details on these foundational descriptions of cryptographic cliques.

<center>
<table width="100%">
  <tr>
    <td width="50%">
      <b>Cliques Video:</b>

{% include video id="EFHvyrUj7Kk" provider="youtube" %}

    </td>
    <td width="50%">
      <b>Cliques Presentation:</b>
        <a href="https://developer.blockchaincommons.com/assets/pdfs/cliques.pdf"><img src="/assets/pdfs/cliques.jpg" style="border: solid 1px white;"></a>
    </td>
  </tr>
</table>
</center>
  
## Why Are Cliques Important?

Cliques recognize identification as a fundamentally relational activity. As such, they create the opportunity to model identity with a totally new eye, avoiding many of the pitfalls of traditional identity. Other advantages that are detailed in ["Edge Identifiers & Cliques"](https://www.blockchaincommons.com/musings/musings-cliques-1/#conclusion) include:

* Decentralized Identity Management
* Identity Validation
* Resilience Against Single Points of Failure
* Secure Group Decision Making
* Enhanced Privacy in Group Interactions

From the start, self-sovereign identity was intended to be [about individuals interacting with a larger world](https://www.blockchaincommons.com/musings/origins-SSI/). Some self-sovereign identity models have instead become too self-centric. Cliques offer a different way.

## Links

**Cliques:**

* ["Edge Identifiers & Cliques" (2024)](https://www.blockchaincommons.com/musings/musings-cliques-1/) (Musings)
* ["Open & Fuzzy Cliques" (2024)](https://www.blockchaincommons.com/musings/musings-cliques-2/) (Musings)

**Schnorr:**

* ["A Layperson's Intro to Schnorr" (2023)](https://www.blockchaincommons.com/musings/Schnorr-Intro/) (Musings)

**Self-Sovereign Identity:**

* ["The Path to Self-Sovereign Identity" (2016)](https://www.lifewithalacrity.com/article/the-path-to-self-soverereign-identity/) (Life with Alacrity)
* ["Self-Sovereign Identity: Five Years On" (2021)](https://www.blockchaincommons.com/musings/SSI-5-Years-On/) (Musings)
* ["The Origins of Self-Soveriegn Identity" (2023)](https://www.blockchaincommons.com/musings/origins-SSI/) (Musings)

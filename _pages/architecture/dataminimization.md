---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: Data Minization
tagline: 'Revealing Only What's Needed"
hide_description: true
classes:
  - wide
permalink: /architecture/data-minization/
redirect_from:
  - /architecture/dataminimization/
sidebar:
  nav:
    - dataminimization
    - archdesign
    - architecture
---

Data minimization is the practice of limiting the data you share to
only what's necessary for a specific purpose. It follows the
principle: **"Share what you must, protect what you can."**

## Beyond Anonymity and Pseudonymity

[Pseudonymous Trust Building](/architecture/pseudonym) is
another Identity [Design Principle](/architecture/design/), but
neither it nor a fully anonymous digital is sufficient in itself.
That's because anonomized data can be de-anonymized, while pseudonyms,
which accumulate data histories over time can eventually be linked to
real identities, especially if they contain coontextual information
(and if they reveal it unncessarily).

Data minimization addresses these limitations by reducing all data
shared to the minimum needed for each specific interaction.

### Elision as a Data Minimization Tool

Though zero-knowledge proofs and others sorts of more complex,
less-tried cryptography can be used to ensure data minimization,
Blockchain Commons suggests a simpler methodology, [Elision
Cryptography](/architecture/elision-cryptography).

This methodology is built into [Gordian Envelope](/envelope/), which
allows the selective removal of specific pieces of information while
maintaining the cryptographic integrity of the whole.

## Data Maximalism Risks

Data minimization is intended to resolve a number of human rights
risks with data sharing. It supports:

1. **Privacy**: The right to control what personal information is shared and with whom.
2. **Autonomy**: The ability to make choices without undue influence based on profiled data.
3. **Non-discrimination**: Protection from judgments made on irrelevant personal data.
4. **Security**: Reduced attack surface for identity theft and other harms.

### Privacy Risks of Excessive Data Sharing

Every piece of information shared increases potential privacy risks:

1. **Correlation**: When data from different sources is combined, it
can reveal far more than intended. Even seemingly harmless details can
complete a revealing puzzle about a person.
2. **Secondary Use**: Once data is shared, it may be repurposed beyond
its original intent, potentially in ways that harm the subject's
interests.
3. **Disclosure Risks**: Sharing excessive data can create prejudice
or disadvantage, particularly for marginalized individuals or
communities.
4. **Digital Permanence**: Unlike conversations that fade from memory,
digital data can persist indefinitely and be copied without limit.

## Data Minimization Links

* [**Data Minimization Best Practices**](/architecture/data-minimization/best-practices)
* [**Data Minimization Use Cases**](/architecture/data-minimization/use-cases)

* [**Gordian Envelope**](/envelope/)
* [**Elision Cryptography**](/architecture/elision-cryptography/)
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
permalink: /architecture/data-minimization/
redirect_from:
  - /architecture/dataminimization/
sidebar:
  nav:
    - dataminimization
    - archdesign
    - architecture
---

Understanding data minimization principles is one thing; implementing
them effectively is another. Here's how these principles translate
into practical action with Gordian Envelopes:

## Practical Implementation: Data Minimization

1. **Create a Complete Source Document**
   - Begin with a comprehensive envelope containing all possible information.
   - Use careful organization of assertions for later selective sharing.
   - Include both essential and contextual information.
2. **Identify Context-Based Sharing Requirements**
   - Define specific audiences and what each needs to know.
   - Create profiles for different sharing contexts (public, professional, trusted).
   - Determine what information is appropriate for each trust level.
3. **Implement Through Elision**
   - Sign documents before elision to maintain verifiability.
   - Use elision to create different views of the same document.
4. **Visually Indicate Data Minimization**

Using [Gordian Envelope](/envelope/) as an example, a professional
 profile shared in a public context would visually indicate elided
 content:

```
"BRadvoc8" [
   "name": "BRadvoc8"
   "publicKeys": ur:crypto-pubkeys/hdcx...
   "domain": "Distributed Systems & Security"
   ELIDED
   ELIDED
   ELIDED
]
```

The `ELIDED` markers make it clear to recipients that information has
been intentionally minimized rather than simply omitted. This
transparency builds trust by acknowledging the data minimization
process.

For the technical details of how elision works cryptographically, see
the [Elision Cryptography](elision-cryptography.md) document.

## Data Minimization Best Practices

1. **Purpose Analysis**: Clearly identify why information is being shared and what the minimum required is.
2. **Contextual Assessment**: Consider the specific audience and their legitimate need to know.
3. **Differential Disclosure**: Create multiple views of the same information for different contexts.
4. **Regular Review**: Periodically assess whether previously shared information should be updated or withdrawn.
5. **Transparency about Minimization**: Make it clear when information has been minimized to set expectations.

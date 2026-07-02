---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Known Values
hide_description: true
classes:
  - wide
permalink: /known-values/
sidebar:
  nav: envelope
redirect_from:
  - /known-value/
---

## Overview

Known Values are numbers drawn from a standardized namespace of 64-bit unsigned integers that represent ontological concepts, potentially across many vocabularies. Though they are used in [Gordian Envelope](/envelope/), they are available for use in any code that needs to represent concepts of this type.

## Why Are Known Values Important?

Known Values ensure that the same concepts are always identified in the same ways, whether it's a singular entity who is representing data in multiple places or many entities who are doing so.

* **Known Values Simplify Data.** The Known Values have already done the hard work of boiling down concepts to their core; a user just needs to pick the correct Known Value.
* **Known Values Make Data Deterministic.** The same data will always be represented in the same way.
* **Known Values Improve the Reliability of Data.** Without a specification, coder sloppiness can cause the same concept to be represented in different ways at different types. Known Values mean that you don't have to guess about whether two concepts were actually meant to be equivalent.
* **Known Values Support Interoperability.** Standardization for concept representation means that not only does each organization always represent the same data in the same way, but also that multiple organizations will represent the same data in the same way, creating interoperability potential for both their data and their applications.
* **Known Values Enhance Security.** With Known Values in place, the risk of URI manipulation is mitigated.

## How Do Known Values Work?

Known Values are integers that are recorded in a [registry](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-002-known-value.md#appendix-a-registry). Blockchain Commons has defined values in the [lowest range of the registry](https://github.com/BlockchainCommons/Research/blob/master/known-value-assignments/markdown/0_blockchain_commons_registry.md) and has also added a number of other ontologies, such as RDF, OWL 2, and Dublin Core elements. Community members may add their own values for things that they have specified by [submitting](https://github.com/BlockchainCommons/Research/blob/master/community-known-values/README.md) a JSON schema in a PR.

Each Known Value is simply a correlation of an integer with a canonical name (plus a bit of other information).

The following listing shows the first ten values at the time of this writing, but the [registry](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-002-known-value.md#appendix-a-registry) should be considered the master source.

| Codepoint | Canonical Name   | Type     | URI                                                   | Description                                                                                      |
| --------- | ---------------- | -------- | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| 0         | `''`             | unit     |                                                       | The Unit type, and its sole inhabitant '', which is a value conveying no information.            |
| 1         | `isA`            | property | http://www.w3.org/1999/02/22-rdf-syntax-ns#type       | The subject is an instance of the class identified by the object.                                |
| 2         | `id`             | property | http://purl.org/dc/terms/identifier                   | The object is an unambiguous identifier of the subject within a given context.                   |
| 3         | `signed`         | property |                                                       | The object is a cryptographic signature of the subject.                                          |
| 4         | `note`           | property | http://www.w3.org/2000/01/rdf-schema#comment          | The object is a human-readable note about the subject.                                           |
| 5         | `hasRecipient`   | property |                                                       | The subject can be decrypted using the private key that decrypts the content key in the object.  |
| 6         | `sskrShare`      | property |                                                       | The subject can be decrypted by a quorum of SSKR shares including the one in the object.         |
| 7         | `controller`     | property | https://www.w3.org/ns/solid/terms#owner               | The object is the subject's controlling entity.                                                  |
| 8         | `key`            | property |                                                       | The entity identified by the subject holds the private half of the public keys(s) in the object. |
| 9         | `dereferenceVia` | property |                                                       | The content referenced by the subject can be dereferenced using the object.                      |

When using a Known Value library, a developer simpler enters the canonical name (e.g., `hasRecipient`), and then the integer is encoded, providing a smaller, simpler recording of the data.

Envelope Notation in [Gordian Envelope](/envelope/) shows Known Values by their canonical name, in single quotes `'CANONICAL-NAME'`
```
 {
    XID(5f1c3d9e) [
        'dereferenceVia': URI(https://github.com/BRadvoc8/BRadvoc8/raw/main/xid.txt)
        'key': PublicKeys(a9818011, SigningPublicKey(5f1c3d9e, Ed25519PublicKey(b2c16ea3)), EncapsulationPublicKey(96209c0f, X25519PublicKey(96209c0f))) [
            'allow': 'All'
            'nickname': "BRadvoc8"
            ELIDED
        ]
        'provenance': ProvenanceMark(1896ba49) [
            ELIDED
        ]
    ]
} [
    'signed': Signature(Ed25519)
]
```
For example here, `derefenceVia`, `key`, `allow`, `All`, `nickname`, `provenance`, and `signed` are all Known Values. They're compactly encoded in the envelope, but are easily expanded to their canonical name when displayed to a user.

## Libraries

{% include lib-knownvalues.md %}

## Links

**Research Papers:**

* [**BCR-2023-002: Known Values**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-002-known-value.md)
* [**BCR-2023-003: Gordian Envelope Extension: Known Values**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-003-envelope-known-value.md)
* [**BCR-2026-001: Unit, the Known Value for Deliberate Emptiness**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2026-001-unit.md)

**Registration:**

* [**Known Value Registry**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-002-known-value.md#appendix-a-registry)
* [**Blockchain Commons Codepoints**](https://github.com/BlockchainCommons/Research/blob/master/known-value-assignments/markdown/0_blockchain_commons_registry.md)
* [**How to Submit Known Values**](https://github.com/BlockchainCommons/Research/blob/master/community-known-values/README.md)

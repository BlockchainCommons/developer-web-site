---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Envelope
hide_description: true
classes:
  - wide
permalink: /envelope/
sidebar:
  nav: envelope
redirect_from:
  - /envelope/overview/
---

## Overview

_**Latest News:** Released [GSTP](/envelope/gstp/) and [GSTP Tech Overview](/envelope/gstp/tech/) pages (8/20/24). Released [Undestanding Envelope](https://www.youtube.com/watch?v=-vcLCFKQvik), [Understanding Envelope Extensions](https://www.youtube.com/watch?v=uFxStP3ATkw), and [Understanding GSTP](https://www.youtube.com/watch?v=QnH14LkJOnI) videos (8/13/24)._
{: .notice--info}

<a href="/core-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-core-envelope.png" style="float: right; margin-left: 20px;" width="25%"></a>

Gordian Envelope is a specification for the achitecture of a “smart
document". It builds on the binary format of the [IETF CBOR standard](https://cbor.io/) to support the secure, reliable, and
deterministic storage and transmission of data such as seeds, keys,
decentralized identifiers, and verifiable credentials in a way that
enables privacy while preserving structure. The format is very simple
and compact, with minimal overhead, but thanks to its recursive design, documents can ultimately be as
complex as needed. Gordian Envelope's privacy features are built on a
Merkle-like Tree that supports cryptography and privacy-related
methodologies such as [progressive
trust](https://www.blockchaincommons.com/musings/musings-progressive-trust/)
and Merkle-based selective disclosure.

Blockchain Commons is currently working with multiple companies on the
development and deployment of Gordian Envelopes via regular monthly
meetings; [subscribe](https://www.blockchaincommons.com/subscribe/) to our announcements-only mailing list or join our Signal channel if you'd like to get involved. You can also
[contact us](mailto:team@blockchaincommons.com) if you'd prefer to get more information first.  

Envelope is also on the experimental track as an
[Informational Draft for the
IETF](https://blockchaincommons.github.io/WIPs-IETF-draft-envelope/draft-mcnally-envelope.html).
Further, ongoing discussions are occurring with the W3C Credentials
Community Group.

### The Envelope as Metaphor

The name "envelope" was chosen for this smart-document architecture
because that provides an excellent metaphor for its capabilities.

<figure class="half">
  <a href="/assets/images/envelope/envelope-canhold.jpg"><img src="/assets/images/envelope/envelope-canhold.jpg"></a>
  <a href="/assets/images/envelope/envelope-cando.jpg"><img src="/assets/images/envelope/envelope-cando.jpg"></a>
</figure>

These capabilities include:

* **Envelopes can have things written on them.** Plaintext parts of a
    Gordian Envelope can be read by anyone.
* **Envelopes can have routing instructions.** That plaintext
    information can include data on how to use the Gordian Envelope,
    such as how to open or close it.
* **Envelopes can contain things.** Things can be placed within the
    structure of a Gordian Envelope.
* **Envelopes can contain envelopes.** The Gordian Envelope structure
    is fully recursive: any part of an envelope can actually be
    another envelope.
* **Envelopes can have a seal.** A signature can be made for the
    contents of a Gordian Envelope, verifying their authenticity and
    that they haven't been changed.
* **Envelopes can be certified.** Beyond just guarding against
    changes, a Gordian Envelope signature can also act as a
    certification of the envelope's contents by some authority.
* **Envelopes can be closed.** Encryption allows any part of a Gordian
    Envelope to be protected from prying eyes.
* **Envelopes can have windows.** Selective disclosure allows for some
    parts of a Gordian Envelope to be readable while others have been
    redacted. Merkle proofs can proof that those parts were present in
    the original envelope.
* **Different recipients can open envelopes in different ways.** Just
    as people might use letter openers, their fingers, or a machine to
    open a normal envelope, special permits can grant people different
    ways to open a Gordian Envelope.

## Why Are Envelopes Important?

The Gordian Envelope is intended as a more privacy-focused encoding
architecture than existing data formats such as JWT and JSON-LD. We
believe it has a better security architecture than JWT and that it
doesn't fall victim to the barriers of canonicalization complexity
found in JSON-LD — which should together permit better security
reviews of the Gordian Envelope design.

However, new features of Gordian Envelope not available in JWT or
JSON-LD offer some of the best arguments for using the Smart Document
structure.

* For a more extensive overview of Gordian Envelopes, see [Executive Summary](/envelope/summary).
* For a list of Gordian Envelope features, see [Features List](/envelope/features).
* For a technical look at Gordian Envelope, see [Technical Overview](/envelope/tech).

## Envelope Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>Understanding Envelopes I:</b>

{% include video id="-vcLCFKQvik" provider="youtube" %}

    </td>
    <td width="640px">
      <b>Understanding Envelopes II:</b>

{% include video id="uFxStP3ATkw" provider="youtube" %}

    </td>    
    <td width="640px">
      <b>Understanding GSTP:</b>

{% include video id="QnH14LkJOnI" provider="youtube" %}

    </td>    
  </tr>
</table>  

_See the [Gordian Envelope playlist](https://www.youtube.com/playlist?list=PLCkrqxOY1FbooYwJ7ZhpJ_QQk8Az1aCnG) for more._


## Libraries

{% include lib-envelope.md %}

{% include lib-dcbor.md %}

## Envelope Links

**Intro:**

* [**Executive Summary**](/envelope/summary/)
* [**Technical Overview**](/envelope/tech/)
* [**Feature List**](/envelope/features/)
* [**IETF Problem Statement: Deterministic Hashed Data Elision**](https://datatracker.ietf.org/doc/draft-appelcline-hashed-elision/) (IETF)

**Industry Intros:**

* [**The Dangers of Digital Credentials in Education**](https://www.blockchaincommons.com/articles/Dangerous-Educational-Credentials/) (blog article)
* [**Protecting Your Wellness Data with Hashed Elision**](https://www.blockchaincommons.com/articles/Dangerous-Wellness-Data/) (blog article)

**Developer Resources:**

* [**Envelope Technical Documents**](https://github.com/BlockchainCommons/Gordian/tree/master/Envelope#articles) (GitHub repo)
* [**IETF Draft: The Envelope Structured Data Format**](https://blockchaincommons.github.io/WIPs-IETF-draft-envelope/draft-mcnally-envelope.html) (Editor's Draft)
* [**dCBOR: Deterministic CBOR**](/dcbor/)

**Developer Extension Resources:**

{% include links-envelope-extensions.md %}

**Developer Reference Apps:**

* [**bc-envelope-cli-rust**](https://github.com/BlockchainCommons/bc-envelope-cli-rust)
* [**envelope-cli-swift**](https://github.com/BlockchainCommons/envelope-cli-swift) (CLI implementation)
  * [**envelope-cli-swift Docs**](https://github.com/BlockchainCommons/envelope-cli-swift/tree/master/Docs)
  * [**envelope-cli-swift Usage Overview Transcript**](https://github.com/BlockchainCommons/envelope-cli-swift/blob/master/Transcripts/1-OVERVIEW-TRANSCRIPT.md) (GitHub repo)


### Specs & Research Papers:

* [**I-D: The Gordian Envelope Structured Data Format**](https://blockchaincommons.github.io/WIPs-IETF-draft-envelope/draft-mcnally-envelope.html) (IETF Draft)
* [**BCR-2023-003: Known Values**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-003-envelope-known-value.md) (GitHub repo)
* [**BCR-2023-004: Symmetric Encryption**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-004-envelope-symmetric-encryption.md) (GitHub repo)
* [**BCR-2023-005: Compression**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-005-envelope-compression.md) (GitHub repo)
* [**BCR-2023-006: Attachments**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-006-envelope-attachment.md) (GitHub repo)
* [**BCR-2023-009: Cryptographic Seeds**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-009-envelope-seed.md) (GitHub repo)
* [**BCR-2023-010: Bitcoin Output Descriptors (v3)**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-010-output-descriptor.md) (GitHub repo)
* [**BCR-2023-012: Expressions**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-012-envelope-expression.md) (GitHub repo)
* [**BCR-2023-013: Cryptography**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-013-envelope-crypto.md) (GitHub repo)
* [**BCR-2023-014: Gordian Sealed Transaction Protocol (GSTP)**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-014-gstp.md) (GitHub repo)
* [**BCR-2024-006: Representing Graphs**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-006-envelope-graph.md) (GitHub repo)
* [**BCR-2024-007: Decorrelation**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-007-envelope-decorrelation.md) (GitHub repo)

### Use Cases:

* [**Overview of Envelope Use Cases**](/envelope/use-cases/)
* [**Summary of Use Cases**](/envelope/use-cases/summary/)
* [**Educational Use Cases**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md) (GitHub repo)
* [**Wellness Use Cases**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md) (GitHub repo)
* [**Data Distribution Use Cases**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md) (GitHub repo)
* [**Asset Control Use Cases**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md) (GitHub repo)
* [**Software Release Use Cases**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md) (GitHub repo)


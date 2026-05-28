---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Hashed Elision
hide_description: true
classes:
  - wide
permalink: /hashed-elision/
sidebar:
  nav: envelope
redirect_from:
  - /elision/
  - /hashed-ellision/
  - /hashed-elission/
  - /hashed-ellission/
---

## Overview

Hashed elision structures credentials so that any field can be replaced by a one-way hash of itself. Issuers sign across those hashes rather than the raw data. The holder can then elide any field at the moment of presentation, and the issuer's signature still verifies. What remains is provably authentic; and what was removed is provably untampered. The verifier learns only what it asked for.

## Why is Hashed Elision Important?

Digital credential data is largely revealed on an all-or-nothing basis. To prove a single fact such as being over 21 typically requires that you hand over the whole document. As a result, the verifier often keeps far more than it needed: every verifier becomes a data honeypot it never set out to be.

Hashed elision offers an alternative through its support of [selective disclosure](https://www.blockchaincommons.com/musings/musings-data-minimization/), which is the ability for a holder of credentials to only reveal the parts of the credential that are important to a particular transaction. 

Though selective disclosure is theoretically available in credential formats such as SD-JWT and mdoc, it's commonly implemented in ways that reduce holder agency. That's because they typically require the issuer to decide in advance which fields may be selectively disclosed. BBS, applied to credentials, similarly allows an issuer to mark specific parts of a credential as mandatory. That is issuer-controlled minimization, and it's not enough. Best practice instead requires that the holder can elide at presentation time, on their own authority, without the issuer pre-approving each field (and without contacting the issuer at all).

## How Does Hashed Elision Work?

In [Gordian Envelope](/envelope/), data is arranged into leaves that exist as part of a fully recursive tree structure. Every leaf of data can be represented by a hash, and every node connecting multiple leaves (or branches) can be represented by a combination of the hashes below it. When a signature occurs, it occurs across a hash of the appropriate node. When data is removed, higher levels of hashes are kept untouched, which means that signatures across those hashes remain valid.

* Any holder can remove any data.
* Any visible signature remains valid, even if part of the data that was signed is removed.
* A holder can commit to the existence of data by revealing the hash, even after the data is elided.
* Anyone can later prove the existence of data with an inclusion proof that reveals the data that made up the hash.

## What Are the Limitations of Hashed Elision?

**Separate Correlation.** A general critique of hashed elision as a class is that it does not stop a verifier from correlating one disclosure with another: the issuer's signature sits unchanged on every presentation, so colluding verifiers can link a holder's appearances. Unlinkable presentation (such as BBS signatures and zero-knowledge proofs) would be a stronger property, and this recommendation is not a substitute for them. However, BBS asks issuers to adopt a pairing-based scheme still in standardization and outside the FIPS-approved suite, and Longfellow is new and still under security review, so hashed elision should be considered as a more proven and accessible technology, especially at the current time.

**Data Organization.** Even hashed elision doesn't give total control to the user: it still is subject to the atomic granularity of the data itself. Therefore, an issuer who records multiple pieces of data (such as a city and state) in a single leaf keeps a holder from revealing just part of that data (such as the state alone).

## Links

* [**Gordian Envelope**](/envelope/)
* [**Data Minimization & Selective Disclosure**](https://www.blockchaincommons.com/musings/musings-data-minimization/) (Musings)

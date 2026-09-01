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
  - /envelope/hashed-elision/
  - /elision/
  - /hashed-ellision/
  - /hashed-elission/
  - /hashed-ellission/
---

## Overview

Hashed elision structures information so that any field can be
replaced by a one-way hash of itself. Issuers sign across those hashes
rather than the raw data. The holder of the data packet can then elide
any field at the moment of presentation, and the issuer's signature
still verifies. What remains is provably authentic; and what was
removed is provably untampered. The verifier learns only what it asked
for.

## Why is Hashed Elision Important?

Digital credential data is largely revealed on an all-or-nothing
basis. To prove a single fact such as being over 21 typically requires
that you hand over the whole document. As a result, the verifier often
keeps far more information than it needed: every verifier becomes a
data honeypot it never set out to be.

Hashed elision offers an alternative through its support of [selective
disclosure](https://www.blockchaincommons.com/musings/musings-data-minimization/),
which is the ability for a holder of credentials to only reveal the
parts of the credential that are important to a particular
transaction.

Though selective disclosure is theoretically available in credential
formats such as SD-JWT and mdoc, it's commonly implemented in ways
that reduce holder agency. That's because those formats typically
require the issuer to decide in advance which fields may be
selectively disclosed. BBS, applied to credentials, similarly allows
an issuer to mark specific parts of a credential as mandatory. That is
issuer-controlled minimization, and it's not enough. Best practice
instead requires that the holder can elide at presentation time, on
their own authority, without the issuer pre-approving each field (and
without contacting the issuer at all).

## How Does Hashed Elision Work?

Blockchain Commons' [Gordian Envelope](/envelope/) has been
specifically built to support the elision of some or all content
within an envelope: the hierarchical hash-based structure enables
selective removal while maintaining overall integrity.

### Envelope Structure

Each element of an envelope is cryptographically bound through hashes:

1. Every element (subject, predicate, object) is individually hashed.
2. These hashes form a Merkle-like tree structure.
3. Parent nodes sum up hashes of their children.
4. The envelope's root hash sums up the entire structure.

### The Elision Process

Any holder of a Gordian Envelope can elide data, which is a critical
element of self-sovereignty. When they do so, the content undergoes a
cryptographically secure one-way transformation:

```
Original Content:  "name": "BRadvoc8"
↓
Elision Process:   hash("name": "BRadvoc8" + [optionally] salt)
↓  
Result:            ELIDED: h'8d7f117fa8511c9c...'
```

The elided content is replaced by its cryptographic digest, which:

- Is a fixed-length representation of the original data.
- Cannot be reversed to reveal the original content (one-way function).
- Uniquely identifies exactly what was elided.
- Can avoid correlation if combined with salt.
- Preserves the existing relationships in the document's tree structure.

This elision provides these specific security guarantees:

1. **Structural Integrity**: Any signatures remain intact and verifiable.
2. **Tamper Evidence**: Any modification to elements in the Envelope invalidate those signatures.
3. **Commitment Hashes**:  The existence of data can be committed to by revealing a hash, even after elision.
4. **Commitment Proof**: The existence of data can later be proven by revealing the data that made up a hash.
5. **Non-Reversibility**: Elided content cannot be recovered from its hash.
6. **Salt-Based Privacy**: With salting, identical content produces different hashes.
7. **Mathematical Soundness**: Protection is based on cryptographically secure hash functions.

### The Merkle-Like Tree

Gordian Envelopes implement a structure similar to a Merkle tree, which enables selective removal:

```
                Root Hash
                /      \
          Hash A        Hash B
         /     \        /    \
    Hash A1   Hash A2  Hash B1  Hash B2
    (elided)         (elided)
```

When elision occurs, the content is replaced with its hash, but all
parent hashes remain valid because they incorporate those child
hashes. This hash-based architecture is what allows for selective
disclosure while still ensuring that the overall structure remains
cryptographically sound.

### Signature Preservation During Elision

One of the most powerful features of elision is signature preservation. Here's why signatures remain valid:

1. **Digital Signature Process**:
   - A signature in a Gordian Envelope covers the whole envelope or a sub-envelope.
   - It signs that envelope's root hash or a sub-envelope's hash.
2. **Hash Substitution During Elision**:
   - As discussed above, hashes remain intact even when the data underlying them is elided.
3. **Verification After Elision**:
   - When verifying a signature on an elided document:
     - The signature validates against the envelope or sub-envelope's hash, not the original content.

This mechanism allows for the removal of sensitive content while
ensuring that signatures attesting to the contents' authenticity
remain valid.

### Types of Elision and Their Effects

Gordian Envelope supports different types of elision for different
disclosure needs. Each one substitutes one part of an envelope with a
hash.

1. Subject Elision: hides the identity (e.g., "Alice")
2. Predicate Elision: hides the assertion predicate (e.g., "read")
3. Object Elision: hides the assertion object (e.g., "Pride & Prejudice")
4. Assertion Elision: Hides all of the assertion (e.g., "read Pride & Prejudice")
5. Envelope Elision: hides the entire envelope or subenvelope (e.g., "Alice read Pride & Prejudice")

Obviously, different types of elision will have different uses
depending on the sensitivity of the various parts (is identity
sensitive? is category of information sensitive? is value of
information sensitive?) and some are more powerful than others

## How Does Hashed Elision Work?

In [Gordian Envelope](/envelope/), data is arranged into leaves that
exist as part of a fully recursive tree structure. Every leaf of data
can be represented by a hash, and every node connecting multiple
leaves (or branches) can be represented by a combination of the hashes
below it. When a signature occurs, it occurs across a hash of the
appropriate node. When data is removed, higher levels of hashes are
kept untouched, which means that signatures across those hashes remain
valid.

* Any holder can remove any data.
* Any visible signature remains valid, even if part of the data that was signed is removed.
* A holder can commit to the existence of data by revealing the hash, even after the data is elided.
* Anyone can later prove the existence of data with an inclusion proof that reveals the data that made up the hash.

## What Are the Limitations of Hashed Elision?

**Separate Correlation.** A general critique of hashed elision as a
class is that it does not stop a verifier from correlating one
disclosure with another: the issuer's signature sits unchanged on
every presentation, so colluding verifiers can link a holder's
appearances. Unlinkable presentation (such as BBS signatures and
zero-knowledge proofs) would be a stronger property, and this
recommendation is not a substitute for them. However, BBS asks issuers
to adopt a pairing-based scheme still in standardization and outside
the FIPS-approved suite, and Longfellow is new and still under
security review, so hashed elision should be considered as a more
proven and accessible technology, especially at the current time.

**Data Organization.** Even hashed elision doesn't give total control
to the user: it still is subject to the atomic granularity of the data
itself. Therefore, an issuer who records multiple pieces of data (such
as a city and state) in a single leaf keeps a holder from revealing
just part of that data (such as the state alone).

## Links

* [**Gordian Envelope**](/envelope/)
* [**Data Minimization & Selective Disclosure**](https://www.blockchaincommons.com/musings/musings-data-minimization/) (Musings)
* [**Deterministic Hashed Data Elision: Problem Statement and Areas of Work**](https://datatracker.ietf.org/doc/html/draft-appelcline-hashed-elision-00) (IETF Draft)

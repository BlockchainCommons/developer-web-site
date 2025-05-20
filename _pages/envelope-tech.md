---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Envelope Technical Overview
hide_description: true
classes:
  - wide
permalink: /envelope/tech/
sidebar:
  nav: envelope
---

Please see the [Introduction to Gordian Envelopes](/envelope/) for
introductory discussion of what Gordian Envelopes are, why they're
useful, and how they're used.

> Gordian Envelope is a specification for the achitecture of a “smart
document". It supports the storage, backup, encryption,
authentication, and transmission of data, with natively supported
cryptography and explicit support for selective
disclosure using hashed data elision. It's designed to protect digital assets including seeds,
keys, Decentralized Identifiers (DIDs), Verifiable Credentials
(VCs), and Verifiable Presentations (VPs).

This document outlines the most important technical features of
Gordian Envelope in abstract Alice and Bob form. See our [Use Case
Intro](/envelope/use-cases/) for more real-world focused designs.

## Envelope Structure

Gordian Envelope is organized with semantic triples:
subject-predicate-object. These are essentially statements such as
"Alice Knows Bob". Given that Gordian Envelope is designed to store
and transmit digital assets, they might actually be storage statements
such as "Seed is XXX", credential statements such as "Bob is certified
as a welder", or certification statements such as "This statement is
certified by the Welder's Guild of Normal, Illinois".

Each envelope has one subject, but can have zero or more
predicate-object pairs, which are called assertions.

_All examples in this overview were generated with our older `envelope-cli-swift` app. We now suggest our newer [`envelope-cli-rust`](https://github.com/BlockchainCommons/bc-envelope-cli-rust) as an excellent way to further explore the
technical details of Gordian Envelope. Please see the [Envelope-CLI
Docs](https://github.com/BlockchainCommons/bc-envelope-cli-rust/tree/master/docs#readme)
for its usage._

Though an envelope can include just a Subject, the most basic example of a full semantic triple is:
```
"Alice" [
    "knows": "Bob"
]
```
In this case, "Alice" is the subject, while the predicate of "knows"
and the object of "Bob" form an assertion.

Using `envelope-cli`, all Gordian Envelope output can be displayed in three forms. The standard text output is shown above. 

The `--tree` text output uses a different text formatting that also
includes hashes:
```
e54d6fd3 NODE
    27840350 subj "Alice"
    55560bdf ASSERTION
        7092d620 pred "knows"
        9a771715 obj "Bob"
```
The third form is the graphical `--mermaid` output, which also
includes hashes, but it can get very large (and the text can get very
tiny) for big trees. However, it's excluded from these examples mainly
because GitHub doesn't support Mermaid usage on GitHub pages.

Envelopes can be more complex with multiple assertions possible for
every subjects, such as in this example Alice's differing feelings
toward different people:

```
"Alice" [
    "hates": "Charlie"
    "likes": "Bob"
]
```

```
00643efa NODE
    13941b48 subj "Alice"
    4cce83cb ASSERTION
        5ad5faee pred "hates"
        ee8e3b02 obj "Charlie"
    e6adc5a4 ASSERTION
        c0b2b377 pred "likes"
        13b74194 obj "Bob"
```
However the abstract examples in this technical overview will largely constrain themselves to a simple single-assertion envelope.

> _See the [Envelope
Docs](https://github.com/BlockchainCommons/Gordian/tree/master/Envelope#articles)
for more information on basic envelope structure, especially
[Overview](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Overview.md),
[Examples](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Examples.md),
and [Envelope
Notation](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Notation.md). Also
see the [Envelope-CLI
Docs](https://github.com/BlockchainCommons/bc-envelope-cli-rust/tree/master/docs#readme),
especially
[Overview](https://github.com/BlockchainCommons/bc-envelope-cli-rust/blob/master/docs/Overview.md)._

## Sub Envelopes

Each element of a semantic triple can itself be an envelope. This
allows for an infinitely recursive structure.

One purpose of doing so is to allow for the inclusion of more metadata
about any individual element (usually the subject or the object)

For example, the following shows a slightly more realistic usage of
our abstract example, where Alice and Bob are actually represented by
DIDs, which each contain more info on the user, including a user name
and public keys that can be used to identify them:

```
{
    CID(d44c5e0a) [
        "userName": "Lil Alice"
        controller: CID(d44c5e0a)
        publicKeys: PublicKeyBase
    ]
} [
    "knows": CID(24b5b23d) [
        "userName": "Bob Bobbery"
        controller: CID(24b5b23d)
        publicKeys: PublicKeyBase
    ]
]
```

> _Complex metadata offers the best examples of sub-envelopes. See [the
Metadata Example in the Envelope
Docs](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Examples.md#example-10-complex-metadata)
and
[Complex Metadata Example](https://github.com/BlockchainCommons/bc-envelope-cli-rust/blob/master/docs/MetadataExample.md)
in the Envelope-CLI docs._

## Wrapped Envelopes

In a semantic triple the assertion always refers to the
subject. Multiple assertions _all_ refer to the same subject, such as
the above example of Alice both liking Bob and hating Charlie. But
what if you wanted an assertion to apply to an entire triple? For
example, what if you wanted to say that Alice's knowing Bob was a fact
that was known by Victor?

This **does not** do the job:
```
"Alice" [
    "knows": "Bob"
    "knownBy": "Victor"
]
```
That actually says that Alice knows Bob and that Alice is knownBy
Victor. That's not the same thing at all!

In order to instead allow Victor to known about the other semantic
triple requires the enclosure of the Alice-knows-Bob triple in a new
envelope, which is then used as a subject that Victor can known. (As
you should recall, _everything_, including the Subject, can be an
envelope).

This is the other major use of sub-envelopes, but it's a common enough
pattern that it has its own name: it's a "wrapped envelope".

Here's what that looks like:
```
{
    "Alice" [
        "knows": "Bob"
    ]
} [
    "knownBy": "Victor"
]
```
The wrapping becomes clearer when you view this as a tree:
```
8758f67b NODE
    fd881a24 subj WRAPPED
        8955db5e subj NODE
            13941b48 subj "Alice"
            78d666eb ASSERTION
                db7dd21c pred "knows"
                13b74194 obj "Bob"
    505eb3a8 ASSERTION
        51daff8f pred "knownBy"
        87f54611 obj "Victor"
```

There are many different cases where applying an assertion to not just
a subject but a full triple (that's been made into an envelope and
then _used_ as a subject) is useful. One of those uses is a very
common use case: signing.

## Signing Envelopes

One of the biggest advantages of wrapped envelopes is that you can use
to allow someone to verify the entire assertion (or set of assertions)
by using the wrapped envelope as a subject and applying a signature as
an object using the "known" predicate of `verifiedBy`.

This is what that would look like:
```
{
    "Alice" [
        "knows": "Bob"
    ]
} [
    verifiedBy: Signature
]
```

```
cac42f48 NODE
    fd881a24 subj WRAPPED
        8955db5e subj NODE
            13941b48 subj "Alice"
            78d666eb ASSERTION
                db7dd21c pred "knows"
                13b74194 obj "Bob"
    f2660bef ASSERTION
        9d7ba9eb pred 3
        8cb59f6e obj Signature
```

These verifications can later be checked to ensure that the subject
envelope is what was actually signed by the `verifiedBy` assertion.

More complexity is possible, for example by attaching additional information on who is doing the verifying:
```
{
    {
        "Alice" [
            "knows": "Bob"
        ]
    } [
        "verifierInfo": CID(d44c5e0a) [
            "trustedSource": "https://www.blockchaincommons.com/pki/d44c5e0a"
            "userName": "Lil Alice"
            controller: CID(d44c5e0a)
            publicKeys: PublicKeyBase
        ]
    ]
} [
    verifiedBy: Signature
]
```

Here we see that the core Alice-knows-Bob information has been wrapped
and `verifierInfo` has been attached to it, and then that has been
wrapped and signed. Whew!

(Obviously, care still needs to be taken, with the verifier having a
high degree of responsibility: they need to make sure the verifierInfo
is correct to know that the signature is meaningful! A `trustedSource`
has been suggested as a root of truth, but even that needs to be taken
with a grain of salt!)

> _Further examples of Signing, including Multi-Signing, can be found in
the [Examples of the Envelope
Docs](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Examples.md#example-2-signed-plaintext)
and the [Examples of the Envelope-CLI
Docs](https://github.com/BlockchainCommons/bc-envelope-cli-rust/blob/master/docs/BasicExamples.md)._

## Hashing Envelopes

The `--tree` examples in this introduction all include the first four
bytes of a hash digest for each node. This is a standard feature of
Gordian Envelope: its nested triples are stored as a Merkle-like Tree.

We use the phrase "Merkle-like Tree" because the structure is
not quite the same as a classic Merkle Tree:

* A classic [Merkle Tree](https://en.wikipedia.org/wiki/Merkle_tree) hashes data blocks to form its leaves, then its inner nodes contain hashes of the sums of those leaf hashes.
* Gordian Envelope's Merkle-like Tree instead is generated as needed.
   * Its leaves typically contain data.
   * Hashes are generated for leaves primarily when the data is elided (currently, elision can be due to encryption, compression, or purposeful redaction of information), creating a cryptographic commitment for the original data that has been removed or transformed.
   * A hash of an envelope's subject is also ephemerally calculated (but not stored) when a signature occurs, as signature occurs across the hash, not the underlying data.
   * Otherwise, hashes are generated when a Tree is deserialized and placed in memory.
   * During deserialization, inner nodes are also generated, with their hashes built appropriately from the hashes of the leaves (which may have been previously generated due to elision or generated now during deserialization).

In other words, Gordian Envelope's Merkle-like Tree can be mapped to a Merkle Tree through the hashing of all of its leaves, but this is done in a dynamic fashion: only as needed. This allows it to enjoy the advantages of a classic Merkle Tree, but with limited overhead: you only pay for you use, as you only generate hashes that are required.

As for the advantages of a Merkle-like tree? The dynamically generated hashes ensure the consistency and verifiability of the underlying data even when it is elided or encrypted (or compressed), which are additional major features of Gordian Envelope.

## Eliding Envelopes

One of the biggest advances of Gordian Envelope is that it includes
elision as a fundamental principle. Any element of an envelope may be
elided; the elision may be done by the Holder (not just an Issuer, as
is the case with most extant credential data structures); and elision
does not change the hashes of the envelope.

Take as an example the signed Alice Knows Bob example:

```
{
    "Alice" [
        "knows": "Bob"
    ]
} [
    verifiedBy: Signature
]
```

```
42fc2541 NODE
    fd881a24 subj WRAPPED
        8955db5e subj NODE
            13941b48 subj "Alice"
            78d666eb ASSERTION
                db7dd21c pred "knows"
                13b74194 obj "Bob"
    06898dda ASSERTION
        9d7ba9eb pred 3
        2dc27493 obj Signature
```

Note the hashes: `13941b48` for Alice, `78d666eb` for the Knows-Bob
Assertion, `8955db5e` for the overall Node, and `fd881a24` for the
Wrapped Envelope of that Node. (We're ignoring the `verifiedBy`
assertion and also the overall node because signature hashes _will_
change every time they're signed, as a natural characteristic of how
signing works: it has a random factor.)

We can elide everything but the signature and see that the hashing
holds up:

```
ELIDED [
    verifiedBy: Signature
]
```

```
42fc2541 NODE
    fd881a24 subj ELIDED
    06898dda ASSERTION
        9d7ba9eb pred 3
        2dc27493 obj Signature
```

That `42fc2541` was indeed the hash of the signed envelope that is currently elided.

As we reveal things step-by-step, known hashes appear, allowing for
selective disclosure of exactly the material we want to reveal, while
maintaining verifiability:
```
42fc2541 NODE
    fd881a24 subj WRAPPED
        8955db5e subj ELIDED
    06898dda ASSERTION
        9d7ba9eb pred 3
        2dc27493 obj Signature
```

```
42fc2541 NODE
    fd881a24 subj WRAPPED
        8955db5e subj NODE
            13941b48 subj ELIDED
            78d666eb ELIDED
    06898dda ASSERTION
        9d7ba9eb pred 3
        2dc27493 obj Signature
```

In this case, we might reveal that Alice is the subject of an assertion, without having to reveal the other half of the equation:

```
42fc2541 NODE
    fd881a24 subj WRAPPED
        8955db5e subj NODE
            13941b48 subj "Alice"
            78d666eb ELIDED
    06898dda ASSERTION
        9d7ba9eb pred 3
        2dc27493 obj Signature
```
However, this example is obviously abstract, and intended mainly for its simplicity. In a more realistic case, a Gordian Envelope with multiple assertions would be partially elided to only reveal some of what it contains.

Remember Alice's two-part envelope?

```
00643efa NODE
    13941b48 subj "Alice"
    4cce83cb ASSERTION
        5ad5faee pred "hates"
        ee8e3b02 obj "Charlie"
    e6adc5a4 ASSERTION
        c0b2b377 pred "likes"
        13b74194 obj "Bob"
```
In this example, she might want to reveal that she likes Bob without revealing that she hates Charlie!

```
"Alice" [
    "likes": "Bob"
    ELIDED
]
```

```
00643efa NODE
    13941b48 subj "Alice"
    4cce83cb ELIDED
    e6adc5a4 ASSERTION
        c0b2b377 pred "likes"
        13b74194 obj "Bob"
```

Again, note that all the hashes have stayed the same, even for the
Elided branch and its parent.

To make this abstract example even more realistic, in the case of
credentials the Holder could choose which credentials to share and
which not to.

> _Further examples of redaction may be found in
  [Elision/Redaction](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Elision.md)
  in the Envelope Docs and
  [VC with Detailed Elision Example](https://github.com/BlockchainCommons/bc-envelope-cli-rust/blob/master/docs/VCElisionExample.md)
  in the Envelope-CLI docs._

## Encrypting Envelopes

Obviously, the critical tool in the Gordian Envelope toolbox is encryption, as it allows for the protection of contents.

This penultimate example uses the old favorite:
```
"Alice" [
    "knows": "Bob"
]
```

```
8955db5e NODE
    13941b48 subj "Alice"
    78d666eb ASSERTION
        db7dd21c pred "knows"
        13b74194 obj "Bob"
```

Just as with any assertion, encryption applies to the Subject. So the default application of Encryption on Alice-Knows-Bob would result in the following:
```
ENCRYPTED [
    "knows": "Bob"
]
```

```
8955db5e NODE
    13941b48 subj ENCRYPTED
    78d666eb ASSERTION
        db7dd21c pred "knows"
        13b74194 obj "Bob"
```

(Note once more than the hash of the root node stayed the same!)

The fact that only the subject is encrypted means that wrapping is
again required if you want to encrypt the entire contents of an
envelope.

Wrapping results in:
```
{
    "Alice" [
        "knows": "Bob"
    ]
}
```

```
fd881a24 WRAPPED
    8955db5e subj NODE
        13941b48 subj "Alice"
        78d666eb ASSERTION
            db7dd21c pred "knows"
            13b74194 obj "Bob"
```

Then encrypting reduces that to:
```
ENCRYPTED
```

```
fd881a24 ENCRYPTED
```

Which is probably what was intended in the first place!

There is a change in the hash from the standard Alice-Knows-Bob
envelope, but that's because we're now seeing the hash of the wrapped
envelope:


A wrapped, encrypted envelope can continue to be layered, for example
by signing the encrypted data:

```
ENCRYPTED [
    verifiedBy: Signature
]
```

```
3d6b2821 NODE
    fd881a24 subj ENCRYPTED
    7472e444 ASSERTION
        9d7ba9eb pred 3
        704c58e3 obj Signature
```

Once more, it's envelopes all the way down!

> _For further examples see [Symmetric
Encryption](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Examples.md#example-4-symmetric-encryption)
and additional examples in the Envelope Docs and [Symmetric
Encryption](https://github.com/BlockchainCommons/bc-envelope-cli-rust/blob/master/docs/BasicExamples.md#example-4-symmetric-encryption)
and additional examples in the Envelope-CLI docs._

## Salting Envelopes

The above examples on hashing, eliding, and encrypting all depend on
selective correlation: we're using hashes to purposefully allow
correlation, in order to suport the use of inclusion proofs or other
means of selective disclosure.

This is _not_ always a desirable characteristic. If guesses can be
made about the contents of elided or encrypted data, they can be
solved! In situations where that is an issue, Gordian Envelope
supports salting.

The methodology behind salting is simple: add a new assertion to an envelope that contains `salt` as a predicate and a random number as an object.

This is particularly useful when adding it to a subject, predicate, or
object that might be easily guessable and that you don't _want_ to be
easily guessable. The subject, predicate, or object becomes an evelope
containing salt as an assertion.

The following example shows a salt of the subject "Alice":
```
{
    "Alice" [
        salt: Salt
    ]
} [
    "knows": "Bob"
]
```

```
55494b68 NODE
    e282424e subj WRAPPED
        dd2c718c subj NODE
            13941b48 subj "Alice"
            7b1e3360 ASSERTION
                aa14a6e0 pred 15
                e9fa8ae8 obj Salt
    78d666eb ASSERTION
        db7dd21c pred "knows"
        13b74194 obj "Bob"
```

The standard hash of "Alice", `13941b48` (which can still be seen in
the depths of the envelope), is now replaced with a wrapped envelope
hash of `e282424e` thanks to the inclusion of salt. Where a
brute-force search might be able to determine that Alice were
`13941b48` even when Alice was elided or encrypted, the same would not
be true for the salted hash of `e282424e`.

Different use cases might require that selective correlation should
either be used or foiled; salt is what makes the latter happen.

> For further examples see [08-Noncorrelation](https://github.com/BlockchainCommons/Gordian/blob/master/Envelope/Noncorrelation.md) in the Gordian Envelope docs.

## Final Notes

This technical introduction is intended to give a broad overview of
the major capabilities of Gordian Envelope with diagrams
demonstrating what those envelopes actually look like.

For further details please see the [Envelope
Docs](https://github.com/BlockchainCommons/Gordian/tree/master/Envelope#articles)
and if you are able, follow-along with the [Envelope-CLI
Docs](https://github.com/BlockchainCommons/bc-envelope-cli-rust/tree/master/docs#readme). Those
documents provide _much_ more detail on all the examples here. In some
cases the examples here were drawn from those docs.

Also see the [Envelope Use Cases](/envelope/use-cases/) for many more pragmatic examples of Envelopes in use (with diagrams).

## Futher Links

* [**Envelope Internet-Draft**](https://datatracker.ietf.org/doc/draft-mcnally-envelope/) (IETF)
{% include links-envelope-extensions.md %}



---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "SSKR FAQ"
hide_description: true
permalink: /sskr/faq/
sidebar:
  nav: sskr
toc: true
toc_label: SSKR FAQ
---

## Using SSKR

### What Can SSKR Do?

SSKR allows you to protect a 32-byte seed. It's a tool for
[Resilience](https://github.com/BlockchainCommons/Gordian#gordian-principles),
intended to protect a seed from loss.

### What Are the Caveats to using SSKR?

You will need to be careful of a few things when utilizing SSKR:

* **Seed Randomness.** The seed being encrypted must be truly random,
with high entropy.
* **Share Non-Determinism.** The shares are non-deterministic due to a
random factor in their creation. This means that shares will look
different every time you generate them.
* **32-Byte Limit.** SSKR can only protect data that is no larger than
32 bytes.

### How Can I Protect Data Longer than 32 bytes?

Protecting larger amounts of data is often important.  Most notably, a
standard BIP-32 HD wallet is typically built with a 32-byte private
key and its 32-byte chain, which is twice what SSKR can
protect. However, there are other use cases, such as protecting a seed
with associated metadata.

There are a number of ways this can be accomplished:

1. Create multiple SSKR objects.
2. Protect a 32-byte symmetric key using SSKR, and then use that key
to protect the larger data.
3. Use [Gordian Envelope](https://www.blockchaincommons.com/introduction/Envelope-Intro/).

Gordian Envelope was specifically written to resolve this dilemma.

## SSKR Standards

### What Sharding Standards Does SSKR Use?

Currently, SSKR supports one secret-sharing algorithm: Shamir's Secret
Sharing, which is version `0` of SSKR types. As such, it's intended to
replace SLIP-39 as a specification for Shamir's Secret Sharing.

### Why Use Shamir's Secret Sharing?

Shamir's Secret Sharing (SSS) is built on an old, well-respected
cryptographic proof, and so we have faith in its foundational
concepts.

Though we believe that [the usage of
multisig](https://github.com/BlockchainCommons/Gordian/blob/master/Docs/Multisig.md)
is superior to SSS, because Shamir's Secret Sharing can expose the
user to vulnerability on the machine where a key is reconstructed, we
nonetheless acknowledge that many users prefer Shamir's Secret
Sharing, in large part for compatibility with last-generation
single-signature addresses.

So, while we're leading the way in using multisig to better protect
our digital wealth, we're also interested in improving the
interoperability of resilient, single-signature addresses, and that
primarily means improving the interoperability of Shamir's Secret
Sharing.

### Why Did We Create a New Specification?

We are well aware of the problems of creating yet more standards, as
depicted by the ever-insightful Randall Munroe:

![](https://imgs.xkcd.com/comics/standards.png)

In the case of SSKR we felt that the creation of a new specification
for Shamir's Secret Sharing was critical due to problems that we
identified with the previous implementations, primarily SLIP-39, which
we discovered [did not round trip with
BIP-39](https://github.com/BlockchainCommons/bc-lethekit/issues/38).

The result is SSKR, which is similar to SLIP-39, but not
compatible. It includes the same Shamir's Secret Sharing technology,
but uses a different key-derivation methodology, for compatibility
with BIP-39.

Seeing the need for a new specification that roundtripped with BIP-39
also allowed the possibility for expansion, which permitted us to
introduce two-level shares as a major new feature.

## SSKR Words

### What are Bytewords?

SSKR shares can be output as
[Bytewords](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-012-bytewords.md),
which is a specification for outputting encoded binary data as English
words.

Bytewords has several advantages. It's as efficient as hex, the words
are all a uniform four letters, there are no homophones, and the first
and last characters of each word differ from other words. Words were
also specifically chosen to either be concrete or else to have
valence. All around, the goal was to make ByteWords very easy to
distinguish.

See [the ByteWords page](/bytewords/) for more information.

BytesWords were purposefully chosen for use with SSKR to differentiate
it from the BIP-39 mnemonics and thus avoid confusion. If something is
encoded with BIP-39 words, it's likely a SLIP-39 share, while if
something is encoded with ByteWords, it's likely an SSKR share.

## What do the different parts of an SSKR share mean?

<figure class="half">
  <a href="/assets/images/sskr/export-2.jpg"><img src="/assets/images/sskr/export-2.jpeg"></a>
  <a href="/assets/images/sskr/export-3.jpg"><img src="/assets/images/sskr/export-3.jpeg"></a>  
</figure>

An SSKR share contains a secret, but there are about a dozen
additional words that help to define the share.

To start with, note the overlap in words in different
shares:
this is expected. The first four words will always be the same as they
describe the share as SSKR ("tuna acid epic") of a specific length
("gyro"). The next two ("flap pose") are a fingerprint that match all
the shares in a split, and the next two ("able acid") describe the
group threshold, count, and index, plus the member threshold, so
they're the same for all the shares in a group.

There's also checksum at the end ("toys flap solo film"), which is
part of the [Uniform Resource](/ur/) specification that is used in
conjunction with Bytewords: it allows for error detection when
decoding the UR.

SSKR | length | ID | group & member info | secret share | checksum
---|---|---|---|---|---
tuna acid epic | gyro | flap pose | able acid able | even iris... taco wolf | toys flap solo film

Note also that the non-repetitive words will change each time you
regenerate SSKR shares from a secret. This is also expected: there is
a random factor in SSKR generation.


## SSKR Libraries

### Why Did We Create New Libraries?

Obviously, with a new specification, we also needed libraries for that
specification. We created C reference libraries for SSS and SSKR and
modularizing the contents to allow a developer to pick and choose how
they were put together. 

{% include lib-shamir.md %}

{% include lib-sskr.md %}

Our new libraries also resolve one of our long-standing problems with
SSS: we've always felt that Shamir's Secret Sharing looked deceptively
easy to code, which has resulted in more than one implementation
incorporating fundamental problems, from the lack of round-tripping in
SLIP-39, to incorrect use of entropy in other implementations. Thus,
we were happy to offer our own implementation, building on the
cryptographic expertise of Blockchain Commons members and other
experts in the field such as Daan Sprenkels.

## SSKR Expansions

### How Does SSKR Compare to Previous Sharding Standards?

SSKR expands on the capabilities of previous standards, including:


1. Compatibility with BIP-39.
2. Implementation of two-level shares.
3. Encoding of shares as Bytewords.
4. Integration with URs.

### What are Two-Level Shares?

Using SSKR, you can implement Shamir's Secret Sharing as normal, using
a group of shares with a threshold for reconstruction. However, you
can also implement a two-level hierarchy with multiple groups, each of
which contain some shares. You can then specify a threshold for both
groups and shares.

For example, you could create shares for 2 groups: the first group
might require 2 of 3 shares and the second group might require 3 of 5
shares. With a group threshold of 2, individual thresholds from both
groups must be met.

### What are URs?

URs are [Uniform
Resources](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md),
another Blockchain Commons specification. They describe a methodology
for efficient, self-describing resources, and are easily interoperable
with Bytewords. We choose them as another output format for SSKR.

See [A Guide to Using URs for SSKRs](ur-3-sskrs.md) for how Byteword
SSKRs and UR SSKRs are encoded, and how they slightly differ as well
as the [URs page](/ur/) for more general information.

## SSKR History

### Where Did SSKR Come From?

SSKR is the end-result of over two years of effort. It most
immediately grew out of workgroups at the Rebooting the Web of Trust
design workshops.  At [RWOT8 in
Barcelona](https://github.com/WebOfTrustInfo/rwot8-barcelona), a group
of experts worked on "Shamir's Secret Sharing Best Practices" and the
foundation of a new library, while at [RWOT9 in
Prague](https://github.com/WebOfTrustInfo/rwot9-prague), a new cohort
further discussed use cases, formats, and the two-level scheme. These
workgroups included experts such as Christopher Allen, Bryan Bishop,
Laurence Chen, Hank Chiu, Mark Friedenbach, Chris Howe, Yancy Ribbens,
Daan Sprenkels, ChiaWei Tang, and others.

Initial work on Blockchain Common's `bc-shamir` library grew from
Daan's work on his own [SSS
library](https://github.com/dsprenkels/sss). Further encouragement to
produce the new SSKR specification came from Ken Sedgwick, who
identified the round-tripping problems between BIP-39 and SLIP-39.

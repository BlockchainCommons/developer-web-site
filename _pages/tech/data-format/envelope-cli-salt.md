---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataformat.jpg
  og_image: /assets/images/bc-card.jpg
title: "🧂 Salting Your Data"
tagline: "Learning Envelope from the Command Line"
hide_description: true
classes:
  - wide
permalink: /envelope/cli/salt/
sidebar:
  nav: envelope
---

_This is a hands-on command-line introduction to Gordian Envelope in
the style of Blockchain Commons' [from the Command Line
courses](/courses/). It makes use of the Rust-based
[`envelope-cli`](https://github.com/BlockchainCommons/bc-envelope-cli-rust)._

_The focus of this course is on salting: it demonstrates how salting
can protect the privacy of data stored in an envelope and then
elided._

_Also see ["Learning Envelope from the Command Line"](/envelope/cli/)
for basics such as assertions and signing._

## Overview

[Hash-based elision](/hashed-elision/) is one of the core features of
[Gordian Envelope](/envelope/). It allows data to be selectively
redacted by the envelope holder, supporting [data
minimization](https://developer.blockchaincommons.com/architecture/data-minimization/),
while also maintaining signatures, which are made across the hash
rather than the original data.

However, hash-based elision has one notable vulnerability: if someone
can guess the contents of an elided item, they can verify its
existence using the hash.

The following examples use this envelope:

```sh
ur:envelope/tpsplrtpsoihfpjziniaihoytpsoisihjnjojzjlkkihjptpsoimfekshsjnjojzihcxfxjloytpsoinidinjpjyisfyhsjyihtpsosecydmesmkaeoytpsoiyieihiojpihihtpsojefwguiacxgdiskkjkiniajkwfnnzmtl
```

Which was generated as follows with the `envelope-cli`:

```sh
ALICE=$(envelope subject type string "Alice" | envelope assertion add pred-obj string  "birthDate" date 1994-07-30 | envelope assertion add pred-obj string "degree" string "BSc Physics" | envelope assertion add pred-obj string "employer" string "Example Co" | envelope subject type wrapped)
```

It contains the following wrapped data:

```sh
envelope format $ALICE

| {
|     "Alice" [
|         "birthDate": 1994-07-30
|         "degree": "BSc Physics"
|         "employer": "Example Co"
|     ]
| }
```

(Why wrapped? Because the next step would be to sign it, but we'll
work with the simpler, unsigned data here.)

## Revealing the Problem

You can view the hashes of the envelope with the `envelope format
--type tree` command:

```sh
envelope format --type tree $ALICE

| a5d7df2d WRAPPED
|     3f0da808 cont NODE
|         13941b48 subj "Alice"
|         46209a0d ASSERTION
|             a61e96a8 pred "employer"
|             b7927b5c obj "Example Co"
|         a6a5f777 ASSERTION
|             22daf383 pred "birthDate"
|             9733441e obj 1994-07-30
|         c5ce91c6 ASSERTION
|             80ce7253 pred "degree"
|             506b2ec4 obj "BSc Physics"
```

When you elide part of the data, such as Alice's birth date, the hash
remains (which is the point!)

```sh
BIRTHDATE=$(envelope extract wrapped $ALICE | envelope assertion find predicate string "birthDate" | envelope extract object | envelope digest)
ALICE_MINIMIZED=$(envelope elide removing $BIRTHDATE $ALICE)

envelope format $ALICE_MINIMIZED

| {
|     "Alice" [
|         "birthDate": ELIDED
|         "degree": "BSc Physics"
|         "employer": "Example Co"
|     ]
| }

envelope format --type tree $ALICE_MINIMIZED

| a5d7df2d WRAPPED
|     3f0da808 cont NODE
|         13941b48 subj "Alice"
|         46209a0d ASSERTION
|             a61e96a8 pred "employer"
|             b7927b5c obj "Example Co"
|         a6a5f777 ASSERTION
|             22daf383 pred "birthDate"
|             9733441e obj ELIDED
|         c5ce91c6 ASSERTION
|             80ce7253 pred "degree"
|             506b2ec4 obj "BSc Physics"
```

Here's what the birthdate looked like before elision:

```sh
9733441e obj 1994-07-30
```

And here's what it looks like afterward:

```
9733441e obj ELIDED
```

The hash remains the same, which is what allows the holder to later
write an [inclusion proof](/envelope/inclusion-proof/) that
demonstrates that the birthdate of `1994-07-30` is in the credential.

However, dates have a limited data space. If someone knows the format
of the data (which is here just the `date` type), they could make a
table listing all of the possible entries. That's 365 or 366 entries
per year, times the number of years. Not a lot in the scope of
computing power!

| Hash | Date |
|------| -----|
| ... | ... |
| 6df068fe | 1994-07-26 | 
| 770424db | 1994-07-27 |
| 82594a76 | 1994-07-28 |
| adc767ef | 1994-07-29 |
| 9733441e | 1994-07-30 |
| ... | ... |

It would take mere seconds to crunch dates until you found the hash of
an elided entry in an envelope, and worse an attacker could have a
[rainbow table](https://en.wikipedia.org/wiki/Rainbow_table) of all
the dates for the last 100 years that they could check against
instaneously.

### Solving with Salts

The answer to this problem is [🧂 salt](/envelope/salt/). This is a
random value that can be added to any subject, predicate, object,
assertion, or sub-envelope within an envelope.

The following adds salt to the date object:

```sh
SALT_DATE=$(envelope subject type date 1994-07-30 | envelope salt)
```

The date now has an assertion of `salt`:

```sh
envelope format $SALT_DATE

| 1994-07-30 [
|     'salt': Salt
| ]
```

Examining the hashes demonstrates that the date's hash stays the same
(`9733441e`), but it and the salt are now incorporated into a node
with a hash that isn't guessable (`71b7042d`).

```sh
envelope format --type tree $SALT_DATE

| 71b7042d NODE
|     9733441e subj 1994-07-30
|     2a26b1d7 ASSERTION
|         618975ce pred 'salt'
|         8a53fe12 obj Salt
```

You can then rebuild Alice's envelope by substituting the date with the date-with-salt envelope:

```sh
SALTY_ALICE=$(envelope subject type string "Alice" | envelope assertion add pred-obj string  "birthDate" envelope $SALT_DATE | envelope assertion add pred-obj string "degree" string "BSc Physics" | envelope assertion add pred-obj string "employer" string "Example Co" | envelope subject type wrapped)

envelope format $SALTY_ALICE

| {
|     "Alice" [
|         "birthDate": 1994-07-30 [
|             'salt': Salt
|         ]
|         "degree": "BSc Physics"
|         "employer": "Example Co"
|     ]
| }
```

You can then use the exat same technique to elide the date:

```sh
BIRTHDATE_WITH_SALT=$(envelope extract wrapped $SALTY_ALICE | envelope assertion find predicate string "birthDate" | envelope extract object | envelope digest)
SALTY_ALICE_MINIMIZED=$(envelope elide removing $BIRTHDATE_WITH_SALT $SALTY_ALICE)
```

The elided envelope looks just the same to the naked eye:

```sh
envelope format $SALTY_ALICE_MINIMIZED

| {
|     "Alice" [
|         "birthDate": ELIDED
|         "degree": "BSc Physics"
|         "employer": "Example Co"
|     ]
| }
```

But the content is no longer guessable from the digest because of the salt that was part of the elided content:

```
envelope format --type tree $SALTY_ALICE_MINIMIZED

| 2d28261e WRAPPED
|     ad885cbb cont NODE
|         13941b48 subj "Alice"
|         46209a0d ASSERTION
|             a61e96a8 pred "employer"
|             b7927b5c obj "Example Co"
|         64b92257 ASSERTION
|             22daf383 pred "birthDate"
|             71b7042d obj ELIDED
|         c5ce91c6 ASSERTION
|             80ce7253 pred "degree"
|             506b2ec4 obj "BSc Physics"
```

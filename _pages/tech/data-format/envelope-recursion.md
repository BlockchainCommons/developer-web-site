---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/headers/tech-dataformat.jpg
  og_image: /assets/images/bc-card.jpg
title: Radical Recursion
hide_description: true
classes:
  - wide
permalink: /envelope/recursion/
sidebar:
  nav:
    - envelope
    - dataformat
    - technology
redirect_from:
  - /recursion/
  - /envelope/radical-recursion/
  - /radical-recusion/
---

## Overview

Gordian Envelope is built with radical recusion. Any subject,
predicate, object, or assertion can itself be an envelope, which can
itself contain additional envelopes. It's literally envelopes all the
way down.

## Why is Radical Recursion Important?

Fundamentally, radical recursion provides **structure** to Gordian
Envelope. It allows data to be categorized in a variety of
ways. Advantages of this include:

- **Organization.** Different types of data can be placed in different
places within an envelope.
- **Standardization.** Data can be placed in regularized places, as
defined by other standards, with no need to adjust the envelope
format. Data can even be replicated in multiple places to meet the
needs of multiple standards.
- **Data Complexity.** Additional data can be
encoded about any element within an envelope. For example, the claims
made in
[edges](https://learningxids.blockchaincommons.com/03_0_Edges/) in
[XIDs](/xids/) use a simple model of a "subject" making a claim about
a "target", but with radical recursion additional data can also be encoded
about the subject, the target, or the claim.
- **Data Obscuration.** Radical recursion is what allows for the use of [salt](/envelope/salt/), which obscures data that could otherwise be guessed from its hash.
- **Repackaging.** Envelopes can be repackaged within envelopes, which allows envelopes to be commented on, signed, or otherwise offered in new forms, all without impacting the original envelope (or its root hash).
- **Elision.** Radical recursion integrates tightly with [hashed
elision](/hashed-elision/), as elision allows very complex structures to be
pared down to just what an individual recipient needs. For example, if
data is encoded to meet multiple standards, only those that are relevant for a single recipient need be revealed.
- **Versatility.** Finally, radical recurions allows the same
substrate to carry property graphs, hypergraphs, labeled directed
multigraphs, and several other shapes because its structure is general
enough that those graph models fall out of it. This is done without
the need for cryptographic agility or other means that would change
the fundamental structure of a Gordian Envelope.

## How Does Radical Recursion Work?

Radical recursion is simple: any element in an envelope can actually
be an envelope itself.

The following is simple envelope with a subject, a predicate, and an
object.

```sh
"Alice" [
    "knows": "Bob"
]
```

The following shows how the object ("Bob") could be changed into an envelope:

```sh
"Alice" [
    "knows": "The Bobbing Bobkins" [
        "member": "Bob"
        "member": "Brad"
        "member": "Buckaroo"
    ]
]
```

The following additionally recurses the predicate ("knows") to offer more details:

```sh
"Alice" [
    "knows" [
        "relationshipType": "casual"
    ]
    : "The Bobbing Bobkins" [
        "member": "Bob"
        "member": "Brad"
        "member": "Buckaroo"
    ]
]
```

As we said, it's envelopes all the way down, as shown by this addition of another sub-envelope to the "Bob" object:

```sh
"Alice" [
    "knows" [
        "relationshipType": "casual"
    ]
    : "The Bobbing Bobkins" [
        "member": "Brad"
        "member": "Buckaroo"
        "member": "Bob" [
            "lastName": "Bobford"
        ]
    ]
]
```

Deeper structures and parallel structures are possible, because it's
all _radically_ recursive. In addition, new sub-envelopes can also be
attached to envelopes or to full assertions.

### Creating Recursion with Envelope-CLI

These examples were all created with the Rust-based
[envelope-cli](https://github.com/BlockchainCommons/bc-envelope-cli-rust). Using
envelope-cli, it's usually easiest to create envelopes from the bottom
up: creating the lowest level envelopes and then attaching those to
higher level envelopes until you're ready to attach those to your core
subject.

We first create the "Bob" envelope:

```sh
BOB=$(envelope subject type string Bob | envelope assertion add pred-obj string "lastName" string "Bobford")

envelope format $BOB

| "Bob" [
|     "lastName": "Bobford"
| ]
```

We can then create the "Bobkins" envelope, which incorporates Bob:

```
BOBKIN=$(envelope subject type string "The Bobbing Bobkins" | envelope assertion add pred-obj string "member" envelope $BOB |  envelope assertion add pred-obj string "member" string "Brad" |  envelope assertion add pred-obj string "member" string "Buckaroo")

envelope format $BOBKIN

| "The Bobbing Bobkins" [
|     "member": "Brad"
|     "member": "Buckaroo"
|     "member": "Bob" [
|         "lastName": "Bobford"
|     ]
| ]
```

That fills in the "Bob" component of the top-level Alice-knows-Bob
envelope. But we need to create the sub-envelope for knows as well:

```sh
KNOWS=$(envelope subject type string "knows"  | envelope assertion add pred-obj string "relationshipType" string "casual")

| "knows" [
|     "relationshipType": "casual"
| ]
```

Finally, we can put Alice together with the "knows" and "Bob" envelopes:

```
ALICE=$(envelope subject type string Alice | envelope assertion add pred-obj envelope $KNOWS envelope $BOBKIN)

envelope format $ALICE

| "Alice" [
|     "knows" [
|         "relationshipType": "casual"
|     ]
|     : "The Bobbing Bobkins" [
|         "member": "Brad"
|         "member": "Buckaroo"
|         "member": "Bob" [
|             "lastName": "Bobford"
|         ]
|     ]
| ]
```

Other envelope UIs may use other means to recurse envelopes and sub-envelopes.

## Links

* [**Gordian Envelope**](/envelope/)
* [**Hashed Elision**](/hashed-elision/)
* [**Salt**](/envelope/salt/)
* [**BCR-2024-006: Representing Graphs using Gordian Envelope**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-006-envelope-graph.md)
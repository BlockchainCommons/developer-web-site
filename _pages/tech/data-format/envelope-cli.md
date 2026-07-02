---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataformat.jpg
  og_image: /assets/images/bc-card.jpg
title: "Learning Envelope from the Command Line"
hide_description: true
classes:
  - wide
permalink: /envelope/cli/
sidebar:
  nav:
    - envelopecli
    - envelope
    - dataformat
    - technology
---

_This is a hands-on command-line introduction to Gordian Envelope in the style of Blockchain Commons' [from the Command Line courses](/courses/). It makes use of the Rust-based [envelope-cli](https://github.com/BlockchainCommons/bc-envelope-cli-rust)._

_Also see ["Learning Envelope Seeds from the Command Line"](/envelope/seed/)._

## What is a Gordian Envelope?

A Gordian Envelope is a data structure that combines:

- Structured semantic information (like who did what)
- Cryptographic verification (like digital signatures)
- Selective disclosure capabilities (through elision)

Think of an Envelope as a container that can hold information in a
structured way, be securely sealed to verify its source, and have
parts selectively revealed while keeping other parts private.

### The Subject-Assertion-Object Model

Gordian Envelopes use a structure similar to sentences in natural language:

```sh
<SUBJECT> [
   <PREDICATE>: <OBJECT>
   <PREDICATE>: <OBJECT>
   ...
]
```

For example:
```sh
"BRadvoc8" [
   "name": "BRadvoc8"
   "domain": "Distributed Systems & Security"
   "experienceLevel": "8 years professional practice"
]
```

In this structure:
- **Subject**: The main entity the envelope is about ("BRadvoc8")
- **Predicate**: A property or relationship ("domain", "experienceLevel")
- **Object**: The value of that property ("Distributed Systems & Security", "8 years professional practice")

This structure creates clear, semantic relationships that are both
human-readable and machine-processable.

### Types of Assertions You Can Make

You can make various types of assertions within an Envelope:

1. **String assertions**: Simple text values

```sh
"name": "BRadvoc8"
```

2. **Structured data assertions**: Complex data types

```sh
"location": {
   "latitude": 47.6062
   "longitude": -122.3321
}
```

3. **Nested envelope assertions**: Envelopes within envelopes

```sh
"project": ProjectEnvelope [...]
```

4. **Cryptographic assertions**: Digests, signatures, etc.

```sh
"documentHash": SHA256(a7f3ec...)
```

Anything can be held in an Envelope, from small declarations to large
sets of data.

## Signing and Verification

Envelopes can be cryptographically signed to verify:

- Who created or endorsed an Envelope
- That the content hasn't been altered since signing

The signing process consists of two steps:

1. A private key is used to generate a digital signature of the Envelope.
2. The signature is attached to the Envelope.

In the [envelope-cli
tool](https://github.com/BlockchainCommons/bc-envelope-cli-rust), the
process looks like this:

```sh
PRIVATE_KEYS=$(envelope generate prvkeys)
PUBLIC_KEYS=$(envelope generate pubkeys "$PRIVATE_KEYS")
SIGNED_PROPOSAL=$(envelope sign -s "$PRIVATE_KEYS" "$WRAPPED_PROPOSAL")
```

The public key of the keypair can then be used to verify the
signature. This confirms the Envelope was signed by the corresponding
private key.

```sh
envelope verify -v "$PUBLIC_KEYS" "$SIGNED_PROPOSAL"
```

This creates non-repudiation: the signer cannot deny creating the
signature if it verifies with their public key.

### Wrapping & Signing

Any assertion in an Envelope always applies to its subject: the subject
predicates the object. This applies to signatures too: a signature
signs the subject. It does _not_ sign the other assertions on that
subject.

As a result, the following is usually not what's intended:

```sh
"BRadvoc8" [
   "name": "BRadvoc8"
   "domain": "Distributed Systems & Security"
   "experienceLevel": "8 years professional practice"
   SIGNATURE
]
```

This `SIGNATURE` only applies to the subject, `BRadvoc8`, not to the
assertions about their experience.

The solution is to "wrap" the envelope before signing it, creating a
new Envelope with the original Envelope as its subject. This way, the
signature applies to the entire original Envelope, including all its
assertions. (That's why a `$WRAPPED_PROPOSAL` was used in the example above.)

First, create your envelope with all assertions

```sh
ENVELOPE=$(envelope subject type string "BRadvoc8")
ENVELOPE=$(envelope assertion add pred-obj string "name" string "BRadvoc8" "$ENVELOPE")
ENVELOPE=$(envelope assertion add pred-obj string "domain" string "Distributed Systems & Security" "$ENVELOPE")
ENVELOPE=$(envelope assertion add pred-obj string "experienceLevel" string "8 years professional practice" "$ENVELOPE")
```

Then, wrap the envelope before signing

```sh
WRAPPED_ENVELOPE=$(envelope subject type wrapped "$ENVELOPE")
```

Finally, sign the wrapped envelope

```sh
SIGNED_ENVELOPE=$(envelope sign -s "$PRIVATE_KEYS" "$WRAPPED_ENVELOPE")
```

This creates a structure where the signature applies to the entire original Envelope:

```sh
envelope format $SIGNED_ENVELOPE

| WRAPPED {
|    "BRadvoc8" [
|       "name": "BRadvoc8"
|       "domain": "Distributed Systems & Security"
|       "experienceLevel": "8 years professional practice"
|    ]
| } [
|    SIGNATURE
| ]
```

This ensures the signature verifies the integrity of all assertions in
the original envelope, not just its subject.

## Enabling Both Verification and Privacy

One of the most powerful features of Gordian Envelopes is the ability
to maintain cryptographic verification even when parts of the data are
hidden through elision.

For example, if you have a properly wrapped and signed envelope:

```sh
WRAPPED {
   "BRadvoc8" [
      "name": "BRadvoc8"
      "domain": "Distributed Systems & Security"
      "experienceLevel": "8 years professional practice"
   ]
} [
   SIGNATURE
]
```

You can elide (remove) the `experienceLevel` assertion while maintaining the signature's validity:

```sh
WRAPPED {
   "BRadvoc8" [
      "name": "BRadvoc8"
      "domain": "Distributed Systems & Security"
      ELIDED
   ]
} [
   SIGNATURE
]
```

The signature remains valid because all of the elements in an Envelope
are hashed, then the hash of the wrapped Envelope (which is the sum of
the hashes below it) is what's actually signed. The hashes remain,
even when the content that produced them is elided.

This allows for:

- Privacy-preserving information sharing
- Minimizing data exposure while maintaining verification
- Revealing different information to different audiences
- Progressive disclosure as trust develops

## Using Envelopes with XIDs

XIDs use Gordian Envelopes as their container format. This supports
structured, semantic representation of identity information and allows
selective disclosure of that information to protect privacy. It can be
used for use cases such as:

1. **Identity Information**: Structuring claims about a person or entity
2. **Signed Documents**: Creating verifiable records and attestations
3. **Evidence Commitments**: Committing to evidence without revealing it prematurely
4. **Trust Assertions**: Making claims that others can verify and build upon

The [Learning XIDs from the Command Line course](https://learningxids.blockchaincommons.com/) offers extensive details on how XIDs and Envelope interrelate.

## Check Your Understanding

1. What is the basic structure of a Gordian Envelope?
2. How does the subject-assertion-object model represent information?
3. What happens when an Envelope is signed, and how is the signature verified?
4. How can an Envelope be elided while maintaining signature validity?
5. Why is the combination of verification and privacy so powerful?

## Next Steps

You may also want to read ["Learning Envelope Seeds from the Command
Line"](/envelope/seed/) for an in-depth example of using Envelope to
store a specific sort of cryptographic object.

## Download the Software

* **Envelope CLI:** `cargo install bc-envelope-cli`
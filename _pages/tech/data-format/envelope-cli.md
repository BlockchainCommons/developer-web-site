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

## Elision

Elision allows for the removal of data from a Gordian Envelope while
retaining its signatures.

### Single Field Elision

The following signed envelope is used for the basis of elision.

```sh
SIGNED_DOC=$(envelope subject type string "API Security Enhancement" | envelope assertion add pred-obj string methodology string "Static analysis with open source tools" | envelope assertion add pred-obj string limitations string "No penetration testing performed" | envelope assertion add pred-obj string dataSources string "Public API documentation" | envelope subject type wrapped | envelope sign -s $PRIVATE_KEYS)
envelope format $SIGNED_DOC

| {
|     "API Security Enhancement" [
|         "dataSources": "Public API documentation"
|         "limitations": "No penetration testing performed"
|         "methodology": "Static analysis with open source tools"
|     ]
| } [
|     'signed': Signature
| ]
```

A single field can be elided by digging down through wrapped
envelopes, finding the hash of the content to be elided and then removing it with the `elide removing` command.


```sh
LIMITATIONS_DIGEST=$(envelope extract wrapped $SIGNED_DOC | envelope assertion find predicate string "limitations")
ELIDED_DOC=$(envelope elide removing $LIMITATIONS_DIGEST $SIGNED_DOC)
envelope format $ELIDED_DOC

| {
|     "API Security Enhancement" [
|         "dataSources": "Public API documentation"
|         "methodology": "Static analysis with open source tools"
|         ELIDED
|     ]
| } [
|     'signed': Signature
| ]
```

The signature verification still works because the hash maintains the cryptographic structure:

```sh
envelope verify -s -v $PUBLIC_KEYS $ELIDED_DOC
# Result: ✅ No repsonse means success
```

### Multiple Field Elision

Multiple fields can similarly be removed by creating an array of digests.

```sh
ELIDED_DIGEST=()
ELIDED_DIGEST+=$(envelope extract wrapped $SIGNED_DOC | envelope assertion find predicate string "limitations")
ELIDED_DIGEST+=" "
ELIDED_DIGEST+=$(envelope extract wrapped $SIGNED_DOC | envelope assertion find predicate string "methodology")
DOUBLE_ELIDED_DOC=$(envelope elide removing "$ELIDED_DIGEST" $SIGNED_DOC)
envelope format $DOUBLE_ELIDED_DOC

| {
|     "API Security Enhancement" [
|         "dataSources": "Public API documentation"
|         ELIDED (2)
|     ]
| } [
|     'signed': Signature
| ]
```

Again, this demonstrate how elision preserves both the signature
validity and structural integrity of documents while allowing
appropriate content sharing for different contexts.

## Salting

The
[envelope-cli](https://github.com/BlockchainCommons/bc-envelope-cli-rust)
can explicitly add salt or not to any Envelope element. It does so by
adding salt just like any other assertion.

This following will protect the "alice" envelope when elided:

```sh
envelope subject type string alice | envelope assertion add pred-obj string knows string bob | envelope salt | envelope format

| "alice" [
|     "knows": "bob"
|     'salt': Salt
| ]
```

This will instead protect the "knows bob" assertion

```sh
KB=$(envelope assertion create string knows string bob | envelope salt)
AKB_S=$(envelope subject type string alice | envelope assertion add envelope $KB)
envelope format $AKB_S

| "alice" [
|     {
|         "knows": "bob"
|     } [
|         'salt': Salt
|     ]
| ]
```

In each of these cases, the hash of the sub-envelope can no longer be
guessed by a dictionary attack, because it now includes a random Salt
element. However, the content of the sub-envelope can still be proven,
even after hashing, by releasing both the content and the salt as an
inclusion proof.

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
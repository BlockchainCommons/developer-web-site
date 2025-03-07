---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: XID Presentation
hide_description: true
classes:
  - wide
permalink: /xid/presentation/
sidebar:
  nav: xid
---

## Video

<table width="100%">
  <tr>
    <td width="40%">
      <b>Video:</b>

{% include video id="k1iIO-bfVhM" provider="youtube" %}

    </td>
  </tr>
  <tr>
    <td width="40%">
      <b>Presentation:</b>
        <a href="/assets/pdfs/xid-intro.pdf"><img src="/assets/pdfs/xid-intro.jpg" style="border: solid 1px white;"></a>
    </td>
  </tr>
</table>

## Annotated Transcript

## Gordian Envelope, Elision, and Controller Documents

I'm Wolf McNally, Lead Researcher for Blockchain Commons. This presentation is about Gordian Envelope, Elision, and Controller Documents.

Blockchain Commons is a proudly not-for-profit social benefit corporation, domiciled in Wyoming but operating worldwide. We have a strong commitment to open source and a defensive patent strategy. Anyone can use or improve our tools, and no one can take them away. We engage in a variety of activities with various communities, including the development of guidelines, specifications, reference implementations of libraries, and apps that demonstrate the use of our technologies.

We're sponsor-supported, so please consider becoming a sustaining sponsor, a GitHub sponsor, or making a one-time BTCPay donation. Join the conversation at [blockchaincommons.com](https://blockchaincommons.com), including our discussion forums on GitHub, low-volume announcements via email, various signal groups, our monthly Gordian developer meetings on the first Wednesday, and special interest group meetings, for example, FROST implementers and Silicon Salon for hardware developers.

---

### Recap of Blockchain Commons' Technologies

I’ll start with a quick recap of some of our technologies relevant to this presentation. Our introductory materials include videos, particularly the [Envelope playlist on YouTube](https://youtube.com/@blockchaincommons), featuring a brief Gordian Envelope teaser and the more extensive *Understanding Gordian Envelope Parts 1 and 2*. Our websites offer additional resources, and our main portal for envelope development is at [developer.blockchaincommons.com/envelope](https://developer.blockchaincommons.com/envelope).

**Envelope** is a smart document system built on Deterministic CBOR (dCBOR). CBOR is a concise binary object representation like JSON, but in binary form. It’s concise, self-describing, ideal for IoT and constrained environments, and platform- and language-agnostic.

- **Deterministic at the Binary Level Up**: Numeric values have a single encoding (no variations like 0, -0.0, or 0.000).
- **Strings** are always in Unicode Normalization Form C (NFC).
- **Maps** or dictionary keys are automatically sorted, eliminating the need for a separate canonicalization step.

---

### Gordian Envelope: Semantic Structure and Cases

**Gordian Envelope** defines a simple semantic structure based on semantic triples: *subject*, *predicate*, *object*.

```plaintext
<subject> [
    <predicate>: <object>
    <predicate>: <object>
    ...
]
```

> Note: This is not typically the meaning of “subject” used in the verifiable credentials domain, where it refers to a person or organization about which claims are made, though it can serve this purpose in some cases.

**Gordian Envelope** has five basic cases:

- **LEAF**: Any encoded CBOR data.
- **NODE**: A subject envelope with one or more assertion envelopes.
- **ASSERTION**: An envelope that pairs a predicate and an object.
- **ELIDED**: A digest left behind when a branch is replaced with this node.
- **WRAPPED**: Contains a single child envelope, allowing for meta assertions.

Additionally, there are three extension cases:

- **ENCRYPTED**: Encrypts a branch with IETF’s ChaCha20-Poly1305 symmetric encryption.
- **KNOWN VALUE**: A 64-bit integer space for frequently referenced concepts.
- **COMPRESSED**: Uses the deflate algorithm to compress a branch.

---

### It's Envelopes All the Way Down

Envelopes are recursive; every node in an envelope’s tree is itself an envelope. **NODE** and **ASSERTION** cases have child nodes, and any child node can be replaced by a NODE with assertions. This includes predicates, allowing assertions on predicates and deeply rich metadata.

Each node has a unique digest, with transformations (e.g., **Elision**, **Encryption**, and **Compression**) preserving the top-level digest.

---

### Elision: The Game-Changer

With **Elision**, you can selectively transmit data, preserving document signatures as long as the digest tree maintains proof of the elided data. Inclusion proofs allow verifiable revelation of document parts later, a process we call progressive trust.

Example of a Gordian Envelope for a driver’s license:

```plaintext
{
    "E281029" [
        'isA': "Driver License"
        "firstName": "John"
        "lastName": "Doe"
        "photograph": 🙂
        "dateOfBirth": 1994-07-30
        "address": "123 Elm St., Town USA"
        "issuer": "State of Example"
        "issued": 2021-03-17
        "expires": 2029-03-17
    ]
} [
    'verifiedBy': Signature
]
```

The icons beside each line are unique LifeHashes, a Blockchain Commons technology. More information on LifeHashes can be found at [lifehash.info](https://lifehash.info). In real-world applications, we may add **salt** to fields (random values) to make them harder to correlate across documents. 

When asked to present a document for a specific purpose, elision allows the holder to omit irrelevant fields. For instance, when proving a date of birth and photo, the holder can elide all unnecessary fields, such as the driver's license number. The document's top-level hash remains unchanged, keeping the signature valid. Additional proof requests, such as for the address, can be satisfied with an **inclusion proof**, revealing only the needed data.

---

### Gordian Sealed Transaction Protocol (GSTP)

The **Gordian Sealed Transaction Protocol (GSTP)** extends Gordian Envelopes for secure peer-to-peer data exchange over unreliable channels (e.g., internet, Bluetooth, NFC, Tor, and even QR codes). It is request-response-based, supports **key agreement**, **data exchange**, **confidential backups**, and **multisig coordination**.

GSTP requests include encrypted state continuations, packets of data encrypted back to the sender and/or recipient, allowing stateless and scalable solutions. 

Example of a GSTP request:

```plaintext
{
    request(ARID(c66be27d)) [
        'body': «do_it» [
            ❰arg1❱: Type1
            ❰arg2❱: Type2
        ]
        'senderPublicKey': PublicKeyBase
        'senderContinuation': ENCRYPTED [
            'hasRecipient': SealedMessage
        ]
        'recipientContinuation': ENCRYPTED [
            'hasRecipient': SealedMessage
        ]
    ]
} [
    'verifiedBy': Signature
]
```

The **sender continuation** is data encrypted back to the sender, retrievable when sent back with a response. The **recipient continuation** is encrypted for the recipient, facilitating scenarios where state needs offloading without storing locally, essential for environments requiring horizontal scaling.

---

### Improving Controller Documents

Controller documents can be improved using three formats:

1. **CBOR-LD**
2. **Bespoke CBOR formats**
3. **Gordian Envelope** (recommended due to its flexibility and **holder-based elision**).

With **Holder-Based Elision**, only required parts of a document are revealed, supporting the principle of data minimization.

For example, in a DID system, Gordian Envelope allows selective elision of unnecessary verification keys or endpoints, complying with privacy policies and ensuring that verifiable data is only revealed when needed.

---

### Example of Eliding a Controller Document

To demonstrate how **Gordian Envelope** can be used for controller documents, here is an example with various keys and settings.

#### Original Controller Document:

```plaintext
{
    XID(2d9296d0) [
        'allow': 'all'
        'dereferenceVia': "https://resolver.example.com"
        'key': SigningPublicKey
        'key': 'group' [
            'controller': XID(5802b4ff) [
                'dereferenceVia': "btc:01234567"
            ]
            'key': SigningPublicKey [
                'deny': 'verify'
            ]
            'key': AgreementPublicKey [
                'endpoint': "https://messaging.example.com"
            ]
        ]
    ]
} [
    'verifiedBy': Signature
]
```

#### Elided Controller Document with Salt:

Adding salt to fields allows further privacy by making values unique per document.

```plaintext
{
    XID(2d9296d0) [
        'allow': 'all'
        'dereferenceVia': "https://resolver.example.com"
        'key': SigningPublicKey [
            'salt': Salt
        ]
        'key': 'group' [
            'controller': XID(5802b4ff) [
                'dereferenceVia': "btc:01234567"
                'salt': Salt
            ]
            'key': SigningPublicKey [
                'deny': 'verify'
                'salt': Salt
            ]
            'key': AgreementPublicKey [
                'endpoint': "https://messaging.example.com"
                'salt': Salt
            ]
        ]
    ]
} [
    'verifiedBy': Signature
]
```

#### Elided with Only Essential Fields Visible

Further elision hides unnecessary fields while maintaining the document’s integrity and signature.

```plaintext
{
    XID(2d9296d0) [
        'key': SigningPublicKey
        ELIDED (3)
    ]
} [
    'verifiedBy': Signature
]
```

Inclusion proofs can be generated later to reveal additional elided parts as needed.

---

### Integrating with Existing Infrastructure

Example of a DID with Gordian Envelope-elided services:

```plaintext
"did:example:123456789abcdefghi" [
    'service': "https://example.com" [
        'isA': 'LinkedDomainsEndpoint'
        'salt': Salt
    ]
    'service': "https://messaging.example.com" [
        'isA': 'MessagingEndpoint'
        'salt': Salt
    ]
]
```

The elided document can be serialized, Base64 encoded, and embedded in a JSON controller document for verifiable revelation. This flexibility is beneficial for revealing elided items later, allowing integration of **Gordian Envelopes** with JSON-based infrastructure.

---

**Contact Information**

- **Christopher Allen**  
  Email: christophera@lifewithalacrity.com  
  Twitter: [@BlockchainComns](https://twitter.com/BlockchainComns)

- **Wolf McNally**  
  Email: wolf@wolfmcnally.com  
  Twitter: [@WolfMcNally](https://twitter.com/WolfMcNally)


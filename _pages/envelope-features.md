---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Envelope Features & Benefits
hide_description: true
classes:
  - wide
permalink: /envelope/features/
sidebar:
  nav: envelope
---

## Structural Fundamentals

_Gordian Envelope is built on the following structural fundamentals._

* **Data Storage.** The initial inspiration for Gordian Envelopes was secure data storage.
* **Structured Merkle-like Tree.** Data is stored using a variant of the Merkle Tree structure where hashes for data are generated dynamically as needed (either due to elision or when the data is deserialized).
* **Flexible Data Structures.** The nested and recursive design of the Merkle-like Tree allows Envelopes to encapsulate other Envelopes, facilitating complex data scenarios and structures. A comprehensive range of data formats are thus supported including relational hierarchical structures, simple linked lists, and node or edge-labeled graphs, accommodating complex data relationships and ensures adaptability across different use cases.
* **Schema Flexibility.** Structures do not require schemas, enhancing adaptability and ease of use. Nonetheless, Envelope is capable of supporting formal schemas when needed for specific regulatory compliance or data integrity requirements.
* **Deterministic Representation.** There is only one way to encode any semantic representation within a Gordian Envelope. This is accomplished through the use of [Deterministic CBOR](/dcbor/) and the sorting of the Envelope by hashes to create a lexicographic order. Any Envelope that doesn't follow these strict rules can be rejected; as a result, there's no need to worry about different people adding the assertions in a different order or at different times: if two Envelopes contain the same data, they will be encoded the same way.

## Elision & Signing Support

_Hashed data elision is the core of Gordian Envelope._

* **Provable Elision.** Gordian Envelopes innately support elision as a best practice, allowing removal of any part of its data, including subjects, predicates, and objects. This is done while maintaining provability because a hash of data that acts as a cryptographic commitment is created whenever elision occurs, supporting auditability and verifiability. The technique aligns with data minimization principles and is critical for protecting individual privacy and human rights and maintaining compliance with global data protection regulations.
* **Redaction, Compression, and Encryption.** Elision can be used for a variety of purposes including redaction (removing information) and optionally compression (removing duplicate information) and encryption (enciphering information).
* **Holder-initiated Redaction.** Elision can be performed by the holder of a Gordian Envelope, not just the issuer, providing users with the autonomy to manage their data exposure based on personal or business risk.
* **Granular Holder Control.** Elision can not only be performed by any Holder, but also for any data, allowing each entity to elide data as is appropriate for the management of their personal (or business) risk.
* **Progressive Trust.** The elision mechanics in Gordian Envelopes allow for [progressive trust](https://www.blockchaincommons.com/musings/musings-progressive-trust/), where increasing amounts of data are revealed over time. It can even be optionally combined with encryption to escrow data to later be revealed.
* **Consistent Hashing.** Even when elided (or compressed or encrypted), hashes for those parts of the Gordian Envelope remain the same.
* **Signing.** Embedded and detached signatures enhance data security and remain valid even when data is elided, as signatures occur across the data hashes, not the underlying data.
* **SSH Signing.** Gordian Envelope is very flexible. Though Schnorr has been advanced as its main Signing extension, other signing options are possible. To support software-release use cases, the [Envelope-CLI Reference App](https://github.com/BlockchainCommons/bc-envelope-cli-rust) now also supports [SSH signing](https://github.com/BlockchainCommons/bc-envelope-cli-rust/blob/master/docs/Signing.md).

## Privacy Support

_The fundamental methodology of hashed data elision dramatically improves privacy._

* **Minimal Disclosure.** Gordian Envelope is built to allow minimal disclosure, a general precept of privacy.
* **Proof of Inclusion.** When data has been redacted, proofs of inclusion can be generated to allow private proof that data exists.
* **Herd Privacy.** Proofs of inclusion allow for herd privacy where all members of a class can share data such as a VC or DID without revealing individual information.
* **Non-Correlation.** Encrypted Gordian Envelope data can optionally be made less correlatable with the addition of salt or through zero-knowledge proofs.

## Encryption Support

_The expanded spec of Gordian Envelope supported through Blockchain Commons' [Research papers](https://github.com/BlockchainCommons/research/?tab=readme-ov-file#contents) provides support for optional encryption as well._

* **Symmetric Key Permits.** Gordian Envelopes can be locked
    ("closed") using a symmetric key.
* **SSKR Permits.** Gordian Envelopes can alternatively be locked
    ("closed") using a symmetric key sharded with Shamir's Secret
    Sharing, with the shares stored with copies of the Envelope, and
    the whole enveloped thus openable if copies of the Envelope with a
    quorum of different shares are gathered.
* **Public Key Permits.** Gordian Envelopes can alternatively be
    locked ("closed") with a public key and then be opened with the
    associated private key, or vice versa.
* **Multiple Permits.** Gordian Envelopes can simultaneously be locked
    ("closed") via a variety of means and then openable by any
    appropriate individual method, with different methods likely held
    by different people. This meets diverse security requirements and allows Envelope to adapt to various cryptographic landscapes.

### Dynamic & Communication Support

_With Blockchain Commons' expanded specs, data can be dynamic and support dynamic communications._

* **Expressions.** Envelope elements can be recognized as functions and parameters, allowing for the implementation of application-specific logic within Envelopes, facilitating responsive and adaptive data interactions.
* **Requests/Responses.** Expressions can be wrapped up in [Requests](/envelope/request/), allowing for the communication between two entities. Responses can then be sent in reply to Requests.
* **Sealed Transactions (GSTP).**  Requests and responses can be built up into the [Gordian Sealed Transaction Protocol](/envelope/gstp/) to support encrypted and authenticated communications across insecure channels such as QR codes and NFC, ensuring secure data exchanges even in vulnerable environments. GSTP can manage state using [Encrypted State Continuations (ESC)](/envelope/esc/), which allow for local state to be stored in a distributed way.

### Data Management Support

_Finally, data can be managed in a variety of ways, either as it changes, or as a holder protects data that's unchanging._

* **Data Compression.** Optional data compression can optimize storage and streamline data integration.
* **Differencing & Merging.** Tools allow for the differencing of two versions of an Envelope and the merging of their distinct content, supporting dynamically changing data.
* **Decorrelation.** Advanced decorrelation techniques, including zero-knowledge proofs, prevent unintended data correlation, thereby securing data confidentiality and enhancing privacy. Desirable correlation can nonethless be maintained simply by not using these techniques.
* **Distributed or Decentralized Identifiers.** Self-Certifying Identifiers (SCIDs) can be created and shared with peers, certified with a trust authority, or registered on blockchain.

### Use Cases Across Industries

_Though digital wallets are some of the earliest adapters of Gordian Envelope, it has use across many industries._

* **Credentials & Identity.** Gordian Envelope signing techniques allow for the creation of credentials and the ability to present them to different verifiers in different ways. This enables comprehensive self-sovereign identity management, promoting autonomy and reducing reliance on centralized authorities.
* **Healthcare & Finance.** Gordian Envelope provides robust solutions for the secure handling and sharing of sensitive information, ensuring compliance with industry-specific regulations such as HIPAA in healthcare and stringent security requirements in finance.
* **Supply Chain & Software Integrity.** Gordian Envelope can also facilitate transparency and integrity in supply chains and support the maintenance of software release integrity.

### Community Support

_Blockchain Commons is built around supporting a developer community!_

* **Developer Support.** Blockchain Commons offers extensive development support with libraries available in multiple programming languages, ensuring that developers have the tools they need to implement Gordian Envelope effectively across various platforms and applications.
* **Community Engagement .** Blockchain Commons also actively promotes community engagement through regular meetings and specialized roundtable discussions, driving continuous improvement and fostering a collaborative environment for industry adaptation.
  
### Future Looking

_Gordian Envelope was built to be future looking._

* **Future Techniques.** Beyonds its technical specifics, Gordian
    Envelopes still allows for cl-sigs, bbs+, and other
    privacy-preserving techniques such as zk-proofs, differential
    privacy, etc.
* **Cryptography Agnostic.** Finally, the Gordian Envelope
    architecture is cryptography agnostic, allowing it to work with
    everything from older algorithms with silicon support through more
    modern algorithms suited to blockchains and to future zk-proof or
    quantum-attack resistent cryptographic choices.

_Please see the [Technical Overview](/envelope/tech/) for more
specifics on how Envelopes work._

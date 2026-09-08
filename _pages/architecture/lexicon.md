---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Architecture Lexicon
hide_description: true
classes:
  - wide
permalink: /architecture/lexicon/
sidebar:
  nav: architecture
---

_The following words & phrased are used in the Gordian
architecture.

## Architectural Terms

* **Coercion Resistance.** The philosophy that people should be able
to make their own choices. We all often instead invoke _Privacy_, but
_Coercion Resistance_ instead defines what _Privacy_ is meant to
protect.
* **Functional Partition.** The philosophy of separating different
functions as different parts of an interoperable ecosystem, and also
dividing data up into different locations, all to improve
_Resilience_. This is done with _Partitions_ and could include
_Airgaps_ or _Torgaps_. This is one way to accomplish _Heterogeneity_.
* **Data Minimization.** The philosophy of always offering the least amount of data needed for a task. This is a subset of _Least & Necessary_.
* **Gordian Principles.** Four fundamental precepts at the heart of
the Blockchain Commons' work: _Independence_, _Privacy_, _Resilience_,
and _Openness_. See [Gordian Principles](/principles/).
   * **Independence.** The ability to work in a self-sovereign way without centralization. A _Gordian Principle_.
   * **Privacy.** Protection of personal data and usage against correlation and censorship. A _Gordian Principle_.
   * **Resilience.** The ability to prevent loss of assets or data, including resilience against theft and resilience against accidental loss. A _Gordian Principle_.
   * **Openness.** Interoperability of systems and easy portability of data. A _Gordian Principle_.
nnn* **Heterogeneity.** The philosophy that things are better protected if they're different. _Functional Partition_ is one element of this.
* **Least & Necessary.** A authentication-related philosophy that says the least required (or most necessary) permissions should be granted. When extended to information, this becomes the _Data Minimization_ principle.
* **Open Development.** The philosphy that open source isn't sufficient: it must be part of a larger life cycle that communicates with community and includes support.
* **Progressive Trust.** The concept of gradually building trust over
time. See [Musings of a Trust Architect: Progressive
Trust](https://www.blockchaincommons.com/musings/musings-progressive-trust/).
* **Pseudonymous Trust Building.** The philosophy of being able to build _Progressive Trust_ through peer-to-peer interactions in a _Web of Trust_ for a static identity not connected to the physical world.
* **Reference App.** An application that shows an example of the usage
of a _Specification_, usually built with a _Reference Library_.  See
[Gordian Reference Apps](/architecture/apps/).
* **Reference Library.** A library that provides an API for using a
_Specification_.  See [Libraries](/libraries/).
* **SmartCustody.** Documents, instructions, and _Specifications_
intended to improve the _Resilience_ of digital assets, either at the
personal or the ecosystem level. See
[SmartCustody](https://www.smartcustody.com).
* **Specification.** A specific design intended to support
communication, data, or backup _Encoding_ and backup, to ensure the
_Openness_ of interoperability, to support UX Design, or to ensure
other _Gordian Principles_. Part of the _Gordian System_. See Gordian
Specifications in [Blockchain Commons Research
Repo](https://github.com/BlockchainCommons/Research/blob/master/README.md)
and of course, the whole [developer website](/).
* **Web of Trust.** A linked set of identities that can offer peer-to-peer attestations for _Pseudonymous Trust Building_.

## Macro-Architecture Terms

* **Airgap.** A _Partition_ between two _Services_ such that they are
not _Networked_ on the same network. (Often, at least one _Service_ is
not _Networked_ at all). See [Airgap
(Wikpedia)](https://en.wikipedia.org/wiki/Air_gap_(networking)).
* **Closely Held Device.** A hardware device such as a phone or a
hardware wallet that is privately held by an individual, that has
a small attack surface due to careful and consistent sandboxing,
and that is not constantly _Networked_ in the way that a full
computer tends to be.
* **Gordian Macro-architecture.** A suggested design for a data-asset
ecosystem that supports the [Gordian Principles](/principles/). It's
built upon a foundational idea of _Functional Partition_. See
[Gordian Architecture](/architecture/).
* **Microservice.** A _Service_ that provides a capability which is
very specific and/or infrequently used. See [Gordian
Architecture](/architecture/).
* **Networked.** Directly connected to an online network.
* **Partition.** A division between two or more _Services_. A
partition could be as simple as ensuring those _Services_ are on
different machines, but can also include an _Airgap_ or _Torgap_.
* **Service.** An application providing a specific capability as part
of the _Functional Partition_ of a digital-asset ecosystem. Includes
_Microservices_.
* **Torgap.** A _Partition_ between two _Services_, created to ensure
that they are anonymous to each other. See [Torgap](/architecture/torgap/).

## Technological Terms

* **Animated QRs.** An animation of a QR made up of several
frames. Used for transmitting data larger than the max possible
with _Quick Response (QR) Codes_. See [Animated QRs](/animated-qrs/).
* **ByteWords.** An Encoding Specification that represents binary
data as English words. Used in the by Blockchain Commons
specifications primarily to represent _CBOR_ in _URs_. See
[ByteWords](/bytewords/).
* **CBOR.** A Data Format: it represents data in a binary
format. See [RFC 8949](https://cbor.io/).
* **Collaborative Key Management (CKM).** A _Seed Recovery_ _Service_ for the
collaborative generation and usage of keys. See [CKM](/ckm/).
* **Collaborative Seed Recovery (CSR).** A _Seed Recovery_ _Service_ to improve
_Resilience_ by storing _Shares_ of keys or seeds that are created by
_Sharding_. See [CSR](/csr/).
* **Encoding.** A conversion of data into a specific form. See
[Encoding (Techopedia)](https://www.techopedia.com/definition/948/encoding).
* **Encoding Specification.** A _Specification_ for _Encoding_
* **Envelope.** A communication _Specification_ for a Smart Document
that supports the storage, backup, encryption & authentication of
data, with explicit support for Merkle-based selective disclosure. See
[Envelope](/envelope/).
* **Lifehash.** A UX Design that creates a visual hash as part of an
_OIB_ to allow for visual identification of data. See
[Lifehash](/lifehash/).
* **Object Identity Block (OIB).** A UX Design for an array of data
that can together allow a user to easily and uniquely identify
data. Can include a _Lifehash_. See [OIB](/oib/).
* **Quick Connect.** A Data Format for a _URI_ or _QR Code_ that can
be used to securely connect together two devices that are separated by
a _Partition_. See [Quick Connect](/quickconnect/).
* **Quick Response (QR) Code.** An Encoding Specification that
represents data in a graphical format. _URs_ are built to allow for
efficient encoding as a QR Code. With them, Gordian [Animated
QRs](/animated-qrs/) support animation of larger data sets using
foundation codes. See [QR Code
(Wikipedia)](https://en.wikipedia.org/wiki/QR_code) & [URs](/ur/).
* **Seed Recovery.** A method for recovering a seed that has been
lost, thanks to proactive backups. Uses _Sharding_ or other similar
techniques. See [Seed Recovery](/seedrecovery/).
* **Share.** A fraction of a seed or a key created by an algorithm
such as Shamir's Secret Sharing or VSS. Intended to improve
_Resilience_ of data and enable _Seed Recovery_. See [SSKR](/sskr/).
* **Sharding.** The process of creating _Shares_ from seeds or
keys. See [SSKR](/sskr/).
* **Uniform Resources (URs).** An Encoding Specification of a _URI_
for data. It is created by representating data as _CBOR_ and then
encoding it with minimal _Bytewords_. URs are also built to allow
efficient _Encoding_ as _QR Codes_. URs allow for interoperable
communication. See [URs](/ur/).
* **Uniform Resource Identifier (URI).** A unique sequence for
identifying a resource. A _UR_ is a URI. See [URI
(Wikipedia)](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier).


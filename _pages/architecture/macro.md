---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Macro-architecture
hide_description: true
classes:
  - wide
permalink: /architecture/macro/
sidebar:
  nav:
    - archmacro
    - architecture
---

The Gordian Macro-architecture is built on a design pattern of
[functional partition](/architecture/heterogeneity/): separating
services from each other. Doing so improves privacy and security by
reducing the value of honeypots and also improves functional design by
ensuring that each application is precisely and concisely able to
perform a specific function. Many of the Gordian reference apps are
actually microservices, intended to perform small and simple but
necessary activities as part of the blockchain ecosystem.

* **Service.** An app or server that fulfills some large-scale need
for digital-asset management, most commonly the ability to store
private information, create transactions, sign transactions, or send
transactions. *Examples:* [Gordian Seed
Tool](https://github.com/BlockchainCommons/GordianSeedTool-iOS)
(signing device), Gordian Coordinator (transaction coordinator).

* **Microservice.** An app or server that fulfills a small need in the
overall Gordian ecosystem such as price lookups. *Examples:*
[SpotBit](https://github.com/BlockchainCommons/spotbit).

Using functionally partitioned services, the Gordian architecture
creates a powerful and safe methodology for financial, data, and
information operations on the internet. It also creates an ecosystem
that allows for the inclusion of multiple developers, each producing
their own applications that are all interoperable thanks to usage of
Blockchain Commons' interoperable specifications. This improves the
overall architecture through these competitive designs and ensures
survivability of the model as a whole.

## Macro-Architectural Transport

With a properly partitioned ecosystem, the transport & communication
methodology among services becomes less important. Nonetheless, some
transport methods may improve privacy and security.

* **Networked.** The standard methodology for transport: direct
network communications, presumably encryped with TLS for security.

* **[AirGap](/airgap/).** A physical gap between services or
microservices, where either side might not even be networked. This
improves the security of the non-networked side of the communication,
as it will not be vulnerable to network attacks. A service protected
by an AirGap communicates through the reading of QR Codes or through
transmission of data on MicroSD cards, NFC tags, or other removable
media. See our [Airgap Overview](/airgap/).

* **[TorGap](/torgap/).** A link between services or microservices that connect
via [Tor](https://www.torproject.org/). Though both services likely
remain fully networked, they are anonymous to each other. This
improves privacy and also deters network attacks due to the
anonymity. See our [Torgap
Overview](/torgap/).

## Reference Apps

Reference apps are exemplar programs meant to show how the Gordian
architecture and its specifications are meant to work, as
demonstrations for developers. Our most up-to-date and supported
reference apps include:

* [**Gordian Seed Tool**](https://github.com/BlockchainCommons/GordianSeedTool-iOS) — An example of a seed vault and signing device, demonstrating how airgaps work.
* [**Gordian Coordinator**]() — An example of an on-network transaction coordinator, connected to airgapped devices such as Gordian Seed Tool. (Pending.)
* [**SpotBit**](https://github.com/BlockchainCommons/spotbit) — An example of a TorGapped microservice, providing pricing services.

## How the Gordian Maco-Architecture Supports Gordian Principles

The Gordian Architecture supports the four [Gordian
Principles](/principles/) as follows:

* **Independence.** Users can choose which applications to use within an open ecosystem.
* **Privacy.** The partioning of services reduces data collation. Even stronger protections can be created by Airgaps and Torgaps.
* **Resilience.** The paritioned design minimizes Single Points of Compromise.
* **Openness.** Standard data-format specifications such as [UR](/ur/) allow anyone to connect to the ecosystem

## Links

**Architectural Nodes:**

* [**Gordian Architecture Apps**](/architecture/macro/apps/) — Example of Macro-architecture for Bitcoin
* [**Gordian Architecture Roles**](/architecture/macro/roles/) - Roles in the Bitcoin Macro-architecture example

**Architectural Edges:**

* [**Airgap**](/airgap/) - Full separation
* [**Torgap**](/torgap/) - Privacy separation

**Further Explanations:**

* [**Gordian Architecture FAQ**](/macro/architecture/faq/)
* [**Gordian Lexicon**](/architecture/lexicon/)

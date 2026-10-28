---
cover: true
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/blueprint.jpg
  og_image: /assets/images/bc-card.jpg
title: "Touchpoints - Protecting Seeds"
hide_description: false
classes:
  - wide
permalink: /touchpoints/seeds/
sidebar:
  nav:
    - touchpoints
    - technology
    - resources
---

**Target Technology:** Cryptocurrency & Digital Identity<br>
**Target Audience:** Crypto Developers

_Seeds are the foundations for the keys that protect cryptocurrencies
and digital assets. They must be protected so that they're secure
(avoiding compromise) and resilient (avoiding loss). This touchpoint
details Blockchain Commons tutorials and technologies that accomplish
these goals._

# I: Resilience Tutorials (for Users & Developer)

_The following user-level documents demonstrate best practices for
protecting seeds, some of them using various Blockchain Commons
technologies._

## ![](/assets/badges/smart-custody.png) Smart Custody

**Book & Articles.** Our [Smart Custody
book](https://www.smartcustody.com/) was our initial release on how to
protect your seeds with a variety of devices available at the time of
writing. We've since supplemented it with articles on technologies
such as multisig and Shamir's Secret Sharing, which were not easily
available at the time of our initial writing.

This is the simple beginning to protecting seeds, using systematic
risk assessment and guarding against the threats that endanger you
personally.

* For more, see [**Smart Custody Book**](https://bit.ly/SmartCustodyBookV101) (PDF download)
* For more, see [**Designing Multisigs**](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Multisig.md) (repo article)
* For more, see [**Designing SSKR Share Scenarios**](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/SSKR-Sharing.md) (repo article)
* For more, see [**Using Timelocks**](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Timelocks.md) (repo article)
* For more, see [**Multisig Self-Custody Scenario**](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Scenario-Multisig.md) (repo article)

## ![](/assets/badges/seeds.png) Protecting Seeds

**CLI Guides.** Blockchain Commons has produced a number of CLI guides
that demonstrate how to generate and/or protect seeds for use
generally or with specific blockchains such as Bitcoin and Ethereum.

It does so via a variety of methods, including storing metadata in a
Gordian Envelope, encryption, and sharding shares of a secret using
SSKR envelopes.

* For a general guide, see [**Protecting Your Seeds**](/envelope/cli/seed/)
* For Bitcoin seeds, keys, and descriptors, see [**Protecting Your Bitcoin Secrets**](https://learningbitcoin.blockchaincommons.com/10_0_Working_with_Secrets/)
* For Ethereum seeds and keys, see [**Protecting Your Ethereum Secrets**](/chains/ethereum/cli/seeds/)

# II: Protective Technologies (for Developers)

_Though our Smart Custody guide includes general best practices, other
seed protection is offered by specific Blockchain Commons
technologies._

## ![](/assets/badges/envelope.png) Gordian Envelope

**Data Format.** Envelope is a smart document format that allows for
the structured storage of information in a radically recursive format,
with the ability to elide, encrypt, compress, or lock any information
within the envelope.

Gordian Envelope is an excellent format for storing seeds (and other
secrets such as private keys, or even just sensitive information) not
just for its elision and encryption abilities, but also because the
secrets can be stored along with metadata, which can improve the
resilience of secrets by offering hints on how to recover them or what
to do with them when they're recovered.

* For more, see [**Envelope**](/envelope/)
* For more, again see [**Protecting Your Seeds**](/envelope/cli/seed/)

## ![](/assets/badges/sskr.png) Sharded Secret Key Reconstruction (SSKR)

**Data Resilience.** Storing a secret in just one place makes it
vulnerable to a Single Point of Failure (SPOF), but storing it in lots
of places makes it vulnerable to a Single Point of Compromise
(SPOC). Shamir's Secret Sharing offers a great middle ground: it
shards a secret into multiple shares. With a threshold such as 2-of-3,
no single share can compromise the secret, but a share can be lost and
the secret is preserved.

SSKR is Blockchain Commons' expansion of Shamir's Secret Sharing that
allows for thresholds of not just individuals, but also groups. It's
also available in a security-reviewed library.

Envelopes can be locked with SSKR shares, where the content of the
envelope can only be recovered if a threshold of envelopes, each with
an SSKR share, are combined. This is an SSKR Envelope.

SSKR is also built into a number of Blockchain Commons' [reference
apps](https://developer.blockchaincommons.com/apps/), including
`envelope-cli`, `seedtool-cli`, and the iOS Seedtool.

* For more, see [**SSKR**](/sskr/)
* For reference apps, see the [**Apps page**](/apps/), or the list below.

## ![](/assets/badges/ur.png) Uniform Resources (URs)

**Data Format.** URs are a data format that encodes binary CBOR data
into text strings. Because envelopes are CBOR, this means that they
can be encoded as text strings, which allows for their long-term
storage. This is done using [ByteWords](/bytewords/).

URs are also self-describing. For example, any envelope UR starts with
`ur:envelope/". This makes URs resilient because it's easy to see what
they are (and therefore how to recover them) into the far future.

* For more, see [**URs**](/ur/)
* For moer, see [**A Guide to Using URs for Envelopes**](/ur/envelope/)

## ![](/assets/badges/csr.png) Collaborative Seed Recovery (CSR)

**Data Resilience.** CSR utilizes SSKR to shard a secret and then
stores those secrets on independent remote servers. This offers
considerable resilience over _ad hoc_ methods of share storage (such
as keeping them with relatives and friends).

Though Blockchain Commons has prototyped the Gordian Depository, which
allows for the exchange of seed envelopes using
[GSTP](/envelope/gstp/), to date the technology remains largely
theoretical because of the need for multiple, independent servers
willing to accept the liability of seed storage.

* For more, see [**CSR**](/csr/)
* For more, see [**Gordian Depository**](https://github.com/BlockchainCommons/bc-depo-rust) (repo)
* For more, see [**Gordian Depository API**](https://github.com/BlockchainCommons/bc-depo-api-rust) (repo)

# III: Reference Applications (for Developers)

_Several reference apps demonstrate the usage of Blockchain Commons'
seed protection technologies as well as other best practices._

## ![](/assets/badges/seed-tool.png) iOS Seedtool

**iOS App.** iOS Seedtool demonstrates the creation of seeds and the
backup of seeds as envelopes with notes metadata, as URs, as SSKR
shares, and as SSKR envelopes.

* For more, see [**iOS Seed Tool in the Apple Store**](https://apps.apple.com/us/app/gordian-seed-tool/id1545088229) (app store)
* For more, see [**iOS Seed Tool on GitHub**](https://github.com/BlockchainCommons/GordianSeedTool-iOS) (repo)
* For more, see [**Gordian Seed Tool Manual**](/apps/gst-ios/manual/)

## ![](/assets/badges/envelope-cli.png) Envelope-CLI

**Rust CLI App.** `envelope-cli` parallels iOS Seedtool from the
command line. It similarly allows for the creation of seeds as
envelopes with freeform metadata and the backup of seeds as URs and as
SSKR envelopes.

* **`cargo install bc-envelope-cli` to install**
* For more, see [**CLI Envelope on GitHub**](https://github.com/BlockchainCommons/bc-envelope-cli-rust) (repo)


## ![](/assets/badges/keytool-cli.png) Keytool-CLI

**Python CLI App.** `keytool-cli` is a transformer, allowing seeds to
be converted into a number of other formats, most notably including
account keys and descriptors for Bitcoin.

Keytool is somewhat deprecated, as it's never been updated to Rust,
but remains a crucial reference for seed storage.

* For more, see [**CLI Keytool on GitHub**](https://github.com/BlockchainCommons/keytool-cli) (repo)

## ![](/assets/badges/seed-tool-cli.png) Seedtool-CLI

**Rust CLI App.** `seedtool-cli` focuses on the creation of seeds,
which can be output as URs, as Envelopes, as SSKR shares, and as SSKR
envelopes (plus a number of other formats).

* **`cargo install seedtool-cli` to install**
* [**CLI Seed Tool on GitHub**](https://github.com/BlockchainCommons/seedtool-cli-rust)

# IV. Sample Seeds

_Blockchain Commons uses the following sample seeds in its examples and docs._

## ![](/assets/badges/seeds.png) 128-Bit seed

{% include seed-128.md %}   

_These PDFs, which were exported from iOS SeedTool, demonstrate our
best practices of documenting seeds and SSKR shares, including [object
identity blocks](/oib/), [Lifehash](/lifehash/),
[Bytewords](/bytewords/) to help identification, and metadata in the
form of notes to provide more content (and in some cases help recovery)._

<table width="100%">
  <tr width="100%">
    <td width="100%">
      <a href="/assets/pdfs/seedtool-seed.pdf">
        <img src="/assets/pdfs/seedtool-seed.jpg">
      </a>
    </td>
    <td width="100%">
      <a href="/assets/pdfs/seedtool-sskr.pdf">
        <img src="/assets/pdfs/seedtool-sskr.jpg">
      </a>
    </td>
  </tr>
</table>

## ![](/assets/badges/seeds.png) 256-Bit seed

{% include seed-256.md %}   

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Envelope Use Cases Listing
hide_description: true
classes:
  - wide
permalink: /envelope/use-cases/summary/
sidebar:
  nav: envelopeuse
---

The [Gordian Envelope](/envelope/) Smart Document system is a powerful new
method of data storage and transmission that focuses on protecting the
privacy of its contents through cryptographic functions such as
signatures, elision, and inclusion proofs.

But what does that mean? Why would you use it? To answer these
questions we've written a set of use cases that not only demonstrate
many innovative uses for envelopes, but also show precisely
how those envelopes would be structured — because these use cases
aren't theoretical, but instead real possibilities with the current
iteration of the Gordian Envelope specification.

Most of the following use cases are offered progressively: additional
use cases build on earlier ones, expanding the fundamental ideas with
new functionality in each example. (That functionality is listed as
part of each use case's name.)

For a broader overview of these use cases and some discussions of what
they have in common, see our [use case overview](/envelope/use-cases/).

## [Educational & Credential Industry Use Cases](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md)

Educational use cases demonstrate how Gordian Envelope can transmit sensitive student information, including educational credentials.

#### [Part One: Official Credentials](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#part-one-official-credentials)

The first set of use cases demonstrates how recognized issuers can create and use credentials.

1. [**Danika Proves Her Worth (Credentials, Signature)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#1-danika-proves-her-worth-credentials-signature) — Issuing authenticated credentials with Gordian Envelope
2. [**Danika Restricts Her Revelations (Elision)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#2-danika-restricts-her-revelations-elision) — Using elision to allow a holder to selectively hide Eenvelope contents.
3. [**Thunder & Lightning Spotlights Danika (Third-Party Repackaging)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#3-thunder--lightning-spotlights-danika-third-party--repackaging) — Adding content to an existing envelope & republishing it.

#### [Part Two: Web of Trust Credentials](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#part-two-web-of-trust-credentials)

Individuals may instead want to create peer-to-peer credentials.

4. [**Omar Offers an Open Badge (Web of Trust Credentials)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#4-omar-offers-an-open-badge-web-of-trust-credentials) — Creating a credential based on personal authentication.

#### [Part Three: Herd Privacy Credentials](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#part-three-herd-privacy-credentials)

Another possibility for credential release is through large data dumps that allow the user to stay in control over whether they're ever revealed.

5. [**Paul Privately Proves Proficiency (Herd Privacy)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#5-paul-privately-proves-proficiency-herd-privacy) — Creating highly private credentials.
6. [**Paul Proves Profiency with Improved Privacy (Herd Privacy with Non Correlation)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#6-paul-proves-proficiency-with-improved-privacy-herd-privacy-with-non-correlation) — Using design formats to improve herd privacy.
7. [**Burton Bank Avoids Toxicity (Herd Privacy with Selective Correlation)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-educational.md#7-burton-bank-avoids-toxicity-herd-privacy-with-selective-correlation) — Avoiding toxic data by selective correlating unrevealed information.

## [Wellness Use Cases](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md)

Wellness use cases demonstrate how Gordian Envelope can protect highly sensitive information while still supporting its sharing in a variety of ways.

#### [Part One: Personal Sensor Data](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#part-one-personal-sensor-data)

How to structure and protect sensitive data with Gordian Envelope.

1. [**Nadia Gets Fit (Metadata)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#1-nadia-gets-fit-metadata) — Creating a structure for sensitive data.
2. [**Nadia Protects Her Privacy (Encryption)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#2-nadia-protects-her-privacy-encryption) — Protecting sensitive data with encryption.
3. [**Nadia Protects Her Key (SSKR)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#3-nadia-protects-her-key-sskr) — Ensuring the resilience of closely held data.

#### [Part Two: Personal Shared Sensor Data](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#part-two-personal-shared-sensor-data)

The value of Wellness information is that it can help you improve your health. Gordian Envelope can make sure that doesn't compromise your privacy.

4. [**Nadia Hearts Her Doctor (Elision)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#4-nadia-hearts-her-doctor-elision) — Eliding data so that you don't pass on person or health information that could be detrimental.
5. [**Nadia is a Bit Remote (Multi-Permit)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#5-nadia-is-a-bit-remote-multi-permit) — Using multipermits to give third-party monitors regular access to your data.
6. [**Nadia Steps Up (Signature)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#6-nadia-steps-up-signature) — Using signatures to prove the provenance of your data.
7. [**Nadia Foils Fusion (Salting)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#7-nadia-foils-fusion-salting) — Using salt to prevent sensor fusion.

#### [Part Three: Clinically Shared Sensor Data](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#part-three-clinically-shared-sensor-data)

Your personal health data can also help our society (and its public health), and Gordian Envelope can ensure that your privacy is _still_ safe.

8. [**Nadia Gets Clinical (Elision, Third-Party Encryption)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#8-nadia-gets-clinical-elision-third-party-encryption) — Contributing to a clinical trial and protecting _everyone_ with encryption.
9. [**Nadia Proves Her Worth (Proof of Inclusion)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#9-nadia-proves-her-worth-proof-of-inclusion) — Proving your contribution with hash-based elision.
10. [**Nadia Becomes a Number (Anonymization, Provenance)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#10-nadia-becomes-a-number-anonymization-provenance) — Blurring data while maintaining its provenance.

#### [Part Four: The COVID-19 Appendix](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#part-four-the-covid-19-appendix)

11. [**Nadia Goes Viral (Herd Privacy)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-wellness.md#11-nadia-goes-viral-herd-privacy) — Contact tracing without the privacy problems.

## [Software Industry Use Cases](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md)

Software use cases demonstrate how the structure of Gordian Envelope can innovate procedures requiring signing, such as software releases.

#### [Part One: Chained Signing](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#part-one-chained-signing)

Gordian Envelope can automate releases of data over time by creating and updating a root of trust within the Envelope.

1. [**Casey Codifies Software Releases (Multiple Signatures, Structured Data)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#1-casey-codifies-software-releases-multiple-signatures-structured-data) — Structuring release data and authenticating it with multiple signatures.
2. [**Blockchain Everyday Confirms Casey (Repackaging Data, Third-Party Verification)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#2-blockchain-everyday-confirms-casey-repackaging-data-third-party-verification) — Adding additional levels of data verification by repackaging envelopes.
3. [**Casey Chains His Software Releases (Chained Data)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#3-casey-chains-his-software-releases-chained-data) — Using envelope structure to automate the release of future data.
4. [**Casey Checks Compliance (Attestation, Metadata)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#4-casey-check-compliance-attestation-metadata) — Adding signed metadata to a structured data set.
5. [**Casey Changes Up His Software Releases (Chained Changes)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#5-casey-changes-up-his-software-releases-chained-changes) — Using structured data to announce changes in trust over time.

#### [Part Two: Anonymous Signing](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#part-two-anonymous-signing)

Authentication can be combined with elision to allow for signing that is pseudonymous yet validated.

6. [**Amira Signs Anonymously (Anonymous Signature, Web of Trust)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#6-amira-signs-anonymously-anonymous-signature-web-of-trust) — Using a Web of Trust to verify a signature made pseudonymous through elision.
7. [**Amira Reveals Her Identity (Progressive Trust)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-software.md#7-amira-reveals-her-identity-progressive-trust) — Removing elision over time to gain reputation from previously published works.

## [Data Distribution Use Cases](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md)

Data distribution is crucial for a variety of use cases, from the supply chain to the medical industry. The use cases in this section take as an example the distribution of user data based on a [WebFinger](https://www.rfc-editor.org/rfc/rfc7033.html)-like protocol, highlighting the advantages of building a privacy-first data structure.

#### [**Part One: Public CryptFinger**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#part-one-public-cryptfinger)

The most fundamental usage of Gordian Envelope is to publish verifiable data that is entirely public.

1. [**Carmen Makes Basic Information Available (Structured Data)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#1-carmen-makes-basic-info-available-structured-data) — Using Gordian Envelope to release structured data.
2. [**Carmen Makes CryptFinger Verifiable (Signatures)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#2-carmen-makes-cryptfinger-verifiable-signatures) — Adding authentication to allow for data portability.
3. [**Carmen Adds Chronology to CryptFinger (Timestamp)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#3-carmen-add-chronology-to-cryptfinger-timestamp) — Easily expanding verifiable data with improtant metadata.

#### [**Part Two: Private CryptFinger**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#part-two-private-cryptfinger)

Building on the elision capabilities of Gordian Envelope can produce data that is not (initially) viewable by everyone, but which remains provable and ultimately releasable.

4. [**Carmen Protects CryptFinger (Elision)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#4-carmen-protects-cryptfinger-elision) – Eliding data for some viewers and not others.
5. [**Carmen Makes CryptFinger Provable (Inclusion Proof)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#5-carmen-makes-cryptfinger-provable-inclusion-proof) — Using inclusion proofs to purposefully allow selective correlation.
6. [**Carmen Makes CryptFinger Progressive (Progressive Trust)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#6-carmen-makes-cryptfinger-progressive-progressive-trust) — Building a progressive trust algorithm using selective correlation.

#### [**Part Three: Herd Private CryptFinger**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#part-three-herd-private-cryptfinger)

A herd-privacy variant of CryptFinger's design can allow users to maintain their privacy as much as they wish, as discussed in this overview.

#### [**Part Four: Data Distribution Advancements**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#part-four-data-distribution-advancements)

There are many other options for cryptographic data distribution, building on signatures, provability, repackaging, and encryption permits, as is discussed in this further overview.

## [Financial Industry Use Cases](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md)

Although the financial industry can make many uses of Gordian Envelope to preserve assets, these use cases concentrate on self-sovereign control of assets: how an individual can use Gordian Envelope to make sure he doesn't lose them. 

#### [Part One: Self-Sovereign Storage of Secrets](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md#part-one-self-sovereign-storage-of-secrets)

Envelopes can simply and securely store digital assets.

1. [**Sam Stores a Secret (Secure Storage with Metadata)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md#1-sam-stores-a-secret-secure-storage-with-metadata) — Using metadata in an envelope to increase the resilience of stored assets.
2. [**Sam is Salty about Compliance (Non-Correlation)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md#2-sam-is-salty-about-compliance-non-correlation) — Salting data to eliminate correlation dangers.
3. [**Sam Gets Paranoid about Privacy (Wrapped Encryption)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md#3-sam-gets-paranoid-about-privacy-wrapped-encryption) — Improving privacy at the cost of resilience.

#### [Part Two: Raising Resilience of Restricted Results](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md#part-two-raising-resilience-of-restricted-results)

However, resilience can be further improved with Gordian Envelope permits.

4. [**Sam Gets Rigorous about Resilience (SSKR Permit)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md#4-sam-gets-rigorous-about-resilience-sskr-permit) — Sharding keys to improve resilience.
5. [**Sam Promotes a Partner (Multi-Permit)**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-assets.md#5-sam-promotes-a-partner-multi-permit) — Improving resilience through multiple permits.

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-identity.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Clubs
tagline: "Autonomous, Cryptographic Data Transmission"
hide_description: true
classes:
  - wide
permalink: /clubs/
redirect_from:
  - /club/
sidebar:
  nav:
    - clubs
    - identity
    - technology
---

<div class="hexline hexgrid72">
  <div class="hex11">
    <a href="/ssi/">
      <img src="/assets/badges/ssi.png">
    </a>
  </div>
  <div class="hex12top">
    <a href="/identity/">
      <img src="/assets/badges/cat-identity-half.png">
    </a>
  </div>
  <div class="hex21 opaqued">
    <a href="/xid/">
      <img src="/assets/badges/xid.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/identity/attestation/">
      <img src="/assets/badges/attestations.png">
    </a>
  </div>
  <div class="hex32 opaqued">
    <a href="https://learningxids.blockchaincommons.com/">
      <img src="/assets/badges/learning-xids.png">
    </a>
  </div>
  <div class="hex41 opaqued">
    <a href="/identity/fair-witness/">
      <img src="/assets/badges/fair-witness.png">
    </a>
  </div>
  <div class="hex51 opaqued">
    <a href="/cliques/">
      <img src="/assets/badges/cliques.png">
    </a>
  </div>
  <div class="hex52">
    <a href="/garner/">
      <img src="/assets/badges/garner.png">
    </a>
  </div>
  <div class="hex61 highlighted">
    <a href="/clubs/">
      <img src="/assets/badges/clubs.png">
    </a>
  </div>  
  <div class="hex71 opaqued">
    <a href="/ppp/">
      <img src="/assets/badges/ppp.png">
    </a>
  </div>
</div>

_Gordian Clubs are **autonomous cryptographic objects** that provide read and write access to data using [permits](/envelope/features/#encryption-support).  Gordian Clubs publish individual Editions, which are updated data files verified by the publisher (or some publishing group)._

_Here's a bit more on the major elements:_

* **Autonomous.** Gordian Clubs are wholly self-contained. There's no phoning home and no verification with an external source.
* **Cryptographic.** Access to Gordian Clubs is determined by mathematical means such as private keys and [SSKR](/sskr/) shares.
* **Object.** A Gordian Club is a [Gordian Envelope](/envelope/) containing content, permits, and a [provenance mark](/provemark/).
* **Envelope.** Gordian Envelope allows for the "smart" storage of information as a recursive set of semantic triplet.
* **Permit.** Envelopes can be encrypted, and permits allow for the decryption of that data in a variety of ways.
* **Edition.** A Gordian Club publishes Editions, which are linked by [provenance marks](/provemark/) and signed by publishers.
* **Publishing Group.** Though an edition can be signed by an individual, it can also be signed by a [FROST threshold](/frost/).

_Gordian Clubs leverage cryptographic permits (SSKR shares, FROST
thresholds, XID keys), tamper-evident provenance chains, and offline
operation without servers. You may want to read more on the specific
[technologies](/clubs/technology/) used to create Gordian Clubs, as
well as their [history](/clubs/history/)._
  
## Why Are Gordian Clubs Important?

_Preserving Agency When Infrastructure Fails._

Gordian Clubs embody a radical proposition: that communities can self-organize, self-govern, and exchange their own information through cryptography alone, without asking permission, without trusting third parties, without requiring infrastructure that can be attacked, corrupted, or shut down. They trade the convenience of centralized control for the dignity of true autonomy, the requirement of asking permission for the possibility of self-determination.

Use cases include:
    
* Dissidents organizing without surveillance
* Journalists privately talking with sources
* Long-term archival that outlives companies
* Credentials that must survive institutional collapse
* Emergency coordination during outages
* Cooperative governance for non-profits
* Identity without the need for state permission

See ["The Power of Autonomy"](/clubs/autonomy/) for more.

## How Do Gordian Clubs Work?

### Gordian Club Structure

A Gordian Club is a layered onion, with some information widely readable and some not:

1. **Club Envelope:** dCBOR-encoded structure containing everything.
2. **Public Metadata:** Visible information (club ID, version, etc.).
3. **Encrypted Payload:** The actual club content and member data.
4. **Access Layer:** Collection of permits for different entry methods.
5. **Governance Layer:** Signatures and provenance mark.

### Gordian Club Read Capability

Gordian Clubs can be read if a user can unlock the symmetric key locked by a permit, which requires them to have the access method for any permit attached to a Gordian Club.

1. **Public Key Lock.** Requires unlocking with the corresponding private key.
2. **XID Lock.** Also requires a private key to unlock, but a XID is provided as a metadata "clue".
3. **Password Lock.** Requires a password to unlock (which is stretched into a corresponding symmetric key).
4. **SSKR Lock.** Can be unlocked if a threshold of shares of the SSKR sharding are brought together, which can be done asynchronously & offline.
5. **Adaptor Signature.** Allows delegation of read capability from one user to another for a specific Edition.

See ["Gordian Clubs Delegation & Cryptographic ocaps"](/clubs/ocaps/) for more on the research into delegation and ocaps, which is currently a powerful but less proven methodology.

### Gordian Club Write Capability

Gordian Clubs publish "Editions", which are encrypted data packets. New "Editions" can be published over time through "updates".
The authenticity of an update is ensured by two things:

1. **Provenance Mark.** A provenance mark verifies that two Editions belong to the same group and that there are no missing Editions between them.
2. **Signature.** A publisher signs each Edition. This could be a singular publisher, but more powerfully it's the threshold of a FROST group.

Again, see [Gordian Technology](/clubs/technology/) for how all of
this fits together.

## Clubs Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>Clubs Overview & Demo:</b>

{% include video id="bqxW7iDk4QI" provider="youtube" %}

    </td>    
  </tr>
</table>  

## Libraries

{% include lib-clubs.md %}

## Clubs Links

**Gordian Clubs:**

* [Overview: Preserving Agency When Infrastructure Fails](https://www.blockchaincommons.com/musings/musings-clubs/)
* [History Overview: Xanadu](/clubs/history/)
* [Tech Overview: Gordian Stack](/clubs/tech/)
* [Tech Overview: OCAPS](/clubs/ocaps/)
* [The Power of Autonomy](/clubs/autonomy/)

**Demos:**

* [Gordian Clubs Meeting (10/25)](https://developer.blockchaincommons.com/meetings/2025-10-clubs/)
* [Demo Script](https://github.com/BlockchainCommons/clubs-cli-rust?tab=readme-ov-file#demonstration-script)
* [Demo Log](https://github.com/BlockchainCommons/clubs-cli-rust/blob/master/demo-log.md)

**Other Presentations:**

* ["Beyond Bitcoin"](https://developer.blockchaincommons.com/assets/pdfs/2025-10-tabconf-bb.pdf) (PDF)
  
**Software:**

* [clubs-rust library](https://github.com/BlockchainCommons/clubs-rust)
* [clubs-cli-rust](https://github.com/BlockchainCommons/clubs-cli-rust)

**Secure Comm PoCs:**

* [Garner Tor Onion Service](/garner/)
* [Hubert Dead-Drop Hub](/hubert/)

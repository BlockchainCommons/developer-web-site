---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Gordian Clubs
hide_description: true
classes:
  - wide
permalink: /clubs/
sidebar:
  nav: clubs
---

## Overview

Gordian Clubs are **autonomous cryptographic objects** that provide read and write access to data using [permits](/envelope/features/#encryption-support).  Gordian Clubs publish individual Editions, which are updated data files verified by the publisher (or some publishing group).

Here's a bit more on the major elements:

* **Autonomous.** Gordian Clubs are wholly self-contained. There's no phoning home and no verification with an external source.
* **Cryptographic.** Access to Gordian Clubs is determined by mathematical means such as private keys and [SSKR](/sskr/) shares.
* **Object.** A Gordian Club is a [Gordian Envelope](/envelope/) containing content, permits, and a [provenance mark](/provemark/).
* **Envelope.** Gordian Envelope allows for the "smart" storage of information as a recursive set of semantic triplet.
* **Permit.** Envelopes can be encrypted, and permits allow for the decryption of that data in a variety of ways.
* **Edition.** A Gordian Club publishes Editions, which are linked by [provenance marks](/provemark/) and signed by publishers.
* **Publishing Group.** Though an edition can be signed by an individual, it can also be signed by a [FROST threshold](/frost/).

You may want to read more on the specific [technologies](/clubs/technology/) used to create Gordian Clubs, as well as their [history](/clubs/history/).
  
## Why Are Gordian Clubs Important?

Gordian Clubs allows for the distribution and storage of information when network infrastructure can't be trusted. Use cases include:

* Dissidents organizing without surveillance
* Long-term archival that outlives companies
* Emergency coordination during outages

The focus on cryptography ensures that the data remains decentralized and self-sovereign. This is in contrast to access that was [historically](/clubs/history/) controlled by centralized administration, introducing the possibility of corruption or censorship. Gordian Clubs avoid\ these issues. 

## Links

**Gordian Clubs:**

* [History](/clubs/history/)
* [Gordian Technology](/clubs/technology/)

**Demos:**

* [Gordian Clubs Meeting (10/25)](https://developer.blockchaincommons.com/meetings/2025-10-clubs/)
* [Demo Script](https://github.com/BlockchainCommons/clubs-cli-rust?tab=readme-ov-file#demonstration-script)
* [Demo Log](https://github.com/BlockchainCommons/clubs-cli-rust/blob/master/demo-log.md)

**Software:**

* [clubs-rust library](https://github.com/BlockchainCommons/clubs-rust)
* [clubs-cli-rust](https://github.com/BlockchainCommons/clubs-cli-rust)

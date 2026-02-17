---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Garner: Tor Onion Service"
hide_description: true
classes:
  - wide
permalink: /garner/
sidebar:
  nav: stack-core
---

## Overview

A developer tool that provides a Tor endpoint for self-sovereign identity. It serves static files over HTTP to allow the retrieval of authenticated identity documents.

## Why is Garner Important?

Garner serves self-sovereign identity documents. It has five major strengths, many of them advantages over use of `HTTPS` or even bare `tor`.

**Self-Sovereignty:** The ultimate goal of Garner is self-sovereignty. [XIDs](/xid/) allow users to have a truly self-sovereign identity that they can issue, hold, and redact as they see fit. Garner offers the next step because it allows them to also distribute their own identity documents in a self-sovereign way.

**Accessibility:** Garner is easy to install: it's just a cargo install. It's also easy to run: there are just three commands, one to generate a keypair, one to start a server, and one for a remote user to get your content. This is a huge accessibility advance over HTML, which requires the setup of complex Apache config files and the acquisition of a certificate, all of which will be beyond the average user.

**Privacy:** Because Garner runs across the Tor network, everything is private. Your identity serving address is hidden (protecting any pseudonymous identities) and the requester's address is hidden. Perhaps most importantly, this makes the identity documents served through Garner censorship-resistant. As long as Tor is available, no attacker can prevent you from serving them or the requester from asking for them.

**Authentication:** Garner builds its Tor address from the private key you supply, which means that you're running a Garner server (which other people connect to with the corresponding public key) implicitly offers live proof of key control. This is very powerful authentication: remote users know that you controlled the private key when the server was started. Not only does this avoid the need for external dependencies like DNS or a Certificate Authority (CA), but it also protects from stale-signature and post-compromise replay attacks.

**Security:** Garner is purposefully very constrained. It is not a general-purpose web-server, but only a limited identity-document server. This is an intentional design to minimize its attack surface. Garner was also built in close coordination with Tor technology through use of the [official Arti crate](https://gitlab.torproject.org/tpo/core/arti/), which should improve its security level.

## How Does Garner Work?

A user runs the [Garner CLI](https://github.com/BlockchainCommons/garner-rust) to create a keypair. They distribute the public key to people who they want to send identity documents to and keep the private key safe. They then use the private key to start up the Garner server, which generates a deterministic address based on the key. Users with the public key can then use their own version of Garner to access that server with the public key (or with the deterministic address once they know it). This allows them to download identity documents from the server that are implicitly authenticated and verified as belonging to the holder of the private key. 

This allows the pseudonymous, censorship-resistant, self-sovereign distribution of identity documents. Its primary use case is serving [Gordian Envelope](https://developer.blockchaincommons.com/envelope/), including [XIDs](https://developer.blockchaincommons.com/xid/) and [Gordian Clubs](https://developer.blockchaincommons.com/clubs/). However, it can distribute any type of identity document including W3C DID documents, Verifiable Credentials, VC Controller documents, or any other sort of signed content.

## Garner Links

**Software:**

* [**garner-cli**](https://github.com/BlockchainCommons/garner-rust?tab=readme-ov-file)

## Related Technology

* [**"The Exodus Protocol"**](https://www.blockchaincommons.com/musings/musings-exodus-protocol/) (Musings)
* [**Hubert**](https://developer.blockchaincommons.com/hubert/)

## Related Use Cases

* [**CryptFinger**](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/envelope-usecases-data.md#part-one-public-cryptfinger) (GitHub, out-of-date)

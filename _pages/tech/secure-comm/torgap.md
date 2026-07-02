---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/headers/tech-securecomm.jpg
  og_image: /assets/images/bc-card.jpg
title: Torgaps
tagline: Security through Privacy
hide_description: true
classes:
  - wide
permalink: /torgap/
redirect_from:
    - /torgaps/
sidebar:
  nav:
    - securecomm
    - technology
---

<div class="hexline hexgrid71">
  <div class="hex11">
    <a href="/airgap/">
      <img src="/assets/badges/airgap.png">
    </a>
  </div>
  <div class="hex12top highlighted">
    <a href="/secure-comm/">
      <img src="/assets/badges/cat-comm-half.png">
    </a>
  </div>
  <div class="hex21 highlighted">
    <a href="/torgap/">
      <img src="/assets/badges/torgap.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/lifehash/">
      <img src="/assets/badges/lifehash.png">
    </a>
  </div>
  <div class="hex41 opaqued">
    <a href="/oib/">
      <img src="/assets/badges/oib.png">
    </a>
  </div>  
  <div class="hex51 opaqued">
    <a href="/animated-qrs/">
      <img src="/assets/badges/animated-qr.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/garner/">
      <img src="/assets/badges/garner.png">
    </a>
  </div>
 <div class="hex71 opaqued">
    <a href="/hubert/">
      <img src="/assets/badges/hubert.png">
    </a>
  </div>
</div>


_A Torgap is a security and privacy architecture model for creating
gaps between connected apps and microservices using Tor.  It's
supported by [QuickConnect](/quickconnect/) and Blockchain Commons'
[Gordian Architecture](/architecture/)._

## Why is Torgap Important?

Torgap supports privacy, service anonymity, identity psuedonymity,
non-correlation, censorship-resistance, and seperation-of-interests
and reduces single-points-of-failure. (Basically, it allows for
networked connections without sharing all of the information about the
participants, reaping various rewards as a result.)

## How Does Torgap Work?

The following network diagram demonstrates an architecture that mixes
airgaps and torgaps to produce a well-protected network architecture.

![](/assets/images/appmap-black.png)

In this example:
* The remote iOS [Gordian Wallet](https://github.com/blockchainCommons/gordianwallet-ios) app uses a Torgap to communicate with the Mac [Gordian Server](https://github.com/BlockchainCommons/GordianServer-macOS) bitcoin full-node located at home, which confirms Bitcoin transactions.
* The remote iOS [Gordian Wallet](https://github.com/blockchainCommons/gordianwallet-ios) app seperately communicates through a Torgap with a Linux [Spotbit](https://github.com/BlockchainCommons/spotbit) server in the cloud, which aggregates bitcoin price information.

### More on Spotbit

[Spotbit](https://github.com/BlockchainCommons/spotbit) is currently
our premiere Torgap application because it demonstrates a fully
functional, publicly accessible example of Torgap usage. Though
Bitcoin has long provided Tor interaction as an alternative method for
accessing your full node, Bitcoin pricing services have remained a
security hole: you could access your node, and so spend Bitcoin, but
if you ever looked up pricing information, the whole world would know
you were using Bitcoin. Worse, a pricing service could reveal to an
attacker every single IP address that had accessed it, putting all of
those users' coins in jeopardy.

Spotbit resolves these privacy and security problems by creating a
Torgap between your wallet and the pricing service, and also serves as
the first example of a [Gordian microservice](/architecture/), which
can provide additional functionality to data-asset applications.


## Torgap Links

* [**Airgap**](/airgap/)
* [**Spotbit**](https://github.com/BlockchainCommons/spotbit) (GitHub Repo)
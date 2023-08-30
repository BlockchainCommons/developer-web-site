---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Quick Connect
hide_description: true
classes:
  - wide
permalink: /quickconnect/
sidebar:
  nav: dataformat
---

## Overview

QuickConnect is a deep link URI combined with a scannable QR Code and
a [TorGap](/torgap/), intended to be universally compatible among a
number of different software projects and hardware products.

## Why is Quick Connect Important?

Quick Connect allows for the connection of networked transaction
coordinators with seed vaults and cosigners. By using an interoperable
specification, it allows for this connection to occur among products
created by different manufacturers, creating a modular
[architecture](/architecture/) that empowers user agency and
independence.

In addition, user privacy is improved through the use of Tor,
decreasing the possibility of attacks on a user's digital assets.

### How Does Quick Connect 1.0 Work?

The simple Quick Connect specification requires a Tor link in the
following form:

```
btcstandup://<your rpcuser>:<your rpcpassword>@<your tor hostname>.onion:<your hidden service port>/?label=<optional node label>
```

The optional argument allows node hardware manufacturers the choice of hard coding a label for the node.

Example with `label` :

```
btcstandup://rpcuser:rpcpassword@kshcahsaihslalsichs78yb2ud8d.onion:8332/?label=Your%20Nodes%20Name
```

Example without `label` :

```
btcstandup://rpcuser:rpcpassword@kshcahsaihslalsichs78yb2ud8d.onion:8332/?
```

Ideally, there would be a two-factor authentication where a user
inputs a V2 or V3 auth cookie into the client app manually, so that if
the URL leaks somehow it would not give an attacker access to the
node.

### Quick Connect 1.0 Partners

Server-side node manufacturers or providers supporting this protocol
include [BTCPayServer](https://btcpayserver.org),
[Nodl](https://www.nodl.it/), [MyNode](http://www.mynodebtc.com),
[RaspiBlitz](https://github.com/rootzoll/raspiblitz), and of course
[GordianServer](https://github.com/BlockchainCommons/GordianServer-macOS). The
iOS application
[GordianWallet](https://github.com/BlockchainCommons/GordianWallet-iOS)
offers proof of concept of a light client built to use this protocol.

## Quick Connect 2.0 Plans

We'd like to evolve this specification for 2.0 to address at least two new requirements:

* There is a need to be able to offer multiple services from a single
  server, for instance Bitcoin mainnet, testnet, Lightning, Spotbit,
  etc. Thus primary goal for a Quick Connect 2.0 specification is a
  single QR from a server that can offfer a client a list of all
  services available to it.
* A secondary requirement is that some servers may be able to use Web
  camera APIs to scan QRs from clients, and we'd like a QR with Tor v3
  client information in it that can be passed back to the server.

We'd love to have more discussion with other wallet developers about
any additional requirements for this initial connection between a full
node and a remote device via tor. This could include a possible TOFU
two-round auth so that the node can know that the specific remote
device is the same one that requested it originally. If these QRs
might get large, we might also want to offer this as an animated QR
using self-describing UR format rather than javascript.

If you would like to participate in this discussion, we have it as a
topic in the [Gordian Developer Community](https://github.com/BlockchainCommons/Gordian-Developer-Community/discussions/33)

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Gordian FROST-CLI Meeting (08/25): Summary"
permalink: /meetings/2025-08-frost-cli/summary/
hide_description: true
classes:
  - wide
sidebar:
  nav: meetings
---

_The following is a **rough summary** of the FROST-CLI demo meeting on August 6, 2025._

## Intro

There are two great tools for FROST and for Bitcoin:

* [ZF FROST](https://frost.zfnd.org)
* [Bitcoin Dev Kit (BDK)](https://bitcoindevkit.org/)

But how do we combine ZF FROST with BDK for PSBT signing on Bitcoin?

* ZF FROST needed secp256k1 fully enabled
* Then BDK needed to apply signature with a tweak
   * And some of what was needed wasn't available in CLI

## Demo

The demo spends a taproot Bitcoin
- with a 2-of-3 FROST Signature

Description & outline of demo is available at:
* https://hackmd.io/@bc-community/H1MfEMdvel

Step-by-step demo is at:
* https://hackmd.io/@bc-community/BJ2VtYKUxl

The demo sends money to a FROST wallet, then sends it back.

### Generating a FROST Wallet

Done with `trusted-dealer` from ZF FROST tools with our `secp256k1-tr` patch.
* Can then generate verifying key and descriptors from package

BDK-CLI can then be used to generate wallet from descriptors.
- but it's watch-only!

We can now send money to FROST wallet.

### Sending the Money Back

We can't send the money straight back from the wallet using BDK, because we set the wallet up as watch only. Instead, the FROST quorum has to sign to spend the money from the new FROST wallet

### Extracting Transaction sighash

We generate an unsigned PSBT from the FROST wallet.

We did not hack the BDK-CLI, but instead created Rust helpers (with full code in the [step-by-step](https://hackmd.io/@bc-community/BJ2VtYKUxl)).

The first tool is the `sighash-helper` tool, which reads the PSBT and extracts sighash from it.

(In a production demo, each signer would extract the sighash themself because they have to know what they're signing!)

### Running the FROST Ceremony

For this, we run the FROST-tools `coordinator`, which waits for participants to offer shares.

 We then run `participant` and send the key packages we created. We do this twice, using the two different key packages.

`coordinator` then creates signing package for Taproot and waits for participants to send signing shares, which participants do with `participant`.

Raw signature is generated!

### Signing PSBT

We have a second tool, `psbt-sig-attach`, that inserts FROST signature into PSBT.

Creates signed PSBT.

The PSBT can then be finalized using `bdk-cli`.

We then broadcast and the transaction goes out! Voila!

## Open Questions on ZF FROST

* **What is Status of PRs?**

ZF FROST is considered to be in a complete state right now, but PRs will be reviewed.

* **What has been security reviewed?**

Did not include Schnorr BIP340 review.

But more security reviews might be possible! There might be a grant for them already!

## Other Open Questions

* **What is right place to do tweak?**

- Probably have the coordinator do it!

* **Are there privacy implications for servers when using tweaks?**

There are generally security issues for collaborative custody, esp. where one collaborator might be a third-party server
- Third-party can always spy even if it's not signing

But the chain code can be an additional security measure
- There's only one chain code for FROST!
- You don't have to tell it to everyone
- Not telling it to the server secure things from that third-party
- Because of the tweak, they won't recognize children keys
- And if you require them to sign, they can do a blind-signature
- Possibly with a zero-knowledge proof

* **Are there ways to use FROST quorum for encryption?*8

There's a paper setting out possibilities.

* **What are best practices of storing DKG and VSS values?**

(Definitely an open question!)

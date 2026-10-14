---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-chains.jpg
  og_image: /assets/images/bc-card.jpg
title: Protecting Ethereum Secrets from the CLI
hide_description: true
classes:
  - wide
permalink: /chains/ethereum/cli/seeds/
sidebar:
  nav:
    - chains
    - technology
---

<div class="hexline hexgrid71">
  <div class="hex21">
    <a href="https://github.com/BlockchainCommons/GordianSeedTool-iOS">
      <img src="/assets/badges/seed-tool.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/chains/bitcoin/">
      <img src="/assets/badges/bitcoin.png">
    </a>
  </div>
  <div class="hex32top highlighted">
    <a href="/chains/">
      <img src="/assets/badges/cat-blockchains-half.png">
    </a>
  </div>
 <div class="hex41 highlighted">
      <img src="/assets/badges/ethereum.png">
  </div>
  <div class="hex61">
    <a href="/envelope/">
      <img src="/assets/badges/envelope.png">
    </a>
  </div>
  <div class="hex71">
    <a href="/sskr/">
      <img src="/assets/badges/sskr.png">
    </a>
  </div>
</div>

_Wallets hold your secrets: the seed recovery phrases, seeds, and
private keys that allow you to access your assets. But they don't
necessarily offer resilience, and there's ultimately no one to help
you if you lose your secrets._

_The following demonstrates how to interoperate with
[MetaMask](https://metamask.io/) to improve the resilience of your
Ethereum funds, while maintaing your privacy, using two Blockchain
Commons technologies: [Envelope](/envelope/) and [SSKR](/sskr/)._

## Extracting Secrets from Metamask

When you work with Metamask, you will create wallets and within them
accounts, each of which can generate individual addresses.

* A **Seed Recovery Phrase** can be used to restore a wallet.
* A **Private Key** is derived from a seed and can be used to restore an account.

It is beneficial to privacy to use multiple accounts, each one for a
different purpose on the Ethereum network (e.g., one for a DAO, one
for game-related spending, one for a trip out of country, etc). But
you might be unwilling to because of the fear of losing an
account. That's where resilience comes in: by backing up your secrets,
you can lower the fear of loss, and so encourage better privacy
practices.

Accessing secrets on Metamask is simple:

1. Click the down arrow next to your current account.
2. Click the "..." next to the account you want to access.
3. Choose account details.
4. To access the private keys for an account, choose "Private Keys".
5. To access the seed recovery phrase for a wallet, choose "Secret Recovery Phrase".

Be sure you're in a private place that can't be surveiled before
taking these steps. On a modern computer, Metamask will require your
biometrics to unlock a passkey. Afterward, you can use a secure
computer to back them up using Envelope and SSKR.

## Storing with Gordian Envelope

[Gordian Envelope](/envelope/) is a Smart Envelope system that allows
you to securely store information with metadata. This allows you to
not only make a backup of your secrets, but also to add information
about the secrets, so that you can later have a better chance at
recovering them.

### Installing Gordian Envelope

Gordian Envelope is available as a CLI that is installed from a Rust crate:
```
cargo install bc-envelope-cli
```

Developers can also use the Gordian Envelope libraries to build their
own tools:

* [https://developer.blockchaincommons.com/envelope/#libraries](https://developer.blockchaincommons.com/envelope/#libraries)

Once you have installed Gordian Envelope, you'll be able to store
information in it. This is done by adding subjects and then assertions
about that that subject, made up of predicate and an object. 

### Storing a Recovery Phrase

If you "Copy to Clipboard" your Secret Recovery Phrase, it will look
as follows:

```sh
already oak actual turtle mirror decorate rose permit toy host relief escape
```

You can store that in an Envelope as a subject:

```sh
envelope subject type string "already oak actual turtle mirror decorate rose permit toy host relief escape"

| ur:envelope/tpsoksgshsjzjpihhsiekkcxjlhsjecxhsiajykphsjzcxjykpjpjyjzihcxjninjpjpjljpcxieihiajljphsjyihcxjpjljkihcxjoihjpjninjycxjyjlkkcxisjljkjycxjpihjzinihiycxihjkiahsjoihlnpeiedt
```

Rather than showing all the output, we're going to store everything in
variables for easier reuse.

```sh
RECPHRASE=$(envelope subject type string "already oak actual turtle mirror decorate rose permit toy host relief escape")
```

You can then add assertions that describe what you're storing, so you
know what it is in the future.

```sh
RECPHRASE=$(envelope assertion add pred-obj known 'isA' string "recoveryPhrase" $RECPHRASE)
RECPHRASE=$(envelope assertion add pred-obj known 'asset' known 'Ethereum' $RECPHRASE)
```

Some of these assertions are of type "string" (a freeform
description), but others are of type "known", which is a [specifically
defined known
value](https://github.com/BlockchainCommons/Research/blob/master/known-value-assignments/markdown/0_blockchain_commons_registry.md). You
could use strings for everything, but using known values when they
exist improves the inteoperability of your data, and so the likelihood
of recovering it in the future.

After you've filled out your envelope, you can use `envelope format` to see its complete contents:

```sh
envelope format $RECPHRASE

| "already oak actual turtle mirror decorate rose permit toy host relief escape" [
|     'isA': "recoveryPhrase"
|     'asset': 'Ethereum'
| ]
```

You've successfully backed up your recovery phrase along with a
description of what it is! You could add more details, for example
what accounts you created, with the exist format de[ending on what you
will need in the future to get from this raw data (the secret) to your
actual assets.

### Storing a Private Key

Another option is just to store the private key for an address you're
using.

```sh
RECKEY=$(envelope subject type string "2cebbeb69d8f390f709bde1d571a44ee09446d18d7f07f21fb8dc72de786c00f")
RECKEY=$(envelope assertion add pred-obj known 'isA' known 'PrivateKey' $RECKEY)RECKEY=$(envelope assertion add pred-obj known 'asset' known 'Ethereum' $RECKEY)

envelope format $RECKEY

| "2cebbeb69d8f390f709bde1d571a44ee09446d18d7f07f21fb8dc72de786c00f" [
|     'isA': 'PrivateKey'
|     'asset': 'Ethereum'
| ]
```

### Encrypting Your Data

Obviously, you don't want to leave your secrets sitting around in
plain text. You can generate a new symmetric key and use that to
encrypt your envelopes:

```
SKEY=$(envelope generate key)

RECPHRASE_ENC=$(envelope encrypt --key $SKEY $RECPHRASE)
RECKEY_ENC=$(envelope encrypt --key $SKEY $RECKEY)
```

There's a catch here! Encryption only encrypts the subject, not the
assertions:

```
envelope format $RECPHRASE_ENC

| ENCRYPTED [
|     'isA': "recoveryPhrase"
|     'asset': 'Ethereum'
| ]

envelope format $RECKEY_ENC

| ENCRYPTED [
|     'isA': 'PrivateKey'
|     'asset': 'Ethereum'
| ]
```

This is a purposeful design! In this situation, your secret (the
recovery phrase or private key) is encrypted, but your metadata about
it is not. This means that you can include metadata that might help
you recover your secret (for example, by telling you where the
symmetric encryption key is!).

### Encrypting the Whole Envelope

If you prefer, you can encrypt the whole envelope. This is done by
"wrapping" the envelope (which turns the full body into the subject),
then encrypting that.

```sh
RECPHRASE_FULLENC=$(envelope subject type wrapped $RECPHRASE | envelope encrypt --key $SKEY)

envelope format $RECPHRASE_FULLENC

| ENCRYPTED
```

This is what makes Envelope very important as a privacy tool. You get
to decide exactly the level of privacy that every element of your data
deserves.


---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Envelope Permits"
hide_description: true
classes:
  - wide
permalink: /envelope/permits/
redirect_from:
  - /envelope/permit/
sidebar:
  nav: envelope
---

Permits are cryptographic access control mechanisms that seal [Gordian Envelopes](/envelope/). They come in a variety of flavors, including symmetric keys, stretched passwords, public keys, SSKR shares, FROST multisig, and emerging quantum-resistant algorithms. Multiple permits can coexist in one Envelope, allowing different parties to access the same content in different ways. 

## Why Are Permits Important?

Permits are an autonomous authorization system for Gordian Envelope. They control who can access an envelope (that's the "authorization") and they do so solely with cryptography, ensuring that you don't have validate or verify with any external source: if you have the envelope, and the authorization, you can open it (that's the "autonomous"). That system ensures that an envelope is also accessible and can never be censored, which is the heart of the [Gordian Clubs technology](/clubs/) that we built atop Gordian Envelope.

Gordian Envelope's system of multiple permits also improves the _resilience_ and _usability_ of envelopes. You can let different people access an envelope each in the way that suits them best. You might share a password or symmetric key with a close associate; you might lock an envelope with the public key of a pseudonymous contact; or you might shard an envelope to allow it to be reconstructed by a threshold of share holders in the future. Each methodology can support the specific needs of a particular envelope. Because multiple users might have different ways to open an envelope, you've also ensured that it can't be lost through the loss of a single key, resolving one of the fundamental issues we've faced to date with the control of digital assets.

## How Do Permits Work?

Gordian Envelope is a "Smart Document" data storage format. It can be compressed, elided, or encrypted, to offer users the exact level of privacy needed for their data. The default methodology for the [encryption](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-004-envelope-symmetric-encryption.md) of a Gordian Envelope is [IETF-ChaCha20-Poly1305](https://datatracker.ietf.org/doc/html/rfc7539), a symmetric encryption method: the content is encrypted or decrypted with the same symmetric key. This is called the "content key."

Permits allow a variety of methods to be used to decrypt a Gordian Envelope. They do so by encrypting the envelope with ChaCha20-Poly1305 (as usual), then encrypting that "content key" via a different means and including that encrypted content key in the envelope! It's entirely secure because it requires a decryption key to get to the content key, but it allows not only variable permits, but also multiple permits: the content key can be encrypted in multiple ways, with each of those encryptions stored using a different permit.

This shows an example of an envelope with permits for two different private keys. Each permit is a lockbox that was locked with the user's associated public key:
```
ENCRYPTED [
    'hasRecipient': SealedMessage
    'hasRecipient': SealedMessage
    'note': "Zcash seed"
]
```
This shows an example of en envelope with a public key permit and a an SSKR permit. The first permit is (once again) the content key encrypted with a public key. The second permit is instead a share that was generated when the content key was sharded with [SSKR](/sskr/). If a threshold of shares are brought together, then the content key (and so the envelope) can instead by decrypted in that way:
```
ENCRYPTED [
    'hasRecipient': SealedMessage
    'sskrShare': SSKRShare
]
```
Both of these examples are drawn from our [Seed page](/envelope/seed/), which demonstrates how Gordian Envelope works in a hands-on way.

There are currently several major ways to open envelopes:

* **Symmetric Keys.** Envelopes are by default encrypted with symmetric content keys. If you have the key, you can unlock the envelope.
* **Public Key Permits.** The symmetric key can be encrypted with a public key, then unlocked for use with the associated private key.
* **SSKR Permits.** The symmetric key can be sharded with SSKR, then reconstructed when a threshold of SSKR shares are brought together.
* **Password Permit.** A password is stretched into a key, and the symmetric key is encrypted with that stretched key.
 
The creation of permits can be done easily and automatically with the [`envelope-cli-rust`](https://github.com/BlockchainCommons/bc-envelope-cli-rust).

This is all that's required to create a public key permit:
```
SEED_E_ENCPUB=$(envelope encrypt --recipient $PUBKEYS $SEED_E)
```
This is all that's required to create an SSKR permit:
```
envelope sskr split $SEED_E -g 2-of-3
```
Reconstructing from those SSKR shares just requires bringing them back together with the CLI:
```
envelope sskr join
ur:envelope/lftansfwlrhddwksfemkimwymwvdehltoeimwtplpyhdgoctgygawekbspgtwkfmonwmlbmklpenskayeswmgomyenhydmrlwncajogssnematwpgantwzetehmhssiogdztwtsrckswvstiinmkspdpimdwsskeqzhddatansfphdcxmojsjpryadbgroleykrdaahscyvarttitszssfdsykisynctctsblysaftcfvsgmoyamtpsotantkphddagdclaeadaeaebkmegwsssrftaymnuyfmsohedacmswhgdrsfoybacloyfsbtmykbhlihdechkbvthkmdgh 
ur:envelope/lftansfwlrhddwksfemkimwymwvdehltoeimwtplpyhdgoctgygawekbspgtwkfmonwmlbmklpenskayeswmgomyenhydmrlwncajogssnematwpgantwzetehmhssiogdztwtsrckswvstiinmkspdpimdwsskeqzhddatansfphdcxmojsjpryadbgroleykrdaahscyvarttitszssfdsykisynctctsblysaftcfvsgmoyamtpsotantkphddagdclaeadadbkrpvaaaidswpelsoxcejekikncmcszcktolkkidaddikshtbdhtwlwptdeessnyhngdoygo
```
Again examples are drawn from our from our [Seed page](/envelope/seed/).

## Permit Links

* [**BCR-2025-004: Permits in Gordian Envelope**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2025-004-permit.md)
* [**The Gordian Envelope Structured Data Format**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2025-004-permit.md) (IETF Draft)

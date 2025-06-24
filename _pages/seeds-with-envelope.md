---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Seeds: Gordian Envelope Support"
hide_description: true
classes:
  - wide
permalink: /ckm/
sidebar:
  nav: ckm
redirect_from:
  - /ckm/overview/
---

## Overview

Cryptographic seeds are the heart of crypto asset control. [#SmartCustody](https://www.smartcustody.com/), one of Blockchain Commons' earliest initiatives, was all about keeping them safe. That's continued forward, with resilience being a core [Gordian principles](https://developer.blockchaincommons.com/principles/). We believe that loss of a seed or private key is one of the most likely ways for the average user to lose a digital asset; Blockchain Commons is working to help developers and users to avoid that.

One of the major ways to keep a seed safe is encode it in a Gordian Envelope. Not only is it a well-known, well-specified format that should be readable into the far future, but it also allows for encryption, sharding, multiple permits, and in the future storage with [GSTP](/envelope/gstp/) and [CSR](/csr/).

The following examples demonstrate how many of these techniques work using the [Rust envelope-cli](https://github.com/BlockchainCommons/bc-envelope-cli-rust). The [bytewords-cli](https://github.com/BlockchainCommons/bytewords-cli) and [cbor2diag](https://github.com/cabo/cbor-diag) are also used for a few minor examples, but they're not necessary to fully understand this tutorial (so if you don't have them, no problem). 

⚠️ **Warning:** ⚠️ Do not work with real assets using envelope-cli. Because it's a command-line tool, it's probably not secure enough for most digital assets; but, as a reference app, envelope-cli shows what envelopes can do for seeds and how they work. It can also be used to generate sample envelopes for testing elsewhere.

## Using Envelopes

Remember that envelopes are built on semantic triples. Each envelope has a subject and optionally one or more assertions, which consist of a predicate and an object.

The following example demonstrates this:
```
"alice" [
    "likes": "bob"
    "dislikes": "charlie"
]
```
The subject is "alice", and two assertions that relate to that subject are "likes bob" and "dislikes charlie".

For a seed, the subject will usually be the seed and the assertions will be information about the seed, such as notes, dates, and derivation paths.

## Generating Seeds

Seeds and their associated private keys and public keys can all be generated using `seedtool-cli`, but this capability should be used solely for testing purposes. 

(You'll ideally want a hardened offline wallet for generating your real seeds.)

```
SEED=$(envelope generate seed)
echo $SEED
ur:seed/oyadgdcmynztcnronejkqzadamkbaesroytsgtgmbsdrvs

PRVKEYS=$(envelope generate prvkeys --seed $SEED)
echo $PRVKEYS
ur:crypto-prvkey-base/gdtburldbkjpjeclprcnwpfnsrcakkgdwmnbjzdeae

PUBKEYS=$(envelope generate pubkeys $PRVKEYS)
echo $PUBKEYS
ur:crypto-pubkeys/lftanshfhdcxldrlemtomeiarlnsfsdloybgzoeeecbyzctpjlmslenyuochehgufetkwtemrhfttansgrhdcxjzytdndyuodaamhfaeaywmhdlbrdnnpsmwsteozmttuytbgyrpfzgltptboedpdtdtzomhbn
```
These variables will be used for the following examples

## Examining Seeds

Seeds are generated as `ur:seed`s, in accordance with the [crypto-seed CDDL](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md#cryptographic-seed-seed). If you want to examine a seed more closely, you can do so by stripping off the `ur:seed` prefix. What's left is the CBOR of the seed, prepared per the CDDL, in minimal [Bytewords](https://developer.blockchaincommons.com/bytewords/) form. Those minimal Bytewords can be converted to hex with the `bytewords-cli`.
```
SEED_CBOR=$(bytewords -i minimal -o hex `echo $SEED | awk -F"/" '{print $2}'`)
echo $SEED_CBOR
a10150d6df890a726b21b223ec3cc31d7950eb
```
In [CBOR](https://cbor.me/), that's:
* a map [`a1`]
* Whose first entry [`01`]
* Is 16 bytes [`50`]
* Which is the seed `d6df890a726b21b223ec3cc31d7950eb`

The `cbor2diag` utility will do that breakdown for you, which is why it's a convenient tool:
```
cbor2diag -x $SEED_CBOR
{1: h'd6df890a726b21b223ec3cc31d7950eb'}
```
Per the [CDDL](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md#cryptographic-seed-seed), there could have been an optional creation date, name, or note, as map entries 2, 3, or 4, respectively, but there aren't in this simple example.

## Putting a Seed in an Envelope

The envelope-cli allows a seed to easily be placed in an envelope: you just define the seed you've generated as the subject of an envelope, using type `ur`:
```
SEED_E=$(envelope subject type ur $SEED)
echo $SEED_E
ur:envelope/tpsotantjzoyadgdtburldbkjpjeclprcnwpfnsrcakkgdwmprkgvlzc
```
### Examing a Seed Envelope

The envelope-cli will directly output the CBOR of an envelope for you:

```
% envelope format --type cbor $SEED_E 
d8c8d8c9d99d6ca10150d6df890a726b21b223ec3cc31d7950eb
```

Note this is slightly different than the CBOR for the UR! That's because it's now in an envelope. The CBOR can again be broken down with cbor2diag or with [cbor.me](https://cbor.me/):
```
SEED_E_CBOR=$(envelope format --type cbor $SEED_E)
cbor2diag -x $SEED_E_CBOR
200(201(40300({1: h'd6df890a726b21b223ec3cc31d7950eb'})))
```

That's now:
* a envelope (CBOR tag 200)
* With a single leaf (CBOR tag 201)
* That contains a seed (CBOR tag 40300)
* Which is still a map with one entry for key `1` (`{1:`)
* Which is the seed `d6df890a726b21b223ec3cc31d7950eb`

One important difference between the UR Seed and the Envelope Seed is that the UR seed was preceded by `ur:seed`, so it did not need a CBOR tag for the seed. However, once it's an envelope, it now needs the CBOR tag of `40300` to define what that part of the envelope is. A particular element will self-identify with a CBOR tag (like `200` or `40300`) or a UR tag (like `ur:envelope` or `ur:seed`) but not both.

Another way to examine a seed envelope is to use the `format` command of the `envelope-cli` without the `--type cbor` tag. This just shows the envelope listing:
```
envelope format $SEED_E
Seed
```
That's just the seed as a subject. (The envelope listing will get more complex when more is added to it.)

### Modifying a Seed Envelope

There are many advantages to having a seed in an envelope, rather than it being a bare seed. One advantage is that you can add metadata as assertions in the envelope (with the actual seed remaining the subject). This adds an assertion with a [known value](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-002-known-value.md) of 'note' and the object being "Zcash seed":
```
SEED_E=$(envelope assertion add pred-obj known note string "Zcash seed" $SEED_E)
```

The seed now looks like this:
```
envelope format $SEED_E
Seed [
    'note': "Zcash seed"
]
```

### Encrypting a Seed Envelope

You can also encrypt an envelope, or rather you can encrypt the _subject_ of an evelope.

The following example generates a symmetric key, then uses that key for encryption:
```
SKEY=$(envelope generate key)
SEED_E_ENCS=$(envelope encrypt --key $SKEY $SEED_E) 
```

The envelope is unchanged other than the subject (the seed!) being encrypted:
```
envelope format $SEED_E_ENCS
ENCRYPTED [
    'note': "Zcash seed"
]
```

Envelopes can be encrypted in different ways. The following then uses the public key generated above to encrypt the seed. (It can be decrypted with the pirvate key.)
```
SEED_E_ENCPUB=$(envelope encrypt --recipient $PUBKEYS $SEED_E)
```
This actually creates an ephemeral symmetric encryption key, encrypts the subject (the seed) with that key, encrypts the symmetric encryption key with the public key, then stores the encrypted key in a new `hasRecipient` assertion. The recipient of the envelope can then extract the encrypted key, decrypt it with their private key, and use the decrypted key to decrypt the subject.
```
envelope format $SEED_E_ENCPUB
ENCRYPTED [
    'hasRecipient': SealedMessage
    'note': "Zcash seed"
]
```
An envelope can be encrypted in multiple ways simultaneously:
```
SEED_E_ENCMULTI=$(envelope encrypt --recipient $PUBKEYS --recipient $PUBKEYS2  $SEED_E)
```
This is a system of "multiple permits", where any permit can be used individually to descrypt the envelope. In this case, either `$PRVKEYS` or `$PRVKEYS2` can do so:
```
ENCRYPTED [
    'hasRecipient': SealedMessage
    'hasRecipient': SealedMessage
    'note': "Zcash seed"
]
```

### Sharding a Seed Envelope

[SSKR](https://developer.blockchaincommons.com/sskr/), Blockchain Commons implementation and expansion of Shamir's Secret Sharing can also be used to encrypt an envelope.

Doing so works much the same way as encrypting with a public key:

1. An ephemeral symmetric key is created.
2. The envelope subject is encrypted with the symetric key.
3. The symmetric key is sharded.
4. Multiple envelopes are created, each containing the full contents of the envelope (including the encrypted subject) and a share of the symmetric key.
5. If sufficient envelopes are put together, then their shares allow for the reconstruction of the symmetric key, and then the decryption of the envelope subject.

The following shows the splitting of an envelope using a [2 of 3 sharing scenario](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/SSKR-Sharing.md).
```
envelope sskr split $SEED_E -g 2-of-3
ur:envelope/lftansfwlrhddwksfemkimwymwvdehltoeimwtplpyhdgoctgygawekbspgtwkfmonwmlbmklpenskayeswmgomyenhydmrlwncajogssnematwpgantwzetehmhssiogdztwtsrckswvstiinmkspdpimdwsskeqzhddatansfphdcxmojsjpryadbgroleykrdaahscyvarttitszssfdsykisynctctsblysaftcfvsgmoyamtpsotantkphddagdclaeadaeaebkmegwsssrftaymnuyfmsohedacmswhgdrsfoybacloyfsbtmykbhlihdechkbvthkmdgh ur:envelope/lftansfwlrhddwksfemkimwymwvdehltoeimwtplpyhdgoctgygawekbspgtwkfmonwmlbmklpenskayeswmgomyenhydmrlwncajogssnematwpgantwzetehmhssiogdztwtsrckswvstiinmkspdpimdwsskeqzhddatansfphdcxmojsjpryadbgroleykrdaahscyvarttitszssfdsykisynctctsblysaftcfvsgmoyamtpsotantkphddagdclaeadadbkrpvaaaidswpelsoxcejekikncmcszcktolkkidaddikshtbdhtwlwptdeessnyhngdoygo ur:envelope/lftansfwlrhddwksfemkimwymwvdehltoeimwtplpyhdgoctgygawekbspgtwkfmonwmlbmklpenskayeswmgomyenhydmrlwncajogssnematwpgantwzetehmhssiogdztwtsrckswvstiinmkspdpimdwsskeqzhddatansfphdcxmojsjpryadbgroleykrdaahscyvarttitszssfdsykisynctctsblysaftcfvsgmoyamtpsotantkphddagdclaeadaobbinlbtamusobdahtnglmwrdbzfxbkpfchdtryfnbedpaywfadfmgrdkbebepkpmzooldpte
```
Joining two of those shares:
```
envelope sskr join
ur:envelope/lftansfwlrhddwksfemkimwymwvdehltoeimwtplpyhdgoctgygawekbspgtwkfmonwmlbmklpenskayeswmgomyenhydmrlwncajogssnematwpgantwzetehmhssiogdztwtsrckswvstiinmkspdpimdwsskeqzhddatansfphdcxmojsjpryadbgroleykrdaahscyvarttitszssfdsykisynctctsblysaftcfvsgmoyamtpsotantkphddagdclaeadaeaebkmegwsssrftaymnuyfmsohedacmswhgdrsfoybacloyfsbtmykbhlihdechkbvthkmdgh 
ur:envelope/lftansfwlrhddwksfemkimwymwvdehltoeimwtplpyhdgoctgygawekbspgtwkfmonwmlbmklpenskayeswmgomyenhydmrlwncajogssnematwpgantwzetehmhssiogdztwtsrckswvstiinmkspdpimdwsskeqzhddatansfphdcxmojsjpryadbgroleykrdaahscyvarttitszssfdsykisynctctsblysaftcfvsgmoyamtpsotantkphddagdclaeadadbkrpvaaaidswpelsoxcejekikncmcszcktolkkidaddikshtbdhtwlwptdeessnyhngdoygo
```
Then restores the original envelope:
```
ur:envelope/lftpsotantjzoyadgdcmynztcnronejkqzadamkbaesroytsgtoyaatpsoimhtiahsjkiscxjkihihientjooxmw
envelope format ur:envelope/lftpsotantjzoyadgdcmynztcnronejkqzadamkbaesroytsgtoyaatpsoimhtiahsjkiscxjkihihientjooxmw

Seed [
    'note': "Zcash seed"
]
```
An SSKR permit can also be combined with other forms of encryption:
```
envelope sskr split $SEED_E -g 2-of-3 -r $PUBKEYS
```
Each of the three envelope shares generated will appear as follows:
```
ENCRYPTED [
    'hasRecipient': SealedMessage
    'sskrShare': SSKRShare
]
```
Someone with the corresponding private key could decrypt the `hasRecipient` object, which is the symetric key. In addition, any two `sskrShare` could be combined to reconstruct the symmetric key. That can then be used to decrypt the subject (the seed).

### Wrapping a Seed Envelope

All of the above encryption examples talk about encrypting the subject of an envelope. That's the standard way that Gordian Envelope works. Assertions (including encryption!) are applied to the subject of an envelope.

There's a strong use case for this:  you might want to have notes or other metadata that are readable even after the sensitive data (such as a seed) have been encrypted. This can include tips on how to decrypt the data, which can be crucial for heirs, or even for an envelope that you haven't touched in several years:
```
ENCRYPTED [
    'note': "Decryption key is engraved QR code at safety deposit box"
]
```
But, you might also want to encrypt the entire contents of an envelope. To do so, you "wrap" the envelope, which turns the entire contents of an envelope into the subject of a new envelope:
```
SEED_E_W=$(envelope subject type wrapped $SEED_E)
envelope format $SEED_E_W
{
    Seed [
        'note': "Zcash seed"
    ]
}
```
Now, when you encrypt the wrapped envelope, you include everything (because it's all the subject):
```
SEED_E_W_ENCPUB=$(envelope encrypt --recipient $PUBKEYS $SEED_E_W)
envelope format $SEED_E_W_ENCPUB
ENCRYPTED [
    'hasRecipient': SealedMessage
]
```
## What's Next

* [**Gordian Envelope Developer's Page**](https://developer.blockchaincommons.com/envelope/)
* [**Envelope-CLI Docs**](https://github.com/BlockchainCommons/bc-envelope-cli-rust/tree/master/docs#readme)
* [**128-bit sample seed**](https://developer.blockchaincommons.com/seed-128/)
* [**256-bit sample seed**](https://developer.blockchaincommons.com/seed-256/)

## Download the Software

* **Envelope CLI:** `cargo install bc-envelope-cli`
* **Bytewords CLI:** compile from https://github.com/BlockchainCommons/bytewords-cli?tab=readme-ov-file#recommended-installation-instructions
* **Cbor2Diag:** compile from https://github.com/hildjj/node-cbor/tree/main/packages/cbor-cli#cbor2diag

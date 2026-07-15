---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "SSKR Lexicon"
hide_description: true
classes:
  - wide
permalink: /sskr/lexicon/
sidebar:
  nav:
    - sskr
    - seedlayer
---

An [incomplete
paper](https://docs.google.com/document/d/1rZJlFZcftrCM_KaxFnHUIskJKlSQzF0zFn4WIRQGDLU/edit#)
from RWOT9 suggested the following terminology to standardaize the
discussion of reconstruction techniques. As that paper says: "It is
not necessarily complete, and it does not necessarily reflect the
overall understanding of the cryptographic community, though best
efforts have been applied to both issues."

One change has been made to the definitions from that paper: whereas
it uses "share" and "shard" to differentiate shares based on their
contents, the following applies the more standard usage of "shard"
being _only_ a verb, describing how a "secret" is divided into
"shares"

**Deck.** A collection of shares that can be combined together to reconstruct a sharded secret.

**Deck identifier.** The public key derived from the sharded secret, unmodified, with no derivation and no other modification. It uniquely identifies a deck. This key can sign each share.

**Derived Secret.** The private key derived from a sharded secret. It is used to decrypt the private data associated with each share.

**Private Data.** Encrypted data transferred with each share. The decryption key, or derived secret, can be computed by recombining all the shares, producing the sharded secret. Alternatively: encrypted blob or deck blob.

**Quorum.** Any set of shares sufficient to meet the script policy for reconstruction.

**Recovery.** The method by which one or more keys in a multisig are used to sweep forward funds after the loss of one or more keys in that multisig. Not to be confused with reconstruction.

**Reconstruction.** The method by which a sharded secret is restored from a threshold of shares created by a technique such as Shamir’s Secret Sharing. Not to be confused with recovery.

**Script Policy.** A script that specifies a policy for how a deck's sharded secret can be reconstructed from some combination of shares.

**Share Custodian.** A user that holds a number of shares, possibly from multiple decks.

**Share Dealer.** An individual that has a secret that is sharded using a secret sharing scheme. The user creates a number of shares that are dealt out to different users, turning each user into a share custodian.

**Share Pool.** A collection of shares that a share custodian is responsible for, usually implemented by a software application. The share pool allows for querying over the set of shares to find particular shares in response to a request.

**Share Value.** A mathematical or cryptographic value that can be used in a secret sharing scheme to reconstruct a shared secret. Alternatively: y value.

**Sharded secret.** A high entropy secret that is turned into shares by a share dealer. It can also be used to create a derived secret, in which case it is used as both a symmetric key and a private key.

**Share.** A split of a secret, created with Shamir’s Secret Sharing. May also have unencrypted metadata associated with it.

**Threshold.** The number of shares required to reconstruct a sharded secret.

**Unencrypted Metadata.**  Data associated with a shares that describes the share and the deck, among other things. This data can include birthdate, deck identifier information, and so on. Alternatively: public metadata.

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Developer II (2024): Summary"
hide_description: true
classes:
  - wide
permalink: /frost/developers2/summary/
  nav:
    - frost
    - meetings
---

Our developer's meetings focus on helping developers to implement FROST. The first occurred in [early 2024](https://developer.blockchaincommons.com/frost/developers1/) and featured a presentation from Jesse Posner on his current work on FROST. This second meeting, held on December 4, 2024 and sponsored by [HRF](https://hrf.org/) was much more all-encompassing and featured not just an intro to FROST, but also discussions from developers who were already implementing FROST.

## Intro to FROST

FROST is a Schnorr Threshold Signature system
- All about Schnorr
- An elegant digital signature format
- Can aggregate & split keys
- Allows for quorums (thresholds)
- Is incorporated into Bitcoin in the Taproot family of transactions

Threshold means:
- M of N people can sign
- Where M ≤ N
- Looks exactly the same as a single siignature
- In contrast to MuSig, which can't *natively* do thresholds

FROST signing is easy
- User has a secret share of a signing key
- a threshold of participants sign

Private key is split into secret shares

Public key verifies aggregate signature

Key are generated with either Trusted Dealer Generation (TDG) or Distributed Key Generation (DKG)

TDG = a single party splits up & sends out keys
- Transitional!
- Traditional
- Must trust dealer

DKG = multiparty protocol
- Key is never in memory!
- But there are a bunch of ways to do DKG
- PedPop is common
- There are variants!

### Why use FROST?

SECURITY!
- Key is split
- With interoperability, can even avoid single implementations failures
   - heterogeneity of implementations

SCALABLE
- All signatures are the same size
   - No matter how many people sign!

PRIVATE
- All sigs look the same
- Can't even tell which members of a group signed!

FLEXIBLE
- Can change groups & thresholds!
   - Without changing public keys
- Needs more standardization!
   - So this can be a challenge

But Not ROBUST
- Members can Deny Service
- ROAST is a solution

### How To Use

That's the topic of today's meeting!

There are lots of libraries available!
- [ZF FROST](https://github.com/ZcashFoundation/frost)
  - [UNiFFI SDK](https://github.com/zecdev/frost-uniffi-sdk)
- [Serai][(https://crates.io/crates/bitcoin-serai)](https://stackwallet.com/)
- Many more!
 
## FROST for the masses (Stack Wallet)

Cypher Stack does a lot of work in privacy space.
- [Stack Wallet](https://stackwallet.com/) is an open source wallet
- Stack Wallet Duo supports just Bitcoin & Monero

Uses Luke Parker's [Serai DKG library](https://stackwallet.com/)
- Uses DKG model
- Claimed [HRF bounty for FROST with changeable thresholds (#9)](https://hrfbounties.org/)

UX of Crypto/FROST
- It's bad!
- Cryptocurrency in general has bad UX
- But improved UX has decentralization tradeoffs

THE BIGGEST ISSUE WITH FROST ADOPTION WILL BE THAT UX ISSUE.
- Won't be used if it's hard to use!
- Every participant has to get content from all participants to create a sig!
   - Twice!
   - If someone messes up, non-ROBUSTness means you need to start over!
- That's horrible for a big sig
- Improving this is where RESEARCH needs to go!

Difficulty: Two-Rounded Handshake
- Creates user friction 
- Interactivity results in drop-outs
- Can we get this down to one-round? That's needed for success!

Difficulty: Accurate Messaging
- There's a lot of vocabulary
- Resharing (changing thresholds) doesn't do what people think
   - Old shares still remain valid
   - Old participants can still see history

### Solutions?

Grinbox/Epicbox
- central server that does coordination
- Does multiple handshakes for everyone online
- holds messages for people not online
- can take a few seconds (if online)
- Decentralization tradeoff

Could do same with FROST
- Companies could set up these servers
- Hopefully FEDERATED

Another option: Guided setup & sending

Another option: Reduce use case to simple things used by average user

Question: What are problems of privacy with one party?
- You're trusting their COMPETENCY
- You're trusting their MORALITY

### Other Solutions (from Blockchain Commons)

Blockchain Commons is trying to solve problems
- Seen similar problems with [multisigs](https://developer.blockchaincommons.com/musig/sequence/). They're tough due to UX complexity!
- As well as [with Musig](https://developer.blockchaincommons.com/musig/sequence/)
- A [request/response system](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Scenario-Multisig-RR.md) such as [GSTP](https://developer.blockchaincommons.com/envelope/gstp/) can help through automation

## Zcash FROST UniFFI SDK

[https://girhub/pacu/frost-uniffi-sdk](https://girhub/pacu/frost-uniffi-sdk)

Brings the ZFFROST crates to other languages without rewriting
- GoLang, Kotlin, SWIFT
- UniFFI allows 1 different FFI per language

An extensive walk-through demonstrated how to build bindings and test them
- [See the video](https://www.youtube.com/watch?v=FbrB1SCXCNc&t=3944s)

Allows use of FROST in a native app in Java, GoLang server, etc!

## FROST Federation

Using FROST Federation to manage a mining pool & pay out miners.

Benefit of FROST is that you can distribute trust
- Obvious use in wallets
- But in mining pool too, replace centralized pool operator with a federation

Membership is decided out of band based on Proof of Work

KeyGen using Bracha's Broadcast
- Can be optimized later

Implementing in Rust

DKG working over network stack using Tokio and Tower Services

For misbehaving parties
- Only handle network failures currently
- Need to identify culprits in round 1 & round 2 of DKG protocol

Signing
- Not started
- But should be easier than DKG

Challenge: Signing
- Need Robustness with ROAST

Next Steps
- Make KeyGen Robust
- Support signing with ROAST optimization



---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Round Table I: Summary"
hide_description: true
classes:
  - wide
permalink: /frost/meeting1/summary/
sidebar:
  nav: frost
---

_This is a rough summary of the FROST Implementer's Round Table on November 8, 2023._

## Meeting Our Participants

We led off the meeting with the participants answering some ice breakers about FROST.

What do we love about FROST?

* Interesting work with quorums.
* How it can impact recover flows.
* The power of resilience.

What is the most difficult part of FROST?

* Getting FROST deployed!
* Understanding what's going on in the community and what people are doing.
* Making it human-oriented and easy to use.
* Generally, the user experience is difficult & a concern.
* The DKG is the most difficult part!

What is the biggest upcoming concern?

* STORM, a new key-generation scheme for use with FROST.
* Getting it into the hands of users!

## Project Introductions

Participants were present from two major FROST projects.

### ZCash FROST

[Repo](https://github.com/ZcashFoundation/frost) has been audited. Publishing 1.0 release this week, finishing up main part of work. 

Have a second, [demo repo](https://github.com/ZcashFoundation/frost-zcash-demo).

Final goal will be to have FROST in ZCash wallet.

Meanwhile, waiting for [IETF process](https://datatracker.ietf.org/doc/draft-irtf-cfrg-frost/) to finish. 

### Secp/ZKP FROST

Working on [a PR](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/138), which is a mature status.

Plan is to split out signing code, which is ready to go.
- Working on a FROST BIP, which will create spec for how to do DKG.
- Then will update DKG code.
- The goal is to have it merged by end of the year.
- BIP will be how to implement FROST in the Bitcoin world.

## Distributed Key Generation (DKG)

We next moved on to questions about specific elements of FROST, beginning with its innovative DKG.

Secp-ZKG is still working on simple DKG, without robustness requirement. 
- Explicit check at end so that we know that everyone agrees.
- Includes secure channels & backup facilities.
- The idea is that engineers can pick up & implement, without worrying about setup requirements or other complexities.
- Secure channel communication just uses public keys to exchange a single message!
- Public-key encryption identifies participants. [so assumption is that everyone agrees on each others' public keys.]

Zcash has fundamentals without communication, which has to be handled by caller.
- Implements as specified in FROST paper, as opposed to variants.
- It assumes FROST under hood, not necessarily used in other settings.
   - There are other variants that aren't proven secure! Need separate proofs for different settings!
   - STORM is proven for discrete-log keys.
- It's very straight-forward
- But communication is the hard part!
- It's important to provide, because it's what people want!

## Trusted Dealer Generation

Zcash also implemented Trusted Dealer Generation
- It's simpler, no lines of communication needed
- But in practice, people want DKG so that keys are never in memory
- Still, Trusted Dealer Generation has its usage because you could generate keys offline or in trusted hardware

But other feeling is that it's not worth the effort: if you're going to the effort, DKG makes sense

But there are use cases for Trusted Dealer Generation!
- Secp-ZKG may need Trusted Dealer Generation when it splits out its signing protocol
   - But they feel that's mainly for test vectors
- There might be other use cases, for people currently stuck in single-key models
- Might be useful for some custodians, for example for signing with shares without bringing them together.

## Resilience & Trust

We understand secrecy requirements of private keys. But not so much for the privacy & care of generated shares.
So what are new trust models for DKG?

One of the issues is that traditionally a user could write down a seed phrase, but FROST is more complex: there's a lot more information to backup. You can still sign if you lose a key, and a threshold of people can restore a share, so maybe there's not much as concern. But there's still some!

There are some difficult use cases! Need to agree on states. And FROST is only part of the answer!
- How do you develop using Lightning Channels?
- How do you develop in silicon and make sure there's no tricking into reuse of nonces?

## Curves

Zcash
- Implemented in Ristretto (subset of 25519) + Secp

Secp-ZKP
- Implemented on Secp

What about 25519? What are limitations? Do we need to move to Ristretto?
- IETF draft covers a range of curves
- There's a range of ciphersuites
- Can use one or both
- 25519 is a bit less efficient than Ristretto
- But it works!
- Signatures created by this manner are compatible!

Likely couldn't generate keys simultaneously for Ristretto & Secp because they're very different.
- Key generation is strongly tied to group order!

But probably Ristretto & 25519 shares could be used together!

## Trusted Channels

Expanding on trusted channels: signing doesn't require trusted channels, but the lack of trusted channels in signing introduces privacy and correlation issues. Is anyone doing anything on this?

- Not being currently considered for specification.
- Theoretically could run signing on DKG's secure channel.

## Alignment

What alignment is needed with crypto or other projects?

Zcash is obviously focusing most on Zcash
- Don't have bandwidth to work with Bitcoin
- But obviously would like to see specification & implementation

Secp-ZKP is similarly focused on own work
- Feels like ideas & specs are pretty compatible other than curves

Blockchain Commons is interested in specifications & test vectors, because they work with Bitcoin, W3C, and IETF.

## Performance

There's a desire to do FROST on _very_ low-power devices. Has anyone looked at performance issues & minimal processor requirements?
	
Not a lot of focus!
- But there are speed-ups possible
- Low-power devices _should_ work, because lots of optimization on curves

Could use low-power devices to generate shares
- And then anyone on any computer could combine them into a single signature

## Standards & Other Protocols

IETF is just [an informational draft](https://datatracker.ietf.org/doc/draft-irtf-cfrg-frost/).
- Goal is to allow people to implement things in close to parity.
- Any standardization would be a separate effort.

FROST will be a submission to threshold schemes with NIST.

BIP work ongoing too.

## Wild Card Uses of FROST

FROST with Lightning!
- Issues with consensus on state channels

More generally: there's an idea of nesting FROST. To what extent can you nest FROST, for example with MuSig keys? 
- No security proofs
- But there are interesting use cases!

Or, a BIP32-style key hierarchy with FROST.

## ROAST

Design goal was that [ROAST](https://eprint.iacr.org/2022/550.pdf) be an extension of FROST
- Supposed to provide robustness, so you're guaranteed to get a signature
- But may not be anyone working on it
- So not much purpose in writing a standard at the moment

## FROST Quorums

What other proofs can a FROST quorum do?

There's some use cases for non-accountable signatures, where you don't know who signed
- But what about if you need accountable signature, where you know who signed?
- Just publish the signature shares! Each one is independently verifiable as long as you know individual public keys.
- The deficit is you lose size advantage of aggregated signatures.
- So perhaps publish FROST signature and you also have a ledger where you persist the individual signature shares or even aggregate them in MuSig to minimize size
- There was a question on whether individual signature shares are forgeable!
   - If you're a participant you can forge signature shares of other participants, at least in MuSig2
      - Doesn't work against classic FROST1
      - This was exact difference with security in FROST2

There's a paper from Chelsea on [private & accountable signatures](https://eprint.iacr.org/2022/1636.pdf), but you're always going to need to a second accountability layer

## Other Expansions

Adapter signatures offer opportunity for multi-hop locks

Could use MuSig-style ZKPs to create deterministic nonces for FROST.

There's also the possibility of a FROST inside a FROST
- If inner-FROST are all DKGed, have to do something else
- But can convert independent random values into Shamir Shares
   - End up with independent sharing of new secret
   - And it's all done without communication!
   - (As long as n & k aren't too big!)

There are definitely opportunities!

## The Next Step

The outreach to people working on FROST was helpful! And the meeting! Nice to have round table.

What is the next step?
- Deployment!
   - Need to work with wallets!
- Do this again! (In a few months)
   - Great information transmission
   - Repos are public, but are easy to lose track of!

And how do we get developers to use FROST?
- By releasing "batteries included", with all the pieces needed, and doing the best to keep "foot guns" away from developers



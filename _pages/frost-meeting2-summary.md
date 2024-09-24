---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Round Table II (29924): Summary"
hide_description: true
classes:
  - wide
permalink: /frost/meeting2/summary/
sidebar:
  nav: frost
---

_This is a rough summary of the FROST Implementer's Round Table on September 18, 2024._

## Presentation: secp-zkp FROST (Jesse Posner)

New research:

### Proactive and Dynamic Secret Sharing Protocols 

* Changing, repairing, refreshing shares without changing secrets
* No sweeps required!

But how do they work with FROST?
* They require a bivariate symmetric polynomial
   * Where FROST uses univariate polynomial
   * So we might need a different type of VSS!
* Potentially, Feldman VSS
* Commit to values for repair, etc.
* Test implementation:
   * [github.com/jesseposner/FROST-BIP340](https://github.com/jesseposner/FROST-BIP340)

No show stoppers so far!

### Diffie-Hellman Key Exchange with FROST

* BIP-352 says it's possible
* Create Partial Shared Share
* Experimenting with this
* Works with any set of Shamir shares whether they're created with FROST or not
   * A lot of opportunities to do things on top of FROST!
   * We need good naming to describe them!

### Unsafe to Use Raw DKG Output Directly On-chain

* BIP-341 recommends adding an unspendable script path to disallow hidden script paths
* So, better not to output an x-only public key from DKG
* Gives confusing impression that key is safe to use onchain
* Leave derivation of key outside of DKG
* So, the x-only negation logic should not be handled in the DKG
   * OK to use in signing, the question is whether to use it in key generaiton

### Next Steps for secp256k1-zkp

* PR 278 (trusted dealer)
   * Review & merge first!
* DKG will then be additive, limited to key gen
   * Separate PR
   * Based on DKG BIP

## Presentation: ZF FROST (Conrado)

Audited Rust crates in ZF FROST implement FROST
- very close to releasing 2.0.0
- API adjustments related to serialization
- Working on refresh shares functionality
- There is a PR for Taproot, waiting for review

Working on a Demo code
* [github.com/ZcashFoundation/frost-zcash-demo](https://github.com/ZcashFoundation/frost-zcash-demo)

FROST Server:
* Just helps participants to communicate

FROST Client:
* CLI-tool to run FROST using the server

Goals:
* Have ZF FROST creates used in Zcash-supporting wallets

## Presentation: frost-uniffi-sdk (Pacu)

https://github.com/pacu/frost-uniffi-sdk

Uniffi is a wrapper for Rust where a single FFI interface allows you to generate bindings for different language (Python, Swift, Ruby, Kotlin, others from third-party extensions such as GoLang)

So this project brings Zcash ZF FROST crates to other languages!
* A unified tool generates bindings for Kotlin, Swift, GoLang
* Then wrapped into more ergonomic API

Use Case: FROST Companion Application
* Mobile app that creates signing UX
* Can participate in TSS or DKG signature
* Can backup or restore shares
* In development
* Early, POC
* In the future, talks to the ZF FROST Server that's being worked on

## Presentation: ChillDKG (Jonas Nick & Tim Ruffing)

Work started with Jesse's PR for secp.
* But lots of challenges especially in key generation

DKG is the big challenge
* Because FROST RFC doesn't specify DKG
* So had to write a specification for DKG themselves
* "Practical Schnorr Threshold Signatures without the Algebraic Group Model" offered a new model
   * Replaces broadcast abstraction with Equality protocol
* SimplPedPop!
   * Requires making sure there's no malicious signer
   * That's where the new Eq (Equality) protocol comes in
   * Need to check INTEGRITY and AGREEMENT
  
CHILLDKG
* Wraps EncPedPop
* Wraps SimplePedPop

EncPedPop
* Encrypts SImplePedPop
* ECDH key pairs

ChillDKG
* Signers have long-term key pair
* Eq uses concerte CertEq
   * Everyone sends a signature of their Eq input to everyone
   * Signers terminate when they receive valid signatures from all 'n' parcipants
 
But do you backup?
* Seed once
* Recovery data per DKG session
   * The same for all participants!

There is a ChillDKG BIP

Discussion of Ambiguous Blame: either a certain participant _or_ the coordinator is misbehaving
* Goal is to debug process
* So you better know why process is failing
* But the coordinator can always assign blame! 
   * And that can be the network if the participants are just talking to each other
   * So you have to trust network/coordinator!

## FROST Federation (Kulpreet)

How we can create an online federation, even if there are occasional outages for parties?

Want to use FROST TSS

Want to cleave near to FROST Key Gen
- Robustness is important
- But can't use a coordinator

If no honest majority, Federation with disband
- OK to have a few have outages at any times

Current plan
- focus on DKG
- BFT broadcast spec
- Stay simple even if it costs runtime
- https://github.com/pool2win/frost-federation

## SeraiDEX-FROST (Luke Parker)

Serai is a decentralized exchange
- Large signing sets (up to 150 signers in a multisig!)
- adversarial environment

Implements PedPoP from FROST paper
- share encryption offered (D-H)
- do not handle auth/communication/consensus
   - who sent what, etc

Novel one-round DKG (DKG-576)
- Solely relies on the hardness of the EC DDH problem
- Does not require consensus on context/messages
- n of n, everyone just needs to get commitment from everyone else!
- t of n, need agreement on which t participated

Accomplished with eVRFs
- Exponent Verifiable Random Fucntions
- https://eprint.iacr.org/2024/397

Beyond PedPop with identifiable aborts,
* Serai also implements FROST
* The IETF specification
* Modular to challenge function
* Modular to the signing protocol
* Does not include public verification (a regret)
   * Have to be an active signer for verification
   * Greatly encourage adding public verification!

Creates available and reviewed:
* dkg with pedpop audited in March 2023
* modular-frost audited in March 2023
* bitcoin-serai audited in August 2023

## FROST Signatures on GOrdian Envelope

Using ZF FROST Implementation
- signatures are verifiable

Gordian Envelope has some real advantages

## Q&A

Raised many questions, limited discussion due to hour.

## Q&A: Key Formats

Should be OK to generate BIP-32 keys once you have DKG
- Could affect unforgeability of signing scheme, but don't think so. Need proof!
- Need to apply an unspendable script path!

## Q&A: VSS Compatibility

Probably want a single VSS for entire DKG
* But interoperability shouldn't be an issue because they're simple commitments

## Q&A: Trusted Channels

Any plans for Trusted Channels using NFC or QR?

## Next Up

[includes links]

What more could we do:
* actively soliciting groups to do security proofs

## Presentation: Gordian Envelope/FROST (Wolf McNally)

Using secp256k1 and frostsecp256k1 crates

Can convert from (and to) FROST keys
- allows FROST signing method
- uses Trusted Dealer

Entirely agnostic to how the key was generated!


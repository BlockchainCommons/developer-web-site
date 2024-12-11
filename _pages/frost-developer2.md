---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Developer II (2024): Overview"
hide_description: true
classes:
  - wide
permalink: /frost/developers2/
sidebar:
  nav: frost
---

The second FROST Developer Meeting occurred on **December 4, 2024**. The goal was to introduce FROST to developers and to present them with the opportunity to integrate FROST in 2025 as well as discussing the challenges of integration and revealing use cases that demonstrate how FROST is widely useful for a variety of tasks.

For an intro to FROST, see the [FROST page](/frost/) and ["A Layperson's Intro to Schnorr"](https://www.blockchaincommons.com/musings/Schnorr-Intro/).

## Media

<table width="100%">
  <tr>
    <td width="640px">
      <b>Video:</b>

{% include video id="FbrB1SCXCNc" provider="youtube" %}

    </td>
  </tr>
</table>

## Presentation Slides

<table width="100%">
  <tr>
    <td width="640px">

     <b>Intro to FROST:</b><br>

<a href="/assets/pdfs/frost-dev2-intro.pdf"><img src="/assets/pdfs/frost-dev2-intro.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>FROST for the Masses:</b><br>

<a href="/assets/pdfs/frost-dev2-stack.pdf"><img src="/assets/pdfs/frost-dev2-stack.jpg" style="border:2px solid white"></a>

    </td>
  </tr>
  <tr>
    <td width="640px">

     <b>FROST UNiFFI SDK:</b><br>

<a href="/assets/pdfs/frost-dev2-uniffi.pdf"><img src="/assets/pdfs/frost-dev2-uniffi.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>FROST Federations:</b><br>

<a href="/assets/pdfs/frost-dev2-federation.pdf"><img src="/assets/pdfs/frost-dev2-federation.jpg" style="border:2px solid white"></a>

    </td>
  </tr>
</table>

For more, also see the [rough summary](/frost/developers2/summary/) or the [raw transcript](/frost/developers2/transcript) of the event.

## Key Quotes

Quotes are drawn from [raw transcripts](/frost/meeting2/transcript/) and may not be entirely precise as a result, but convey many of the major themes of the meeting. See the video for more.

### Schnorr Signatures

_Overview:_ "I think the key thing to understand is that it really is all about Schnorr and it allows for quorums and for thresholds."
{: .notice--info}

_Aggregation:_ "One of the cool things about Schnorr is its ability to aggregate and split keys."
{: .notice--info}

_Legacy:_ "Schnorr is not applicable to the older legacy signature formats."
{: .notice--info}

### Challenges

_Bad Participants:_ "A misbehaving participant can denial of service as signature. ... Because of those privacy things we were just talking about, it makes it harder to identify the misbehaving or non-participating member."
{: .notice--info}

_Communication:_ "We needed some way for all parties to know that none of the parties is equivocating as in telling a bunch of parties X and telling the other parties Y."
{: .notice--info}

_Complexity:_ "We’ve made it as simple as we possibly can. It’s still really difficult."
{: .notice--info}

_Mistakes:_ "If a user messes this up, you can’t really go back a step. ... The non-robustness means you have to start from the beginning, which is very, very frustrating."
{: .notice--info}

_UX:_ "The user experience is bad. The user experience of crypto as a whole is bad. ... The features can be as good as they can possibly be. They can be the best in the business. But if it’s so difficult to use, nobody’s going to use it."
{: .notice--info}

_Challenges:_ "There’s a lot of vocabulary here that the general public ... don’t have. ... Again, a lot of this is shared with multi-sig in general and with cryptocurrency in general."
{: .notice--info}

## Decentralization

_Maximal Privacy:_ "First of all, I try to make available the most decentralized private version of something to exist. I think that is important to have out there in some capacity. Because then the users can decide how they want to exchange information out of band or whatever."
{: .notice--info}

_Trust:_ "We don’t want to centralize the trust."
{: .notice--info}

_UX:_ "Every decision to make UX easier that exists right now often has decentralization or trust trade-offs."
{: .notice--info}

### Distributed Key Generation

_Key Creation:_ "With distributed key generation, DKG, it’s a multi-party protocol to create the key."
{: .notice--info}

_Standardization:_ "There are a variety of ways to implement the distributed key generation."
{: .notice--info}

### Privacy

_FROST Advantages:_ "You cannot distinguish a quorum signature from an individual signature."
{: .notice--info}

_FROST Secrecy:_ "You can’t even tell which members of a group signed a threshold unless they reveal a lot of secret info."
{: .notice--info}

_Internet:_ "Anything that inherently touches the internet is less private."
{: .notice--info}


### Reshares

_Changing Participants:_ "Yes, you can reshare, meaning you can kick some people off, bring some people on, etc. Just completely do the entire scheme. But that doesn’t invalidate old share schemes."
{: .notice--info}

### Rounds

_One Round:_ "I am absolutely interested in any sort of thing that can bring this down to a one round handshake, which is still not great, but it’s better than two."
{: .notice--info}

### Scalability

_Size:_ "Even simple multisigs will be smaller."
{: .notice--info}

### Security

_SPOFs:_ "The key is split. That means the risks of one single point of failure can be radically reduced."
{: .notice--info}

### Trusted Dealers

_Transitional:_ "In many ways, it’s a great transitional technology as we move toward the distributed key generation, which is the next approach."
{: .notice--info}

## Key URLs

### Presentation Links

* **Stack Wallet**
   * [Stack Wallet](https://stackwallet.com/)
   * [Cypher Stack](https://cypherstack.com/)
   * [Serai Library](https://crates.io/crates/bitcoin-serai)
* **ZF FROST**
   * [ZF FROST Book](https://frost.zfnd.org/)
   * [UNiFFI SDK](https://github.com/pacu/frost-uniffi-sdk)
* **FROST Federation**
   * [FROST Federation Repo](https://github.com/pool2win/frost-federation)
  
### Specifications

*Cryptology ePrints:*
* [FROST: Flexible Round-Optimized Schnorr Threshold Signatures](https://eprint.iacr.org/2020/852) (Komlo & Goldberg)
* [ROAST: Robust Asynchronous Schnorr Threshold Signatures](https://eprint.iacr.org/2022/550.pdf)
* [STORM: Synchronous, disTributed, and Optimized geneRation of key Material](https://eprint.iacr.org/2023/292.pdf)

*BIPs:*
* [BIP 340: Schnorr Signatures](https://github.com/bitcoin/bips/blob/master/bip-0340.mediawiki)
* [BIP Draft: Signing BIP](https://github.com/siv2r/bip-frost-signing)
* [BIP Draft: ChillDKG](https://github.com/BlockstreamResearch/bip-frost-dkg)

### Blockchain Commons Resources

* [FROST Developers Page](https://developer.blockchaincommons.com/frost/)
* [FROST Implementers Signal Group](https://signal.group/#CjQKICnRahYHb6OM8OtK6CZgyb0nDwUXiBwly9ZC9nNXlC2HEhAlpdqY9LqSoX0zdbUVCs9Z)

## Sponsors

This meeting was sponsored by the [Human Rights Foundation](https://hrf.org/).

<center><a href="https://hrf.org/"><img src="https://www.blockchaincommons.com/images/sponsors/hrf-white.png"></a></center>


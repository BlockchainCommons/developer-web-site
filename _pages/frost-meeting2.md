---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Round Table II: September 2024"
tagline: "Presentations from Six Groups"
hide_description: true
classes:
  - wide
permalink: /frost/meeting2/
sidebar:
  nav: frost
---

The second FROST Round Table occurred on **September 18, 2024**. Almost twenty experts in the field, including  members of the [ZF Frost team](https://github.com/ZcashFoundation/frost) and the [secp-zkp FROST team](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/138) as well as a variety of designers working on libraries, federations, and deployments came together to talk about their experience with FROST.

For an intro to FROST, see the [FROST page](/frost/) and ["A Layperson's Intro to Schnorr"](https://www.blockchaincommons.com/musings/Schnorr-Intro/).

## Media

<table width="100%">
  <tr>
    <td width="640px">
      <b>Video:</b>

{% include video id="VxLTJ_OxGT4" provider="youtube" %}

    </td>
  </tr>
</table>

## Presentation Slides

<table width="100%">
  <tr>
    <td width="640px">

     <b>ChillDKG:</b><br>

<a href="/assets/pdfs/frostimp2/presentation-chilldkg.pdf"><img src="/assets/pdfs/frostimp2/presentation-chilldkg.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>FROST Federation:</b><br>

<a href="/assets/pdfs/frostimp2/presentation-federation.pdf"><img src="/assets/pdfs/frostimp2/presentation-federation.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>secp256k1-zkp:</b><br>

<a href="/assets/pdfs/frostimp2/presentation-secp.pdf"><img src="/assets/pdfs/frostimp2/presentation-secp.jpg" style="border:2px solid white"></a>

    </td>
  </tr>
  <tr>
    <td width="640px">

     <b>Serai DEX:</b><br>

<a href="/assets/pdfs/frostimp2/presentation-serai.pdf"><img src="/assets/pdfs/frostimp2/presentation-serai.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>FROST UniFFI SDK:</b><br>

<a href="/assets/pdfs/frostimp2/presentation-uniffi.pdf"><img src="/assets/pdfs/frostimp2/presentation-uniffi.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>ZF FROST Updates:</b><br>

<a href="/assets/pdfs/frostimp2/presentation-zffrost.pdf"><img src="/assets/pdfs/frostimp2/presentation-zffrost.jpg" style="border:2px solid white"></a>

    </td>
  </tr>
</table>

For more, also see the [rough summary](/frost/meeting2/summary/) or the [raw transcript](/frost/meeting2/transcript) of the event.

## Key Quotes

Quotes are drawn from [raw transcripts](/frost/meeting2/transcript/) and may not be entirely precise as a result, but convey many of the major themes of the meeting. See the video for more.

### Accessibility

_Using FROST:_ "FROST itself is straightforward, right? The algorithm. So the hard part is to use it."
{: .notice--info}

### Dangers

_Script Paths:_ "Unlike MuSig, the FROST group public key is not randomized and that means a malicious party could add an undetectable script path during the DKG."
{: .notice--info}

_DKG Outputs:_ "It’s better not to output an x-only public key from the DKG because we really don’t want the API callers to think it’s safe to just use that directly on-chain without adding an unspendable script path."
{: .notice--info}

_Signing:_ "We definitely want to do some negation logic during signing."
{: .notice--info}

_Checking Integrity & Agreement:_ "An integrity property is not enough. We need an additional property and we call that agreement or to be more precise conditional agreement. ... This agreement property is often an overlooked requirement in the in the FROST world."
{: .notice--info}

_Networks:_ "You always have to trust your network."
{: .notice--info}

### Efficiency

_n^2:_ "I also wanted to share my personal opinions that protocols with N-squared complexity are too expensive to be regularly run."
{: .notice--info}

### Use Cases

_Mining Pools:_ "We want to replace the centralized Bitcoin mining pool operator with the [FROST] Federation of pool operators."
{: .notice--info}

### Channels

_Requirements:_  "It requires you to have, first of all, secure channels between the participants and, secondly and a more difficult problem, a broadcast channel between the participants."
{: .notice--info}

### Distributed Key Generation

_Specifications:_ "The RFC for Frost ... does not specify a DKG."
{: .notice--info}

### Public Keys

_Distribution:_ "We’re assuming the public keys are already distributed."
{: .notice--info}

### Secret Shares

_Share Updates:_ "Can we use proactive and dynamic Secret sharing protocols? These are protocols where you can refresh shares, repair shares, add and remove participants, and increase and lower the threshold without changing the underlying secret."
{: .notice--info}

_Univariate Issues with Updates:_ "Unless there’s a way to convert the FROST shares in the univariate polynomial shares into bivariate shares and then back again, I think we need a different type of VSS."
{: .notice--info}

_Avoiding Sweeps:_ "Sweep transactions ... are expensive and they’re bad for privacy because they link all the UTXOs together."
{: .notice--info}

_Verification:_ "I regret not offering public verification."
{: .notice--info}

## Key URLs

### GitHub & Crates

* **FROST Federation**
   * [FROST Federation Repo](https://github.com/pool2win/frost-federation)
* **secp256k1-zkp**
   * [FROST-BIP340 PR](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/138)
   * [FROST Trusted Dealer PR](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/278)
   * [FROST-BIP340 Current Work Repo](https://github.com/jesseposner/FROST-BIP340)
* **Serai DEX - FROST**
   * [dkg Repo](https://github.com/serai-dex/serai/tree/next/crypto/dkg)
      * [dkg crate](https://crates.io/crates/dkg)
   * [modular-frost crate](https://crates.io/crates/modular-frost)
   * [bitcoin-serai crate](https://crates.io/crates/bitcoin-serai)
* **ZF FROST**
   * [ZF FROST Repo](https://github.com/ZcashFoundation/frost)
   * [FROST Demos Repo](https://github.com/ZcashFoundation/frost-zcash-demo)
   * [FROST UniFFI SDK Repo](https://github.com/pacu/frost-uniffi-sdk)
   * **PRs & Issues**
      * [BIP-32 PR](https://github.com/conduition/frost/compare/add-secp256k1-tr..taproot-bip32)
      * [Updating Shares Issue](https://github.com/ZcashFoundation/frost/issues/519)
  
### Specifications

* [BIP 340: Schnorr Signatures](https://github.com/bitcoin/bips/blob/master/bip-0340.mediawiki)
* [BIP Draft: Signing BIP](https://github.com/siv2r/bip-frost-signing)
* [BIP Draft: ChillDKG](https://github.com/BlockstreamResearch/bip-frost-dkg)

### Papers

* [Exponent-VRFs and Their Applications](https://eprint.iacr.org/2024/397) (Boneh, Haitner & Lindell)
   * [Robust eVRF Gist](https://gist.github.com/kayabaNerve/cfbde74b0660dfdf8dd55326d6ec33d7) (Parker)
* [FROST: Flexible Round-Optimized Schnorr Threshold Signatures](https://eprint.iacr.org/2020/852) (Komlo & Goldberg)
* [Practical Schnorr Threshold Signatures without the Algebraic Group Model](https://eprint.iacr.org/2023/899) (Chu, Gerhart, Ruffing & Schröder)
* [Requirements for a nested MuSig/FROST scheme Gist](https://gist.github.com/jonasnick/6a56ae6bdc7c3a444d01817a897fbcf6) (Nick)
* [SPRINT: High-Throughput Robust Distributed Schnorr Signatures](https://eprint.iacr.org/2023/427) (Benhamouda, Halevi, Krawczyk, Ma & Rabin)

### Blockchain Commons Resources

* [FROST Developers Page](https://developer.blockchaincommons.com/frost/)
* [FROST Implementers Signal Group](https://signal.group/#CjQKICnRahYHb6OM8OtK6CZgyb0nDwUXiBwly9ZC9nNXlC2HEhAlpdqY9LqSoX0zdbUVCs9Z)

## Sponsors

This meeting was sponsored by the [Human Rights Foundation](https://hrf.org/).

<center><a href="https://hrf.org/"><img src="https://www.blockchaincommons.com/images/sponsors/hrf-white.png"></a></center>


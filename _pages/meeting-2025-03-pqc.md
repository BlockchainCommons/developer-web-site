---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
tagline: Post Quantum Cryptography
title: "Gordian Developer Meeting: March 2025"
hide_description: true
classes:
  - wide
permalink: /meetings/2025-03-pqc/
redirect_from:
  - /meeting/2025-pqc/
sidebar:
  nav: architecture
---

The [Gordian Developer Meeting](https://www.blockchaincommons.com/subscribe/#gordian-developers) on **March 5, 2025** focused on Post-Quantum Cryptography, including a presentation from [Foundation](https://foundation.xyz/) on their QuantumLink communications protocol, built into [Passport Prime](https://foundation.xyz/buy-passport-prime/) and a presentation from Blockchain Commons Lead Researcher [Wolf McNally](https://wolfmcnally.com/) on the Post-Quantum Cryptography incorporated into the Blockchain Commons stack to support Foundation's work.

This meeting also includes a short discussion of the [ZeWIF project](/chains/zcash/zewif) and its importance.

## Media

<table width="100%">
  <tr>
    <td width="640px">
      <b>Video:</b>

{% include video id="ZfsH2fIHCIA" provider="youtube" %}

    </td>
  </tr>
</table>

## Presentation Slides

<table width="100%">
  <tr>
    <td width="640px">

     <b>Meeting Overview:</b><br>

<a href="/assets/pdfs/202503-pqc-overview.pdf"><img src="/assets/pdfs/202503-pqc-overview.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>QuantumLink:</b><br>

<a href="/assets/pdfs/202503-quantumlink.pdf"><img src="/assets/pdfs/202503-quantumlink.jpg" style="border:2px solid white"></a>

    </td>
    <td width="640px">

     <b>PQC @ Blockchain Commons:</b><br>

<a href="/assets/pdfs/202503-pqc-bc.pdf"><img src="/assets/pdfs/202503-pqc-bc.jpg" style="border:2px solid white"></a>

    </td>
  </tr>
</table>

For more, also see the [rough summary](/meeting/2025-pqc/summary/) and the [raw transcript](/meeting/2025-pqc/transcript/).

## Key Quotes

Quotes are drawn from the [raw transcript](/meeting/2025-pqc/transcript/) and may not be entirely precise as a result, but convey many of the major themes of the meeting. See the video for more.

### Quantum Computing Overview

***What is Quantum Computing?*** "Quantum computers take advantage of the superposition and entanglement of very small particles."
{: .notice--info}

***Are Quantum Computers Real?*** "Quantum computers do really exist. This isn't science fiction, but there are some limitations."
{: .notice--info}

***When Will Quantum Computers Break Crypto?*** "Practical crypto-attacks by major funded, say, sovereign governments are still probably five to ten years off, but we really don’t know. Thus, in order to be proactive, we need post-quantum cryptography."
{: .notice--info}

***Is all Cryptography Endangered?*** "Even with quantum attacks, ChaCha20Poly1305 remains secure for the foreseeable future. ... But public-key cryptography, RSA, ECC, Diffie-Hellman, is completely broken by Shor’s algorithm."
{: .notice--info}

### QuantumLink

***What's the basis of QuantumLink?*** "I wanted to talk today about a protocol that we’ve been implementing called QuantumLink, and it’s based on a lot of underlying Blockchain Commons protocols and specifications."
{: .notice--info}

***Why was QuantumLink needed as a new communication method?*** "We started to work on the design of [Passport Prime], and we realized this QR code scanning is not really going to scale to the next billion people that are going to onboard to Bitcoin ... we wanted a protocol that would provide sort of all the benefits of air gap security, but with a lot of improvements to the user experience."
{: .notice--info}

***How does QuantumLink work?*** "QuantumLeak works by using out of band key exchange to establish initial trust. ... Then we create an encrypted tunnel between two devices [over Bluetooth], and when a message is sent, every message is signed with a cryptographic signature. So every message is both encrypted and digitally signed."
{: .notice--info}

***How are devices identified?*** "In addition to some of the metadata, it includes something called a XID document, which is another Blockchain Commons standard, where there’s an extensible identifier, which is a unique ID for each of the parties."
{: .notice--info}

***Why use Bluetooth?*** "The bandwidth on Bluetooth is high enough that we can do firmware updates or downloads of new apps over the air. The other thing is now Passport could be kept up to date, so it can have the current Bitcoin price, it can have blockchain status info, it can have UTXO data, it can know which addresses have been used."
{: .notice--info}

***Is the Bluetooth chip secure?*** "The main MCU and the Bluetooth MCU are separate chips. So it’s not possible to compromise the Bluetooth chip and have that affect the main MCU."
{: .notice--info}

***How is Quantum Link quantum resistant?*** "For QuantumLink, we chose ML-KEM for encryption. This is the module lattice-based key encapsulation mechanism. ... It’s not actually used for asymmetric encryption of the payload, but instead it’s used to encrypt a symmetric encryption key like ChaCha20POLY1305 or AES256. And for the digital signatures, we chose ML-DSA, which is Module Lattice-Based Digital Signature Algorithm."
{: .notice--info}

***How hard is quantum resistance to implement?*** "We were able to essentially drop in the post-quantum encryption over top of the GSTP version we were already running."
{: .notice--info}

***What Blockchain Commons protocols were used?*** "All the protocols we mentioned, all the standards are open source. So GSTP, UR, Envelope, XID. These are all provided by Blockchain Commons, I think almost all under the BSD2 clause plus patent license."
{: .notice--info}

## Post-Quantum Cryptography at Blockchain Commons

***What Cryptographic Algorithms Are Available in Blockchain Commons' Reference Libraries?*** "You can basically choose between classical algorithms and quantum algorithms just by changing essentially one line of code. ... we don’t consider this to be crypto agility as is used by some standards organizations, but we are crypto agnostic."
{: .notice--info}

***How Hard Is It To Use?*** "Part of one of our guiding principles is that average engineers, like myself in many ways, should not have to be cryptographers. So we choose best of breed kind of algorithms and try to make it very hard to do the wrong thing with our APIs."
{: .notice--info}

***What Are the Disadvantages of Quantum-Resistant Algorithms?*** "quantum signatures and encapsulated keys are significantly slower and larger."
{: .notice--info}

***Quantum Keys and Signatures Are Bigger?*** "Schnorr, ECDSA, and ED25519 have 32-byte keys and 64-byte signatures ... but that’s dwarfed by the size of the ML-DSA keys and signatures."
{: .notice--info}

## The ZeWIF Project

***See [the ZeWIF pages](/chain/zcash/zewif/) for more on the ZeWIF project.***

***What is Blockchain Commons Working with Zcash?*** "Our goal is to protect everybody. So if you’re doing self-sovereign digital wallets, we want you to use our kind of layer zero standards to make sure that your customers do not have losses or are vulnerable to various kinds of attacks."
{: .notice--info}

***Why Is an Interchange Format Important?*** "Interchange allows people to freely move their funds. We don’t want people to be locked into a single wallet. We want to encourage cooperation between wallets so that they can do things, but we also want to make sure that users don’t get locked in."
{: .notice--info}

***How Does This Relate to Blockchain Commons' Vision?*** "These are fundamental [Gordian principles](https://developer.blockchaincommons.com/principles/) of openness, independence and resilience. These are fundamental to self-sovereign management of digital assets."
{: .notice--info}

## Key URLs

### Presentation Links

* **Quantum Link**
   * [Foundation](https://foundation.xyz/)
   * [Passport Prime](https://foundation.xyz/buy-passport-prime/)
* **Blockchain Commons Standards**
   * [Envelope](/envelope/)
   * [GSTP](/envelope/gstp/)
   * [XIDs](/xid/)
* **Blockchain Commons Reference Libraries**
   * [bc-components](https://github.com/BlockchainCommons/bc-components-rust)
* **ZeWIF**
   * [ZeWIF Overview](/chains/zcash/zewif/)

## Sponsors

Thanks to our sponsor [Foundation](https://foundation.xyz/) for their presentation and their support of our PQC work.

<center><a href="https://foundation.xyz//"><img src="https://www.blockchaincommons.com/images/sponsors/foundation-logo-black.png"></a></center>


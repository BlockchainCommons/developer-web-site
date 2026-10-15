---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-sigsystem.jpg
  og_image: /assets/images/bc-card.jpg
title: "Cloudflare Passkeys"
tagline: "Creating Passkey-only Identity"
hide_description: true
classes:
  - wide
permalink: /cloudflare-passkey/
redirect_from:
  - /cloudflare-passkeys/
  - /passkey-cloudflare/
sidebar:
  nav:
    - signatures
    - technology
---

<div class="hexline hexgrid71">
  <div class="hex11 opaqued">
    <a href="https://learningfrost.blockchaincommons.com/">
      <img src="/assets/badges/learning-frost.png">
    </a>
  </div>  
  <div class="hex21 opaqued">
    <a href="/frost/">
      <img src="/assets/badges/frost.png">
    </a>
  </div>
  <div class="hex12top">
    <a href="/signatures/">
      <img src="/assets/badges/cat-sig-half.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/musig/">
      <img src="/assets/badges/musig.png">
    </a>
  </div>  
  <div class="hex41 highlighted">
    <a href="/cloudflare-passkey/">
      <img src="/assets/badges/cloudflare-passkey.png">
    </a>
  </div>
  <div class="hex51 opaqued">
    <a href="/open-integrity/">
      <img src="/assets/badges/open-integrity.png">
    </a>
  </div>
  <div class="hex61">
    <a href="/open-integrity/forge/">
      <img src="/assets/badges/open-integrity-forge.png">
    </a>
  </div>
  <div class="hex71 opaqued">
    <a href="/provemark/">
      <img src="/assets/badges/provenance-marks.png">
    </a>
  </div>
</div>

_The Cloudflare passkey system
([`passkey-cloudflare`](https://github.com/BlockchainCommons/passkey-cloudflare))
is a passkey-only identity library for web apps, built on Cloudflare
Workers and Durable Objects._

## Why is `passkey-cloudflare` Important?

In the modern internet ecosystem, auth diversification is growing
increasingly poor. Too much depends on either Apple or Google, which
can turn a supposedly secure multifactor authentication system into
something that can be subverted by a single centralized entity.

However, an answer that puts identity straight into the hands of the
users will fail without sufficient support. A non-centralized method
is required that nonetheless can piggyback on existing secure code,
closely controlled proofs of identity such as biometrics, and robust
recovery methods.

`passkey-cloudflare` offers the compromise needed, as it puts the
passkey fully into the hands of the user (or their preferred agent),
but does so within the framework of Cloudflare's robust system.

## How Does `passkey-cloudflare` Work?

The library answers one question: which principal is presenting.
WebAuthn verification is done by @simplewebauthn/server, which leaves
storage, sessions and recovery to the application. This library is the
rest of the identity layer: identity records and passkeys kept in
Durable Objects on Cloudflare (no D1 or KV), sessions and step-up,
recovery codes, operator tools, rate limits and the uniform refusal.

The application will decide then what that user may do once they have
identified themselves with a passkey. A user may hold more than one
passkey, and an application may see them as equivalent or totally
separate.

### Design Principles

* Identity separate from permission.
* Plural passkeys from the first registration.
* No fallback to a weaker credential.
* Recovery codes are stored only as hashes.
* Step-up for what can lock the owner out.
* A new user handle for every ceremony.
* One refusal for every failure.
* Operators see state and counts, not device detail.

### Gordian Technologies

* [**UR**](/ur/) and [**dCBOR**](/dcbor/). Each recovery code is 128 random bits typed as a Gordian seed (dCBOR tag 40300) and shown as a ur:seed UR. That is the text form Gordian Seed Tool imports, so a person can keep their codes there.
* [**Bytewords**](/bytewords/) spell each passkey's label, and offer a words form of each recovery code for reading aloud or writing by hand.

## `passkey-cloudflare` Demo

<table width="100%">
  <tr>
    <td width="50%">
      <a href="https://passkeydemo.gordianstack.com/"><img src="/assets/screenshots/passkey.png"></a>
    </td>
    <td>
      <p>A simple demo of `passkey-cloudflare` allows you to create a passkey at `gordianstack.com` and log in with it. For a more extensive example of a passkey in use, see the <a href="https://forgedemo.gordianstack.com/">Open Integrity Forge demo</a>.</p>
    </td>
  </tr>
</table>  
 
## Open Integrity Forge Slides

<table width="100%">
  <tr>
    <td width="50%">

<a href="/assets/pdfs/2026-10-passkeys.pdf"><img src="/assets/pdfs/2026-10-passkeys.jpg" style="border:2px solid white"></a>


    </td>
    <td>
    
      <p>These slides were taken from the October 2026 Gordian Developers meeting. Slides 7-12 provide the basic facts of the system: how it works and what its intent was.</p>
      
    </td>
  </tr>
</table>  

## `passkey-cloudflare` Links

* [**`passkey-cloudflare` Repo**](https://github.com/BlockchainCommons/passkey-cloudflare)
* [**Live Demo**](https://passkeydemo.gordianstack.com/)
* [**Open Integrity Forge**](https://developer.blockchaincommons.com/open-integrity/forge/) (built with passkeys)
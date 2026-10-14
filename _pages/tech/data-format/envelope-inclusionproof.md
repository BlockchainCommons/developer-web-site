---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataformat.jpg
  og_image: /assets/images/bc-card.jpg
title: "Envelope Inclusion Proof"
hide_description: true
classes:
  - wide
permalink: /envelope/inclusion-proof/
redirect_from:
  - /envelope/inclusionrpoof/
sidebar:
  nav:
    - envelope
    - dataformat
    - technology
---

<div class="hexline hexgrid72">
  <div class="hex12top">
    <a href="/data-formats/">
      <img src="/assets/badges/cat-dataformat-half.png">
    </a>
  </div>
 <div class="hex11 opaqued">
    <a href="/envelope/recursion/">
      <img src="/assets/badges/recursion.png">
    </a>
  </div>
 <div class="hex21">
    <a href="/hashed-elision/">
      <img src="/assets/badges/hashed-elision.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/known-values/">
      <img src="/assets/badges/known-values.png">
    </a>
  </div>
  <div class="hex32 highlighted">
    <a href="/envelope/inclusion-proof/">
      <img src="/assets/badges/inclusion-proof.png">
    </a>
  </div>
  <div class="hex41">
    <a href="/envelope/">
      <img src="/assets/badges/envelope.png">
    </a>
  </div>
  <div class="hex51 opaqued">
    <a href="/envelope/permits/">
      <img src="/assets/badges/permits.png">
    </a>
  </div>
  <div class="hex52 opaqued">
    <a href="/envelope/salt/">
      <img src="/assets/badges/salt.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/envelope/request/">
      <img src="/assets/badges/request-response.png">
    </a>
  </div>
  <div class="hex71 opaqued">
    <a href="/envelope/gstp/">
      <img src="/assets/badges/gstp.png">
    </a>
  </div>
  <div class="hex72 opaqued">
    <a href="/envelope/esc/">
      <img src="/assets/badges/esc.png">
    </a>
  </div>
</div>

_An inclusion proof allows you to prove the existence of something
contained within a [Gordian Envelope](/envelope/) that was elided._

## Why Are Inclusion Proofs Important?

Envelope supports the design philosophies of [data
minimization](/architecture/data-minimization/) and [progressive
trust](/architecture/progressive-trust/). These allow a user to
minimize the amount of data that they reveal, but then increase that
disclosure over time as trust increases.

A simplistic system would just allow users to reveal new packets of
data over time. But a more sophisticated system such as Gordian
Envelope allows users to log their data, hide it using [hashed
elision](/hashed-elision/), and then reveal it at a future time,
simultaneously proving that it had been there the whole time.

This is what an inclusion proof does. It's powerful because data that
was present but hidden the whole time should have more weight than a
newly introduced claim.

## How Do Inclusion Proofs Work?

In a Gordian Envelope, you first create a data set. This could be the
full contents of an envelope, a sub-envelope, or an edge within a
[XID](/xid/). You then redact the data set, which removes the core
information but leaves behind the hash.

At a future date, you reveal the redacted information as an "inclusion
proof". The fact that its hash matches the hash that was left in the
envelope proves that it was originally part of that envelope.

## What Are the Limitations of Inclusion Proofs?

The biggest limitation of inclusion proofs is that you don't know what
else might be redacted. For example, if someone wanted to trick people
into thinking that they were a potent prophet, they could log a whole
set of predictions in an envelope, but also log their opposites, and
reveal only the proper one as that event occurs.

This means that any validator must be thoughtful. They must consider
things like the specificity of an attestation and how many things are
redacted within the envelope.

## Inclusion Proof Links

* [**Gordian Envelope**](/envelope/)
* [**Hashed Elision**](/hashed-elision/)
* [**XID**](/xid/)

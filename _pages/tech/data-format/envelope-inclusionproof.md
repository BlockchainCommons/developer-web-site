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

## Overview

An inclusion proof allows you to prove the existence of something
contained within a [Gordian Envelope](/envelope/) that was elided.

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

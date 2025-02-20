---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: A Guide to Using URs for Envelope
hide_description: true
classes:
  - wide
permalink: /ur/envelope/
sidebar:
  nav:
    - ur
---

[Gordian Envelope](/envelope/) is a privacy-preserving smart document system that allows for the storage and transmissions of collections of data and metadata and also features extensions for elision, encryption, compression, and more. It is generally considered a replacement for bare URs, as data can be better documented and better protected by Envelopes.

However, Gordian Envelopes can also be expressed in the UR format, and this is a helpful format for storing and transmitting them!

## Why Use URs for Envelope?

Though Envelope could be natively stored as raw CBOR, URs were developed to make CBOR data more portable, more interoperable, and self-describing. These advantages all apply to Envelopes as well:

* The `ur:envelope` prefix tells a future holder of an Envelope exactly what the data is.
* UR Envelopes can be encoded as QRs to allow easy transmission over airgaps or easy (printed) storage.
* Animated QRs allow for the easy transmission of larger data files.

## Envelope: [envelope](https://datatracker.ietf.org/doc/draft-mcnally-envelope/)

The Envelope spec appears as an IETF draft, which includes the CDDL for Envelope, which is currently:
```
   envelope = #6.200(envelope-content)
   envelope-content =
       leaf /
       elided /
       node /
       assertion /
       wrapped

   leaf = #6.201(any)

   elided = sha256-digest
   sha256-digest = bytes .size 32

   node = [subject, + assertion-element]
   subject = envelope-content
   assertion-element = assertion / elided-assertion
   elided-assertion = elided           ; MUST represent an assertion.

   assertion = { predicate => object }
   predicate = envelope-content
   object = envelope-content

   wrapped = envelope
```
(Refer to the Envelope RFC to ensure this is up-to-date.)





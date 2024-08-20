---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Envelope GSTP Technical Overview
hide_description: true
classes:
  - wide
permalink: /envelope/gstp/tech/
sidebar:
  nav: envelope
---

## Step 1: Build a Request/Response Message

GSTP messages are built as [Request/Response messages](/envelope/request/) using [Gordian Envelope](/envelope/). A Request/Response message basically means that a sender is asking a remote Envelope recipient to do something and return the results. 

<img src="/assets/images/gstp-ex-1.jpeg" style="border: 1px solid gray !important">

## Step 2: Add Public Key

A Authentication Layer is now created. This begins by the sender adding a public key, which they will later prove that they own with a signature.

<img src="/assets/images/gstp-ex-2.jpeg" style="border: 1px solid white !important">

## Step 3: Add Sender's Continuation

Both participants can include continuation information in messages, which allows them to maintain state without having to do so locally. This is Encrypted State Continuation (ESC). Here, the sender adds their own continuation information to the message.

<img src="/assets/images/gstp-ex-3.jpeg" style="border: 1px solid white !important">

## Step 4: Encrypt Sender's Continuation

All continuation information is encrypted so that it's only readable by the party that created it. Here, the sender encrypts their continuation.

<img src="/assets/images/gstp-ex-4.jpeg" style="border: 1px solid white !important">

## Step 5: Copy Recipient's Continuation

The sender also copies over the continuation sent by the recipient in a previous message (if any). Because it's encrypted, they can't read it: they just copy it over.

<img src="/assets/images/gstp-ex-5.jpeg" style="border: 1px solid white !important">

## Step 6: Wrap the Content

All Envelope assertions apply to a subject. That means that whenever you want something to apply to the entire contents of an Envelope, and not just the subject, you need to wrap it, essentially creating a new Envelope with the entirety of the previous Envelope as the subject. Wrapping the envelope-to-date is the next step.

<img src="/assets/images/gstp-ex-6.jpeg" style="border: 1px solid white !important">

## Step 7: Sign the Wrapped Envelope

With the Envelope now wrapped, the sender can sign the entire contents with the private key matching the public key that's included in the contents.

<img src="/assets/images/gstp-ex-7.jpeg" style="border: 1px solid white !important">

## Step 8: Wrap the Envelope Again

The Envelope is now wrapped a second time. This is so that when encryption is applied, it applies to the entire Envelope. Otherwise, it would only apply to the subject, which does not include the brand-new signature. This wrapping creates the foundation for the Encryption layer.

<img src="/assets/images/gstp-ex-8.jpeg" style="border: 1px solid white !important">

## Step 9: Encrypt the Wrapped Envelope

The wrapped envelope is encrypted using a symmetric key.

<img src="/assets/images/gstp-ex-9.jpeg" style="border: 1px solid white !important">

## Step 10: Encrypt the Symmetric Key

Finally, the symmetric key is encrypted using the recipient's public key and an ephemeral public key from the sender. The recipient will be able to decrypt it using their private key.

<img src="/assets/images/gstp-ex-10.jpeg" style="border: 1px solid white !important">

## Step 11: Reverse the Process

The recipient then reverses the process:

1. They use their private key and the recipient's ephemeral public key to decrypt the symmetric key
2. They use the symmetric key to decrypt the message.
3. They verify the signature against the sender's public key.
4. They decrypt their continuation.
5. They decide whether to run the Request using their continuation.
6. Assuming they do, they create a Response, which is the start of their own Steps 1-10.

_These slides are all drawn from the [Understanding GSTP video](https://www.youtube.com/watch?v=QnH14LkJOnI). Take a look for more details or if you prefer video learning._

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Extensible Identifiers (XIDs)
hide_description: true
classes:
  - wide
permalink: /xid/
sidebar:
  nav: xid
---

## Overview

<a href="/core-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-core-id.png" style="float: right; margin-left: 20px;" width="25%"></a>

The eXtensible IDentifier (XID) is an idea for how to use [Gordian Envelope](/envelope/) to create a DID-like method and perhaps more importantly how to use dCBOR and Envelope to create a DID controller document. It is not necessarily conforming to the [DID spec](https://www.w3.org/TR/did-core/), but it is inspired by the same needs and desires.

Our initial presentation on XIDs was given at the October 22, 2024 meeting of the W3C Credentials Community Group (CCG). It suggests how to build a controller document using Gordian Envelope, desmonstrates how to elide that document, and reveals how to integrate with existing infrastructure.

<br clear=all>

<center>
<table width="100%">
  <tr>
    <td width="40%">
      <b>XID Video:</b>

{% include video id="lKVp5WzBZD4" provider="youtube" %}

    </td>
  </tr>
  <tr>
    <td width="40%">
      <b>XID Presentation:</b>
        <a href="https://developer.blockchaincommons.com/assets/pdfs/xids.pdf"><img src="/assets/pdfs/xids.jpg" style="border: solid 1px white;"></a>
    </td>
  </tr>
  <tr>
    <td width="40%">
      <b>Overview Video:</b>

{% include video id="k1iIO-bfVhM" provider="youtube" %}

    </td>
  </tr>
  <tr>
    <td width="40%">
      <b>Overview Presentation:</b>
        <a href="/assets/pdfs/xid-intro.pdf"><img src="/assets/pdfs/xid-intro.jpg" style="border: solid 1px white;"></a>
    </td>
  </tr>
</table>
</center>

Also see the transcript on our [XID Presentation page](/xid/presentation/)

We expect to expand on this topic more in the future! If you'd like to support its development, please <a href="mailto:team@blockchaincommons.com">contact us</a>!

## Why is XID Important?

The main advantage of XIDs is that they allow for the **redaction** of content in a controller document. 

Currently, controller documents tend to include public keys and service end-points. This is a great way to associate a variety of content into a singular identity. But you may not want to publicly reveal all of that information. Public keys can create vulnerabilities when exposed and you may wish to divide different parts of your identity, to only be revealed to specific groups!

With Envelope-enabled controller documents you can still gather and validate all of your information as part of a singular identifier. But, you can elide most of it most of the time, only revealing private elements through inclusion proofs to specific groups. Your data remains organized and you don't have to manage a whole bunch of different identities, but at the same time, vulnerable material remains secure and private.

## Links

* ["Gordian Envelope, Elision, and Controller Docs" Presentation](/assets/pdfs/xid-intro.pdf) (PDF)
* ["Gordian Envelope, Elision, and Controller Docs" Video](https://www.youtube.com/watch?v=k1iIO-bfVhM) (YouTube)
* [Gordian Envelope pages](/envelope/)

* [DID v1.0](https://www.w3.org/TR/did-core/) (W3C) 
* [Controller Doc v1.0](https://www.w3.org/TR/controller-document/) (W3C)

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

An eXtensible IDentifier (XID) is a stable decentralized identifier generated from the hash of an inception key. XIDs resolve to an [envelope](/envelope/)-based controller document for managing keys, credentials, and other assertions, and leverage provenance chains for key rotation and revocation without changing the identifier. It does not necessarily to the [DID spec](https://www.w3.org/TR/did-core/), but it is inspired by the same needs and desires.

## Why is XID Important?

The main advantage of XIDs is that they allow for the **redaction** of content in a DID-like controller document while also maintaining cryptographic verification.. 

Currently, controller documents tend to include public keys and service end-points. This is a great way to associate a variety of content into a singular identity. But you may not want to publicly reveal all of that information. Public keys can create vulnerabilities when exposed and you may wish to divide different parts of your identity, to only be revealed to specific groups!

With Envelope-enabled controller documents you can still gather and validate all of your information as part of a singular identifier. But, you can elide most of it most of the time, only revealing private elements through inclusion proofs to specific groups. Your data remains organized and you don't have to manage a whole bunch of different identities, but at the same time, vulnerable material remains secure and private.

This supports compartmentalized disclosure for [progressive trust architectures](/progressive-trust/) where the holder, not the issuer, controls which aspects are revealed to different parties.

## How Does XID Work?

A XID (“eXtensible IDentifier”) is a unique 32-byte identifier for a subject entity:
```
XID(71274df133169a0e2d2ffb11cbc7917732acafa31989f685cca6cb69d473b93c)
```
XIDs are encoded using CBOR. The hex is marked as length 32-bytes and then tagged with the CBOR tag `#6.40024`.
```
40024(h'71274df133169a0e2d2ffb11cbc7917732acafa31989f685cca6cb69d473b93c')
```
Converted to binary this is:
```
D9 9C58                                 # tag(40024)
   58 20                                # bytes(32)
      71274DF133169A0E2D2FFB11CBC7917732ACAFA31989F685CCA6CB69D473B93C
```
XIDs can also be encoded as [URs](/ur/):
```
ur:xid/hdcxjsdigtwneocmnybadpdlzobysbstmekteypspeotcfldynlpsfolsbintyjkrhfnvsbyrdfw
```

A XID is generated from the SHA-256 hash of the CBOR representation of a specific `PublicSigningKey` structure called the inception key. It can be resolved into a XID Document that declares public keys and their associated attributes.

This specifics on all of this, XID resolution methods, permissions, delegation, rotation, and much more can be found in ["BCR-2024-10: XIDs"](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-010-xid.md).

## XID Videos & Presentations

<center>
<table width="100%">
  <tr>
    <td width="50%">
      <b>XID Video:</b>

{% include video id="lKVp5WzBZD4" provider="youtube" %}

    </td>
    <td width="50%">
      <b>XID Presentation:</b>
        <a href="https://developer.blockchaincommons.com/assets/pdfs/xids.pdf"><img src="/assets/pdfs/xids.jpg" style="border: solid 1px white;"></a>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <b>Overview Video:</b>

{% include video id="k1iIO-bfVhM" provider="youtube" %}

    </td>
    <td width="50%">
      <b>Overview Presentation:</b>
        <a href="/assets/pdfs/xid-intro.pdf"><img src="/assets/pdfs/xid-intro.jpg" style="border: solid 1px white;"></a>
    </td>
  </tr>
</table>
</center>

Also see the transcript on our [XID Presentation page](/xid/presentation/)

## Links

* [**BCR-2024-010: XID: Extensible Identifiers**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-010-xid.md) (Blockchain Commons Research Document)
* [**"Gordian Envelope, Elision, and Controller Docs" Presentation**](/assets/pdfs/xid-intro.pdf) (PDF)
* [**"Gordian Envelope, Elision, and Controller Docs" Video**](https://www.youtube.com/watch?v=k1iIO-bfVhM) (YouTube)
* [**Gordian Envelope pages**](/envelope/)

* [**W3C: DID v1.0**](https://www.w3.org/TR/did-core/) (W3C) 
* [**W3C: Controller Doc v1.0**](https://www.w3.org/TR/controller-document/) (W3C)

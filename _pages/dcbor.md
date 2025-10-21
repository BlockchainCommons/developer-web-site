---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Deterministic CBOR (dCBOR)
hide_description: true
classes:
  - wide
permalink: /dcbor/
sidebar:
  nav: dcbor
redirect_from:
  - /cbor/overview/
---

## Overview

<a href="/core-stack/"><img src="https://developer.blockchaincommons.com/assets/images/bc-stack-core-dcbor.png" style="float: right; margin-left: 20px;" width="25%"></a>

dCBOR is deterministic CBOR, an application profile for CBOR as specified in [RFC
8949](https://www.rfc-editor.org/rfc/rfc8949.html#name-deterministically-encoded-c). dCBOR
itself is specified in [an IETF
Internet-Draft](https://datatracker.ietf.org/doc/draft-mcnally-deterministic-cbor/).

## Why is dCBOR Important?

Determinism ensures that the same data is always encoded in the same
way. It has strict rules for how data is represented and ensures that
edge cases are always represented in the same way. For dCBOR, for
example, this required the reduction of numbers to a canonical
encoding: 10.00, 10.0, and 10 must be the same thing.

Determinism is important for a wide variety of applications that
require data to remain consistent, usually across multiple
platforms. This includes data signatures, reproducible software
builds, hashing functions, version control, consensus algorithms,
secure multi-party computation, and zero-knowledge proofs. It can also
fulfill legal and compliance requirements.

dCBOR doesn't automatically produce determnism, but it _supports_ it,
as does [Gordian Envelope](/envelope/).

## How Does dCBOR Work?

Though RFC 8949 includes some requirements for dCBOR, it leaves other
options up to encoder developers. This results in dCBOR usage that is
not necessarily consistent across multiple encoders. As a result,
Blockchain Commons has produced an
[Internet-Draft](https://datatracker.ietf.org/doc/draft-mcnally-deterministic-cbor/)
of a dCBOR profile, which narrows the range of what can be considered
valid dCBOR.

All dCBOR is decodable by generic CBOR encoders, and generic CBOR
encoders *can* produce valid dCBOR, but not all CBOR is valid dCBOR.

We have also released libraries for dCBOR in Rust and Swift as well as
a command line application for testing encoding and decoding that all
follow the specifications laid out in this Draft.

## dCBOR Videos


<table width="100%">
  <tr>
    <td width="640px">
      <b>Why CBOR?</b>

{% include video id="uoD5_Vr6qzw" provider="youtube" %}

    </td>
    <td width="640px">
      <b>dCBOR Library from Blockchain Commons</b>

{% include video id="NlJE8oF1B5M" provider="youtube" %}

    </td>
  </tr>
</table>

## Libraries

{% include lib-dcbor.md %}

## Links

**Intro:**

* [Why CBOR?](https://www.blockchaincommons.com/introduction/Why-CBOR/) (blog article)
* [dCBOR Goes to IETF](https://www.blockchaincommons.com/specifications/dCBOR-IETF/) (blog article)

**Developer Resources:**

* [**dCBOR IETF I-D**](https://datatracker.ietf.org/doc/draft-mcnally-deterministic-cbor/) (IETF)
* [**Editor's Draft (Repo)**](https://github.com/BlockchainCommons/WIPs-IETF-draft-deterministic-cbor) (GitHub)

**Developer Resource Apps:**

* [**dcbor-cli Parser/Validator**](https://github.com/BlockchainCommons/dcbor-cli) (GitHub)

## dCBOR Discussions

* [8/23/23: Subnormal Floats and Determinism](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=cCMYMTnL56XEHr4N28ihvuSiC70)
* [8/23/23: I-D: draft-bormann-cbor-dcbor-03](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=ttHEFxoyEt_alghRULGlsGz1aAw)
* [8/19/23: CBOR profiling for protocol designers](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=lOwV0RBVb9eiLC1PN9PpgjTwSHs)
* [8/19/23: A deterministic summary](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=JfOG5zIHBzeVsXem6l6YgMVuoYs)
* [8/16/23: dCBOR on cbor.me](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=B8CWOhNdPjopvV13C8PeKTzOVWo)
* [8/15/23: Implementing float->int numeric reduction](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=9s8VA5pfLdfv7Xs3LhwPLhkvI1M)
* [8/8/23: New Version Notification for draft-mcnally](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=uqLJuY_o8QWmqzDwyrbOgrPsInc)
* [8/6/23: Announcement: draft-mcnally-deterministic-cbor-03](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=AmYT-P4ZCuD2NJuAoflQ1YLCrOY)
* [8/4/23: I-D: draft-mcnally-deterministic-cbor-02.txt](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=yW2l8Sj3FkN1Y-mIUPKSEULXm1s)
* [7/23/23: I-D Action: draft-bormann-cbor-dcbor-00.txt](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=ytaPD8bIz51osYXYMJyjMh6zOa8)
* [7/20/23: The rationale behind dCBOR](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=4PBVtODXMNT2oZboae9BBNiplJk)
* [7/18/23: FTZ ("flush to zero") and/or DAZ ("denormals are zero") floating point underflow strategies vs. Deterministic Encoding](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=MBll512W81zsMiEtxYvugDKrVVA)
* [7/17/23: Two drafts about CBOR Deterministic Encoding](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=OP7-Vq0cUjaS7qpP831lJkCk5BI)
* [6/30/23: cbor - Requested session has been scheduled for IETF 117](https://mailarchive.ietf.org/arch/browse/cbor/?gbt=1&index=X4K-UjBZP6wxxZdWgBYEfCp2vH4)
* [5/31/23: draft-mcnally-deterministic-cbor: rationale for not going with pre-encoded data](https://mailarchive.ietf.org/arch/msg/cbor/X4K-UjBZP6wxxZdWgBYEfCp2vH4/)
* [5/8/23: Updated Drafts for dCBOR I-D and Gordian Envelope Structured Data Format I-D & IANA Tag Registration](https://mailarchive.ietf.org/arch/msg/cbor/DOUxXB-IMTPtvDeGh13ob-IjJsE/)
* [5/8/23:  Proposal for Deterministic CBOR (dCBOR) discussion at May 17th meeting](https://mailarchive.ietf.org/arch/msg/cbor/G1oXN5DlSpAt7TI5re-fb1lL69I/)
* [3/12/23: dCBOR moving from numerically-typeless systems](https://mailarchive.ietf.org/arch/msg/cbor/aiGvqw1-sQWJ4pXY3zzQuWwNVzE/)
* [3/10/23: Decoding numbers and compliance verification in dCBOR](https://mailarchive.ietf.org/arch/msg/cbor/LUQ0lMaAA1ADGuRtb1VLahnlQUg/)
* [3/10/23: New IETF Internet-Draft draft-mcnally-deterministic-cbor-00](https://mailarchive.ietf.org/arch/msg/cbor/fnz_F5lQNiDiTJFAaJGB3YJdPik/)
* [3/5/23: Deterministic CBOR as a possible DISPATCH item](https://mailarchive.ietf.org/arch/msg/cbor/qMAOUa8-wIZn5Ts2_53VunGu7Co/)
* [2/16/23: Fwd: New deterministic CBOR Libraries (Rust & Swift) from Blockchain Commons](https://mailarchive.ietf.org/arch/msg/cbor/l7nzQHFjfpK9nfBOHiQ1L-Rr558/)

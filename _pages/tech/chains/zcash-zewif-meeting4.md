---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-chain-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "ZeWIF Meeting #4 (4/16/25)"
tagline: Envelope File Format Demo
hide_description: true
classes:
  - wide
permalink: /chains/zcash/zewif/meeting4/
sidebar:
  nav:
    - zcash
    - meetings
    - chains
redirect_from:
  - /zcash/zewif/meeting4/
---

Blockchain Commons and Zingo Labs held the four public meeting regarding the Zcash Extensible Wallet Interchange Format (ZeWIF) on April 16, 2025. The heart of the meeting was a demo of the Envelope data file format that is the final deliverable for the ZeWIF release. There was also continued discussion of ways to polish the data being stored in ZeWIF for maximum compatibility and future proofing.


## Media

<table width="100%">
  <tr>
    <td width="640px">
      <b>Meeting Video:</b>

{% include video id="oXm2FsR4KTs" provider="youtube" %}

    </td>
  </tr>
</table>

## Demo

_The [zmigrate CLI tool](https://github.com/BlockchainCommons/zmigrate) was demoed, showing how it outputs Envelope data files._

* **Input & Output Formats:** The zmigrate tool can now take inputs from two formats (Zcashd wallet, Zingo wallet) and output in four formats (ZeWIF, UR, Envelope, or diagnostic dump).
   * **ZeWIF File Format:** Lossless data format that stories data in assertions.
   * [**UR:**](/ur/) likely to be infrequently used, but allows for QRs, including [animated QRs](/animated-qrs/).
   * **Envelope:** Envelope diagnostic format is for debugging, shows things in a human readable format.
* **Compression & Encryption:** Compression is now part of the tool, about halves size. Encryption is in process.
   * **Digests:** Hashed digests of content do not change when content is compressed!
  
At this point, ZeWIF format converts 90% of data. We need PRs and Issues from the community to bring it over the finish line.

## Other Key Topics

_The following additional topics were discussed to continue advancing the ZeWIF formula._

* **Decomposition:** If something is _specified_, we should probably just store those bytes to future-proof the content.
* **Tagging:** CBOR tags are only being used for material that might be distributed individually. For ZeWIF that's currently the overall Envelope, which is identified with the 200/201 CBOR tags, which have been registered. Internally, things are identified with `isA` assertions.
* **Versioning:** It's import to version both the ZeWIF format itself and individual data elements. But, it's easy to do. Version could be an assertion, it could be a byte string. Lots of different methods: just use the one that makes sense.
* **Secrets:** It'd be good to zeroize secrets. This obviously isn't possible when reading in from a file, but it'd be good for the in-memory representation.

_ZeWIF is now at a stage where we're looking for the public to issue PRs to help us finalize it. Please feel free to do so at the repos for [zewif](https://github.com/BlockchainCommons/zewif), [zewif-zcashd](https://github.com/BlockchainCommons/zewif-zcashd), [zewif-zingo](https://github.com/BlockchainCommons/zewif-zingo), and [zmigrate](https://github.com/BlockchainCommons/zmigrate)._

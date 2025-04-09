---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-chain-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "ZeWIF Meeting #2 (2/24/25)"
tagline: Zmigrate Demo
hide_description: true
classes:
  - wide
permalink: /chains/zcash/zewif/meeting2/
sidebar:
  nav:
    - zcash
    - meetings
    - chains
redirect_from:
  - /zcash/zewif/meeting2/
---

The second meeting for the ZeWIF project was held on February 25, 2025. The first half of the meeting was spent on logistics of the project: reviewing where we are and ensuring that everyone was getting the resources they needed to proceed forward. One of the main discussions was the fact that some of the initial planning has been turned on its head, with coding of Zcash wallet importers coming first, to inform the development of the specification, rather than the opposite. 

As such, Blockchain Commons had an initial version of the [`zmigrate` crate](https://github.com/BlockchainCommons/zmigrate) to demo. That demo was following by discussion of how tests and more sample wallets could help the project move forward.

The following video covers the latter half of the meeting, starting with the `zmigrate` demo.

## Media

<table width="100%">
  <tr>
    <td width="640px">
      <b>Meeting Video:</b>

{% include video id="wSeAdx6oZLw" provider="youtube" %}

    </td>
  </tr>
</table>

_We continue to welcome comments. The [video itself](https://www.youtube.com/watch?v=wSeAdx6oZLw) might be a great place to leave comments on this work, but also free to leave Issues on the [zcash-wallet-formats repo](https://github.com/zingolabs/zcash-wallet-formats/issues) or the [`zmigrate` crate](https://github.com/BlockchainCommons/zmigrate) as you think appropriate._

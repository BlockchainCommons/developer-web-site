---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-chain-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Zcash Extensible Wallet Interchange Format (ZeWIF)
hide_description: true
classes:
  - wide
permalink: /chains/zcash/zewif/
sidebar:
  nav:
    - zcash
    - chains
redirect_from:
  - /zcash/zewif/
---

ZeWIF is a joint project between Blockchain Commons and [Zingo Labs](https://zingolabs.org/), sponsored by a [Zcash Community Grant](https://zcashcommunitygrants.org/). It was created in part to support the deprecation of the [zcashd reference wallet](https://z.cash/ecosystem/zcashd/) but moreso to create a format that supports [Openness and Indepence](https://developer.blockchaincommons.com/principles/) by allowing users to easily migrate among Zcash wallets without concern of loss of funds.

## Project Schedule

The project has been broken into five phases<sup>*</sup>:

1. Survey of existing wallet data (January 2025)
   * [Survey of Zcash Data Formats](https://github.com/dorianvp/zcash-wallet-formats/tree/master)
   * [Spreadsheet of Zcash Wallet Data](https://docs.google.com/spreadsheets/d/1MdahX4igppx7a4BdrcO5TGB2-mO1EtXrlKssypfEHUQ/)
   * [Meeting #1 on Wallet Data (1/24/25)](/chains/zcash/zewif/meeting1/)
   * [Report on Wallet Data](/chains/zcash/zewif/report1/)
2. Coding of initial ZeWIF crates (February 2025)
    * [Meeting #2 with first zmigrate demo (2/25/25)](https://www.youtube.com/watch?v=wSeAdx6oZLw)
3. Release of documented in-memory ZeWIF format (March 2025)
    * [Meeting #3 with abstraction discussion (3/14/25)](https://www.youtube.com/watch?v=qptTRJP_K2U)
    * [zewif Repo](https://github.com/BlockchainCommons/zewif)
    * [zewif-zcashd Repo](https://github.com/BlockchainCommons/zewif-zcashd)
    * [zewif-zingo Repo](https://github.com/BlockchainCommons/zewif-zingo)
    * [zmigrate CLI Repo](https://github.com/BlockchainCommons/zmigrate)
4. Release of ZeWIF data file format (April 2025)
    * [Meeting #4 with ZeWIF envelope demo (April 2025)](/chains/zcash/zewif/meeting4/)
5. Creation of legacy wallet CLI tool, ZExCavator (April 2025)
    * [Zexcavator Repo](https://github.com/zingolabs/zexcavator)

<i>* This is a revision from the original project schedule. The original proposal suggested a top-down design, with a specification leading to coding, while in practice a bottom-up design was used, with coded crates leading to a specification.</I>

## Interchange Format

The interchange format has two representations: in memory and as a [Gordian Envelope](/envelope/) file. For the creation of ZeWIF files, data is divided into three classes:

1. **First Class Data.** Data that is used by two or more wallets and considered to be current and important. This data is included in the core specification and is largely represented by Envelope typing using `isA` constructs.
2. **Second Class Data.** This will be most other data, especially data that is only used by a single wallet or not considered current or important. This data will not be specified and is instead suggested for inclusion using [attachments](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-006-envelope-attachment.md). Specific attachment points are available in many ZeWIF data structures.
3. **Third Class Data.** There may be some data, especially state, configuration, or encryption data, that is not considered useful for preservation. This will not be included in the core specifications and will not be suggested for inclusion in specifications. However, the best practice will be to preserve the entire original wallet data file, so that this "third class data" is not lost is there is a need for it in the future.

## Meetings

<table width="100%">
  <tr>
    <td width="640px">
      <b>#1: Wallet Survey:</b>

{% include video id="wy06xxpJy-M" provider="youtube" %}

    </td>
    <td width="640px">
      <b>#2: Zmigrate demo:</b>

{% include video id="wSeAdx6oZLw" provider="youtube" %}

    </td>
  </tr>
  <tr>
    <td width="640px">
      <b>#3: Abstraction Discussion:</b>

{% include video id="qptTRJP_K2U" provider="youtube" %}

    </td>
    <td width="640px">
      <b>#4: Envelope Demo:</b>

{% include video id="oXm2FsR4KTs" provider="youtube" %}

    </td>
  </tr>
</table>

Also see the meeting pages for the [survey meeting (January)](/chains/zcash/zewif/meeting1/), the [Zmigrate demo (February)](/chains/zcash/zewif/meeting2/), the [abstraction discussion (March)](/chains/zcash/zewif/meeting3/), and the [Envelope demo (April)](/chains/zcash/zewif/meeting4/) for more information.


## Sponsors

This work is sponsored by a [Zcash Community Grant](https://zcashcommunitygrants.org/). See [our grant proposal](https://github.com/ZcashCommunityGrants/zcashcommunitygrants/issues/3) for more information on the project, its original schedule, and its deliverables.

## Links

**ZeWIF Repos:**
* [zewif Repo](https://github.com/BlockchainCommons/zewif)
* [zewif-zcashd Repo](https://github.com/BlockchainCommons/zewif-zcashd)
* [zewif-zingo Repo](https://github.com/BlockchainCommons/zewif-zingo)
* [zmigrate Repo](https://github.com/BlockchainCommons/zmigrate)

**ZeWIF Crates:**
* [awaiting full release]

**Meeting Records:**
* [Meeting #1 on Wallet Data](/chains/zcash/zewif/meeting1/) [ [Video](https://www.youtube.com/watch?v=wy06xxpJy-M) ]
* [Meeting #2 with first zmigrate code demo](/chains/zcash/zewif/meeting2/) [ [Video](https://www.youtube.com/watch?v=wSeAdx6oZLw) ]
* [Meeting #3 with abstraction discussion](/chains/zcash/zewif/meeting3/) [ [Video](https://www.youtube.com/watch?v=qptTRJP_K2U) ]
* [Meeting #4 with ZeWIF envelope demo (April 2025)](/chains/zcash/zewif/meeting4/) [ [Video](https://www.youtube.com/watch?v=oXm2FsR4KTs) ]

**ZeWIF Survey:**
* [Survey of Zcash Data Formats](https://github.com/dorianvp/zcash-wallet-formats/tree/master)
* [Spreadsheet of Zcash Wallet Data](https://docs.google.com/spreadsheets/d/1MdahX4igppx7a4BdrcO5TGB2-mO1EtXrlKssypfEHUQ/)
* [Report on Wallet Data](/chains/zcash/zewif/report1/)

**Zcash:**
* [Zcash Home Page](https://z.cash/)
* [Zcash Community](https://z.cash/community-hub/)

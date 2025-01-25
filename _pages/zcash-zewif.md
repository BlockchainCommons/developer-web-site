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
  nav: zcash
redirect_from:
  - /zcash/zewif/
---

ZeWIF is a joint project between Blockchain Commons and [Zingo Labs](https://zingolabs.org/), sponsored by a [Zcash Community Grant](https://zcashcommunitygrants.org/). It was created in part to support the deprecation of the [zcashd reference wallet](https://z.cash/ecosystem/zcashd/) but moreso to create a format that supports [Openness and Indepence](https://developer.blockchaincommons.com/principles/) by allowing users to easily migrate among Zcash wallets without concern of loss of funds.

## Project Schedule

The project has been broken into four phases:

1. Survey of existing wallet data (January 2025)
   * [Survey of Zcash Data Formats](https://github.com/dorianvp/zcash-wallet-formats/tree/master)
   * [Spreadsheet of Zcash Wallet Data](https://docs.google.com/spreadsheets/d/1MdahX4igppx7a4BdrcO5TGB2-mO1EtXrlKssypfEHUQ/)
   * [Meeting on Wallet Data](/chains/zcash/zewif/meeting1)
3. Design of wallet interchange specification (February 2025)
4. Creation of Rust import and export libraries (March 2025)
5. Create of legacy wallet CLI tool, ZExCavator (April 2025)

## Interchange Format

The format will be specified as the second phase of this project. The current plan is to specify it in the CBOR-based [Envelope](/envelope/) format. Data will be divided between three classes:

1. **First Class Data.** Data that is used by two or more wallets and considered to be current and important. This data will be included in the core specification and likely assigned specific CBOR tags.
2. **Second Class Data.** This will be most other data, especially data that is only used by a single wallet or not considered current or important. This data will not be specified and is instead suggested for inclusion using [attachments](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-006-envelope-attachment.md).
3. **Third Class Data.** There may be some data, especially state, configuration, or encryption data, that is not considered useful for preservation. This will not be included in the core specifications and will not be suggested for inclusion in specifications. However, the best practice will be to preserve the entire original wallet data file, so that this "third class data" is not lost is there is a need for it in the future.

## Sponsors

This work is sponsored by a [Zcash Community Grant](https://zcashcommunitygrants.org/). See [our grant proposal](https://github.com/ZcashCommunityGrants/zcashcommunitygrants/issues/3) for more information on the project, its schedule, and its deliverables.

## Links

**ZeWIF Survey:**
   * [Survey of Zcash Data Formats](https://github.com/dorianvp/zcash-wallet-formats/tree/master)
   * [Spreadsheet of Zcash Wallet Data](https://docs.google.com/spreadsheets/d/1MdahX4igppx7a4BdrcO5TGB2-mO1EtXrlKssypfEHUQ/)
   * [Meeting on Wallet Data](/chains/zcash/zewif/meeting1)

**Zcash:**
* [Zcash Home Page](https://z.cash/)
* [Zcash Community](https://z.cash/community-hub/)

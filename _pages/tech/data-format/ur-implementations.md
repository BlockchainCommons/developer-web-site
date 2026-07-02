---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: UR Implementations
hide_description: true
classes:
  - wide
permalink: /ur/implementations/
sidebar:
  nav:
    - ur
---

Uniform Resources (URs) have been Blockchain Commons' most successful
interoperable specification to date.

_Please [submit PRs](https://github.com/BlockchainCommons/developer-web-site/blob/master/_pages/ur-implementations.md) if you are aware of different usages or to make corrections._

| Publisher | Wallet | Animated PSBT | ur:crypto-psbt | ur:crypto-* | Future |
| ---------- | ---- | --------- | -------------- | --------------------------------------------------------- | --- |
| BlueWallet | BlueWallet | ? | ? | bc-urv1 |
| Casa | Casa&nbsp;Wallet | YES | YES | -hdkey (-request/response for health check) |
| Cobo | Cobo&nbsp;Wallet | ? | ? | bc-urv1 |
| DIYBitcoin | DIY&nbsp;Hardware&nbsp;Wallet | YES | ? | ? |
| Denton&nbsp;Development| Fully Noded | YES | YES | ? |
| Blockstream | Jade | YES | ? | ? |
| Blockchain&nbsp;Commons | keytool-cli | NO | YES | -hdkey, -request, -response, -seed |
| Keeper | Keeper&nbsp;Wallet | YES | YES | -account |
| Keystone | Keystone&nbsp;Essential/Pro | YES | YES | ? |
| Foundation | Passport | YES | YES | (-request/response with Casa for health check) | -account
| Meteor Software| SeedSigner | YES | ? | ? |
| Blockchain&nbsp;Commons | Seed Tool | YES | YES | -account, -address, -bip39, -hdkey<br/>-output, -seed, -sskr |
| Blockchain&nbsp;Commons | seedtool-cli | NO | NO | -seed, -sskr |
| Craig Raw | Sparrow | YES | YES | -account, -address, -bip39, -hdkey,<br/>-output, -seed |
| SeedHammer | SeedHammer | NO | NO | -output |

More info on URs can be found in [research
papers](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-005-ur.md)
and in the rest of the [UR
docs](/ur/)

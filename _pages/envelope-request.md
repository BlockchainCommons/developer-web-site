---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Envelope Request & Response"
hide_description: true
classes:
  - wide
permalink: /envelope/request/
sidebar:
  nav: envelopereq
---

[Gordian Envelope](/envelope/) includes request and response functionality: one user can issue an Envelope requesting certain information or certain actions and then another user can respond to that request with the data. These functions are important for enabling interoperability and automating complex or error-prone usage of digital assets.

## Implementation Guide for Requests & Responses

Please see the [complete Implementation Guide](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-004-request.md) in our Blockchain Commons Research depo for the specifications of using Requests & Responses in Gordian Envelopes. 

## Best Practices for Request & Response

The follow best practices apply to the use of Request & Response:

**Require User Intervention for New Access.** Never respond to a Request for new data or any type of senesitive signing without user intervention. A user should always be given specific, meaningful, and accessible information on what a request is, alongside any warnings or any suggestions on why they might or might not want to respond. A Response should only occur in these situations after a user has agreed to the Request. (Responses without user intervention may be made when a user has already agreed to the interaction, such as to ensure the seamless and constant backup of data to a pre-agreed location.)

**Separate Out Separate Decisions.** Try to assess which elements of intervention require different cognitive processes and separate them out. For example, if a user is signing a PSBT to send money to a certain address, there might be three cognitive steps: (1) confirm the receipt address [address recognition]; (2) confirm the funding source [seed recognition]; (3) confirm the transaction [assessment of danger/consideration of fraud]. These separate decisions don't each have to have a confirm button, but they should each be clearly distinct, perhaps in different sections on a single page, perhaps on different pages (and obviously you want some explicit confirmation by the time you're done).

**Include OIBs or Other Accessible Information for Digital Assets.** Digital assets are usually identified by long identifiers that aren't intuitive to human brains. To improve the accessibility of information and to aid a user in making a meaningful decision, include an [oib](/oib/) ideally with a [Lifehash](/lifehash/), Blockie, or other visual identifier. Be sure to include everything that will help a user to intuitively identify their asset.

**2FA Any Confirmation.** If you have two-factor authentication built into your application, most Request/Response decisions should be put within your 2FA perimeter. That means that your user should have to do their final authentication (usually a password, a thumbprint, or a faceprint) before they can finalize any Response. Perhaps they do it automatically whenever they open up the app, but if not, be sure they have to by the time they make a decision.

**Put Confirmations after All Details.** Be sure that any confirmation button is placed after all relevant information. You can't force a user to read that information, but you can do you best to put it in front of them and so encourage them to read through it before making a decision. If a decision is particularly important (or dangerous), you might want to have a user do something extra, like 2FA an extra time, type "I AGREE", or something similar.

## Request & Response Links

**Envelope Intro:**

* [**Gordian Envelope**](/envelope/)

**Developer Resources:**

* [**Request & Response Implementation Guide**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-004-request.md) (GitHub repo)
* [**Request & Reponse Test Vectors**](./vectors)

**Specs & Research:**

* [**BCR-2023-012: Expressions**](https://github.com/BlockchainCommons/research/?tab=readme-ov-file) (GitHub repo)
* [**BCR-2023-014: Gordian Sealed Transaction Protocol**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-014-gstp.md) (GitHub repo)

**Use Cases:**

* [**Improving Multisigs with Request/Response**](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Scenario-Multisig-RR.md) (GitHub repo)

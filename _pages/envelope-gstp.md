---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Envelope Gordian Sealed Transaction Protocol (GSTP)
hide_description: true
classes:
  - wide
permalink: /envelope/gstp/
sidebar:
  nav: envelope
---

## Overview

Gordian Sealed Transaction Protocol (GSTP) is a secure, distributed, transport-agnostic communication method for two or more parties built on the Gordian Envelope specification. 

This means:

* **Secure.** Messages are protected by both signatures and encryption.
* **Distributed.** State is securely preserved as part of the messages.
* **Transport-Agnostic.** Less secure or less reliable transportation methods such as Bluetooth, NFCs, or QRs can be used.

GSTP allows for the transmission of data where the sender needs to be verified and/or the data needs to be protected.

<img src="/assets/images/gstp-overview.jpeg" style="border: 2px solid white !important">

## Why is GSTP Important?

Among the main use cases for GSTP are:

* Requesting services (e.g., requesting a pricing service).
* Transmitting sensitive data, even using insecure protocols (e.g. storing data to an NFC tag).
* Transmitting important data, even using unreliable protocols (e.g., sending data via Bluetooth).
* Storing sensitive data on a remote server (e.g., storing a share with [CSR](/csr/)).
* Recovering sensitive data from a remote server (e.g., recovering a share with [CSR](/csr/)).
* Automating systems where sensitive data is transmitted back and forth in multiple passes (e.g., creating a multisig).
* Automating systems requiring state, even when parties can't manage that state on their own (e.g., communicating with load-balancing servers).

GSTP was originally built for digital-asset wallets, to allow the storage and recovery of shares from [Gordian Despositories](https://github.com/BlockchainCommons/bc-depo-rust). However, it can be used for numerous other digital-asset use cases such as requesting pricing information, requesting keys with specific derivations, sharing private metadata, and building multisigs.

## How Does GSTP Work?

Though simple to use, GSTP is built upon a stack of Core Blockchain Commons functionality.

* [**Envelope**](/envelope). Gordian Envelope is a Smart Document system. GSTP transmits data in this format.
* [**Expressions**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-012-envelope-expression.md). Expressions are a standardized methodology for encoding function calls into Envelopes, to be run by the Envelope opener.
* [**Request/Response**](/envelope/request/). The Envelope Request/Response system is a way for one party to request that another party run a specific Expression, and then for that other party to return a Response using the same transaction ID.

GSTP uses this functionality as follows:

* **Discovery**. A discovery system allows a party to identify itself with a public key.
* **Request/Response.** The Request/Response system is used for communication.
* **Encrypted State Continuation (ESC).** The sender encodes their state by encrypting it with their private key and copies over any state previously sent by the recipient (which is encoded with the recipient's private key). [See more on ESC](/envelope/esc/).
* **Signature.** A sender signs the message with their private key. Validating the signature is a requirement for the recipient before processing the message.
* **Symmetric Encryption.** The entire message is encrypted with a symmetric key.
* **Public Key Cryptography.** The symmetric key is encrypted with the recipient's public key.

The recipient will then be able to decrypt the symmetric key, decrypt the message, validate the signature, recover any ESC that they had previously sent, and process the message (often continuing the automation that GSTP is supporting).

See the [GSTP technical overview](/envelope/gstp/tech/) or the following video for a complete example of how GSTP works.

## GSTP Videos

<table width="100%">
  <tr>
    <td width="640px">
      <b>Understanding GSTP:</b>

{% include video id="QnH14LkJOnI" provider="youtube" %}

    </td>    
  </tr>
</table>  

_See the [Gordian Envelope playlist](https://www.youtube.com/playlist?list=PLCkrqxOY1FbooYwJ7ZhpJ_QQk8Az1aCnG) for more._

## Libraries

{% include lib-envelope.md %}

## Envelope Links

**Intro:**

* [**GSTP Technical Overview**](/envelope/gstp/tech/)
* [**ESC Overview**](/envelope/esc/)
* [**BCR-2023-14: GSTP**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-014-gstp.md) (GitHub repo)

* [**Envelope Overview**](/envelope/)
* [**Envelope Technical Overview**](/envelope/tech/)

**Developer Reference Apps:**

* [**Gordian Depository**](https://github.com/BlockchainCommons/bc-depo-rust)
* [**Gordian Depository API**](https://github.com/BlockchainCommons/bc-depo-api-rust)
* [**bc-envelope-cli-rust**](https://github.com/BlockchainCommons/bc-envelope-cli-rust)

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Envelope ESC
hide_description: true
classes:
  - wide
permalink: /envelope/esc/
sidebar:
  nav: envelope
---

## Overview

Encrypted State Continuation (ESC) is a feature of [Gordian Sealed Transaction Protocol (GSTP)](/envelope/gstp) that allows for the distributed storage of state during the communication process. This means that the state of a process doesn't have to be stored locally but instead is incorporated into the communication itself.

## Why is ESC Important?

Local storage of state can be an attack vector. By instead encrypting state as part of a message, this attack surface is weakened, because the key to unlock the state and the ESC itself might be broadly separated in space and time.

ESC is also important for situations that don't allow for the reliable storage of state (or possibly for any storage at all). Use cases include:

* **Constrained Devices.** Devices such as JavaCards and small hardware devices may have tight constraints on their storage.
* **Load-Balanced Servers.** Load-balancing servers can't reliably store state unless the same state in stored on every single server, which is a gross inefficiency. 
* **Time-Separated Communications.** GSTP communications can be widely separated in time if a Request is deliberately stored away from future usage. In this situation, there is no guarantee that the original Requester will still have the state.
 
## How Does ESC Work?

The [GSTP technical overview](/envelope/gstp/tech/) demonstrates how ESC fits into the GSTP process. In short:

1. The active party stores their state in the GSTP message.
2. The active party encrypts that state with their own key, which only they possess.
3. The active party also copies over the receiving party's state from their previous message, if there was any.

This means that ESC content is kept private (so that only the creator can read it) and up-to-date (since it's updated in each message).

## GSTP Videos

ESC is demonstrated in the "Understanding GSTP" video:

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

* [**Envelope Overview**](/envelope/)
* [**Envelope Technical Overview**](/envelope/tech/)
* [**GSTP Overview**](/envelope/gstp/)
* [**GSTP Technical Overview**](/envelope/gstp/tech/)
* [**BCR-2023-14: GSTP**](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-014-gstp.md) (GitHub repo)


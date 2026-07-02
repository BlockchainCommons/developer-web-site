---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Airgaps
hide_description: true
classes:
  - wide
permalink: /airgap/
sidebar:
  nav: architecture
---

## Overview

<img src="/assets/images/airgap.png" align="right">

An **Airgap** is a traditional method of high security that introduces
a literal "gap of air" between devices. Transmitting information
between the devices requires a non-interactive methodlogy such as
writing and reading QRs, NFCs, or MicroSDs.

## Why is Airgap Important?

An airgap is a security measure. It makes it harder for attacks to
occur on the device on the far side of the airgap. There are a few
primary reasons for this:

* **Constrained Transmission.** All methods of transmitting data
across the airgap are constrained. Usually, this means there's a size
contraint, but there may also be constraints on what can be
transmitted.
* **Non-Interactivity.** There's no ability for a malicious program on
one side of the airgap to attack the device on the other side in an
interactive way and get an immediate response. Though attacks might be
possible they would be notably slowed by the fact that data needs to
be loaded across the airgap each time. Something like a buffer
overflow attack couldn't see the immediate response, limiting the
ability to retrieve illicit data (or gain illicit access). More
generally, if any attack requires several sequential queries or
several round-trips to reach conclusion, it's effectively impossible
to use across an airgap.
* **User Agency.** Finally, it's usually a human being doing the
loading of data across the airgap, and so they have the ability to
stop if something looks weird. Moreso, the best practices for airgaps
will usually require a user to verify what's being done at each step.

## How Does Airgap Work?

The most common methodology for bridging an airgap is for a device to
create and display a data-filled QR code and then for another device
to read that QR with its camera. Blockchain Commons' [animated
QRs](/animated-qr) were designed to allow for larger sets of data to
be transmitted in this way.

Similarly one device could write to an NFC or a MicroSD card, and then
another could read it.

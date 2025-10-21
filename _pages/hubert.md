---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Hubert: Dead-Drop Hub"
hide_description: true
classes:
  - wide
permalink: /hubert/
sidebar:
  nav: clubs
---

## Overview

Hubert is a server that enables asynchronous coordination among autonomous cryptographic objects such as [Gordian Clubs](/clubs/) through dead-drops on distributed storage (BitTorrent, IPFS). Participants deposit [GSTP-encrypted](/envelope/gstp/) messages at secret locations where only holders can retrieve them. Write-once semantics ensure immutability. 

## Why is Hubert Important?

A new generation of decentralized services is appearing that dramatically lowers the threat of censorship and surveillance by dividing up work (and information!) among multiple devices. This includes [Secure Multi-party Computation (MPC) models](https://en.wikipedia.org/wiki/Secure_multi-party_computation) and supports multiparty protocols such as [FROST threshold signatures](/frost/) and distributed key generation. These decentralized services can work without servers, persistent connections, or trusted intermediaries. Coordination and collaboration occur through mathematics, not infrastructure.

The problem is getting these mult-party computers to communicate and work together, especially in the most extreme cases where communication end points are constantly changing or where infrastructure appears on an entirely _ad hoc_ basis. That's where Hubert comes in: it's designed to allow communication among multiple parties for MPC-style protocols without impinging on their privacy or anonymity.

Hubert transforms global distributed networks into a mesh of sealed rendezvous points: secure, asynchronous "dead drops" where messages, proofs, and cryptographic state can persist across time, connectivity gaps, or censorship events. It turns unreliable networks into durable coordination channels. 

## How Does Hubert Work?

Hubert uses a cryptographic dead-drop model to allow online collaboration and multi-party computation without centralized services.

Messages in Hubert are published to resilient, decentralized storage using store-carry-forward semantics. There are currently three storage backends:

- **BitTorrent Mainline DHT**: Fast, lightweight, serverless (≤1 KB messages).
- **IPFS**: Large capacity, content-addressed (up to 10 MB messages).
- **Hybrid**: Automatic optimization by size, combining DHT speed with IPFS capacity.

Those messages are addressed via ARIDs (Apparently Random Identifiers): participants generate ARIDs for their messages and embed ARIDs where they expect responses, creating a decentralized dead-drop graph. Storage networks see only key derived via HKDF, never the ARIDs themselves.

The encryption in Hubert comes courtesy of [GSTP (Gordian Sealed Transaction Protocol)](/envelope/gstp/). With GSTP integration, even storage indirection is secured: when Hybrid storage uses references, the references themselves are encrypted to the same recipients as the payload.

### Hubert Workflow

Hubert enables request-response flows without direct connections through dead-drop coordination:

**Requester Workflow:**
1. Generate ARID #2 (`response_arid`), which will be used by responder.
2. Create message (`request`) containing ARID #2.
3. Encrypt message within GSTP.
4. Publish encrypted message at ARID #1 (`request_arid`).
5. Share ARID #1 with responder via secure channel (Signal, QR code, etc.).
6. Monitor ARID #2 for responder's reply.

**Responder Workflow:**
1. Receive ARID #1 via secure channel.
2. Retrieve encrypted message from storage (using derived key from ARID #1).
3. Decrypt and process message.
4. Extract ARID #1 from decrypted message.
5. Create encrypted response.
6. Publish response at ARID #2.

## Hubert Links

**Main Links:**

* [**Hubert Overview**](https://hackmd.io/3OghoiSQTviz1KP_noN3mQ) (HackMD draft)
* [**hubert-cli**](https://github.com/BlockchainCommons/hubert-rust?tab=readme-ov-file)

**Associated Projects:**

* [**Gordian Clubs**](/clubs/)
* [**FROST**](/frost/)

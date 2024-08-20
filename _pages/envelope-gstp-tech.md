---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Envelope GSTP Technical Overview
hide_description: true
classes:
  - wide
permalink: /envelope/gstp/tech/
sidebar:
  nav: envelope
---

## Step 1: Build a Request/Response Message

GSTP messages are built as [Request/Response messages](/envelope/request/) using [Gordian Envelope](/envelope/). A Request/Response message basically means that you're asking the remote Envelope opener to do something and send you back the results. 

![](/assets/images/gstp-ex-1.jpeg)

## Step 2: Add Public Key

A Authentication Layer is now created. This begins by the sender adding a public key, which they will later prove that they own with a signature.

![](/assets/images/gstp-ex-2.jpeg)
![](/assets/images/gstp-ex-3.jpeg)
![](/assets/images/gstp-ex-4.jpeg)
![](/assets/images/gstp-ex-5.jpeg)
![](/assets/images/gstp-ex-6.jpeg)
![](/assets/images/gstp-ex-7.jpeg)
![](/assets/images/gstp-ex-8.jpeg)
![](/assets/images/gstp-ex-9.jpeg)

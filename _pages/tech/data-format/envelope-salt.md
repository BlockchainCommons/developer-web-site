---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: Salting
hide_description: true
classes:
  - wide
permalink: /envelope/salt/
sidebar:
  nav: envelope
redirect_from:
  - /salt/
  - /salting/
  - /envelope/salting/
---

## Overview

Salting is a critical privacy enhancement for that keeps elided data
private. It ensures that even when the same information is elided from
multiple documents, the resulting hashes are different, preventing
correlation attacks.

## Why is Salting Important?

Without salting, elision would have a serious privacy weakness:

- Identical content would produce identical hashes.
- An observer could determine if the same information was elided in multiple documents.
- Common values could be guessed through dictionary attacks.

### How Does Salting Work?

Salting solves this problem by adding random data to an Envelope leaf
or node before hashing:

```
Without salt:  hash("name": "John Smith") → always the same hash
With salt:     hash("name": "John Smith" + random_salt) → different hash each time
```

Salts should be cryptographically random and of sufficient length.
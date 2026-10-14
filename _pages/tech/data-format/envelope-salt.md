---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-dataformat.jpg
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

<div class="hexline hexgrid72">
  <div class="hex12top">
    <a href="/data-formats/">
      <img src="/assets/badges/cat-dataformat-half.png">
    </a>
  </div>
 <div class="hex11 opaqued">
    <a href="/envelope/recursion/">
      <img src="/assets/badges/recursion.png">
    </a>
  </div>
 <div class="hex21 opaqued">
    <a href="/hashed-elision/">
      <img src="/assets/badges/hashed-elision.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/known-values/">
      <img src="/assets/badges/known-values.png">
    </a>
  </div>
  <div class="hex32 opaqued">
    <a href="/envelope/inclusion-proof/">
      <img src="/assets/badges/inclusion-proof.png">
    </a>
  </div>
  <div class="hex41">
    <a href="/envelope/">
      <img src="/assets/badges/envelope.png">
    </a>
  </div>
  <div class="hex51 opaqued">
    <a href="/envelope/permits/">
      <img src="/assets/badges/permits.png">
    </a>
  </div>
  <div class="hex52 highlighted">
    <a href="/envelope/salt/">
      <img src="/assets/badges/salt.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/envelope/request/">
      <img src="/assets/badges/request-response.png">
    </a>
  </div>
  <div class="hex71 opaqued">
    <a href="/envelope/gstp/">
      <img src="/assets/badges/gstp.png">
    </a>
  </div>
  <div class="hex72 opaqued">
    <a href="/envelope/esc/">
      <img src="/assets/badges/esc.png">
    </a>
  </div>
</div>

_Salting is a critical privacy enhancement for that keeps elided data
private. It ensures that even when the same information is elided from
multiple documents, the resulting hashes are different, preventing
correlation attacks._

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

## For More

* Also see the [Envelope-CLI Salting Examples](/envelope/cli/salt/)
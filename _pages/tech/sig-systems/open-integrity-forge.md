---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-sigsystem.jpg
  og_image: /assets/images/bc-card.jpg
title: "Open Integrity Forge"
tagline: "Managing Agents with Open Integrity"
hide_description: true
classes:
  - wide
permalink: /open-integrity/forge/
redirect_from:
  - /openintegrity/forge/
sidebar:
  nav:
    - openintegrity
    - signatures
    - technology
---

<div class="hexline hexgrid71">
  <div class="hex11 opaqued">
    <a href="https://learningfrost.blockchaincommons.com/">
      <img src="/assets/badges/learning-frost.png">
    </a>
  </div>  
  <div class="hex21 opaqued">
    <a href="/frost/">
      <img src="/assets/badges/frost.png">
    </a>
  </div>
  <div class="hex12top">
    <a href="/signatures/">
      <img src="/assets/badges/cat-sig-half.png">
    </a>
  </div>
  <div class="hex31 opaqued">
    <a href="/musig/">
      <img src="/assets/badges/musig.png">
    </a>
  </div>  
  <div class="hex41">
    <a href="/cloudflare-passkey/">
      <img src="/assets/badges/cloudflare-passkey.png">
    </a>
  </div>
  <div class="hex51">
    <a href="/open-integrity/">
      <img src="/assets/badges/open-integrity.png">
    </a>
  </div>
  <div class="hex61 highlighted">
    <a href="/open-integrity/forge/">
      <img src="/assets/badges/open-integrity-forge.png">
    </a>
  </div>
  <div class="hex71 opaqued">
    <a href="/provemark/">
      <img src="/assets/badges/provenance-marks.png">
    </a>
  </div>
</div>

_Open Integrity Forge is a proof-of-concept Git platform, built on the
ideas of [Open Integrity](/open-integrity/), that controls the output
of AI agents, ensuring that a human always oversees any agentic
commit. It does so with passkeys, through Blockchain Commons'
[passkey-cloudflare](/cloudflare-passkey/) library._

## Why is Open Integrity Forge Important?

The core [Open Integrity](/open-integrity/) architectures ensures the
transparency and immutability of your software projects. It creates a
root of trust and builds a chain of trust from that, allowing for the
empowerment and revocation of keys, which must always be used to
authorize commits.

But more is needed to ensure Open Integrity in the age of AI. Open
Integrity Forge takes the next step by reining in AI agents with Open
Integrity rules. This ensures that they don't make unauthorized
changes to your code base, and that a human instead oversees every
update.

## How Does Open Integrity Forge Work?

Building on Open Integrity, Forge starts with an inception commit. A
second commit then lays out a set of rules for the repository:

| Rule | Statement |
| --- | --- |
| R0 | The first commit of main is the root of trust. Its approval verifies against the promoter key in its own policy. Its hash is the repository id. |
| R1 | Each later commit of main requires an approval from a promoter in the policy of its first parent. |
| R2 | Only a commit of main with such an approval can change `.repo/`. A key that a commit adds has no say over that commit. |
| R3 | Each other commit that main reaches must carry a valid SSH signature from a key in the policy in force when it came in. |
| R4 | An approval must name this repository, the ref, the parents, and the tree of its own commit. |
| R5 | An author key may sign only inside its validity window. |
| R6 | A key a promoter revoked may sign nothing after its revocation; its earlier work still verifies. |
| R7 | Where the policy says `mergesOnly`, a commit of main with one parent changes only `.repo/`, so that no squash will drop the authors' signatures. |
| R8 | A release tag of a main commit carries approvals of that tag from as many distinct promoters as `releaseApprovals` requires. |

All AI agents are given their own forks and one-hour signing
keys. Changes they make go to their forks. Only the human with the
passkey can merge these changes into `main`.

Forge keeps these rules in one file,
`.repo/config/verification/policy.json`, rather than the signer files
that the [Open Integrity core tools](https://github.com/OpenIntegrityProject/core)
use, so Open Integrity's own verifier does not yet pass a Forge
repository. How the two converge is not yet decided.

### Cloudflare Integration

Cloudflare runs the whole forge, but it doesn't decide what is valid:
that lives in the repositories.

- Artifacts holds every repository. A project is an Artifacts repository; each agent works in an Artifacts fork of it, with a short-lived write token scoped to that fork alone. Anyone can clone a repository straight from Artifacts with a read-only token that lasts an hour and verify it, with no forge in the loop.
- Workers: the forge is one Worker on a custom domain. It serves the page, prepares each commit for a passkey to sign, speaks Git to Artifacts with isomorphic-git, and serves the one-file verifier that each repository pins by digest.
- Queues: Artifacts push events reach a Queue, and the Worker checks every push against the repository's own rules. A push from outside the forge, by an agent or from a laptop, is judged the same way. The live demo leaves laptop pushes off, since the forge needs a Cloudflare API token to hear them.
- Durable Objects (SQLite) keep each project, each agent run, and each person.

[passkey-cloudflare](/cloudflare-passkey/), Blockchain Commons'
passkey-only identity library, is built for Cloudflare too, and the
forge's sign-in and every approval run on it. It deploys inside the
application's own Worker and keeps identity in Durable Objects, so no
identity provider, password, or email stands between a person and
their account: https://github.com/BlockchainCommons/passkey-cloudflare

## Open Integrity Forge Demo

<table width="100%">
  <tr>
    <td width="50%">
      <a href="https://forgedemo.gordianstack.com/"><img src="/assets/screenshots/forge.png"></a>
    </td>
    <td>
      <p>A full demo of Open Integrity Forge is available at <a href="https://forgedemo.gordianstack.com/">https://forgedemo.gordianstack.com/</a>. After you create & login with a passkey, you will be able to make a first commit, admit two agents, watch them work, promote one change, and bounce another when the AI goes rogue.</p>
    </td>
  </tr>
</table>  
 
## Open Integrity Forge Video

<table width="100%">
  <tr>
    <td width="50%">

{% include video id="_KdOGtynXig" provider="youtube" %}

    </td>
    <td>
      <p>This seven-minute video of Open Integrity Forge is broadly split into three parts: a discussion of the problem and solution, a demo of the project, and a look at the principles and libraries used in this project.</p>
    </td>
  </tr>
</table>  

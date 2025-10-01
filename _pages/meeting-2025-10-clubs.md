---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "Gordian Clubs"
title: "Gordian Developer Meeting: October 2025"
hide_description: true
classes:
  - wide
permalink: /meetings/2025-10-clubs/
sidebar:
  nav: meetings
---

The [Gordian Developer Meeting](https://www.blockchaincommons.com/subscribe/#gordian-developers) on **October 1, 2025** focused on Gordian Clubs. A full demo of [clubs-cli-rust](https://github.com/BlockchainCommons/clubs-cli-rust), built on the [clubs-rust library](https://github.com/BlockchainCommons/clubs-rust), was the first working proof-of-concept for autonomous cryptographic objects, which enable decentralized access control without servers or infrastructure. It represented the culmination of Christopher Allen's 34-year journey to realize cryptographic clubs, originally inspired by Project Xanadu's club system in 1991.

The demo is also avialable as a [log](https://github.com/BlockchainCommons/clubs-cli-rust/blob/master/demo-log.md).

## Media & Slides

<table width="100%">
  <tr>
    <td width="640px">
      <b>Video:</b>

{% include video id="bqxW7iDk4QI" provider="youtube" %}

    </td>
    <td width="640px">
      <b>Slides:</b>

<a href="/assets/pdfs/2025-10-clubs.pdf"><img src="/assets/pdfs/2025-10-clubs.jpg" style="border:2px solid white"></a>

    </td>
  </tr>
</table>

Also see the [transcript](/meetings/2025-10-clubs/transcript/) and a [machine summary of community discussions](/meetings/2025-10-clubs/summary).

## Key Points

- First live demonstration of autonomous cryptographic objects 
- Integration of multiple permits (XID, public keys, SSKR shares)
- Recognition of gaps between autonomous cryptographic objects and traditional OCAP systems (time-based expiration, external event conditionals, revocation mechanisms)
- Identification of key research needs: formal cryptographic proofs for naive delegation protocols and FROST provenance mark VRF construction

## Next Steps

1. Integrate existing FROST and GSTP libraries into Gordian Clubs workflow
2. Develop `hubert` coordinator server for FROST message passing
3. Secure cryptographer partnerships for formal proofs of delegation constructions
4. Document use cases from human rights, journalism, and distributed systems communities
5. Expand demos beyond CLI to showcase UX potential

## Key Quotes and Insights

**Christopher Allen, on 32-year journey:**
> "Some dreams just need the right tools to become real. I think Ryan and others will agree that sometimes it just takes time. Today, we have these tools. Tomorrow, developers like you will use them to build systems to preserve human dignity."

**Christopher, on autonomy trade-offs:**
> "These limitations do offer freedoms. If you're willing to live with these limitations, you don't have platform lock-in, you can't be censored, and you can't be surveilled. Mathematical certainty replaces administrative whim."

**Alan Karp on risk-adaptive access control:**
> "The example I always use is you can read this document unless the US is at war with Canada. That used to be funny..."

**On mathematical certainty:**
> "Mathematical proofs endure when servers don't. No server can be taken down to block access. No authority can revoke mathematical proofs. Control rests with keyholders, not platform operators."

**Wolf McNally, on Schnorr composability:**
> "The nice thing about Schnorr signatures is that a single-party signature and a multi-party signature are mathematically indistinguishable. External systems never need to change - same verification logic, infinite internal complexity."

**Mark S. Miller, on commingled authority:**
> "Norm made a terrible mistake in naming it confused deputy, because the phrase suggests the problem is a bug in the deputy. The real problem is the framework in which the deputy finds himself is one in which the deputy can neither ascertain nor express the distinctions needed to keep authority separate."

## For More

* [**clubs-cli-rust**](https://github.com/BlockchainCommons/clubs-cli-rust)
   * [**Demo Logo**](https://github.com/BlockchainCommons/clubs-cli-rust/blob/master/demo-log.md)
* [**clubs-rust library**](https://github.com/BlockchainCommons/clubs-rust)

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Collaborative Seed Recovery (CSR) FAQs"
hide_description: true
permalink: /csr/faq/
sidebar:
  nav: csr
---

## Privacy

### Why is Linking "Legal Citizen Identity" to Share Storage Problematic?

Linking a real-world named identity to the storage of digital-asset shares is a very conservative view for how identity works. As 
Joe Andrieu wrote in his foundational primer on ["functional identity"](https://github.com/WebOfTrustInfo/rwot12-cologne/blob/main/advance-readings/functional-identity-primer.md), identity is how we recognize and remember people we've met. Names are just handy labels for this ongoing process. Choosing to link a such a label to a share (or to any other data on the internet) misunderstands that tool as the end product and in doing so potentially exposes a user's entire identity. 

Beyond that, it ignores the precepts of [data minimization](https://www.blockchaincommons.com/musings/musings-data-minimization/): it's _way_ more personally identifiable information than is needed.

How do you authenticate a user without violating their privacy? You let them decide! They provide proofs when they shard their secret (be it phone #, email, legal identity, thumbprint, or a word), and then they prove access to their self-declared proofs on the other side.

## Resilience

### Why is It Important for Users to Make Their Own Choices for Share Storage?

If users don't want to make their own choices about share server choices, that's fine. CSR has defaults, as should any similar system. The most important reason for sharding seeds is improving their resilience and if we make that difficult it defeats the purpose: accessibility is king.

But, if a user is willing and able to make the choices for where and how his shares are stored, that's superior. A user will know what his risk profile is: both what he's willing to risk and which risks are the most dangerous to him. A user with well-publicized Bitcoin holdings is going to have a very different risk profile from someone who is more secretive about their digital assets. A user in a fascist regime will have a dramatically different risk profile than someone living in a more tolerant and (theoretically) trustworthy state.

By allowing a user to make those choices (but perhaps guiding them in making those choices), a wallet app ensures that the resilience of sharded secrets are maximized.

### Why are Homogeneous Authentication Methods Problematic?

Share recovery requires authentication: a user in some way proves who they are, and thus receives a stored shared back. Some CSR-like methods have chosen homoegeneity among their share servers' authentication: each server requires the exact same authentication to give up its share.

This can be problematic because it means that if an attacker compromises the one, required authentication method, then they compromise every share, and thus the stored seed. The authentication method becomes a Single Point of Compromise, one of the things that the sharding of a seed was attempting to avoid. Instead, share recovery systems should require (or at least highly suggest, given support for user choice) that each share be stored with a different authentication method and that those authentication methods not be interdependent. For example, a user might have his shares authenticated (1) on his phone, (2) by his email address, and (3) by an in-person recovery, but should ensure that theft of his phone doesn't allow theft of his email address. (Modern biometric protections on mobile devices can ensure this protection, at least in the short term.)

See our article on [Auth Design Patterns](https://developer.blockchaincommons.com/architecture/patterns/auth/) for more on hetergeneous authentication design.

### Why is VSS an Important Expansion beyond Shamir's Secret Sharing?

The reconstruction site becomes a Single Point of Compromise whenever seeds are reconstructed. As a result, seeds should be reconstructed as rarely as possible.

But, on the flipside, you want to ensure that shares are actually still being held by share servers and other recovery partners. Otherwise, when you actually need to reconstruct your seed, you might discover it's not there.

VSS walks that line by allowing for the _testing_ of the reconstruction of a seed without _actually_ reconstructing the seeds. It's all the resilience to avoid Loss without any of the danger of Compromise.

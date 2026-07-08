---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-identity.jpg
  og_image: /assets/images/bc-card.jpg
title: Attestations & Data Model
tagline: Strengthening Identity with Claims
hide_description: true
classes:
  - wide
permalink: /identity/attestations/
redirect_from:
  - /identity/attestation/
sidebar:
  nav:
    - attestations
    - identity
    - technology
---

<div class="hexline hexgrid72">
  <div class="hex11">
    <a href="/ssi/">
      <img src="/assets/badges/ssi.png">
    </a>
  </div>
  <div class="hex12top">
    <a href="/identity/">
      <img src="/assets/badges/cat-identity-half.png">
    </a>
  </div>
  <div class="hex21 opaqued">
    <a href="/xid/">
      <img src="/assets/badges/xid.png">
    </a>
  </div>
  <div class="hex31 highlighted">
    <a href="/identity/attestations/">
      <img src="/assets/badges/attestations.png">
    </a>
  </div>
  <div class="hex32 opaqued">
    <a href="https://learningxids.blockchaincommons.com/">
      <img src="/assets/badges/learning-xids.png">
    </a>
  </div>
  <div class="hex41">
    <a href="/fair-witness/">
      <img src="/assets/badges/fair-witness.png">
    </a>
  </div>
  <div class="hex51 opaqued">
    <a href="/cliques/">
      <img src="/assets/badges/cliques.png">
    </a>
  </div>
  <div class="hex52 opaqued">
    <a href="/garner/">
      <img src="/assets/badges/garner.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/clubs/">
      <img src="/assets/badges/clubs.png">
    </a>
  </div>  
  <div class="hex71 opaqued">
    <a href="/ppp/">
      <img src="/assets/badges/ppp.png">
    </a>
  </div>
</div>

_Attestations and endorsements make claims about digital
identities. They don't necessarily tell us who an identity is, but
they tell us what an identity can do and whether they've been
successful or not in the past. They're especially important for
[public participation profiles](/ppp/)._

## Why Are Attestations & Endorsements Important?

_The New Yorker_ once wrote ["On the Internet, nobody knows you're a
dog"](https://en.wikipedia.org/wiki/On_the_Internet,_nobody_knows_you%27re_a_dog). That's
true whether your identity is pseudonymous or theoretically based on
your physical world identity. In both cases, you need to be able to
convince people online of your credentials, whether that's proving
that you speak knowledgeably on your favorite technical (or fandom)
threads or offering more explicit credentials for a job (or volunteer
position).

Experience and published products associated with your digital
identity can do this, but often you need a bootstrap to get
there. That's where attestations and endorsements come in. They can
prove an identity is who you say it is, they can validate your
experience, they can just speak to your character, and they can gain
weight over time as part of a network of trust.

Common uses of attestations & endorsements include:

1. **Professional Skill Endorsements**: Verifying technical capabilities or domain expertise.
2. **Contribution Validations**: Confirming specific work on projects.
3. **Behavioral Testimonials**: Attesting to how someone works and collaborates.
4. **Credential Confirmations**: Validating education or certifications, potentially without revealing identity.
5. **Reputation Transfers**: Allowing trusted introducers to vouch for someone.

These are critical stepping stones for making digital identity work.

## How Do Attestations & Endorsements Work?

Claims about digital identity can come in two sorts: self-attestations
and third-party endorsements. These claims are then typically attached
to an identifier, such as [XIDs](/xid/).

### Self-Attestations

Self-attestations are claims you make about yourself, your work, or
your capabilities. While necessary, they have inherent limitations in
building trust because they come from the subject of the claim and
they require external validation to be truly trusted.

The best self-attestations include:

- Clear statements of capability or experience.
- Contextual information about how the capability was developed.
- Limitations and boundaries of the claimed expertise.
- Verifiable evidence when possible (without compromising privacy).
- Transparency about potential biases.

See the [Fair Witness Methodology](/identity/fair-witness/) for more
on writing strong claims of all sorts.

### Verifiable Self-Attestations

Self-attestations can gain weight if they are verifiable, offering
some way for third parties to check the claims. This kind of
self-endorsement bridges the gap between pure self-attestation and
peer endorsement by:

- Making specific claims about contributions.
- Providing transparent verification methods.
- Linking to public, independently verifiable evidence.

### Peer Endorsements

Endorsements are attestations made by others about you or your
work. They implicitly provide some level of verification, depending on
the trust level of the third-party endorser.  They can provide diverse
viewpoints on the same work, and they build layers of verification
over time. Additionally, peer endorsements form the foundation of
[public participation profiles](/ppp/).

Because peer endorsements were not made by the subject, they should be
accepted by the subject if they are to become part of the subject's
identity information. This acceptance model ensures that the recipient
maintains control over their identity. This is usually done via a
chain of signatures: the endorser signed the original claim, then the
subject signed its acceptance. This chain of signatures creates a
verifiable trust path.

The requirement for acceptance doesn't mean that the identity holder
has total control over endorsements. Other people can make whatever
claims they want. The subject just controls whether the endorsement
(or other claim) is tightly linked to their identity information.

### Trust Networks

The power of this model of self-attestations and endorsements comes
from combining multiple claims over an extended period of time. A user
can make their own self-attestations to define the core of their
identity, that identity is then directly endorsed, and those endorsers
are themselves endorsed, proving their credibility. New attestations
and endorsements progressively accrue over time, solidifying the trust
in an identity.

A trust network can then be verified by the following steps:

1. **Verify Self-Attestation Signatures**: Confirm an identity holder signed their own claims.
2. **Verify Endorsement Signatures**: Confirm each endorser signed their endorsements.
3. **Check Acceptance Signatures**: Verify the XID holder accepted and signed each endorsement.
4. **Evaluate Context and Disclosures**: Review methodologies, limitations, and potential biases as discussed in the [Fair Witness Methodology](/identity/fair-witness/).
5. **Check External References**: Verify any external evidence that was referenced.
6. **Consider Multiple Perspectives**: Look for consistency across different endorsers.

## Attestation & Endorsement Links

**Learning Attestations from the Command Line:**

* [**Learning Attestations**](/identity/attestations/cli/)
* [**Learning XIDs: Making Claims**](https://learningxids.blockchaincommons.com/02_0_Claims/) (course)
* [**Learning XIDs: Attesting with Edges**](https://learningxids.blockchaincommons.com/03_0_Edges/) (course)

**Related Technologies**

* [**Self-Sovereign Identity**](/ssi/)
* [**XIDs**](/xid/)
* [**Fair Witness Methodology**](/identity/fair-witness)

**Standards**

* [**Verifiable Credentials**](https://www.w3.org/TR/vc-data-model-2.0/)

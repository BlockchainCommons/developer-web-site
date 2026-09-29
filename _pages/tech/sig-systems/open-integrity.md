---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-sigsystem.jpg
  og_image: /assets/images/bc-card.jpg
title: "Open Integrity"
tagline: "Git Repos as Roots of Trust"
hide_description: true
classes:
  - wide
permalink: /open-integrity/
redirect_from:
  - /openintegrity/
sidebar:
  nav:
    - signatures
    - technology
---

<div class="hexline hexgrid71">
  <div class="hex21 opaqued">
    <a href="https://learningfrost.blockchaincommons.com/">
      <img src="/assets/badges/learning-frost.png">
    </a>
  </div>  
  <div class="hex31 opaqued">
    <a href="/frost/">
      <img src="/assets/badges/frost.png">
    </a>
  </div>
  <div class="hex32top">
    <a href="/signatures/">
      <img src="/assets/badges/cat-sig-half.png">
    </a>
  </div>
  <div class="hex41 opaqued">
    <a href="/musig/">
      <img src="/assets/badges/musig.png">
    </a>
  </div>  
  <div class="hex51 highlighted">
    <a href="/open-integrity/">
      <img src="/assets/badges/open-integrity.png">
    </a>
  </div>
  <div class="hex61 opaqued">
    <a href="/provemark/">
      <img src="/assets/badges/provenance-marks.png">
    </a>
  </div>
</div>

_Open Integrity is an [Open
Development](/architecture/open-development/) initiative that applies
cryptographic trust mechanics such as hashes and signatures to Git
repos so that they can serve as cryptographic roots of trust._

## Why is Open Integrity Important?

Open Integrity has five fundamental goals:

* **Immutable Proof-of-Origin** – Verify the authenticity of software artifacts.
* **Signed Commits & Tags** – Ensure authorship integrity through SSH signatures (~128-bit security).
* **Tamper Detection** – Maintain verifiable repository history.
* **Trust Delegation** – Enable controlled transition from inception key to authorized signers.
* **Platform-Agnostic Validation** – Work across GitHub, GitLab, and self-hosted solutions.

This ensures transparency and immutability for software projects in a
way that's platform independent without requiring modifications to
Git.

For more, see the [Open Integrity Problem
Statement](https://github.com/OpenIntegrityProject/core/blob/main/docs/Open_Integrity_Problem_Statement.md#problem-statement).

## How Does Open Integrity Work?

Open Integrity initializes repos with an Inception Commit, creating a
singular Root of Trust, then creates a Chain of Trust from that
foundation.

### How Does the Inception Commit Work?

Open Integrity builds each Git repo with an _inception commit_, which
is a specially crafted empty commit that serves as a cryptographic
foundation. This commit includes both a Ricardian Contract
establishing trust rules and a cryptographic signature. Though Git
commits uses SHA-1, Open Integrity manages SHA-1's limitations with a
very precise, specific definition of the inception commit, which makes
creating purposeful collisions very difficult.

* **Content Constraint.** An empty commit (containing no files) with minimal metadata reduces the attack surface for SHA-1 collisions (~80-bit security).
* **Deterministic State.** The empty tree hash is predictable and easily verified, creating a deterministic state for the Inception Commit.
* **Verifiable Origin.** The first commit is signed by the repository’s first trusted key, creating a verifiable origin. Even if a hash collision were achieved due to the weakness of SHA-1, an attacker would still need to forge a valid SSH signature (~128-bit security).
* **Immutable Inspection.** Because it is platform-agnostic, this commit can be authenticated, affirmed, and audited regardless of where the repository is hosted.

### How Does the Inception Key Work?

With a singular root of trust, created by an inception commit to
authorize an _inception key_, a repo can be managed by the inception
key holder making commits to protected branches and incorporating
changes from others made in unprotected branches.

All work published into protected branches _must_ be verified or
signed by the inception key to maintain the root of trust.

### How Does Merging Work?

Merging content from third parties is tricky because it's easy to lose
critical validation information. The design of Open Integrity merging
avoids this:

* **Agreement Chain of Consent.** A verifiable chain of consent for repository agreements is stored in configuration files.
* **Signature Requirement.** Cryptographic signatures are required from both authors and committers.
* **Signature Preservation.** Original authorial signatures are preserved through merge operations.
* **Attribution Preservation.** Any unauthorized modifications to authorship claims are detected.

### How Does the Chain of Trust Work?

Using a singular inception key doesn't support the needs of larger
teams or the needs of teams working over longer time periods. This
requires additional functionality.

First, delegation of authority is needed:

* **Key Delegation.** A _trust transition commit_ signed by the inception key gives authority to additional keys. This creates an unbroken chain of trust from inception through subsequent authorizations.
* **Transition Commit.** Like the inception commit, the transition commit is designed to be empty to minimize attack surfaces.
* **Decentralized Governance.** Any authorized key can later be removed by trusted keys, allowing key rotation.
* **Delegation File.** A transition commit records rights in the local `allowed_commit_signers` file within the repository. Each modification to `allowed_commit_signers` must be signed by an authorized key (initially the inception key).
* **Superceding Keys.** The inception key is no longer used after a transition commit, with the new delegated keys now having all the authority.

Second, key revocation is needed:

* **Timestamped Revocation.** Revocation of keys is timestamped with Git's notes function.
* **Revocation Reporting.** Attempts to use a revoked key in a commit are not only reported, but will also stop the merging of that commit.

For more, see the [Open Integrity Design Docs](https://github.com/OpenIntegrityProject/core/blob/main/docs/Open_Integrity_Problem_Statement.md#establishing-the-root-of-trust)

## Open Integrity Links

* [**Open Integrity Repo**](https://github.com/openintegrityproject)
* [**Open Integrity Docs**](https://github.com/OpenIntegrityProject/core/tree/main/docs)
* [**Open Development**](/architecture/open-development/) (architecture page)


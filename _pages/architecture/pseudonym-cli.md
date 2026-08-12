---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Learning Pseudonyms from the Command Line"
hide_description: true
classes:
  - wide
permalink: /architecture/pseudonym/cli/
sidebar:
  nav:
    - pseudonym
    - archdesign
    - architecture
---

_Four technologies are broadly required to support Pseudonymous Trust
Building: Verifiable Attestations; Peer Endorsements; Evidence
Commitment; and a Progressive Life Cycle._

_The following shows in brief how these can be accomplished with a
[XID](/xid/). More can be found in [Learning XIDs from the Command
Line](https://learningxids.blockchaincommons.com/)._

## Creating a Pseudonymous Identity

Obviously, the first step is creating a pseudonymous identity. These
examples all use XIDs and the [envelope
CLI](https://github.com/BlockchainCommons/bc-envelope-cli-rust), which
can be installed with `cargo install bc-envelope-cli`.

The following creates the XID we're going to trust-build:

```sh
PRVKEYS=$(envelope generate prvkeys)
XID=$(envelope xid new $PRVKEYS)
XID_ID=$(envelope xid id $XID)

envelope format $XID

| XID(41bd57e1) [
|     'key': PublicKeys(e16ff4a8, SigningPublicKey(41bd57e1, SchnorrPublicKey(50ce5e05)), EncapsulationPublicKey(71b032a4, X25519PublicKey(71b032a4))) [
|         {
|             'privateKey': PrivateKeys(f35774c2, SigningPrivateKey(dd274fec, SchnorrPrivateKey(a8ce20ac)), EncapsulationPrivateKey(ab0f9b0f, X25519PrivateKey(ab0f9b0f)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|     ]
| ]
```

This creates a second XID for use with peer endorsement.

```
CHARLENE_PRVKEYS=$(envelope generate prvkeys)
CHARLENE_XID=$(envelope xid new $CHARLENE_PRVKEYS)
CHARLENE_XID_ID=$(envelope xid id $CHARLENE_XID)
```

## Supporting Verifiable Attestations

Attestations in XIDs are recorded via
[edges](https://learningxids.blockchaincommons.com/03_0_Edges/), which
are links between two XIDs (or in the case of a self-attestation,
between a XID and itself).

An edge has a specific format:

* Subject is unique.
* Contains predicate of `isA`, which defines the claims.
* Contains predicates of `source` and `target`, which define the endorser and endorsee.
* Recursive data under the `target` further describes the claim.

A verifiable attestation will typically use that recursive data to
offer validation URLs or other datums (e.g. ISBNs, etc.)

1. **Setup.** Lay out standard information

```sh
SUBJECT="pr-for-padlock-security-program"
ISA="GitHub PR"
SOURCE=$(envelope subject type ur "$XID_ID")
TARGET=$(envelope subject type ur "$XID_ID")
```

2. **Describe.** Create Target information

```sh
TARGET=$(envelope assertion add pred-obj string "githubURL" uri "https://github.com/OpenSecure/padlock/pull/273/" "$TARGET")
TARGET=$(envelope assertion add pred-obj string "prDescription" string "wrote emergency fix for zero-day exploit" "$TARGET")
```

3. **Construct.** Put the Pieces of the Edge Together

```sh
EDGE=$(envelope subject type string "$SUBJECT")
EDGE=$(envelope assertion add pred-obj known 'isA' string "$ISA" "$EDGE")
EDGE=$(envelope assertion add pred-obj known 'source' envelope "$SOURCE" "$EDGE")
EDGE=$(envelope assertion add pred-obj known 'target' envelope "$TARGET" "$EDGE")
```

4. **Sign.** Sign the Edge (with Envelope Wrapping)

```sh
WRAPPED_EDGE=$(envelope subject type wrapped "$EDGE")
SIGNED_EDGE=$(envelope sign --signer "$PRVKEYS" "$WRAPPED_EDGE")
```

5. **Attach.** Link the Edge to the XID

```sh
XID_WITH_EDGE=$(envelope xid edge add \
    "$SIGNED_EDGE" "$XID")

envelope format $XID_WITH_EDGE

| XID(41bd57e1) [
|     'edge': {
|         "pr-for-padlock-security-program" [
|             'isA': "GitHub PR"
|             'source': XID(41bd57e1)
|             'target': XID(41bd57e1) [
|                 "githubURL": URI(https://github.com/OpenSecure/padlock/pull/273/)
|                 "prDescription": "wrote emergency fix for zero-day exploit"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(e16ff4a8, SigningPublicKey(41bd57e1, SchnorrPublicKey(50ce5e05)), EncapsulationPublicKey(71b032a4, X25519PublicKey(71b032a4))) [
|         {
|             'privateKey': PrivateKeys(f35774c2, SigningPrivateKey(dd274fec, SchnorrPrivateKey(a8ce20ac)), EncapsulationPrivateKey(ab0f9b0f, X25519PrivateKey(ab0f9b0f)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|     ]
| ]
```

### Supporting Peer Endorsements

Best practice for peer endorsements is to have someone else create an
edge and send it to the endorsee, who can then decide whether to
publish it standalone, attach it to their XID, or do none of the
above. As a result, creating a peer endorsement follows much the same
pattern as creating a self endorsement, except it's being created by a
different person and as a link between two XIDs rather than a
self-referential link.

(This is what's meant by self-sovereign identity: you can't control
what other people say about your identity, or even what they publish,
but you can decide whether to closely link it to your identity or not.)

1. **Setup.** Lay out standard information

```sh
P_SUBJECT="charlene-security-endorsement"
P_ISA="attestation"
P_SOURCE=$(envelope subject type ur "$CHARLENE_XID_ID")
P_TARGET=$(envelope subject type ur "$XID_ID")
```

2. **Describe.** Create Target information

```sh
P_TARGET=$(envelope assertion add pred-obj string "attestation" string "Has worked on several security apps, completing them on-time and without known (to date) exploits" "$P_TARGET")
P_TARGET=$(envelope assertion add pred-obj string "relationship" string "Friend, co-worker" "$P_TARGET")
P_TARGET=$(envelope assertion add pred-obj known 'date' string `date -Iminutes` "$P_TARGET")
```

3. **Construct.** Put the Pieces of the Edge Together

```sh
P_EDGE=$(envelope subject type string "$P_SUBJECT")
P_EDGE=$(envelope assertion add pred-obj known 'isA' string "$P_ISA" "$P_EDGE")
P_EDGE=$(envelope assertion add pred-obj known 'source' envelope "$P_SOURCE" "$P_EDGE")
P_EDGE=$(envelope assertion add pred-obj known 'target' envelope "$P_TARGET" "$P_EDGE")
```

4. **Sign.** Sign the Edge (with Envelope Wrapping)

```sh
P_WRAPPED_EDGE=$(envelope subject type wrapped "$P_EDGE")
P_SIGNED_EDGE=$(envelope sign --signer "$CHARLENE_PRVKEYS" "$P_WRAPPED_EDGE")
```

5. **Attach.** Link the Edge to the XID (optional)

```sh
XID_WITH_EDGES=$(envelope xid edge add \
    "$P_SIGNED_EDGE" "$XID_WITH_EDGE")

envelope format $XID_WITH_EDGES

| XID(41bd57e1) [
|     'edge': {
|         "charlene-security-endorsement" [
|             'isA': "attestation"
|             'source': XID(91991568)
|             'target': XID(41bd57e1) [
|                 "attestation": "Has worked on several security apps, completing them on-time and without known (to date) exploits"
|                 "relationship": "Friend, co-worker"
|                 'date': "2026-08-12T10:56-10:00"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'edge': {
|         "pr-for-padlock-security-program" [
|             'isA': "GitHub PR"
|             'source': XID(41bd57e1)
|             'target': XID(41bd57e1) [
|                 "githubURL": URI(https://github.com/OpenSecure/padlock/pull/273/)
|                 "prDescription": "wrote emergency fix for zero-day exploit"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(e16ff4a8, SigningPublicKey(41bd57e1, SchnorrPublicKey(50ce5e05)), EncapsulationPublicKey(71b032a4, X25519PublicKey(71b032a4))) [
|         {
|             'privateKey': PrivateKeys(f35774c2, SigningPrivateKey(dd274fec, SchnorrPrivateKey(a8ce20ac)), EncapsulationPrivateKey(ab0f9b0f, X25519PrivateKey(ab0f9b0f)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|     ]
| ]
```	

### Supporting Evidence Commitments

One of the simplest ways to commit to evidence with a XID is to elide
some of its edges. The hashes remain (proving that the evidence
existed), and if the data is later revealed it can be correlated to
the hash, but until then nothing can be discovered about the data.

1. **Array.** List out content to be elided

```sh
EDGELIST=$(envelope xid edge all $XID_WITH_EDGES)
EDGES=($EDGELIST)
```

2. **Discover.** Find the specific content to be elided

```sh
envelope format ${EDGES[0]}

| {
|     "pr-for-padlock-security-program" [
|         'isA': "GitHub PR"
|         'source': XID(41bd57e1)
|         'target': XID(41bd57e1) [
|             "githubURL": URI(https://github.com/OpenSecure/padlock/pull/273/)
|             "prDescription": "wrote emergency fix for zero-day exploit"
|         ]
|     ]
| } [
|     'signed': Signature
| ]
```

3. **Hash.** Create a digest for the content to be elided

```
DIGEST=$(envelope digest ${EDGES[0]})
```

4. **Commit.** Elide the content with the digest


```
ELIDED_XID=$(envelope elide removing "$DIGEST" "$XID_WITH_EDGES")

envelope format $ELIDED_XID

| XID(41bd57e1) [
|     'edge': ELIDED
|     'edge': {
|         "charlene-security-endorsement" [
|             'isA': "attestation"
|             'source': XID(91991568)
|             'target': XID(41bd57e1) [
|                 "attestation": "Has worked on several security apps, completing them on-time and without known (to date) exploits"
|                 "relationship": "Friend, co-worker"
|                 'date': "2026-08-12T10:56-10:00"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(e16ff4a8, SigningPublicKey(41bd57e1, SchnorrPublicKey(50ce5e05)), EncapsulationPublicKey(71b032a4, X25519PublicKey(71b032a4))) [
|         {
|             'privateKey': PrivateKeys(f35774c2, SigningPrivateKey(dd274fec, SchnorrPrivateKey(a8ce20ac)), EncapsulationPrivateKey(ab0f9b0f, X25519PrivateKey(ab0f9b0f)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|     ]
| ]
```

### Supporting Progressive Trust

Progressive Trust simply requires eliding your XID in a different
way. Here, we might want to reveal what the GitHub commitment is
about, but still be reluctant to reveal the actual (verifiable) URL
because it might be somewhat correlatable to a real-world
identity. The verifiable URL would then only go out after
progressively reaching a higher level of trust.

(Finding this more minute claim to elide requires a bit more
excavating with the `envelope` CLI, which occurs in step 2, but the
concept nonetheless remains simple.)

1. **Array.** List out content to be elided

```sh
EDGELIST=$(envelope xid edge all $XID_WITH_EDGES)
EDGES=($EDGELIST)
```

2. **Discover.** Find the specific content to be elided

```sh
EDGE_UNWRAPPED=$(envelope extract wrapped "${EDGES[0]}")
EDGE_TARGET=$(envelope assertion find predicate known 'target' "$EDGE_UNWRAPPED")
EDGE_TARGET=$(envelope extract object $EDGE_TARGET)
EDGE_CLAIM=$(envelope assertion find predicate string "githubURL" "$EDGE_TARGET")

envelope format $EDGE_CLAIM

| "githubURL": URI(https://github.com/OpenSecure/padlock/pull/273/)
```

3. **Hash.** Create a digest for the content to be elided

```
CLAIM_DIGEST=$(envelope digest "$EDGE_CLAIM")
```

4. **Commit.** Elide the content with the digest

```
PROGRESSIVE_XID=$(envelope elide removing "$CLAIM_DIGEST" "$XID_WITH_EDGES")

envelope format $PROGRESSIVE_XID

| XID(41bd57e1) [
|     'edge': {
|         "charlene-security-endorsement" [
|             'isA': "attestation"
|             'source': XID(91991568)
|             'target': XID(41bd57e1) [
|                 "attestation": "Has worked on several security apps, completing them on-time and without known (to date) exploits"
|                 "relationship": "Friend, co-worker"
|                 'date': "2026-08-12T10:56-10:00"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'edge': {
|         "pr-for-padlock-security-program" [
|             'isA': "GitHub PR"
|             'source': XID(41bd57e1)
|             'target': XID(41bd57e1) [
|                 "prDescription": "wrote emergency fix for zero-day exploit"
|                 ELIDED
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(e16ff4a8, SigningPublicKey(41bd57e1, SchnorrPublicKey(50ce5e05)), EncapsulationPublicKey(71b032a4, X25519PublicKey(71b032a4))) [
|         {
|             'privateKey': PrivateKeys(f35774c2, SigningPrivateKey(dd274fec, SchnorrPrivateKey(a8ce20ac)), EncapsulationPrivateKey(ab0f9b0f, X25519PrivateKey(ab0f9b0f)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|     ]
| ]
```

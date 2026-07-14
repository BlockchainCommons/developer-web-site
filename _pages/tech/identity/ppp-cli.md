---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-identity.jpg
  og_image: /assets/images/bc-card.jpg
title: Learning PPPs from the Command Line
hide_description: true
classes:
  - wide
permalink: /identity/ppp/cli/
sidebar:
  nav:
    - ppp
    - identity
    - technology
---

_This short course demonstrates how to create and evolve a public
participation profile using [XIDs](/xid/) and the Rust-based [Envelope
CLI
tool](https://github.com/BlockchainCommons/bc-envelope-cli-rust). A
much more extensive example forms the basis of the [Learning XIDs from
the Command Line](https://learningxids.blockchaincommons.com/)
course._

## Creating a Public Participation Profile

A public participation profile can be created by creating a XID, then
populating it with self-attestations and peer endorsements.

### 1. Create Stable Pseudonymous Identifier

Create a stable pseudonymous identifier.

```sh
PRIVATE_KEY=$(envelope generate prvkeys)
PUBLIC_KEY=$(envelope generate pubkeys $PRIVATE_KEY)
PROFILE_XID=$(envelope xid new \
    --nickname "BRadvoc8" \
    --generator include \
    "$PRIVATE_KEY")
XID_ID=$(envelope xid id "$PROFILE_XID")

envelope format $PROFILE_XID

| XID(9678d663) [
|     'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|         {
|             'privateKey': PrivateKeys(0bb10664, SigningPrivateKey(4616af9a, SchnorrPrivateKey(76f22787)), EncapsulationPrivateKey(af898546, X25519PrivateKey(af898546)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "BRadvoc8"
|     ]
|     'provenance': ProvenanceMark(63598645) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
|                 "next-seq": 1
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
```

### 2. Add Value and Purpose Statements

Log value and purpose assertions.

```sh
VALUE_TARGET=$(envelope subject type ur $XID_ID)
VALUE_TARGET=$(envelope assertion add pred-obj string "purpose" string "Contributing to privacy-preserving systems that protect vulnerable populations" "$VALUE_TARGET")
VALUE_TARGET=$(envelope assertion add pred-obj string "values" string "Privacy as a human right, user agency, ethical data use" "$VALUE_TARGET")
```

Record them as an
[edge](https://learningxids.blockchaincommons.com/03_1_Creating_Edges/#step-3-understand-the-edge)
in the XID, with subject (unique identifier), `isA` (description of
the claim), `source` (who made the attestation), and `target` (who the
attestation is about). For a self-attestation, the `source` and
`target` are the same.

```sh
VALUE_EDGE=$(envelope subject type string "value-and-purpose-statement-07-2026")
VALUE_EDGE=$(envelope assertion add pred-obj known 'isA' known 'attestation' "$VALUE_EDGE")
VALUE_EDGE=$(envelope assertion add pred-obj known 'source' ur $XID_ID "$VALUE_EDGE")
VALUE_EDGE=$(envelope assertion add pred-obj known 'target' envelope $VALUE_TARGET "$VALUE_EDGE")
```

Wrap and sign the edge to prove you said it.

```sh
WRAPPED_VALUE=$(envelope subject type wrapped $VALUE_EDGE)
SIGNED_VALUE=$(envelope sign -s "$PRIVATE_KEY" "$WRAPPED_VALUE")
```

Attach it to your XID:

```sh
XID_WITH_EDGE1=$(envelope xid edge add \
  $SIGNED_VALUE $PROFILE_XID)

envelope format $XID_WITH_EDGE1

| XID(9678d663) [
|     'edge': {
|         "value-and-purpose-statement-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|                 "values": "Privacy as a human right, user agency, ethical data use"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|         {
|             'privateKey': PrivateKeys(0bb10664, SigningPrivateKey(4616af9a, SchnorrPrivateKey(76f22787)), EncapsulationPrivateKey(af898546, X25519PrivateKey(af898546)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "BRadvoc8"
|     ]
|     'provenance': ProvenanceMark(63598645) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
|                 "next-seq": 1
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
|
```

### 3. Self-Attestations of Technical Capability

Add technical capability self-attestations.

```sh
TECH_TARGET=$(envelope subject type ur $XID_ID)
TECH_TARGET=$(envelope assertion add pred-obj string "domain" string "Distributed Systems & Security" "$TECH_TARGET")
TECH_TARGET=$(envelope assertion add pred-obj string "experienceLevel" string "8 years professional development" "$TECH_TARGET")
TECH_TARGET=$(envelope assertion add pred-obj string "coreSkills" string "Cryptographic protocols, distributed consensus, mobile security" "$TECH_TARGET")
```

Compile it an edge as before.

```
TECH_EDGE=$(envelope subject type string "tech-experience-07-2026")
TECH_EDGE=$(envelope assertion add pred-obj known 'isA' known 'attestation' "$TECH_EDGE")
TECH_EDGE=$(envelope assertion add pred-obj known 'source' ur $XID_ID "$TECH_EDGE")
TECH_EDGE=$(envelope assertion add pred-obj known 'target' envelope $TECH_TARGET "$TECH_EDGE")
WRAPPED_TECH=$(envelope subject type wrapped $TECH_EDGE)
SIGNED_TECH=$(envelope sign -s "$PRIVATE_KEY" "$WRAPPED_TECH")
```

Attach it to your XID.

```sh
XID_WITH_EDGE2=$(envelope xid edge add \
  $SIGNED_TECH $XID_WITH_EDGE1)

envelope format $XID_WITH_EDGE2

| XID(9678d663) [
|     'edge': {
|         "tech-experience-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|                 "domain": "Distributed Systems & Security"
|                 "experienceLevel": "8 years professional development"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'edge': {
|         "value-and-purpose-statement-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|                 "values": "Privacy as a human right, user agency, ethical data use"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|         {
|             'privateKey': PrivateKeys(0bb10664, SigningPrivateKey(4616af9a, SchnorrPrivateKey(76f22787)), EncapsulationPrivateKey(af898546, X25519PrivateKey(af898546)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "BRadvoc8"
|     ]
|     'provenance': ProvenanceMark(63598645) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
|                 "next-seq": 1
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
```

### 4. Verifiable Evidence Commitments

Create evidence commitment to make a promise of a claim that you're
not willing to disclose publicly.

There are a number of ways to do this, including: creating an
attestation separate from the XID, eliding it, and publishing the
hash; creating an attestation as an edge in a XID and eliding it for
any publication; and creating an attestation separate from the XID,
recording its hash, and publishing the hash as a commitment in an
edge. This example uses the last technique.

Create an envelope of the claim that will be committed to:

```sh
PROJECT_SUMMARY="Designed privacy-preserving location system for safety applications"
```

Hash the claim, then format it using the [ur:digest
CDDL](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2021-002-digest.md#cddl).

```sh
SUMMARY_HASH=$(echo "$PROJECT_SUMMARY" | shasum -a 256 | awk '{print $1}')
SUMMARY_HASH_BW=$(bytewords -i hex "5820"$SUMMARY_HASH -o minimal)
SUMMARY_HASH_UR="ur:digest/"$SUMMARY_HASH_BW
```

Create an edge as usual, with the hash applied to the target.

```sh
COMMITMENT_TARGET=$(envelope subject type ur $XID_ID)
COMMITMENT_TARGET=$(envelope assertion add pred-obj string "commitment" digest $SUMMARY_HASH_UR $COMMITMENT_TARGET)
COMMITMENT_EDGE=$(envelope subject type string "project-commitment-07-2026")
COMMITMENT_EDGE=$(envelope assertion add pred-obj known 'isA' known 'attestation' "$COMMITMENT_EDGE")
COMMITMENT_EDGE=$(envelope assertion add pred-obj known 'source' ur $XID_ID "$COMMITMENT_EDGE")
COMMITMENT_EDGE=$(envelope assertion add pred-obj known 'target' envelope $COMMITMENT_TARGET "$COMMITMENT_EDGE")
WRAPPED_COMMITMENT=$(envelope subject type wrapped $COMMITMENT_EDGE)
SIGNED_COMMITMENT=$(envelope sign -s "$PRIVATE_KEY" "$WRAPPED_COMMITMENT")
```

Attach it to your XID.

```sh
XID_WITH_EDGE3=$(envelope xid edge add \
  $SIGNED_COMMITMENT $XID_WITH_EDGE2)

envelope format $XID_WITH_EDGE3

| XID(9678d663) [
|     'edge': {
|         "project-commitment-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "commitment": Digest(c0d10cdd)
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'edge': {
|         "tech-experience-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|                 "domain": "Distributed Systems & Security"
|                 "experienceLevel": "8 years professional development"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'edge': {
|         "value-and-purpose-statement-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|                 "values": "Privacy as a human right, user agency, ethical data use"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|         {
|             'privateKey': PrivateKeys(0bb10664, SigningPrivateKey(4616af9a, SchnorrPrivateKey(76f22787)), EncapsulationPrivateKey(af898546, X25519PrivateKey(af898546)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "BRadvoc8"
|     ]
|     'provenance': ProvenanceMark(63598645) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
|                 "next-seq": 1
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
```

### 5. Peer Attestations and Endorsements

Add peer attestation from another pseudonymous contributor.

This creates another XID to make the endorsement:

```sh
CHARLENE_PASSWORD="charlenes-own-password"
CHARLENE_PRVKEYS=$(envelope generate prvkeys --signing ed25519)
CHARLENE_PUBKEYS=$(envelope generate pubkeys "$CHARLENE_PRVKEYS")
CHARLENE_XID=$(echo $CHARLENE_PRVKEYS | \
    envelope xid new \
    --private encrypt \
    --encrypt-password "$CHARLENE_PASSWORD" \
    --nickname "Charlene" \
    --generator encrypt \
    --sign inception)
CHARLENE_XID_ID=$(envelope xid id "$CHARLENE_XID")
```

Make a targeted claim as usual.
```
ENDORSEMENT_TARGET=$(envelope subject type ur $XID_ID)
ENDORSEMENT_TARGET=$(envelope assertion add pred-obj string "endorsement" string "BRadvoc8 is athoughtful and committed contributor to privacy work that protects vulnerable communities" $ENDORSEMENT_TARGET)
```

Wrap it up as a signed edge.
```sh
ENDORSEMENT_EDGE=$(envelope subject type string "charlene-endorsement-07-2026")
ENDORSEMENT_EDGE=$(envelope assertion add pred-obj known 'isA' known 'attestation' "$ENDORSEMENT_EDGE")
ENDORSEMENT_EDGE=$(envelope assertion add pred-obj known 'source' ur $CHARLENE_XID_ID "$ENDORSEMENT_EDGE")
ENDORSEMENT_EDGE=$(envelope assertion add pred-obj known 'target' envelope $ENDORSEMENT_TARGET "$ENDORSEMENT_EDGE")
WRAPPED_ENDORSEMENT=$(envelope subject type wrapped $ENDORSEMENT_EDGE)
SIGNED_ENDORSEMENT=$(envelope sign -s "$CHARLENE_PRVKEYS" "$WRAPPED_ENDORSEMENT")
```

The owner of the identity then gets to decide whether to attach the
endorsement to their identity, publish it separately, keep it in their
back pocket for later, or throw it out.

```sh
XID_WITH_EDGE4=$(envelope xid edge add \
  $SIGNED_ENDORSEMENT $XID_WITH_EDGE3)
```

This version of BRadvoc8's public participation profile is complete.

```
envelope format $XID_WITH_EDGE4

| XID(9678d663) [
|     'edge': {
|         "charlene-endorsement-07-2026" [
|             'isA': 'attestation'
|             'source': XID(a87687c1)
|             'target': XID(9678d663) [
|                 "endorsement": "BRadvoc8 is athoughtful and committed contributor to privacy work that protects vulnerable communities"
|             ]
|         ]
|     } [
|         'signed': Signature(Ed25519)
|     ]
|     'edge': {
|         "project-commitment-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "commitment": Digest(c0d10cdd)
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'edge': {
|         "tech-experience-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|                 "domain": "Distributed Systems & Security"
|                 "experienceLevel": "8 years professional development"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'edge': {
|         "value-and-purpose-statement-07-2026" [
|             'isA': 'attestation'
|             'source': XID(9678d663)
|             'target': XID(9678d663) [
|                 "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|                 "values": "Privacy as a human right, user agency, ethical data use"
|             ]
|         ]
|     } [
|         'signed': Signature
|     ]
|     'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|         {
|             'privateKey': PrivateKeys(0bb10664, SigningPrivateKey(4616af9a, SchnorrPrivateKey(76f22787)), EncapsulationPrivateKey(af898546, X25519PrivateKey(af898546)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "BRadvoc8"
|     ]
|     'provenance': ProvenanceMark(63598645) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
|                 "next-seq": 1
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
```

Usually, a user would now advance their [provenance mark](/provemark/)
to show that they've created a new version of their public
participation profile, encrypt the secret data for their own storage
of the XID, and then release a public version of their XID with that
secret data entirely omitted.

```sh
PASSWORD="Amira-Pass"
PRIVATE_XID=$(envelope xid provenance next \
    --sign inception \
    --private encrypt \
    --generator encrypt \
    --encrypt-password "$PASSWORD" \
    "$XID_WITH_EDGE4")
PUBLIC_XID=$(envelope xid export --private elide --generator elide "$PRIVATE_XID")

envelope format $PUBLIC_XID

| {
|     XID(9678d663) [
|         'edge': {
|             "charlene-endorsement-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(a87687c1)
|                 'target': XID(9678d663) [
|                     "endorsement": "BRadvoc8 is athoughtful and committed contributor to privacy work that protects vulnerable communities"
|                 ]
|             ]
|         } [
|             'signed': Signature(Ed25519)
|         ]
|         'edge': {
|             "project-commitment-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(9678d663)
|                 'target': XID(9678d663) [
|                     "commitment": Digest(c0d10cdd)
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|         'edge': {
|             "tech-experience-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(9678d663)
|                 'target': XID(9678d663) [
|                     "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|                     "domain": "Distributed Systems & Security"
|                     "experienceLevel": "8 years professional development"
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|         'edge': {
|             "value-and-purpose-statement-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(9678d663)
|                 'target': XID(9678d663) [
|                     "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|                     "values": "Privacy as a human right, user agency, ethical data use"
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|         'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|             'allow': 'All'
|             'nickname': "BRadvoc8"
|             ELIDED
|         ]
|         'provenance': ProvenanceMark(d39369d9) [
|             ELIDED
|         ]
|     ]
| } [
|     'signed': Signature
| ]
```

## Managing the Participation Profile Lifecycle

A public participation profile can continue to change over
time. Following are examples of some of the changes that might occur
as part of a participation profile lifecycle.

### 1. Selective Disclosure

Gordian Envelope enables the creation of context-specific views of the
same profile. This may be used early in a participation profile
lifecycle to limit disclosure.

You can elide any part of your XID when publishing, but those edges
you added are particularly likely targets.

This will create an array of all your edges:

```sh
EDGES=$(envelope xid edge all $PUBLIC_XID)
EDGE_LIST=($EDGES)
```

You can then list out the contents of each edge along with its digest,
which you can use to identify that edge:

```sh
for i in "${EDGE_LIST[@]}"
do
  envelope digest $i
  envelope format $i
done

| ur:digest/hdcxmuihtezcrorhintsntprpastheykpkglbgotnltsneuemovylomnftdtsopkflmdnliydkfx
| {
|     "value-and-purpose-statement-07-2026" [
|         'isA': 'attestation'
|         'source': XID(9678d663)
|         'target': XID(9678d663) [
|             "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|             "values": "Privacy as a human right, user agency, ethical data use"
|         ]
|     ]
| } [
|     'signed': Signature
| ]
| 
| ur:digest/hdcxvelnnnwslnjyfejsswvatyromkiasrlbwktldnsototnbyclgyfeimlbsbayhlnsuynbesmk
| {
|     "project-commitment-07-2026" [
|         'isA': 'attestation'
|         'source': XID(9678d663)
|         'target': XID(9678d663) [
|             "commitment": Digest(c0d10cdd)
|         ]
|     ]
| } [
|     'signed': Signature
| ]
| 
| ur:digest/hdcxluwsfsfhskgdidiocncwbydrfzlokpecfplobdqdlreeeclddrvtetaeynceprrtrscfdrqz
| {
|     "tech-experience-07-2026" [
|         'isA': 'attestation'
|         'source': XID(9678d663)
|         'target': XID(9678d663) [
|             "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|             "domain": "Distributed Systems & Security"
|             "experienceLevel": "8 years professional development"
|         ]
|     ]
| } [
|     'signed': Signature
| ]
| 
| ur:digest/hdcxtpgtjlbbsrhgvtnsoeisqzvavlwddrdygagdvyfhvwjtdmjepdwpjtoycabkhhfsbbvwgokt
| {
|     "charlene-endorsement-07-2026" [
|         'isA': 'attestation'
|         'source': XID(a87687c1)
|         'target': XID(9678d663) [
|             "endorsement": "BRadvoc8 is athoughtful and committed contributor to privacy work that protects vulnerable communities"
|         ]
|     ]
| } [
|     'signed': Signature(Ed25519)
| ]
```

With that data in hand you can create a new "view" of a XID that only
includes specific information (but maintains commitments of the
rest). The following removes three of the four edges (everything but
the assessment of technical capability).

```sh
LIMITED_VIEW=$(envelope elide removing "ur:digest/hdcxmuihtezcrorhintsntprpastheykpkglbgotnltsneuemovylomnftdtsopkflmdnliydkfx ur:digest/hdcxvelnnnwslnjyfejsswvatyromkiasrlbwktldnsototnbyclgyfeimlbsbayhlnsuynbesmk ur:digest/hdcxtpgtjlbbsrhgvtnsoeisqzvavlwddrdygagdvyfhvwjtdmjepdwpjtoycabkhhfsbbvwgokt" $PUBLIC_XID)

envelope format $LIMITED_VIEW

| {
|     XID(9678d663) [
|         'edge': ELIDED
|         'edge': ELIDED
|         'edge': ELIDED
|         'edge': {
|             "tech-experience-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(9678d663)
|                 'target': XID(9678d663) [
|                     "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|                     "domain": "Distributed Systems & Security"
|                     "experienceLevel": "8 years professional development"
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|         'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|             'allow': 'All'
|             'nickname': "BRadvoc8"
|             ELIDED
|         ]
|         'provenance': ProvenanceMark(d39369d9) [
|             ELIDED
|         ]
|     ]
| } [
|     'signed': Signature
| ]
```

## 2. Verifying Public Contributions

A XID's public key can be used to verify contributions without
revealing identity.

The following example verifies that the technical self-attestation
(the one remaining in the public view) was signed by the XID holder.


```sh
envelope verify -s -v "$PUBLIC_KEY" "${EDGE_LIST[2]}"

|
```

Silence marks success.

### 3. Progressive Trust Building

XIDs support the gradual addition of attestations and
endorsements. This "Reputation Development" occurs after early rounds
of the Participation Profile Lifecycle and allows for the addition of
more endorsements over time.

This comes from a new peer.

```sh
REVIEWER_PRVKEYS=$(envelope generate prvkeys --signing ed25519)
REVIEWER_PUBKEYS=$(envelope generate pubkeys "$REVIEWER_PRVKEYS")
REVIEWER_PASSWORD="devreviewers-own-password"
REVIEWER_XID=$(echo $REVIEWER_PRVKEYS | \
    envelope xid new \
    --private encrypt \
    --encrypt-password "$REVIEWER_PASSWORD" \
    --nickname "Charlene" \
    --generator encrypt \
    --sign inception)
REVIEWER_XID_ID=$(envelope xid id "$REVIEWER_XID")
```

They write a new endorsement.
```sh
REVIEWER_TARGET=$(envelope subject type ur $XID_ID)
REVIEWER_TARGET=$(envelope assertion add pred-obj string "peerEndorsement" string "Writes secure, well-tested code with clear attention to privacy-preserving patterns" $REVIEWER_TARGET)
```

They include a bit of detail on themself.
```sh
REVIEWER_SOURCE=$(envelope subject type ur $REVIEWER_XID_ID)
REVIEWER_SOURCE=$(envelope assertion add pred-obj string "schema:worksFor" string "SisterSpaces" $REVIEWER_SOURCE)
REVIEWER_SOURCE=$(envelope assertion add pred-obj string "schema:employeeRole" string "Head Security Programmer" $REVIEWER_SOURCE)
```

They create an edge.
```sh
REVIEWER_ARID=$(envelope generate arid -x | cut -c 1-16)
REVIEWER_SUBJECT="peer-endorsement-from-devreviewer-$REVIEWER_ARID"
REVIEWER_EDGE=$(envelope subject type string $REVIEWER_SUBJECT)
REVIEWER_EDGE=$(envelope assertion add pred-obj known isA string "peerEndorsement" $REVIEWER_EDGE)
REVIEWER_EDGE=$(envelope assertion add pred-obj known source envelope "$REVIEWER_SOURCE" "$REVIEWER_EDGE")
REVIEWER_EDGE=$(envelope assertion add pred-obj known target envelope "$REVIEWER_TARGET" "$REVIEWER_EDGE")
```

They sign and wrap it.
```sh
REVIEWER_WRAPPED_EDGE=$(envelope subject type wrapped $REVIEWER_EDGE)
REVIEWER_SIGNED_EDGE=$(envelope sign --signer "$REVIEWER_PRVKEYS" "$REVIEWER_WRAPPED_EDGE")
```

You decide whether to add the endorsement to your XID.

```sh
XID_WITH_EDGE5=$(envelope xid edge add \
  $REVIEWER_SIGNED_EDGE $XID_WITH_EDGE4)
```

If so, advance the provenance marker and republish.

```sh
PRIVATE_XID=$(envelope xid provenance next \
    --sign inception \
    --private encrypt \
    --generator encrypt \
    --encrypt-password "$PASSWORD" \
    "$XID_WITH_EDGE5")
PUBLIC_XID=$(envelope xid export --private elide --generator elide "$PRIVATE_XID")

envelope format $PUBLIC_XID

| {
|     XID(9678d663) [
|         'edge': {
|             "charlene-endorsement-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(a87687c1)
|                 'target': XID(9678d663) [
|                     "endorsement": "BRadvoc8 is athoughtful and committed contributor to privacy work that protects vulnerable communities"
|                 ]
|             ]
|         } [
|             'signed': Signature(Ed25519)
|         ]
|         'edge': {
|             "peer-endorsement-from-devreviewer-6376d2bad0981faa" [
|                 'isA': "peerEndorsement"
|                 'source': XID(d9541180) [
|                     "schema:employeeRole": "Head Security Programmer"
|                     "schema:worksFor": "SisterSpaces"
|                 ]
|                 'target': XID(9678d663) [
|                     "peerEndorsement": "Writes secure, well-tested code with clear attention to privacy-preserving patterns"
|                 ]
|             ]
|         } [
|             'signed': Signature(Ed25519)
|         ]
|         'edge': {
|             "project-commitment-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(9678d663)
|                 'target': XID(9678d663) [
|                     "commitment": Digest(c0d10cdd)
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|         'edge': {
|             "tech-experience-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(9678d663)
|                 'target': XID(9678d663) [
|                     "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|                     "domain": "Distributed Systems & Security"
|                     "experienceLevel": "8 years professional development"
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|         'edge': {
|             "value-and-purpose-statement-07-2026" [
|                 'isA': 'attestation'
|                 'source': XID(9678d663)
|                 'target': XID(9678d663) [
|                     "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|                     "values": "Privacy as a human right, user agency, ethical data use"
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|         'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|             'allow': 'All'
|             'nickname': "BRadvoc8"
|             ELIDED
|         ]
|         'provenance': ProvenanceMark(4c216192) [
|             ELIDED
|         ]
|     ]
| } [
|     'signed': Signature
| ]
```

At this point, you might also created new, more limited views, as per the previous section.

Also note that DevReviewer's edge doesn't quite look the same as
Charlene's edge. They choose to identify their edges using different
methodologies (one with a date, one with an ARID), they choose to
define their edge as a "peerEndorsement" instead of a 'attestation',
and they choose to further identify themself while Charlene did
not. This is all to be expected. The iSa/source/target format provides
some standardization, but beyond that edges can be fairly freeform
(while attestations not directly linked to XIDs can be even more
freeform).

### 4. Revocable Credentials

XIDs support revocation of a profile if circumstances change. One way
to do so is to just put the XID in an envelope, explicitly revoke it,
and sign the revocation.

```sh
WRAPPED_XID=$(envelope subject type wrapped $PUBLIC_XID)
WRAPPED_XID=$(envelope assertion add pred-obj string "credentialStatus" string "revoked" "$WRAPPED_XID")
WRAPPED_XID=$(envelope assertion add pred-obj string "revocationDate" string "2026-11-30" $WRAPPED_XID)
WRAPPED_REVOCATION=$(envelope subject type wrapped $WRAPPED_XID)
SIGNED_REVOCATION=$(envelope sign -s $PRIVATE_KEY $WRAPPED_REVOCATION)

envelope format $SIGNED_REVOCATION

| {
|     {
|         {
|             XID(9678d663) [
|                 'edge': {
|                     "charlene-endorsement-07-2026" [
|                         'isA': 'attestation'
|                         'source': XID(a87687c1)
|                         'target': XID(9678d663) [
|                             "endorsement": "BRadvoc8 is athoughtful and committed contributor to privacy work that protects vulnerable communities"
|                         ]
|                     ]
|                 } [
|                     'signed': Signature(Ed25519)
|                 ]
|                 'edge': {
|                     "peer-endorsement-from-devreviewer-6376d2bad0981faa" [
|                         'isA': "peerEndorsement"
|                         'source': XID(d9541180) [
|                             "schema:employeeRole": "Head Security Programmer"
|                             "schema:worksFor": "SisterSpaces"
|                         ]
|                         'target': XID(9678d663) [
|                             "peerEndorsement": "Writes secure, well-tested code with clear attention to privacy-preserving patterns"
|                         ]
|                     ]
|                 } [
|                     'signed': Signature(Ed25519)
|                 ]
|                 'edge': {
|                     "project-commitment-07-2026" [
|                         'isA': 'attestation'
|                         'source': XID(9678d663)
|                         'target': XID(9678d663) [
|                             "commitment": Digest(c0d10cdd)
|                         ]
|                     ]
|                 } [
|                     'signed': Signature
|                 ]
|                 'edge': {
|                     "tech-experience-07-2026" [
|                         'isA': 'attestation'
|                         'source': XID(9678d663)
|                         'target': XID(9678d663) [
|                             "coreSkills": "Cryptographic protocols, distributed consensus, mobile security"
|                             "domain": "Distributed Systems & Security"
|                             "experienceLevel": "8 years professional development"
|                         ]
|                     ]
|                 } [
|                     'signed': Signature
|                 ]
|                 'edge': {
|                     "value-and-purpose-statement-07-2026" [
|                         'isA': 'attestation'
|                         'source': XID(9678d663)
|                         'target': XID(9678d663) [
|                             "purpose": "Contributing to privacy-preserving systems that protect vulnerable populations"
|                             "values": "Privacy as a human right, user agency, ethical data use"
|                         ]
|                     ]
|                 } [
|                     'signed': Signature
|                 ]
|                 'key': PublicKeys(9e13aa34, SigningPublicKey(9678d663, SchnorrPublicKey(39012dbc)), EncapsulationPublicKey(cd1ea624, X25519PublicKey(cd1ea624))) [
|                     'allow': 'All'
|                     'nickname': "BRadvoc8"
|                     ELIDED
|                 ]
|                 'provenance': ProvenanceMark(4c216192afbb7ea0ebc52cff3720c7da8590345dbb1699b51f137c5de442d503) [
|                     ELIDED
|                 ]
|             ]
|         } [
|             'signed': Signature
|         ]
|     } [
|         "credentialStatus": "revoked"
|         "revocationDate": "2026-11-30"
|     ]
| } [
|     'signed': Signature
| ]
```

### 5. Trust Networks

XIDs enable networks of verified relationships between pseudonymous identities:

```sh
XID_TRUST=$(envelope subject type ur $XID_ID)
XID_TRUST=$(envelope assertion add pred-obj string "trusted" string "true" "$XID_TRUST")
XID_TRUST=$(envelope assertion add pred-obj string "trustContext" string "Code Review" "$XID_TRUST")
XID_TRUST=$(envelope assertion add pred-obj string "trustLevel" string "0.8" "$XID_TRUST")
TRUST_ARID=$(envelope generate arid -x | cut -c 1-16)
TRUST_SUBJECT="trust-statement-from-devreviewer-$TRUST_ARID"
TRUST_EDGE=$(envelope subject type string $TRUST_SUBJECT)
TRUST_EDGE=$(envelope assertion add pred-obj known isA known attestation "$TRUST_EDGE")
TRUST_EDGE=$(envelope assertion add pred-obj known source ur $REVIEWER_XID_ID "$TRUST_EDGE")
TRUST_EDGE=$(envelope assertion add pred-obj known target envelope "$XID_TRUST" "$TRUST_EDGE")
WRAPPED_TRUST=$(envelope subject type wrapped $TRUST_EDGE)
SIGNED_TRUST=$(envelope sign -s $REVIEWER_PRVKEYS $WRAPPED_TRUST)

envelope format $SIGNED_TRUST

| {
|     "trust-statement-from-devreviewer-35af7e15e476b6fb" [
|         'isA': 'attestation'
|         'source': XID(d9541180)
|         'target': XID(9678d663) [
|             "trustContext": "Code Review"
|             "trustLevel": "0.8"
|             "trusted": "true"
|         ]
|     ]
| } [
|     'signed': Signature(Ed25519)
| ]
```

This attestation could have meaning either attached to a XID or
published on its own, with the signature.

## Check Your Understanding

1. Why might someone like Amira need a public participation profile rather than using their real identity?
2. What are the key components that make a participation profile both privacy-preserving and trust-building?
3. How does the proportional risk approach help determine appropriate disclosure levels?
4. What role do peer attestations play in strengthening pseudonymous participation profiles?
5. How do XIDs and Gordian Envelopes technically enable participation profiles?

---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/tech-identity.jpg
  og_image: /assets/images/bc-card.jpg
title: Learning Attestations from the Command Line
tagline: 
hide_description: true
classes:
  - wide
permalink: /identity/attestations/cli/
sidebar:
  nav:
    - attestations
    - identity
    - technology
---

_Attestations, endorsements, and other claims can be created using
Gordian Envelope. They can be published in whole or published as a
hash to be revealed later. Alternatively, they can be attached
directly to an identity._

_The following command-line examples use the [Rust
Envelope CLI](https://github.com/BlockchainCommons/bc-envelope-cli-rust)
to create claims. More examples can be found in [Learning XIDs from
the Command Line](https://learningxids.blockchaincommons.com/),
particularly [chapter
2](https://learningxids.blockchaincommons.com/02_0_Claims/), which
demonstrates more ways to make claims, and [chapter
3](https://learningxids.blockchaincommons.com/03_0_Edges/), which
shows how to attach those claims to identities._

_The [Bytewords
CLI](https://github.com/BlockchainCommons/bytewords-cli) is also used
in a single example to generate a UR by hand._

## Creating Self-Attestations

The following example uses the [fair witness
methodology](/identity/fair-witness/) to create a claim. A good self
attestation contains the following elements:

1. **Clear Subject**: What specifically is being attested to

   ```sh
   PROJECT=$(envelope subject type string "Financial API Security Overhaul")
   ```

2. **Specific Claims with Context**:

   ```sh
   PROJECT=$(envelope assertion add pred-obj string "role" string "Lead Security Developer" "$PROJECT")
   PROJECT=$(envelope assertion add pred-obj string "timeframe" string "2022-03 through 2022-09" "$PROJECT")
   ```

3. **Methodology and Evidence**:

   ```sh
   PROJECT=$(envelope assertion add pred-obj string "methodology" string "Security testing with automated scanning and manual code review" "$PROJECT")
   PROJECT=$(envelope assertion add pred-obj string "metricsHash" digest "$METRICS_HASH" "$PROJECT")
   ```

4. **Transparent Limitations**:

   ```sh
   PROJECT=$(envelope assertion add pred-obj string "limitations" string "Backend components only, limited frontend involvement" "$PROJECT")
   ```

5. **Cryptographic Signatures**:

   ```sh
   WRAPPED_PROJECT=$(envelope subject type wrapped $PROJECT)
   SIGNED_PROJECT=$(envelope sign -s "$PRIVATE_KEYS" "$WRAPPED_PROJECT")
   ```

6. **Publication**:

This complete self-attestation looks as follows:

   ```sh
   {
       "Financial API Security Overhaul" [
           "limitations": "Backend components only, limited frontend involvement"
           "methodology": "Security testing with automated scanning and manual code review"
           "metricsHash": Digest(b3cb8720)
           "role": "Lead Security Developer"
          "timeframe": "2022-03 through 2022-09"
       ]
   } [
       'signed': Signature
   ]
```

The subject could choose to store, privately share, or publish the
  attestation, to share or publish only a hash of the attestation, or
  to attach it to their identity (such as linking it as an edge to a
  XID).

## Adding Verifiable Self-Attestations

Self-attestations become more credible when they reference
independently verifiable sources. The follow examples add new
verifiable details to the previous `$PROJECT`.


1. **Git Commit References**:

   This demonstrates how Github records can make a project verifiable.

   ```sh
   # Generate a hash of the hashes in the commit history
   GIT_HISTORY_HASH=$(git log --pretty=format:"%H" | shasum -a 256 | awk '{print $1}')

   # Encode per the [ur:digest CDDL](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2021-002-digest.md#cddl)
   GIT_HISTORY_HASH_BW=$(bytewords -i hex "5820"$GIT_HISTORY_HASH -o minimal)
   GIT_HISTORY_HASH_UR="ur:digest/"$GIT_HISTORY_HASH_BW  

   # Add the verifiable reference to the project
   PROJECT_GIT=$(envelope subject type string "https://github.com/organization/project")   
   PROJECT_GIT=$(envelope assertion add pred-obj string "gitCommitHistory" digest "$GIT_HISTORY_HASH_UR" "$PROJECT_GIT")
   PROJECT_GIT=$(envelope assertion add pred-obj string 'gitCommitHistoryDate' string `date -Iminutes` "$PROJECT_GIT")
   PROJECT_GIT=$(envelope assertion add pred-obj string "gitVerificationNote" string "Commits signed with GPG key fingerprint 3AA5 C34D..." "$PROJECT_GIT")
   PROJECT=$(envelope assertion add pred-obj string "gitRepo" envelope $PROJECT_GIT $PROJECT)
   ```
 
2. **Published Work**:

   This demonstrates how a published work can make a project verifiable.

   ```sh
   PROJECT_DOI=$(envelope subject type string "10.1234/journal.article")
   PROJECT_DOI=$(envelope assertion add pred-obj known date string "2023-05-12" "$PROJECT_DOI")
   PROJECT=$(envelope assertion add pred-obj string "publicationDOI" envelope $PROJECT_DOI "$PROJECT")
   ```

3. **Public Demonstrations**:

   This demonstrates how a demo can make a project verifiable.
   
   ```sh
   PROJECT_DEMO=$(envelope subject type uri "https://example.com/demo-with-timestamp")
   PROJECT_DEMO=$(envelope assertion add pred-obj known date string "2023-07-09" "$PROJECT_DEMO")
   PROJECT=$(envelope assertion add pred-obj string "demoVideo" envelope $PROJECT_DEMO "$PROJECT")
   ```

4. **Cryptographic Signatures**:

   Since we adjusted the core `$PROJECT` data, we must re-sign.
   
   ```sh
   WRAPPED_PROJECT=$(envelope subject type wrapped $PROJECT)
   SIGNED_PROJECT=$(envelope sign -s "$PRIVATE_KEYS" "$WRAPPED_PROJECT")
   ```

5. **Identifier Link**:

   The signed credential has now improved the original attestation
   through additional data that is verifiable in various ways.

```sh
   {
       "Financial API Security Overhaul" [
           "demoVideo": URI(https://example.com/demo-with-timestamp) [
               'date': "2023-07-09"
           ]
           "gitRepo": "https://github.com/organization/project" [
               "gitCommitHistory": Digest(cff55bed)
               "gitCommitHistoryDate": "2026-07-08T11:18-10:00"
               "gitVerificationNote": "Commits signed with GPG key fingerprint 3AA5 C34D..."
           ]
           "limitations": "Backend components only, limited frontend involvement"
           "methodology": "Security testing with automated scanning and manual code review"
           "metricsHash": Digest(b3cb8720)
           "publicationDOI": "10.1234/journal.article" [
               'date': "2023-05-12"
           ]
           "role": "Lead Security Developer"
           "timeframe": "2022-03 through 2022-09"
       ]
   } [
       'signed': Signature
   ]
   ```

## Making Peer Endorsements

You can also create endorsements about other people. Following the
[Fair Witness methdology](/identity/fair-witness/), this requires:

1. **Endorser Information**:

   The endorseer is identified, and then additional data is used to
   describe who they are and what their biases are.
  
   ```sh
   SOURCE=$(envelope subject type ur $MY_XID_ID)
   SOURCE=$(envelope assertion add pred-obj string "endorser" string "TechPM - Project Manager with 12 years experience" "$SOURCE")
   ```

2. **Basis for Endorsement**:

   ```sh
   SOURCE=$(envelope assertion add pred-obj string "basis" string "Direct observation throughout the project plus review of metrics" "$SOURCE")
   ```

3. **Endorser's Limitations**:

   ```sh
   SOURCE=$(envelope assertion add pred-obj string "endorserLimitation" string "Limited technical background in cryptography" "$SOURCE")
   ```

4. **Potential Biases**:

   ```sh
   SOURCE=$(envelope assertion add pred-obj string "potentialBias" string "Had management responsibility for project success" "$SOURCE")
   ```


5. **Relationship Disclosure**:

   ```sh
   SOURCE=$(envelope assertion add pred-obj string "relationship" string "Direct project oversight as Project Manager" "$SOURCE")
   ```

6. **Clear Identification of Subject**:

   ```sh
   TARGET=$(envelope subject type ur $XID_ID)
   ```


7. **Specific Observations**:

   Observations are then applied to the `$TARGET` as a standardized
   methodology described in [Learning
   XIDs](https://learningxids.blockchaincommons.com/03_1_Creating_Edges/#step-3-understand-the-edge).
   
   ```sh
   TARGET=$(envelope assertion add pred-obj string "project" string "Financial API Project" "$TARGET")
   TARGET=$(envelope assertion add pred-obj string "observation" string "BRadvoc8 designed innovative authentication system that exceeded security requirements" "$TARGET")
   ```

8. **Standard Format**:

   This example follows the [edge
   formatting](https://learningxids.blockchaincommons.com/03_1_Creating_Edges/#step-3-understand-the-edge)
   of endorsements, which calls for a unique subject, a descriptive
   `isA` and a `source` and `target`, potentially with additional info
   about each. (Any alternative format could be used.)

   ```sh
   EDGE=$(envelope subject type string "endorsement-from-charlene")
   EDGE=$(envelope assertion add pred-obj known 'isA' known 'attestation' $EDGE)
   EDGE=$(envelope assertion add pred-obj known 'source' envelope $SOURCE $EDGE)
   EDGE=$(envelope assertion add pred-obj known 'target' envelope $TARGET $EDGE)
   ```
   
9. **Cryptographic Signature**:

   ```sh
   WRAPPED_EDGE=$(envelope subject type wrapped $EDGE)
   SIGNED_EDGE=$(envelope sign -s "$MY_KEYS" "$WRAPPED_EDGE")
   ```
   The result is as follows:
   ```sh
   {
       "endorsement-from-charlene" [
           'isA': 'attestation'
           'source': XID(51754269) [
               "basis": "Direct observation throughout the project plus review of metrics"
               "endorser": "TechPM - Project Manager with 12 years experience"
               "endorserLimitation": "Limited technical background in cryptography"
               "potentialBias": "Had management responsibility for project success"
               "relationship": "Direct project oversight as Project Manager"
           ]
           'target': XID(e37356a3) [
               "observation": "BRadvoc8 designed innovative authentication system that exceeded security requirements"
               "project": "Financial API Project"
           ]
       ]
   } [
       'signed': Signature
   ]
   ```

## Accepting an Endorsement

For maximum credibility, endorsements should follow an acceptance model:

1. **Recipient Reviews Endorsement**:

   - Evaluates accuracy and fairness
   - Checks for appropriate context and disclosure
   - Verifies the endorser's signature
   
   ```sh
   # Verify the signature is valid
   if envelope verify -v "$ENDORSER_PUBLIC_KEY" "$SIGNED_EDGE"; then
     echo "✅ Endorsement signature verified"
   else
     echo "❌ Invalid endorsement signature"
   fi
   
   # Review the endorsement content
   envelope format "$SIGNED_EDGE"
   ```

2. **Recipient Decides Whether to Accept**:

   - Can accept the endorsement as is
   - Can request modifications before accepting
   - Can decline the endorsement

3. **Recipient Includes Accepted Endorsement**:

   As noted previously, a user can choose to publish an attestation in
   a variety of ways. One of the most powerful is to add an
   endorsement to a XID, which can be done as an `edge` or an
   `attachment`, depending on the precise formatting.
   
   ```sh
   XID_WITH_EDGE=$(envelope xid edge add \
     --verify inception \
     $SIGNED_EDGE $XID)
   ```

4. **Recipient Publishes Public Version of XID**

   The user can then produce a public version of their XID, which
   involves advancing a provenance mark and eliding unneeded private
   date, as explained in [Learning
   XIDs](https://learningxids.blockchaincommons.com/).
   
   ```sh
   XID_WITH_EDGE=$(envelope xid provenance next \
     --password "$PASSWORD" \
     --sign inception \
     --private encrypt \
     --generator encrypt \
     --encrypt-password "$PASSWORD" \
     "$XID_WITH_EDGE")
   PUBLIC_XID_WITH_EDGE=$(envelope xid export \
     --private elide \
     --generator elide \
     "$XID_WITH_EDGE")
   ```

   Part of this publication is `--sign inception`, which is where the
   user signs the XID with the edge to show that they've agreed for it
   to be part of their identity information.

   The result will look like:

   ```sh
   {
       XID(e37356a3) [
           'edge': {
               "endorsement-from-charlene" [
                   'isA': 'attestation'
                   'source': XID(51754269) [
                       "basis": "Direct observation throughout the project plus review of metrics"
                       "endorser": "TechPM - Project Manager with 12 years experience"
                       "endorserLimitation": "Limited technical background in cryptography"
                       "potentialBias": "Had management responsibility for project success"
                       "relationship": "Direct project oversight as Project Manager"
                   ]
                   'target': XID(e37356a3) [
                       "observation": "BRadvoc8 designed innovative authentication system that exceeded security requirements"
                       "project": "Financial API Project"
                   ]
               ]
           } [
               'signed': Signature
           ]
           'key': PublicKeys(77f5072e, SigningPublicKey(e37356a3, Ed25519PublicKey(234563e3)), EncapsulationPublicKey(20016c72, X25519PublicKey(20016c72))) [
               'allow': 'All'
               'nickname': "BRadvoc8"
               ELIDED
           ]
           'provenance': ProvenanceMark(c2c580a8) [
               ELIDED
           ]
       ]
   } [
       'signed': Signature(Ed25519)
   ]
   ```

## Check Your Understanding

1. What are the key differences between self-attestations and endorsements?
2. How does verifiable external evidence strengthen self-attestations?
3. Why is the acceptance model important for endorsements?
4. What makes an endorsement more credible and trustworthy?
5. How does a network of attestations and endorsements build over time?

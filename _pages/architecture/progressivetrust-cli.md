---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Learning Progressive Trust from the Command Line"
hide_description: true
classes:
  - wide
permalink: /architecture/progressive-trust/cli/
sidebar:
  nav:
    - progressivetrust
    - archdesign
    - architecture
---

_The following example demonstrates how a Progressive Trust Life Cycle
can be conducted and recorded over time through the use of [Gordian
Envelope](/envelope/) as a regularized data store for both information
and sigunatures._

_It uses the [Gordian Envelope
CLI](https://github.com/BlockchainCommons/bc-envelope-cli-rust) to
build a series of envelopes that demonstrate how trust can be
progressively expanded over time.  The contributor and various members
of the dev team are identified by [XIDs](/xid/), which allows them to
engage in [Pseudonymous Trust Building](/architecture/pseudonym/) over
time._


```sh
DEVELOPER_PRVKEYS=$(envelope generate prvkeys)
DEVELOPER_XID=$(envelope xid new "$DEVELOPER_PRVKEYS")
DEVELOPER_XID_ID=$(envelope xid id "$DEVELOPER_XID")

MAINTAINER_PRVKEYS=$(envelope generate prvkeys)
MAINTAINER_XID=$(envelope xid new "$MAINTAINER_PRVKEYS")
MAINTAINER_XID_ID=$(envelope xid id "$MAINTAINER_XID")

SENIOR_DEV_PRVKEYS=$(envelope generate prvkeys)
SENIOR_DEV_XID=$(envelope xid new "$SENIOR_DEV_PRVKEYS")
SENIOR_DEV_XID_ID=$(envelope xid id "$SENIOR_DEV_XID")

SECURITY_TEAM_PRVKEYS=$(envelope generate prvkeys)
SECURITY_TEAM_XID=$(envelope xid new "$SECURITY_TEAM_PRVKEYS")
SECURITY_TEAM_XID_ID=$(envelope xid id "$SECURITY_TEAM_XID")
```

## Phase 0: Context *(Interaction Considered)*

Before trust formation begins, parties consider whether an interaction
requires progressive trust at all. In this case, a PR is being offered
for an open softwareware package and review is needed. Since it's a
production system component, there is a medium level of risk. As a
result, the Maintainer looking over the PR decides to apply a
progressive trust model to see if the code and the coder should be
trusted.


### Phase 1: Introduction *(Assertions Declared)*

Parties make initial declarations and reveal some information while
eliding other data.

```sh
DEVELOPER_DECLARATION=$(envelope subject type string "Code Contribution #PR-123")
DEVELOPER_DECLARATION=$(envelope assertion add pred-obj string "contributor" ur "$DEVELOPER_XID_ID" "$DEVELOPER_DECLARATION")
DEVELOPER_DECLARATION=$(envelope assertion add pred-obj string "repositoryURL" string "https://github.com/example/project" "$DEVELOPER_DECLARATION")
DEVELOPER_DECLARATION=$(envelope assertion add pred-obj string "commitHash" string "7dd42c1be02cc53f70bfd0021d0aac15bf8e2ad5" "$DEVELOPER_DECLARATION")
DEVELOPER_DECLARATION=$(envelope assertion add pred-obj string "description" string "Add authentication module" "$DEVELOPER_DECLARATION")
DEVELOPER_DECLARATION=$(envelope assertion add pred-obj string "experience" string "5 years Node.js development" "$DEVELOPER_DECLARATION")
DEVELOPER_DECLARATION=$(envelope subject type wrapped $DEVELOPER_DECLARATION)
DEVELOPER_DECLARATION=$(envelope sign -s $DEVELOPER_PRVKEYS $DEVELOPER_DECLARATION)

envelope format $DEVELOPER_DECLARATION

| {
|     "Code Contribution #PR-123" [
|         "commitHash": "7dd42c1be02cc53f70bfd0021d0aac15bf8e2ad5"
|         "contributor": XID(e90049a2)
|         "description": "Add authentication module"
|         "experience": "5 years Node.js development"
|         "repositoryURL": "https://github.com/example/project"
|     ]
| } [
|     'signed': Signature
| ]
```

Here, the developer introduces themselves and their contribution,
revealing their XID, the specific contribution details, and relevant
experience. This is all done as part of a [Gordian
Envelope](/envelope/), which will record the evolution of trust over
the course of the Progressive Trust Life Cycle.

### Phase 2: Wholeness *(Integrity Assessed)*

The data assets are checked for their structural integrity and completeness.

```sh
WRAPPED_DEVELOPER_DECLARATION=$(envelope subject type wrapped "$DEVELOPER_DECLARATION")
INTEGRITY_CHECK=$(envelope subject type string "ContinuousIntegrationSystem")
INTEGRITY_CHECK=$(envelope assertion add pred-obj string "lintCheck" string "Passed - 0 errors, 2 warnings" "$INTEGRITY_CHECK")
INTEGRITY_CHECK=$(envelope assertion add pred-obj string "compilationCheck" string "Successful" "$INTEGRITY_CHECK")
INTEGRITY_CHECK=$(envelope assertion add pred-obj string "testCoverage" string "92% (meets minimum 90%)" "$INTEGRITY_CHECK")
INTEGRITY_CHECK=$(envelope assertion add pred-obj known 'date' string `date -Iminutes` "$INTEGRITY_CHECK")
INTEGRITY_CHECK=$(envelope assertion add pred-obj string "assessmentBy" envelope $INTEGRITY_CHECK "$WRAPPED_DEVELOPER_DECLARATION")

| envelope format $INTEGRITY_CHECK
| {
|     {
|         "Code Contribution #PR-123" [
|             "commitHash": "7dd42c1be02cc53f70bfd0021d0aac15bf8e2ad5"
|             "contributor": XID(e90049a2)
|             "description": "Add authentication module"
|             "experience": "5 years Node.js development"
|             "repositoryURL": "https://github.com/example/project"
|         ]
|     } [
|         'signed': Signature
|     ]
| } [
|     "assessmentBy": "ContinuousIntegrationSystem" [
|         "compilationCheck": "Successful"
|         "lintCheck": "Passed - 0 errors, 2 warnings"
|         "testCoverage": "92% (meets minimum 90%)"
|         'date': "2026-08-12T15:00-10:00"
|     ]
| ]
```

This phase is an automated check that verifies that the contribution
compiles, passes linting, and has sufficient test coverage. The
assessment is added to the envelope that the developer created. (The
check is not actually shown, just the reporting.)

### Phase 3: Proofs *(Secrets Verified)*

Cryptographic secrets and other hidden information are verified.

```sh
VERIFIED_CONTRIBUTION=$(envelope subject type string "RepositoryVerifier")
VERIFIED_CONTRIBUTION=$(envelope assertion add pred-obj string "commitSignatureVerified" string "Valid signature from $DEVELOPER_XID" "$VERIFIED_CONTRIBUTION")
VERIFIED_CONTRIBUTION=$(envelope assertion add pred-obj string "declarationSignatureVerified" string "Valid signature from $DEVELOPER_XID" "$VERIFIED_CONTRIBUTION")
VERIFIED_CONTRIBUTION=$(envelope assertion add pred-obj string "xidDocumentVerified" string "Valid" "$VERIFIED_CONTRIBUTION")
VERIFIED_CONTRIBUTION=$(envelope assertion add pred-obj string "verificationMethod" string "SSH ED25519 signature verification" "$VERIFIED_CONTRIBUTION")
VERIFIED_CONTRIBUTION=$(envelope assertion add pred-obj string "verificationBy" envelope $VERIFIED_CONTRIBUTION $INTEGRITY_CHECK)

| envelope format $VERIFIED_CONTRIBUTION
|
| ...
|
|     "verificationBy": "RepositoryVerifier" [
|         "commitSignatureVerified": "Valid signature from ur:xid/tpsplftpsotanshdhdcxwlaegaoevartcmvlinhkmyltmhdwtobeisskwlvllpfnvdlowsvestathdspcmwkoyaylstpsotansgylftanshfhdcxaahklosbpkspasaepspmlfdwmnihcslyrfhngraaehmnoeoxdtdpjysrpypfbtsftansgrhdcxiahyjkfrdkfgpyflzeihdksbsghdosptrnisnechguvyecpyieurvsjsyttkcwkpoycsfncsfglfoycsfptpsotansgtlftansgohdcxgecxatgeasbyjpjetaonoxdmyamylfaevtbyeyhnwffslnaosbttpdttcxltpfpetansgehdcxsaemjsnyrennnndytnaewkfhwfqztlvemtlgfshgnnvojedlonlecybbylkgcsskoybstpsotansgmhdcxamzemdlrjedigubbrtjyfrkeiyprnsendikkierttafpdtdpaehyhppfdsjlchpeftpaioze"
|         "declarationSignatureVerified": "Valid signature from ur:xid/tpsplftpsotanshdhdcxwlaegaoevartcmvlinhkmyltmhdwtobeisskwlvllpfnvdlowsvestathdspcmwkoyaylstpsotansgylftanshfhdcxaahklosbpkspasaepspmlfdwmnihcslyrfhngraaehmnoeoxdtdpjysrpypfbtsftansgrhdcxiahyjkfrdkfgpyflzeihdksbsghdosptrnisnechguvyecpyieurvsjsyttkcwkpoycsfncsfglfoycsfptpsotansgtlftansgohdcxgecxatgeasbyjpjetaonoxdmyamylfaevtbyeyhnwffslnaosbttpdttcxltpfpetansgehdcxsaemjsnyrennnndytnaewkfhwfqztlvemtlgfshgnnvojedlonlecybbylkgcsskoybstpsotansgmhdcxamzemdlrjedigubbrtjyfrkeiyprnsendikkierttafpdtdpaehyhppfdsjlchpeftpaioze"
|         "verificationMethod": "SSH ED25519 signature verification"
|         "xidDocumentVerified": "Valid"
|     ]
|
| ...
```

This phase cryptographically verifies the developer's identity (via
their XID) and the authenticity of their code contribution (via commit
signature). (The verification is not actually shown, just the reporting.)

This data continues to be added to the original envelope, but just the new content is shown here.

### Phase 4: References *(Trust Affirmed)*

Collect trust references from various sources to build a composite trust picture.

```sh
TRUST_REFERENCES=$(envelope subject type ur $MAINTAINER_XID_ID)
TRUST_REFERENCES=$(envelope assertion add pred-obj string "previousContributions" string "27 accepted PRs" "$TRUST_REFERENCES")
TRUST_REFERENCES=$(envelope assertion add pred-obj string "averageCodeQuality" string "4.2/5.0 from peer reviews" "$TRUST_REFERENCES")
TRUST_REFERENCES=$(envelope assertion add pred-obj string "communityStanding" string "Active contributor since 2020" "$TRUST_REFERENCES")
TRUST_REFERENCES=$(envelope assertion add pred-obj string "codeReviewHistory" string "93% approval rate" "$TRUST_REFERENCES")
TRUST_REFERENCES=$(envelope assertion add pred-obj string "referencesCollectedBy" envelope "$TRUST_REFERENCES" $VERIFIED_CONTRIBUTION)

envelope format $TRUST_REFERENCES

|
| ...
|
|     "referencesCollectedBy": XID(1b7fbdaf) [
|         "averageCodeQuality": "4.2/5.0 from peer reviews"
|         "codeReviewHistory": "93% approval rate"
|         "communityStanding": "Active contributor since 2020"
|         "previousContributions": "27 accepted PRs"
|     ]
|
| ...
```

Here, the project maintainer collects references about the developer from past interactions, peer reviews, and community standing.

### Phase 5: Requirements *(Community Compliance)*

The contribution is audited against project requirements including
security standards, code style, documentation, and API compatibility.

```sh
COMPLIANCE_CHECK=$(envelope subject type string "SecurityTeam")
COMPLIANCE_CHECK=$(envelope assertion add pred-obj string "securityReviewResult" string "Passed - No vulnerabilities detected" "$COMPLIANCE_CHECK")
COMPLIANCE_CHECK=$(envelope assertion add pred-obj string "codeStyleCompliance" string "Compliant with project standards" "$COMPLIANCE_CHECK")
COMPLIANCE_CHECK=$(envelope assertion add pred-obj string "documentationCompliance" string "Meets requirements - includes API docs" "$COMPLIANCE_CHECK")
COMPLIANCE_CHECK=$(envelope assertion add pred-obj string "breakingChanges" string "None - API backward compatible" "$COMPLIANCE_CHECK")
COMPLIANCE_CHECK=$(envelope assertion add pred-obj string "complianceAuditedby" envelope $COMPLIANCE_CHECK $TRUST_REFERENCES)

envelope format $COMPLIANCE_CHECK

|
| ...
|
| "complianceAuditedby": "SecurityTeam" [
|         "breakingChanges": "None - API backward compatible"
|         "codeStyleCompliance": "Compliant with project standards"
|         "documentationCompliance": "Meets requirements - includes API docs"
|         "securityReviewResult": "Passed - No vulnerabilities detected"
| ]
|
| ...
```

Here, the security team does a full security review of the contribution.

### Phase 6: Approval *(Risk Calculated)*

Based on all previous phases, the maintainer calculates the risk level
of the interaction, compares it to the trust score, and makes an
approval decision.

```sh
COMPLIANCE_CHECK_WRAPPED=$(envelope subject type wrapped "$COMPLIANCE_CHECK")
RISK_CALCULATION=$(envelope subject type ur $MAINTAINER_XID_ID)
RISK_CALCULATION=$(envelope assertion add pred-obj string "moduleImpact" string "Medium - Core authentication component" "$RISK_CALCULATION")
RISK_CALCULATION=$(envelope assertion add pred-obj string "trustScore" string "0.87" "$RISK_CALCULATION")
RISK_CALCULATION=$(envelope assertion add pred-obj string "riskLevel" string "Low - Sufficient tests and reviews" "$RISK_CALCULATION")
RISK_CALCULATION=$(envelope assertion add pred-obj string "approvalDecision" string "Approved for integration" "$RISK_CALCULATION")
RISK_CALCULATION=$(envelope assertion add pred-obj known 'date' string `date -Iminutes` "$RISK_CALCULATION")
RISK_CALCULATION=$(envelope assertion add pred-obj string "approvalDate" string "2023-11-30T09:15:00Z" "$RISK_CALCULATION")
RISK_CALCULATION=$(envelope subject type wrapped $RISK_CALCULATION)
RISK_CALCULATION=$(envelope sign -s $MAINTAINER_PRVKEYS $RISK_CALCULATION)
RISK_CALCULATION=$(envelope assertion add pred-obj string "approvedBy" envelope $RISK_CALCULATION $COMPLIANCE_CHECK_WRAPPED)

envelope format $RISK_CALCULATION

|
| ...
| 
|    "approvedBy": {
|         XID(1b7fbdaf) [
|             "approvalDate": "2023-11-30T09:15:00Z"
|             "approvalDecision": "Approved for integration"
|             "moduleImpact": "Medium - Core authentication component"
|             "riskLevel": "Low - Sufficient tests and reviews"
|             "trustScore": "0.87"
|             'date': "2026-08-12T15:16-10:00"
|         ]
|     } [
|         'signed': Signature
|     ]
|
| ...
```

The repo maintainer offers initial agreement on the contribution, but a threshold of additional parties is required.

### Phase 7: Agreement *(Threshold Endorsed)* [optional]

Additional approvals are obtained to reach a required threshold.

```sh
THRESHOLD_APPROVAL_1=$(envelope subject type ur "$SENIOR_DEV_XID_ID")
THRESHOLD_APPROVAL_1=$(envelope assertion add pred-obj string "approval1Date" string "2023-11-30T10:20:00Z" "$THRESHOLD_APPROVAL_1")
THRESHOLD_APPROVAL_1=$(envelope subject type wrapped $THRESHOLD_APPROVAL_1)
THRESHOLD_APPROVAL_1=$(envelope sign -s $SENIOR_DEV_PRVKEYS $THRESHOLD_APPROVAL_1)
THRESHOLD_APPROVAL_2=$(envelope subject type ur "$SECURITY_TEAM_XID_ID")
THRESHOLD_APPROVAL_2=$(envelope assertion add pred-obj string "approval2Date" string "2023-11-30T11:45:00Z" "$THRESHOLD_APPROVAL_2")
THRESHOLD_APPROVAL_2=$(envelope subject type wrapped $THRESHOLD_APPROVAL_2)
THRESHOLD_APPROVAL_2=$(envelope sign -s $SECURITY_TEAM_PRVKEYS $THRESHOLD_APPROVAL_2)
THRESHOLD_APPROVAL=$(envelope subject type envelope "$RISK_CALCULATION")
THRESHOLD_APPROVAL=$(envelope assertion add pred-obj string "additionalApproval1" envelope $THRESHOLD_APPROVAL_1 $THRESHOLD_APPROVAL)
THRESHOLD_APPROVAL=$(envelope assertion add pred-obj string "additionalApproval2" envelope $THRESHOLD_APPROVAL_2 $THRESHOLD_APPROVAL)
THRESHOLD_APPROVAL=$(envelope assertion add pred-obj string "thresholdReached" string "Yes - All required approvals obtained" "$THRESHOLD_APPROVAL")

envelope format $THRESHOLD_APPROVAL

| 
| ...
| 
|     "additionalApproval1": {
|         XID(f10659dd) [
|             "approval1Date": "2023-11-30T10:20:00Z"
|         ]
|     } [
|         'signed': Signature
|     ]
|     "additionalApproval2": {
|         XID(88d3b98d) [
|             "approval2Date": "2023-11-30T11:45:00Z"
|         ]
|     } [
|         'signed': Signature
|     ]
| 
| ...
| 
|     "thresholdReached": "Yes - All required approvals obtained"
| 
| ...
|
```

Here, the additional appprovals come from a senior developer and the security team.

### Phase 8: Fulfillment *(Interaction Finalized)*

The interaction is finalized according to the agreed-upon terms.

```sh
INTERACTION_FULFILLED=$(envelope subject type string "DeploymentSystem")
INTERACTION_FULFILLED=$(envelope assertion add pred-obj string "mergeStatus" string "Merged to main branch" "$INTERACTION_FULFILLED")
INTERACTION_FULFILLED=$(envelope assertion add pred-obj string "mergeCommit" string "cf37d69cea18b344d2c9e8aacc430b1d9ac0a74a" "$INTERACTION_FULFILLED")
INTERACTION_FULFILLED=$(envelope assertion add pred-obj string "deploymentStatus" string "Deployed to staging environment" "$INTERACTION_FULFILLED")
INTERACTION_FULFILLED=$(envelope assertion add pred-obj string "fulfillmentDate" string "2023-12-01T08:30:00Z" "$INTERACTION_FULFILLED")
WRAPPED_THRESHOLD_APPROVAL=$(envelope subject type wrapped $THRESHOLD_APPROVAL)
INTERACTION_FULFILLED=$(envelope assertion add pred-obj string "finalizedBy" envelope $INTERACTION_FULFILLED $WRAPPED_THRESHOLD_APPROVAL)

envelope format $INTERACTION_FULFILLED

| 
| ...
| 
|     "finalizedBy": "DeploymentSystem" [
|         "deploymentStatus": "Deployed to staging environment"
|         "fulfillmentDate": "2023-12-01T08:30:00Z"
|         "mergeCommit": "cf37d69cea18b344d2c9e8aacc430b1d9ac0a74a"
|         "mergeStatus": "Merged to main branch"
|     ]
| 
| ...
| 
```

The code contribution is merged and deployed, finalizing the interaction.

### Phase 9: Escalation *(Independently Inspected)* [optional]

A third party inspects and potentially endorses the interaction.

```sh
INDEPENDENT_INSPECTION=$(envelope subject type string "SecureCode Auditors")
INDEPENDENT_INSPECTION=$(envelope assertion add pred-obj string "auditDate" string "2023-12-05T14:00:00Z" "$INDEPENDENT_INSPECTION")
INDEPENDENT_INSPECTION=$(envelope assertion add pred-obj string "auditScope" string "Authentication module security review" "$INDEPENDENT_INSPECTION")
INDEPENDENT_INSPECTION=$(envelope assertion add pred-obj string "auditResult" string "Passed - No critical or high vulnerabilities" "$INDEPENDENT_INSPECTION")
INDEPENDENT_INSPECTION=$(envelope assertion add pred-obj string "auditReport" string "sha256:7d8f9a234c9b67531d5f8b3a1b5d9c7e9a6b7c8d" "$INDEPENDENT_INSPECTION")
INDEPENDENT_INSPECTION=$(envelope assertion add pred-obj string "auditedBy" envelope $INDEPENDENT_INSPECTION $INTERACTION_FULFILLED)

envelope format $INDEPENDENT_INSPECTION

| 
| ...
| 
|     "auditedBy": "SecureCode Auditors" [
|         "auditDate": "2023-12-05T14:00:00Z"
|         "auditReport": "sha256:7d8f9a234c9b67531d5f8b3a1b5d9c7e9a6b7c8d"
|         "auditResult": "Passed - No critical or high vulnerabilities"
|         "auditScope": "Authentication module security review"
|     ]
| 
| ...
| 
```

An independent security firm audits the deployed code, providing additional assurance about its quality and security.

### Phase 10: Dispute *(Independently Arbitrated)* [optional]

If something goes wrong, a dispute process resolves issues through independent arbitration.

```sh
DISPUTE_RESOLUTION=$(envelope subject type wrapped "$INDEPENDENT_INSPECTION")
DISPUTE_RESOLUTION=$(envelope assertion add pred-obj string "disputeRaised" string "Security vulnerability CVE-2023-98765" "$DISPUTE_RESOLUTION")
DISPUTE_RESOLUTION=$(envelope assertion add pred-obj string "disputeDate" string "2023-12-10T09:30:00Z" "$DISPUTE_RESOLUTION")
DISPUTE_RESOLUTION=$(envelope assertion add pred-obj string "raisedBy" string "SecurityResearcher" "$DISPUTE_RESOLUTION")
DISPUTE_RESOLUTION=$(envelope assertion add pred-obj string "vulnerabilityDetails" string "Authentication bypass in edge case" "$DISPUTE_RESOLUTION")
DISPUTE_RESOLUTION=$(envelope assertion add pred-obj string "arbitrator" string "OpenSourceSecurityCommittee" "$DISPUTE_RESOLUTION")
DISPUTE_RESOLUTION=$(envelope assertion add pred-obj string "resolution" string "Developer acknowledged and fixed issue within 24 hours" "$DISPUTE_RESOLUTION")
DISPUTE_RESOLUTION=$(envelope assertion add pred-obj string "trustImpact" string "Minimal - Prompt and transparent response" "$DISPUTE_RESOLUTION")
```

When a vulnerability is discovered, the dispute resolution process
documents the issue, assigns responsibility, and tracks the
resolution, including the impact on trust.

This is what the full envelope depicting the progressive trust looks like at the end:

```sh
| {
|     {
|         {
|             {
|                 {
|                     "Code Contribution #PR-123" [
|                         "commitHash": "7dd42c1be02cc53f70bfd0021d0aac15bf8e2ad5"
|                         "contributor": XID(e90049a2)
|                         "description": "Add authentication module"
|                         "experience": "5 years Node.js development"
|                         "repositoryURL": "https://github.com/example/project"
|                     ]
|                 } [
|                     'signed': Signature
|                 ]
|             } [
|                 "assessmentBy": "ContinuousIntegrationSystem" [
|                     "compilationCheck": "Successful"
|                     "lintCheck": "Passed - 0 errors, 2 warnings"
|                     "testCoverage": "92% (meets minimum 90%)"
|                     'date': "2026-08-12T15:00-10:00"
|                 ]
|                 "complianceAuditedby": "SecurityTeam" [
|                     "breakingChanges": "None - API backward compatible"
|                     "codeStyleCompliance": "Compliant with project standards"
|                     "documentationCompliance": "Meets requirements - includes API docs"
|                     "securityReviewResult": "Passed - No vulnerabilities detected"
|                 ]
|                 "referencesCollectedBy": XID(1b7fbdaf) [
|                     "averageCodeQuality": "4.2/5.0 from peer reviews"
|                     "codeReviewHistory": "93% approval rate"
|                     "communityStanding": "Active contributor since 2020"
|                     "previousContributions": "27 accepted PRs"
|                 ]
|                 "verificationBy": "RepositoryVerifier" [
|                     "commitSignatureVerified": "Valid signature from ur:xid/tpsplftpsotanshdhdcxwlaegaoevartcmvlinhkmyltmhdwtobeisskwlvllpfnvdlowsvestathdspcmwkoyaylstpsotansgylftanshfhdcxaahklosbpkspasaepspmlfdwmnihcslyrfhngraaehmnoeoxdtdpjysrpypfbtsftansgrhdcxiahyjkfrdkfgpyflzeihdksbsghdosptrnisnechguvyecpyieurvsjsyttkcwkpoycsfncsfglfoycsfptpsotansgtlftansgohdcxgecxatgeasbyjpjetaonoxdmyamylfaevtbyeyhnwffslnaosbttpdttcxltpfpetansgehdcxsaemjsnyrennnndytnaewkfhwfqztlvemtlgfshgnnvojedlonlecybbylkgcsskoybstpsotansgmhdcxamzemdlrjedigubbrtjyfrkeiyprnsendikkierttafpdtdpaehyhppfdsjlchpeftpaioze"
|                     "declarationSignatureVerified": "Valid signature from ur:xid/tpsplftpsotanshdhdcxwlaegaoevartcmvlinhkmyltmhdwtobeisskwlvllpfnvdlowsvestathdspcmwkoyaylstpsotansgylftanshfhdcxaahklosbpkspasaepspmlfdwmnihcslyrfhngraaehmnoeoxdtdpjysrpypfbtsftansgrhdcxiahyjkfrdkfgpyflzeihdksbsghdosptrnisnechguvyecpyieurvsjsyttkcwkpoycsfncsfglfoycsfptpsotansgtlftansgohdcxgecxatgeasbyjpjetaonoxdmyamylfaevtbyeyhnwffslnaosbttpdttcxltpfpetansgehdcxsaemjsnyrennnndytnaewkfhwfqztlvemtlgfshgnnvojedlonlecybbylkgcsskoybstpsotansgmhdcxamzemdlrjedigubbrtjyfrkeiyprnsendikkierttafpdtdpaehyhppfdsjlchpeftpaioze"
|                     "verificationMethod": "SSH ED25519 signature verification"
|                     "xidDocumentVerified": "Valid"
|                 ]
|             ]
|         } [
|             "additionalApproval1": {
|                 XID(f10659dd) [
|                     "approval1Date": "2023-11-30T10:20:00Z"
|                 ]
|             } [
|                 'signed': Signature
|             ]
|             "additionalApproval2": {
|                 XID(88d3b98d) [
|                     "approval2Date": "2023-11-30T11:45:00Z"
|                 ]
|             } [
|                 'signed': Signature
|             ]
|             "approvedBy": {
|                 XID(1b7fbdaf) [
|                     "approvalDate": "2023-11-30T09:15:00Z"
|                     "approvalDecision": "Approved for integration"
|                     "moduleImpact": "Medium - Core authentication component"
|                     "riskLevel": "Low - Sufficient tests and reviews"
|                     "trustScore": "0.87"
|                     'date': "2026-08-12T15:16-10:00"
|                 ]
|             } [
|                 'signed': Signature
|             ]
|             "thresholdReached": "Yes - All required approvals obtained"
|         ]
|     } [
|         "auditedBy": "SecureCode Auditors" [
|             "auditDate": "2023-12-05T14:00:00Z"
|             "auditReport": "sha256:7d8f9a234c9b67531d5f8b3a1b5d9c7e9a6b7c8d"
|             "auditResult": "Passed - No critical or high vulnerabilities"
|             "auditScope": "Authentication module security review"
|         ]
|         "finalizedBy": "DeploymentSystem" [
|             "deploymentStatus": "Deployed to staging environment"
|             "fulfillmentDate": "2023-12-01T08:30:00Z"
|             "mergeCommit": "cf37d69cea18b344d2c9e8aacc430b1d9ac0a74a"
|             "mergeStatus": "Merged to main branch"
|         ]
|     ]
| } [
|     "arbitrator": "OpenSourceSecurityCommittee"
|     "disputeDate": "2023-12-10T09:30:00Z"
|     "disputeRaised": "Security vulnerability CVE-2023-98765"
|     "raisedBy": "SecurityResearcher"
|     "resolution": "Developer acknowledged and fixed issue within 24 hours"
|     "trustImpact": "Minimal - Prompt and transparent response"
|     "vulnerabilityDetails": "Authentication bypass in edge case"
| ]
```

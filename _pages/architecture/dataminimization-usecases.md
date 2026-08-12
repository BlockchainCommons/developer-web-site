---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: "Data Minimization Use Cases"
tagline: "Revealing Only What's Needed"
hide_description: true
classes:
  - wide
permalink: /architecture/data-minimization/use-cases/
sidebar:
  nav:
    - dataminimization
    - archdesign
    - architecture
---

Data minimization principles apply to many scenarios. Major use cases include:

1. **Age Verification**: Proving someone is over 21 without revealing exact birthdate.
2. **Professional Credentials**: Demonstrating qualifications without exposing personal history.
3. **Financial Verification**: Proving financial capacity without revealing account details.
4. **Identity Authentication**: Verifying identity without exposing the full identity document.
5. **Collaboration**: Sharing relevant expertise without unnecessary personal disclosure.

## General Use Cases

Many use cases fall into one of two general categories: contextual
information sharing and progressive trust.

### Contextual Information Sharing

Data minimization allows creating different views of the same identity
for different contexts:

1. **Public Context** - Share minimal, non-sensitive information
   - Basic identifiers and public credentials
   - General domain expertise
   - No personal details or private information
2. **Professional Context** - Share relevant professional information
   - Domain-specific credentials
   - Relevant experience and skills
   - Professional history without personal details
3. **Trusted Context** - Share more comprehensive information
   - Detailed professional background
   - Specific methodologies and approaches
   - Limited personal context relevant to the relationship

This contextual approach mirrors how we naturally share different
levels of information in different social contexts in the physical
world.

### Progressive Trust Development

Data minimization also enables [progressive
trust](/archhitecture/progressive-trust/)&mdash;revealing more
information as relationships develop. The following example shows how
[XIDs](/xid/) might be used to offer increasing amounts of information
over time.

1. **Initial Contact**: Share only basic information.
   ```
   "BRadvoc8" [
      "name": "BRadvoc8"
      "publicKeys": ur:crypto-pubkeys/hdcx...
      ELIDED (5)
   ]
   ```

2. **Building Relationship**: Reveal professional information.
   ```
   "BRadvoc8" [
      "name": "BRadvoc8"
      "publicKeys": ur:crypto-pubkeys/hdcx...
      "domain": "Distributed Systems & Security"
      "experienceLevel": "8 years professional practice"
      ELIDED (3)
   ]
   ```

3. **Growing Trust**: Share more specific professional details.
   ```
   "BRadvoc8" [
      "name": "BRadvoc8"
      "publicKeys": ur:crypto-pubkeys/hdcx...
      "domain": "Distributed Systems & Security"
      "experienceLevel": "8 years professional practice"
      "skillAreas": "API security, Zero-knowledge systems, Protocol design"
      ELIDED (2)
   ]
   ```

4. **Established Trust**: Reveal detailed perspectives and methods.
   ```
   "BRadvoc8" [
      "name": "BRadvoc8"
      "publicKeys": ur:crypto-pubkeys/hdcx...
      "domain": "Distributed Systems & Security"
      "experienceLevel": "8 years professional practice"
      "skillAreas": "API security, Zero-knowledge systems, Protocol design"
      "potentialBias": "Particular focus on privacy-preserving systems"
      "methodologicalApproach": "Security-first, user-focused development"
   ]
   ```

This staged approach allows relationships to develop naturally, with
information sharing matching the level of established trust&mdash;just
as we share different levels of personal information at different
stages of relationships in the physical world.

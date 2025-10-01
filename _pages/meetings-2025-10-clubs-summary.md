---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "Gordian Clubs"
title: "Gordian Developer Meeting: October 2025 Community Discussion Summary"
hide_description: true
classes:
  - wide
permalink: /meetings/2025-10-clubs/summary/
sidebar:
  nav: meetings
---

## Community Discussion Highlights

### Attenuation and Caveats

**The question:** Can Gordian Clubs support caveats like expiration times?

**The answer:**
- Pure autonomous mode: No external event conditionals (deterministic only)
- With oracles: Yes, but loses autonomy
  - Timestamp servers
  - Blockchain time-locks (valid until block N is spent)
  - Prediction markets (conditional on market resolution)
- Verifiable Delay Functions (VDFs): "Valid when computation complete"
  - Not clock-based but computation-based
  - 10 days of VDF computation = proof of time passage
  - Hardware-aware, anti-parallel proof of work

**Risk-adaptive access control example:** "You can read this document unless the US is at war with Canada" - requires external oracle.

### Revocation Mechanisms

**Current approach:**
- **Read revocation:** Don't include party in next edition's permits
- **Write revocation:** Don't include party in FROST signing quorum
- **SSKR revocation:** Don't distribute shares to party
- Once revealed, keys/shares can't be "un-revealed" (mathematical permanence)

**Future possibilities:**
- External oracles for revocation lists (loses autonomy)
- Time-locked transactions on blockchains
- Decentralized oracle networks
- Integration with Ryan Grant's DID method using time-locks

### CRDTs and Distributed Systems

**Opportunity identified:** Conflict-Free Replicated Data Types could be excellent fit for serverless Gordian Clubs.

**Wolf's response:** Long-time interest in CRDTs with Gordian Envelopes as carrier. No experimentation yet but eager to collaborate.

**Potential:** Offline-first, eventually consistent replication without central coordination aligns perfectly with autonomous object philosophy.

### UX and Communication (Gordon Mohr)

**Challenges:**
- Command-line tools are proof-of-concept, need better UX for broader adoption
- Analogies help communication:
  - Clubs state like proof-of-stake blockchain
  - Edition progression like Git versioning
  - Need to visualize objects for non-CLI users

**Existing UX work:**
- Gordian Seed Tool (iOS app) shows UX-driven approach
- LifeHash, byte words, byte emojis for recognition
- Object identifier blocks for standardized display

**Need:** Better demos showing how it looks/feels beyond command line.

### Use Cases and Human Rights

**Christopher's priorities:**
- **Journalist/activist protection:** Provenance marks appear as random numbers, hard to identify
- **AMIRA engagement model:** Protect developers creating activism software
- **Infrastructure resilience:** Work during internet outages, infrastructure failures
- **Long-term archival:** Outlive companies and governments

**Real-world concern:** Computer scientist detained at Istanbul airport (2025) highlights risks for developers in adversarial environments.

**Funding landscape:** Government funding for decentralized identity projects declining, need community support and corporate partnerships.

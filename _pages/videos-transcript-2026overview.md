---
cover: true
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/qr-background.jpg
  og_image: /assets/images/bc-card.jpg
title: 2026 Tech Overview Video Transcript
hide_description: false
classes:
  - wide
permalink: /videos/transcripts/overview-2026/
sidebar:
  nav:
    - mainside
    - mainstacks
---

<a href="https://youtu.be/f7MFW8RfcOE" style="float: right"><img src="https://img.youtube.com/vi/f7MFW8RfcOE/0.jpg" width=300px style="border: 1px solid blue"></a>
**Wolf McNally:**

Hi, I'm Wolf McNally, lead researcher for Blockchain Commons.

Blockchain Commons is a not-for-profit social benefit corporation that builds open, interoperable, and secure digital infrastructure. All of the work we do is open source and available for the public benefit. Our technologies are all driven by the four Gordian principles: independence, privacy, resilience, and openness.

Here, at the start of 2026, I want to take you through a quick tour of some of the key technologies we've been developing. Some of these have been around for a few years now, and are already used by third-party developers. Others are new and are being reviewed by our community. But they all fit together and build on each other. They form an architecture for managing digital assets, identities, and private communications that doesn't depend on any single company or platform. We'll start at the bottom of the stack, with how we make digital objects recognizable to humans, and work our way up through encoding, transport, smart documents, decentralized identity, and eventually into some things that demonstrate the promise of our decentralized future, autonomous groups, and untraceable multi-party coordination and signing.

## Data Identification: LifeHash, Bytewords, and OIB

Cryptographic keys, seeds, addresses, they're all long strings of characters. You can't tell one from another by looking. LifeHash fixes this by turning any piece of data into a unique visual icon. Here's how. It takes a SHA-256 hash of the input, uses that as a seed for the famous cellular automaton known as Conway's Game of Life, runs it on a grid until it stabilizes, then applies deterministic symmetry and color palettes. Same input, same image every time. So if two icons look the same, the underlying data matches. But if the underlying data is different, even by a single bit, the icon will look completely different.

Visual recognition is great, but sometimes you need to read binary data out loud or type it by hand. Try reading a string of hexadecimal over the phone. It's not easy to get right. Bytewords is a dictionary of 256 four-letter English words that are carefully chosen to be very difficult to confuse, whether spoken, written, or even condensed to just their first and last letters. Bytewords also adds four more words at the end representing a CRC-32 checksum used to catch errors. Bytewords is the human-readable encoding layer underneath many of the other technologies I'll be talking about.

So we have LifeHash for visual identification and Bytewords for readable encoding. Object Information Blocks put them together into a standardized UI pattern. Think of it as a little ID card for a digital object. An OIB bundles a LifeHash icon, a type indicator and an abbreviated digest, and an optional human-assigned name into one compact display. The point is to make it hard to confuse one key or seed with another.

When all your cryptographic seeds, keys, and digital documents look the same, it's easy to become data snowblind. When they each have a distinct visual identity, it's much easier to avoid costly mistakes.

## Data Encoding: Uniform Resources

Identifying data is one thing, moving it between applications is another, especially across air gaps with no network. You're familiar with URLs that point at information somewhere on the web. But Uniform Resources are the data themselves. They encode structured binary data as plain text strings using Bytewords in the format `ur:type` and then a string of Bytewords.

Every UR is self-describing. The type tag tells you what's inside. Because they just use letters and a couple other symbols, they're optimized to be very efficient when encoded as QR codes, and they work just as well with NFC, deep links, or copy and paste.

Before URs, every wallet developer invented their own encoding. Interoperability was a mess. URs fixed that, and they've been adopted by numerous hardware wallets, mobile apps, and desktop applications as a common transmission format for air-gapped communication.

A single QR code can only hold so much data. A partially signed Bitcoin transaction, for instance, usually won't fit. Multipart URs split a large payload into a sequence of fragments displayed as animated QR codes. Now here's the interesting part. They use an incredible algorithm called Fountain Codes, a technique for rateless encoding, which just means you can start scanning at any point in the animation and you don't need every frame. The math lets you reconstruct the full payload from any sufficient subset. Just point your camera and wait for the progress bar to fill. Missed frames or bad scans don't break the flow or require you to start over. It's a huge improvement in reliability and user experience for air-gapped communication of large data.

## Cryptographic Protections: SSKR

Backing up a cryptographic seed is a dilemma. One backup, single point of failure. Lose it, lose everything. Multiple copies multiply your attack surface. And if you password encrypt your seed, how do you keep that password safe?

Sharded Secret Key Reconstruction, SSKR, uses another really cool algorithm called Shamir's Secret Sharing to split a piece of data into shares where any threshold number can reconstruct the original, but fewer than the threshold reveals nothing. Zero information leakage below the threshold. SSKR goes further than basic Shamir with a two-level group structure. You might require two of three shares from your family and one of two from your lawyers.

## Reference Apps: Gordian Seed Tool

Gordian Seed Tool is our reference iPhone app, and it's where a lot of these technologies show up in practice. It generates cryptographic seeds with true randomness, displays them with LifeHash icons and OIBs, backs them up with SSKR shards, and handles PSBT signing for air-gapped Bitcoin transactions, all through URs and animated QR codes. It also supports iCloud encrypted backup.

It's not a wallet, and it doesn't hold funds, but it's a dedicated seed vault, and out of seeds grow all your keys. With Seed Tool, everything stays under your control, your backups are resilient, and it works with other compatible tools out of the box.

For developers who prefer the terminal, Seed Tool CLI brings many of the same capabilities to the command line. Generate seeds from various entropy sources, convert between hex, BIP-39, Bytewords, and URs, shard with SSKR, pipe output into other Blockchain Commons CLI tools like Envelope and dCBOR. It's a practical utility for scripting and testing and a reference for how seed operations work across our stack.

## Data Encoding: dCBOR

Now let's talk about data encoding. You can think of CBOR, Concise Binary Object Representation, as basically like binary JSON. Compact, self-describing, good for constrained environments. But standard CBOR has a problem. The same logical data can be encoded multiple ways. If two people encode and then sign the same data, this optionality can cause the signatures to be invalid against the other signed object.

Deterministic CBOR, or dCBOR, is a strict set of narrowing rules, also called a profile of CBOR, that eliminates encoding ambiguity. Integers must use their shortest representation. Floating-point numbers that can be integers must become integers. Strings must be Unicode Normalization Form C. Map keys are sorted. This means that two implementations encoding the same data will always produce identical bytes, and this determinism is what makes distributed consensus protocols like Bitcoin possible. We've been developing dCBOR as an IETF Internet Draft, and almost everything I'm going to talk about from here on is built on top of it, so it's a critical piece of our stack.

The dCBOR command-line tool lets you inspect dCBOR data up close. Parse, validate, transform between hex, binary, and human-readable diagnostic notation. It enforces deterministic rules on every input and catches violations immediately. It also supports what we call dCBOR pattern expressions. They work like regular expressions for strings, but for the tree-like structure of dCBOR. Match patterns like number, text, or structural patterns like an array starting with a specific value. Once you dive into pattern expressions, they're hard to live without.

## Gordian Envelope

This is where the pieces start fitting together and things get really exciting. Gordian Envelope is a smart document format built on dCBOR. It organizes data into recursive semantic triples, subject predicate object, forming a tree where every node is independently hashed. That Merkle-like structure enables a number of important capabilities.

But something unique is its fine-grained ability to support holder-based elision. You can redact any part of an envelope and the hash tree stays intact. A signature over the full document remains valid after selective disclosure. You can prove hidden content exists without revealing it. You can progressively reveal information as trust builds.

Envelopes also handle encryption, signing, compression, and access control through a permit system that works with symmetric keys, public keys, passwords, or SSKR shares. Most of the higher-level protocols in our architecture are built on top of Gordian Envelope. Envelope is the subject of our other major IETF Internet Draft, and implementations are available in several programming languages.

The Envelope command-line tool handles creation, signing, encryption, elision, and inspection of envelopes from the terminal. You can pipe it together with Seed Tool and dCBOR for complex workflows.

Envelope pattern expressions build on dCBOR pattern expressions. They're a query syntax for tree structures, the way regular expressions work for strings, pattern expressions work for envelopes. Search for assertions by predicate, extract values, filter whole envelope trees.

Gordian envelopes use predicates like "Alice knows Bob" or "the document is signed by a given signature." For predicates like "knows" and "signed," it's useful to have a compact, unambiguous representation. Known Values are integers exclusively assigned to common concepts, `isA`, `note`, `hasRecipient`, `dereferenceVia`, and so on, registered in a public registry. They replace verbose strings.

The Blockchain Commons Known Value Registry contains core assignments, standard ontologies, and community additions, so independent implementations can agree on what a predicate means without having to coordinate beforehand. Both dCBOR and Envelope CLI tools handle Known Values natively.

## Cryptographic Protections: Provenance Marks

Here's a question that keeps coming up. In an age of AI-generated content and deepfakes, how do you prove a sequence of publications came from a claimed author in a claimed order? Provenance Marks use a forward-commitment hash chain. Each mark cryptographically commits to the next mark before it's even generated or published. That creates a chain where insertion, deletion, or reordering is detectable after the fact. An attacker can't forge a mark without knowing future commitments. The chain is publicly verifiable. No trusted timestamps needed, no witnesses, no blockchain.

Provenance Marks come in several sizes, from 16 to 106 bytes, yielding good security for everyday creators up to industrial and governmental standards, and can be referenced by using only four Bytewords. They can also include any additional metadata such as Gordian envelopes that are baked into the mark. Provenance Marks complement digital signatures and provide traceability back to the source.

## Secure Data Transport: GSTP and ESC

Gordian envelopes structure data. The Gordian Sealed Transaction Protocol, GSTP, moves envelopes securely between parties. It wraps envelope payloads in request and response pairs with encryption, signatures, and state management. The important thing, it's transport-agnostic. Internet, Bluetooth, NFC, Tor, QR codes, or even a USB stick carried across the room. GSTP doesn't care how the bytes travel. It turns any channel into a cryptographically secured pipe.

That matters because the same protocol needs to work for air-gapped hardware wallets and for networked services.

Even though Encrypted State Continuations are a feature of GSTP, they deserve their own special mention. A lot of protocols need to keep track of session state across multiple exchanges. Storing that state on the server creates security risk and scaling headaches.

You can think of Encrypted State Continuations like self-addressed envelopes that you send to yourself and that only you can open and read. In a GSTP message, the server encrypts its own private session state and sends it to the client as a black box. All the client can do with it is return the black box to the server. The server receives the ESC, decrypts it, processes the request, re-encrypts the updated state, and returns it. The client never sees the plain text. The server never stores anything. Completely stateless on the server side, which is exactly what you want for constrained devices and time-separated communications. ESCs can also be used by any protocol participant, client, server, or peer-to-peer, to securely offload state without trusting anyone else with it.

## Self Sovereign Identity: XIDs and Gordian Clubs

Decentralized identity needs identifiers that are stable, self-certifying, and don't depend on a registry or blockchain. An XID, Extensible Identifier, is a 32-byte hash derived from an inception key, the first public key associated with the identity. Because it comes from the key itself rather than from some authority, it's self-certifying from the start. An XID resolves to an XID document, a Gordian envelope that lists current keys, permissions, endpoints, and delegation relationships. You can rotate keys without changing the XID. And because the document is an envelope, you can redact parts of it while preserving cryptographic verification. Multi-key, multi-party control structures are planned, but basic XID documents have already been adopted by commercial developers.

A Gordian Club is an autonomous cryptographic object, a self-contained, encrypted publication that you can distribute, update over time, and restrict to authorized members with no server or infrastructure required. Clubs are Gordian envelopes with permits, XID-based, public key, SSKR shares controlling who can read each edition and who can create new editions. Provenance Marks chain the editions together. You can send a club edition over Signal, mail it on a USB drive, print it as a QR code, post it to a bulletin board. The cryptographic protection is in the object, not the channel. Communities organize through cryptography alone, no permission needed, no third party to trust or to worry about disappearing.

## Secure Data Transport: Hubert and FROST

Gordian Clubs can travel any way you like, but for automation you need infrastructure. Hubert is a dead-drop protocol. It turns distributed storage networks like the BitTorrent mainline distributed hash table and IPFS, the InterPlanetary File System, into a mesh of sealed rendezvous points for asynchronous anonymous communication. Messages go to storage backends: BitTorrent for small messages under a kilobyte, IPFS for larger payloads up to 10 megabytes, at locations derived from ARIDs. Only the holder of an ARID can retrieve its message. Write-once semantics ensure immutability. By default, the storage points in the DHT or IPFS are ephemeral, so the messages themselves eventually self-destruct. Everything is encrypted with GSTP, so the storage network itself can't read the payloads, and to anyone monitoring the traffic, all of Hubert's messages look like random white noise. Hubert lets multi-party protocols work without servers that can be taken offline, without persistent connections, and without anyone in the middle who can eavesdrop or censor.

FROST, Flexible Round-Optimized Schnorr Threshold Signatures, is another extremely cool algorithm that Blockchain Commons has adopted as critical to our decentralized and autonomous future. FROST lets a group of signers collaboratively produce a valid Schnorr signature. Any threshold of participants can sign. The full private key is never reconstructed. The hard part has always been coordination. FROST requires multiple rounds of message exchange, and any central coordinator becomes a point for surveillance and censorship.

Running FROST over Hubert changes that. Signers coordinate through dead drops on decentralized storage. No participant needs to know another participant's identity or network address. No observer can tell who's participating.

The FROST-Hubert CLI demonstrates the full flow, provisioning participants with XID documents, distributed key generation, and threshold signatures over Gordian envelopes.

## Secure Data Transport: Garner

Sometimes you need to expose a service but hide its physical location. Garner spins up a Tor onion service that serves static files, prints the .onion URL when it's reachable, and emits the service's public key in UR form so clients can derive the onion host from the key.

That same key can be used to sign XID documents, making it easy to tie a service's identity to its network presence. It's part of our TorGap architecture, privacy-preserving gaps between connected services using Tor. A clean way to build anonymous partitions between components, and it fits naturally into the rest of our technology stack.

## Information Repositories

If you want to see any of these technologies demonstrated and discussed, we have a YouTube channel with deep dives, walkthroughs, and recordings of every Gordian Developer Meeting.

All our specifications are published as BCR Papers in our research repository on GitHub. Formal definitions, encoding rules, test vectors, design rationales. If you're implementing any part of the stack, start there.

We hold a Gordian Developer Meeting on the first Wednesday of each month, usually at 10:00 AM Pacific. Wallet developers, cryptographers, spec authors, and digital privacy and sovereignty advocates show up to present new work, demo implementations, and work through interoperability questions. In recent months, we've covered post-quantum cryptography, Provenance Marks, interoperability testing, a live FROST signing demo using Hubert, and the first working proof of concept for Gordian Clubs.

If you're building on any of this, or just want to keep up, join the Signal group, the GitHub Discussions forum, or the announcement mailing list.

Everything I've described is open source. These aren't just white papers, they're working tools you can download and use right now.

We think people should be able to control their own digital identity and assets without asking anyone's permission. And that's what we're building toward. Thanks for watching.

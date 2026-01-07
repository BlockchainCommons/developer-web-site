---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Meeting: Post Quantum Cryptography (2025) Transcript"
hide_description: true
classes:
  - wide
permalink: /meetings/2025-03-pqc/transcript/
redirect_from:
  - /meeting/2025-pqc/transcript/
sidebar:
  nav: meetings
---

_The following raw transcript was automatically generated._

---

So welcome everyone. Thank you for coming to our Gordian meeting on March 5th, 2025.

First off, what is Blockchain Commons? We are a community interested in the self-sovereign control of digital assets.

We bring together stakeholders to collaboratively develop interoperable infrastructure.

We design decentralized solutions where everyone wins.

And we are a neutral not-for-profit to enable people to control their own digital destiny.

Today we're going to be featuring a special presentation with one of our partners, Foundation Devices.

Please, we need sponsors to be able to continue our work.

You can become a sponsor through GitHub or you can mail me at team@blockchaincommons.com.

Today's topics are going to be about quantum computing,

Ken's presentation on QuantumLink from Foundation Devices,

and we're going to be talking a little bit about the underlying libraries that are being used in our Gordian stack for post-quantum cryptography,

and then a brief update on the Zcash ZWIFT project.

So first off, let's talk a little bit about quantum computing.

Basically, quantum computers take advantage of the superposition and entanglement of very small particles.

As such, they can do some computing tasks faster than legacy, you know, silicon technology.

In particular, there was a 1994 paper that described how RSA could be broken by quantum computing,

and thus in subsequent developments, ECDSA, ECDH, and DSA are considered insecure given functional quantum computing.

However, not everything is as vulnerable.

AES-256, SHA-2, SHA-3, etc. all function but at reduced levels, but not necessarily to an insecure level.

Quantum computers do really exist.

This isn't science fiction, but there are some limitations.

So Google right now has a chip called Willow with 105 physical qubits,

but due to error correction, this in fact is just a handful of logical qubits,

and we need thousands of logical qubits to break algorithms.

Microsoft claims a new path to get to a million physical qubits in a recent announcement,

which again would be reduced to a much smaller number of logical qubits,

and we don't really know what the right answers are, because ultimately these are laboratory experiments.

Practical crypto-attests by major funded, say, sovereign governments are still probably five to ten years off,

but we really don't know.

Thus, in order to be proactive, we need post-quantum cryptography.

Basically, the mathematicians are well ahead of the theoretical physicists building these very complex devices.

But right now, you really only want to use post-quantum cryptography if you need protection for five to ten years.

So that's what we're going to be focusing on in this talk.

So Foundation recently announced Passport Prime, a personal security system,

which like their previous products stores seeds, in particular around Bitcoin,

but now it can store 2FA codes, security keys, encrypted files, etc.

But there's a lot going on under the hood.

So Passport Prime is moving toward a post-quantum reality, and that's where we wanted to introduce Ken.

So Ken is CTO of Foundation, and he's going to be presenting on QuantumLink.

And we really appreciate you, Ken, for speaking today and for your organization being an ongoing financial sponsor of Blockchain Commons and our work.

Go ahead and take over.

Awesome. Thanks, Christopher. Yeah, I appreciate the call out there.

I've been enjoying working with you guys over the years, and you've done some amazing work, including on the stuff that we're about to talk about now.

So if I can take over the share here.

Let's see.

You guys see this?

Yes.

Okay, good.

Yeah, I wanted to talk today about a protocol that we've been implementing called QuantumLink,

and it's based on a lot of underlying Blockchain Commons protocols and specifications.

So let's get into it.

So the outline today is to talk about what is the problem we're trying to solve with QuantumLink.

We'll talk a little bit about it at a high level, talk a little bit about quantum resistance,

and then we're going to kind of dive under the hood a little bit lower level detail and see what we're actually doing here with QuantumLink.

So what's the problem?

So some of you might know we make a line of air gap Bitcoin hardware wallets under the Passport brand.

And of course, air gapping is a well-known security practice with benefits that are pretty clear.

And it does still come with some downsides to user experience.

And we've seen that with Passport, first gen and second gen devices where scanning QR codes, for example,

is how we interact with the user and the wallet in most cases.

And that can really depend on lighting conditions.

If the screen is too bright or the lighting is too low, it can be hard to scan.

Large transactions might, you know, if you're signing a Bitcoin transaction, it might need hundreds of QR codes.

And that's not really practical to sit there scanning hundreds of QR codes.

And so a lot of users will fall back to micro SD in that case.

And firmware updates on Passport require micro SD card and an adapter.

And a lot of users have had trouble, you know, formatting the card properly or downloading the files or copying the files properly.

And so if we can do something that would avoid them having to do that, we thought that would be an improvement.

But there's other issues too, like if we wanted to implement anti-XFIL in a clean way,

it would require multiple back and forth scans where, you know, you scan with the phone, you scan with Passport,

you scan with the phone, you scan with the Passport.

It's kind of a little clunky to do that sort of thing.

But also Passport doesn't know anything about the blockchain.

It doesn't know the Bitcoin price. It doesn't know which addresses have been used.

And, you know, for security, it's a good idea not to reuse addresses.

And also Passport has a lot of metadata on it, like account labels, multi-sig configs, user settings.

And we have the ability to back those things up, but it's over micro SD right now.

And so, you know, a lot of users don't do that very frequently.

And so they could be left with the device not properly backed up because it's not convenient to do it.

So QuantumLeak is our solution to this.

And Passport Prime, which is our upcoming device, was the impetus for this.

We started to work on the design of it and we realized this QR code scanning is not really going to scale to the next billion people that are going to onboard to Bitcoin.

We didn't want them to have to live with those limitations that I was just describing.

And so we wanted to find a way to make local wireless communication much more secure.

So we spent a lot of time thinking about different ways to do this.

But ultimately, we wanted a protocol that would provide sort of all the benefits of air gap security, but with a lot of improvements to the user experience.

So in a nutshell, QuantumLeak works by using out of band key exchange to establish initial trust.

And we do that by exchanging a QR code.

But other mechanisms would be possible.

It doesn't have to necessarily be QR code.

But we also then we create an encrypted tunnel between two devices.

And when a message is sent, every message is signed with a cryptographic signature.

So every message is both encrypted and digitally signed.

And this transport could work over, this protocol can work over Bluetooth or NFC, but also other transports.

So there's nothing restricting it necessarily to be over these ones.

And of course, it's underpinned by a lot of blockchain commons standards and protocols, which we'll talk about more.

So let's take a quick look at how this goes from the user's perspective.

And we're going to gloss over a couple of technical details here, like the types of keys, until a little later in the presentation.

So how does it work?

So I mentioned that there's an initial key exchange.

And so Passport Prime generates an encryption key pair and a digital signature key pair.

And the public keys for each of these are shared with Envoy via an animated QR code using blockchain commons UR format.

And this is during the onboarding process, typically, when you're setting up your passport for the first time.

So that QR code is scanned by Envoy.

And then Envoy generates its own two key pairs, again, one for encryption and one for signatures.

And it sends its associated public keys over to Prime, this time over Bluetooth, because it has the keys from, it has the private key,

or sorry, the public key from Prime, it's able to encrypt the message over Bluetooth and use its own private key to sign the message.

Now both sides have the necessary key material, and we've established the secure tunnel between Envoy and Prime.

Now on the device, we've architected this so that the MCU and the main MCU and the Bluetooth MCU are separate chips.

So it's not possible to compromise the Bluetooth chip and have that affect the main MCU.

These chips are connected on the PCB over an SPI bus, and all the data that's sent back and forth between them is already encrypted.

So what this means is that we're treating this chip as a black box, and essentially all the data that goes through it is encrypted and digitally signed.

So that means that it's the Bluetooth chip is not able to, you know, effectively modify or inspect any of the messages in transit,

which is good because the chip itself has some source code in it, in something called a soft device that we don't have access to, the whole Bluetooth stack essentially.

And so at this point, we have an active secure tunnel where every message has a unique encryption key, and we'll talk about that in a minute.

The encryption key is encrypted with the receiver's encryption public key.

And so a man in the middle can't inject messages or decode any intercepted messages.

So what are the benefits of this?

So the out-of-band key exchange process makes sure that we're not vulnerable to, you know, man-in-the-middle attacks,

which are possible with things like Diffie-Hellman key exchange.

All messages are encrypted and signed, as we mentioned, and the bandwidth on Bluetooth is high enough that we can do QoS firmware updates or downloads of new apps over the air.

So the other thing is now Passport could be kept up to date, so it can have the current Bitcoin price, it can have blockchain status info, it can have UTXO data, it can know which addresses have been used.

And in particular, one thing we did was we avoided the use of ECC and similar crypto systems.

As Christopher mentioned, these are vulnerable to quantum computing algorithms.

So this makes the QuantumLink protocol quantum resistant.

But what is quantum resistance?

Well, first we have to say, what is quantum vulnerability?

And so I think Christopher alluded to this, that most cryptography and digital signature systems that are in use today are vulnerable to quantum computers.

And in particular, there's an algorithm called Shor's algorithm that, when run on a sufficiently powerful quantum computer, can solve integer factorization, which breaks RSA,

and the discrete logarithm problem, which breaks ECDSA, Schnorr signatures, and a bunch of other cryptography.

So that's quantum vulnerability. What is quantum resistance?

Well, obviously, it means that it's an algorithm that is not susceptible to being broken by a quantum computer.

And mathematicians have come up with systems like this that remain hard to solve, even for quantum computers.

And so some of the mathematics behind this are lattice-based cryptography, hash-based signatures, code-based cryptography, and multivariate cryptography.

We're mainly interested in this case for QuantumLink in the lattice-based ones, and we'll talk about that later.

But these new systems are said to be quantum resistant because they're not vulnerable to quantum computers.

And so that means that our encryption and our digital signatures can't be broken on QuantumLink, even if you were attacking it with a quantum computer.

So for QuantumLink, we chose MLChem for encryption. This is the module lattice-based key encapsulation mechanism.

This had a much cooler name before, which was Kyber, which is a Star Wars-derived reference.

And it's not actually used for asymmetric encryption of the payload, but instead it's used to encrypt a symmetric encryption key like ChaCha20POLY1305 or AES256.

And for the digital signatures, we chose MLDSA, which is Module Lattice-Based Digital Signature Algorithm.

Again, it had a much cooler name before, which was Dilithium, which is, I believe, a Star Trek reference. So they're mixing their references here.

So let's take a closer look at what we actually built here under the hood.

So the key exchange is actually a little bit more complicated than what I mentioned. There's a small reason for that.

But Passport does first show a static QR code with only its Bluetooth device address.

And part of the reason for this is that our phones today, our phone cameras can scan QR codes and launch applications, but they don't understand the blockchain commons UR format.

And so we had a potential way of solving this where we encapsulated the chunks of the UR into a static QR code.

And we're still able to launch into Envoy, but it was a little bit clunky.

And showing the user this animated QR code right off the bat was a little bit jarring for some people.

So we decided to go with this one. And essentially what happens is the static QR code launches Envoy, but it includes the information on the Bluetooth device address.

And at that point, when the user scanned it with their phone camera, Envoy connects to Prime's Bluetooth, but it doesn't send any data yet.

But this is enough because on Prime, we can see that we've had a connection and we switch the static QR code to an animated QR code.

Because Envoy does understand QR code or UR codes, and it's able to properly scan the animated code.

So what's in this animated code? Well, it's a blockchain commons UR code.

And it contains, in addition to some of the metadata, includes something called a ZID document, which is another blockchain commons standard,

where there's an extensible identifier, which is a unique ID for each of the parties.

And it includes the public keys that we mentioned before that have been generated.

So the MLChem or Kyber public key for encryption and the MLDSA, the lithium public key for checking the signatures.

Now, on the Envoy side, it saves this data, it scans the UR code, the animated code and saves the ZID and the public keys that it received.

And it builds its own response. So it's all, as you recall, it's built its own private key and public key pair.

And it sends its own ZID, its own ZID document, in fact, with the identifier and its own public keys.

But it sends these over Bluetooth because it has the necessary key material to encrypt and sign the message in a way that Passport Prime will understand it,

be able to decode it and validate the signature.

But at this point, when Prime has received the message, we have our encrypted tunnel.

But what do these messages actually look like?

So under the hood, we're relying on Blockchain Commons' Gordian sealed transport protocol.

So we were aware of this for a while, but it didn't support post-quantum encryption.

But we started talking with Blockchain Commons and it seemed like it wasn't going to be that much effort to add that.

And so we engaged with them and provided some more financial support to add this post-quantum capabilities to GSTP.

And we've been a sustaining sponsor for Blockchain Commons for several years.

We know them well and we've worked with them before.

And so this process went very smoothly as usual.

They delivered above and beyond.

This change was very quick to get implemented.

As always, Wolf has an amazing architecture and testing and everything just worked.

So we were able to essentially drop in the post-quantum encryption over top of the GSTP version we were already running.

So we're not going to go into too much detail on the low-level bits and bytes of the protocol.

You're welcome to look at the standard on that.

But one thing to be aware of is it's built on top of another protocol, another standard called Gordian Envelope.

And envelopes provide a clean and composable way of building protocols that can easily include encrypted data or signatures,

or they can elide or remove out data while still maintaining the same hash tree.

And I really encourage you to look at the envelope presentation video.

Really open your eyes on what's possible with that standard.

Some very cool things that it's able to do.

So basically, envelopes plus the Gordian Sealed Transport Protocol are the foundation,

and they make a really solid foundation for QuantumLink.

So this is more or less what a message looks like.

We have an encrypted ChaCha20 Poly1305 key, which is our symmetric encryption key.

We have an encrypted payload, which is encrypted by the ChaCha20 key.

We have the ZID, which says, "Who are we talking to?"

And then we have the digital signature, which is signed by the sender and can be verified by the receiver.

So the process on receive, we basically decrypt the ChaCha20 Poly1305 key because we have the private key that corresponds to the public key that we sent out.

So we were able to derive that symmetric key and decrypt the payload with it.

Then we check the digital signature of the payload to make sure that it matches what was sent.

And we look up the right public keys to use for the decryption and validation using the ZID that was included.

And so this is important because it means Prime is able to connect to multiple devices and know who it's talking to at any given time.

So if any of these steps have an error, though, the message is just completely dropped.

And so we don't worry about an attacker trying to send any kind of bogus message because we just ignore it.

So that's the basis of the protocol. But what does our payload look like?

What sort of messages are we actually using GSTP to send back and forth?

So initially, we've got synchronizing the onboarding flow because Envoy and Passport are showing screens to the user.

And we need to be able to tell Passport to move forward in a flow or tell Envoy to move forward in a flow,

depending on which device the user is interacting with.

We use it for signing Bitcoin transactions. So Envoy will send a Bitcoin transaction in PSPT format and Passport will show it to the user.

The user will sign it and then the message will be sent back to Envoy for broadcast.

We also use it to keep the date, time and time zone up to date on Prime, updating the Bitcoin price,

because we actually show you the Bitcoin price on Prime, which is pretty unique for a hardware wallet.

We have a lot more stuff planned, but we'll talk about that in a minute.

But we also have firmware updates and installing new apps, which are being done over Bluetooth, which is pretty cool.

Some stuff we're going to add in the future, maybe some other cryptocurrency prices, relevant UTXOs.

So if the wallet wants to be able to show, for example, the transaction history, we'd be able to do that.

What addresses have been used in the past and then different blockchain metadata or multi-sig setup data.

If we implemented multi-sig in Envoy, for example, we'd be able to send the multi-sig data.

Or if any other existing wallet out there that implements multi-sig wanted to implement or integrate with Passport Prime,

they'd be able to send that data directly over the QuantumLink connection.

So those are just, I think, scratching the surface. But if you have any other ideas in mind, let us know.

We're going to be implementing an SDK for this for third-party apps later this year.

And we'd love to hear what you have planned to do with it.

Let's just talk a little bit about key security storage lifetimes.

On Prime, we store the private and public keys for each Envoy connection in an encrypted file.

And this is all encrypted with AES-256 encryption.

It's unfortunate that we have onboard hardware accelerated AES in our chip, so we use that to make this fast.

And the encryption key for this AES is stored in the secure element.

It's derived from some entropy stored in the secure element.

So it's unique per device and stored in a secure manner.

On Envoy, we store the keys either in the iOS Key Manager or the Android Key Store.

Currently, we're performing a single public key exchange at the time of pairing.

That's the QR process I mentioned earlier, and then the QR to Bluetooth transmission.

But there's no formal key rotation procedure.

A user could, though, perform the pairing again themselves if they were concerned about any kind of compromise.

But we may consider doing an automatic key rotation in the future.

A couple of additional points on security.

So the Bluetooth chip, I mentioned this a little bit earlier, but it's running a vendor-supplied soft device,

which is a Bluetooth stack in a binary blob format.

But we've written a custom app on that, which communicates with Prime's main processor.

And that processor is the microchip SAMA5-D28.

And there's a spy connection between these two for exchanging data.

And all the data sent over that is encrypted and signed.

Messages received by Prime, again, must be properly signed and encrypted by a known partner.

And so if it's a Zid we don't know, we drop the message.

If the decryption fails, we drop it.

And if the signature is invalid, we drop it.

So this way the Bluetooth, again, is treated like a black box, not capable of man-in-the-middle attacks.

It could grief by dropping messages, for example, but it's not going to really be able to do any damage.

And a couple of final points.

All the protocols we mentioned, all the standards are open source.

So GSTP, UR, Envelope, Zid.

These are all provided by Blockchain Commons, I think almost all under the BSD2 clause plus patent license.

And Passport Prime apps, including the Envoy mobile app, EOS operating system, and the QuantumLink protocol are open source.

We haven't released it yet because we haven't released the device, but that will be occurring when we ship.

But we're using GPL3 primarily, Afro GPL for some of our server components.

There's a handful of Apache licensed code as well that we're using directly from other projects.

And we're going to document QuantumLink.

Obviously, the messages underlying it with GSTP are already documented, but we're going to document our layer on top of that.

And also provide a mechanism for third parties to add custom messages later this year.

I alluded to this earlier as well, but we have a Prime SDK, which is going to allow you to make third party apps.

And those apps would also be able to communicate over QuantumLink if you had your own mobile app.

If you'd like to wait for that later in the year, feel free.

But if you want to get a head start, we might be able to work with you sooner if you have an interesting use case.

And that's it.

So I guess I'll turn it back over to Christopher here and see if there are any questions.

Yeah, let's, I think it would behoove us to hold any quantum questions until after Wolf's brief presentation.

But I wanted to open it to any questions specific about Passport Prime from anybody on the stage.

Honk, do you have any questions about Passport Prime?

Anyone else?

Okay.

Wolf, why don't you go ahead and take over and do your presentation and then we'll open up for broader questions.

Okay, great. Thank you, Christopher.

Christopher, I noticed you didn't present the slides about the key sizes or things like that.

Yeah. Was that something you wanted me to do or was that something that you thought?

Yeah, that's in your keynote.

Okay. I apologize.

I'll take a moment to bring that up because I thought that was something that you were taking over.

But sorry for the confusion there.

Let me get that open real quick.

Okay. Green here.

So just got a few little slides here and then I'm going to show just a little bit of code.

But this is kind of building on top of what Christopher talked about initially and also some of what Ken was talking about.

So first I want to talk a little about symmetric encryption.

This was alluded to before the ChaCha20, Poly1305 as part of our stack has been for a long time in terms of symmetric encryption.

It's really the combination of two different cryptographic constructs, ChaCha20, which is a 256-bit stream cipher,

and Poly1305, a 128-bit one-time authenticator.

And as is suggested by the bit size, to brute force ChaCha20, you'd need two to the 256 operations on average.

And a quantum attack, which is Grover's algorithm, it's still unfeasible because even though it's a square root of that,

it's 128, two to the 128, it's still infeasible under any conceivable systems.

Poly1305, same kind of attack, also halves the number of effective bits, which makes it two to the 64,

which is not ideal but still beyond practical feasibility for the foreseeable future.

So even with quantum attacks, ChaCha20, Poly1305 remains secure for the foreseeable future.

But, you know, obviously we're going to keep our finger on the pulse of this because these algorithms are emerging all the time.

Sorry for the frog in my throat.

Okay, so why post-quantum public-key cryptography?

As again has been mentioned, symmetric encryption survives quantum attacks.

And we already use 256-bit keys, which are considered to be sufficient secure for the foreseeable future.

But public-key cryptography, RSA, ECC, Diffie-Hellman, is completely broken by Shor's algorithm,

assuming you have a sophisticated enough quantum computer.

And as Christopher mentioned, you know, people talk about the number of qubits.

It's very important to distinguish between physical qubits, which are highly error-prone,

and logical qubits, which are a number of physical qubits cross-checking each other.

But this basically means to have a certain number of effective logical qubits,

you need orders of magnitude more physical qubits.

So, but in any case, Shor's algorithm, assuming a sufficiently powerful quantum computer,

solves factoring in discrete log and polynomial time, which means the security of those goes to zero, theoretically.

All current asymmetric systems fail under large quantum computers.

So, post-quantum cryptography is needed to replace public-key cryptosystems.

So, as we mentioned before, MLChem and MLDSA are NIST standards now,

based on the previous algorithms Kyber and LLithium.

We'll talk about those a little bit more here.

MLDSA, FIPS 204, has three levels, 44, 65, and 87.

And those are all, again, tradeoffs between size and speed.

They are based on, but not the same as, crystals dilithium.

So, even though you'll hear it's a, you know, also known as, it's not really also known as, it's more based on.

For example, the MLDSA signatures are not deterministic,

which means if you sign the same message twice with the same key,

you will get two different signatures that are nonetheless both valid.

Whereas dilithium, the signatures were deterministic.

And so that's a tweak that they've gone through,

but that basically means that dilithium signatures are not compatible with MLDSA signatures.

Similarly, and also the thing to realize is that they're not linearly composable like Schnorr signatures.

One of the things that has also been our stack forever are Schnorr signatures.

And the nice thing about that is you can compose them,

so you can have multi-sigs that look like a single sig that take up no extra space in the blockchain or whatever,

and nonetheless are verifiable as having been composed of several signatures.

No current post-quantum cryptography algorithm actually has that attribute.

On the encryption side, key exchange or key encapsulation also has three levels.

The ML-Chem has 512, 768, 1024, again, based on, but not the same thing as Kyber.

So in our stack, we basically, and I'll show you a little bit of this, we've abstracted it all.

So you can basically choose between classical algorithms and quantum algorithms just by changing essentially one line of code.

And I'll show what that looks like.

So we don't consider this to be crypto agility as is used by some standards organizations,

but we are considered supposed to be crypto agnostic.

So we can support a subset of crypto algorithms that we feel are most useful.

Part of one of our guiding principles is that average engineers, like myself in many ways, should not have to be cryptographers.

So we choose best of breed kind of algorithms and try to make it very hard to do the wrong thing with our APIs.

And of course, our APIs are all reference implementations of our standards.

Again, they're not standards necessarily blessed by a standards organization.

A couple of them are on that track.

We hope more will be in the future, but they're standards in the sense that we have published specs and reference libraries

that a number of them have third-party implementations, especially our UR has gotten some great traction

and our multi-part UR format or MER algorithm has gotten a lot of good traction.

And GSTP, as Ken just explained, is starting to get traction with some developers.

And so we're very happy about that.

And we've done presentations on these up on YouTube that I recommend you go check out for MER, Gordian Envelope, GSTP, and things like that.

They all build on each other.

And this has been something that's been taking us years to kind of build up to because we'd like to see a lot more cooperation among developers.

Obviously, there's always going to be competition.

We encourage that, too, but we want to make sure that the developers don't have to reinvent the wheel around these things.

And that's why we welcome community support, both technically and especially financially.

So I think this actually slide was added late.

Christopher, could you really speak to this really quickly?

Yeah. So basically, there is one other digital signature algorithm that NIST has approved in FIPS 205.

Like the others, it is based on a number of research on something called Sphinx.

Sphinx is a -- rather than using any of the lattice approaches and some of the other approaches is basically using hashes because hashes are considered to be very strong against quantum attacks.

However, hashes are very large.

And, you know, it's not quite an order of magnitude larger, but it is close to that.

And as well, it's, you know, much more difficult to process.

And since it's redundant with the MLDSA lattice-based standard, we decided not to support SLHDSA at this time.

But, you know, based on our abstraction, it will be easy to add.

Or if, you know, at some point NIST comes along with a composable post-quantum signature scheme, then we could add that as well.

So we just don't want to prematurely add things that are unnecessary.

That's it.

Okay.

So moving forward, obviously quantum signatures and encapsulated keys are significantly slower and larger.

You know, we'd like -- a number of techniques have been developed for so-called hybrid quantum.

Ken was talking about one where they're using Cha-Cha-Poly to encrypt the actual messages going back and forth after the initial quantum key exchange.

Apple and Signal also have post-quantum strategies which involve a certain amount of like automatic key rotation, things like that,

to reduce the number of times larger post-quantum keys have to be exchanged.

And, you know, as I've mentioned, we're primarily focused on a simple key exchange at the front.

But our stack is very flexible.

So if you need to develop something more flexible on top of that, there's nothing in our stack that would slow you down from doing that.

So just really briefly, I want to talk about the orders of magnitude involved here.

In blue, you see the classical algorithms that we support.

At the top, you see the signature algorithms.

At the bottom, the key encapsulation method, the one we've been using is X25519,

which Ken is still considered very sturdy under classical presuppositions.

But as you can see, the key sizes, the private key, public key, and the size of the signature encapsulated key are vastly different between those.

I'm a visual thinker, so here's an actual bar chart showing the difference between them.

The top three, Schnorr, ECDSA, and ED25519 have 32-byte keys and 64-byte signatures, which are double the size.

But that's dwarfed by the size of the MLDSA keys and signatures.

Obviously, we don't recommend that you necessarily feel like you have to adopt post-quantum unless you want to make sure that your data that you're sending,

and of course, Passport has reasons to want this to be very future-proof,

but you want to keep your data encrypted and safe and your signatures secure for 10 to 50 years at this point.

That's what we're currently expecting.

Now, again, at the rate of technological change that we're all experiencing right now, we might be beyond the singularity event horizon.

That could change tomorrow. We really don't know, so there's always a risk surface to evaluate.

So that's that. Now I want to show you a little bit of my code here.

So you're looking at VS Code. You're looking at just a small part of our stack.

This is the BC components, and this is a repo on GitHub. This is Rust code.

And BC components basically includes a number of our primitives that we use for our higher-level things,

including various kinds of signatures and keys.

And so for instance, under symmetric, there are actual symmetric key, which is our ChaCha20 poly305 key and things like that.

So it's all designed to be very kind of easily understandable and maintainable.

So what I want to focus on here is a couple of things here.

And some of these features are new. For example, we've supported a number of signature schemes for a while.

Now we've kind of formally abstracted that into a signature scheme type,

which supports all the different types of signature schemes we support, including several SSH ones we added not too long ago,

which are basically compatible with the SSH command line tools.

Schnorr, which is still our preferred default, as well as ECDSA, ED2559, and then the MLDSA, three levels.

Similarly, on the encryption, we have supported X25519 forever.

Now we've added the MLChem levels to that. So what does this look like in practical code?

So if we pop over here to our signing mod.rs, we have a number of unit tests in here.

And as you can see, they're testing various kinds of things where we take a known public key, derive a signature from it, and then verify that.

But if we jump down here to this, test key pair signing, we've standardized on the idea that when you generate a private key,

you generate a public key. And this was partly due to the fact that the post-quantum libraries we've adopted are very opinionated about

not just having a private key without having its associated public key. They generate them simultaneously.

So we basically abstracted this throughout our whole stack so that now you just say whatever scheme you have, signature scheme you have,

create a key pair. It creates the private and the public key, which again, you manage at that point.

But again, we also provide serialization methods and things like that that are compatible with DCBOR and Gordian Envelope and all those things built in.

And so you sign it with that signature scheme, with the private key generated then, and you verify it. And it's that simple.

Now, then we have these other, that's a helper method. These other methods are the actual tests,

tests with all these various kinds of key systems, including the post-quantum ones.

So as you can see, it's changing one line of code, in fact, one part of one line of code to actually use any of the, adopt any of these crypto systems.

And the same is true with the encapsulation schemes. We have test encapsulation here. We have an encapsulation scheme.

We generate a public private key pair. We encapsulate a new shared secret. This is a new, essentially, it's a, in fact, if you look at the type of secret here,

it's a, let's see, it's a, yeah, it's a symmetric key. So, which is basically our, our, our ChaCha20 poly-1305 key ready to be used.

And then on the other side, it's decapsulated using the private key. And then we just assert three equal.

So that basically means now you can communicate using the symmetric protocols.

And this, again, the same thing is happening here. We're doing with our default encapsulation scheme, which is X25549,

but it's just as easily to use, easy to use MLChem, any of the levels for that as well.

So it's just, you know, changing less than one line of code. So that's what I had to present. I'm open to questions.

Thank you, Wolf. Anyone have any questions about post-quantum in general? Have an opinion, a thought?

I was wondering, Wolf, you mentioned that Kyber is, is not the same as MLChem. Do you know what they actually changed between the two?

I haven't looked into it in detail. I did, I think, ask GBT for a summary of it at some point. It's not anything as dramatic as, for example,

dilithium having deterministic signatures and MLDSA having random signatures.

So I think that there were some things that the cryptographers felt were important to tweak at the last moment.

And I do know they're not compatible, but I couldn't really speak to the exact changes that were made.

I have a question for you in your GSTP. One of the, this is for you, Ken.

We have this capability of doing encapsulated state continuation.

Did you have any need for that in your use of the protocol?

Well, we're still in the middle of implementing some of these things, so not yet, but I see the usefulness of it.

But not yet at the moment.

One thing I would just say, like regarding the size of the keys, that was a really big concern of mine initially.

But for a lot of the use cases, especially the ones where we're transferring large amounts of data,

let's say, for example, a firmware update or a new app being downloaded.

Okay, it's a couple of extra K of data, but what we do when we're transmitting these messages over Bluetooth,

of course, Bluetooth does not have a message frame that can hold kilobytes of data.

So we've got a little chunking protocol that we've built on top of it to take a quantum link message

and send it over the Bluetooth transport.

And so in that case, the overhead of those large keys isn't as important.

But also, in some of the other cases, these messages are not things we're sending that frequently.

So if there's like an extra 4 to 6 K of data transmitted every minute,

the Bluetooth is more than fast enough to transmit that without the user being like, oh my God, what's going on?

It's so slow.

So we felt that that was a sort of worthwhile tradeoff,

given that we could amortize the cost of the keys, especially on larger messages.

Yeah, definitely.

The fact that the stack is abstract on transport, so you could do it.

I mean, technically, you could do this with AirGap on 100 QRs or whatever in an animated, and it would work.

You can do it over NFC.

But then as we start having services and things of that nature,

we can use Tor, other channels that are less secure, etc., because we're fundamentally securing it.

To be clear, we have not implemented any of the hybrid schemes.

So if you look at what Apple and Signal have done, they are not using any of the digital signature schemes.

They are only using the key encapsulation method.

So they basically start with a chem that is quantum resistant,

and then they use the math there to create the mathematical foundation that is used with the classical EC DSA encryption that they use.

So I think long term, meaning our long term, not the 50-year long term, but I think over the next year or so,

we'll be looking at some of the different hybrid schemes, in particular in relationship with Schnorr,

because we want to enable Schnorr.

There are a lot of things that we can do with Schnorr that we don't have a post-quantum equivalent for

that basically gives some of the strength of the quantum resistance to how we do some of the things with Schnorr

until quantum computers are real.

But that's emerging research, and in general, I think the best cryptographers I know will basically deny that they're very good at what they do,

and that they kind of have to have somebody else prove to them that it's a good idea.

So we kind of take that at the next level.

We really want to know what other people are doing, evaluate it carefully,

not necessarily even trust our own work until we've had it reviewed by others.

So we have to be very cautious and careful here.

And I think this demonstrates our proactive design in our architecture,

because we knew these types of things were coming, and we wanted to make it easy to do when it happened.

And I think when Ken came to us and said, "Hey, I think it's time,

given our scenario where somebody might have one of these devices around 10 years from now,

and we don't want that device to become corrupted," it made sense.

So we were able to add it.

Any other comments or questions, either about Passport or about post-quantum?

Okay, well, thank you, Ken, for the presentation, and Wolf, for the greater details.

I wanted to briefly close our meeting with a little bit of discussion of what's been going on in the Zcash community.

So we have long been supported by the Bitcoin community, and we greatly appreciate all the support there.

But our goal is to protect everybody.

So if you're doing self-sovereign digital wallets, we want you to use our kind of layer zero standards

to make sure that your customers do not have losses or are vulnerable to various kinds of attacks.

And I think our work in the Bitcoin community has really shown the possibilities here,

and I think what Foundation Devices is doing is great.

The Zcash community came to us and said, "Hey, we have some other unique problems for our layer one chain,"

especially given that they were originally using source from a very old version of Bitcoin Core,

which they forked to use to create the very complicated Zcash anonymous currency technology.

So that particular stack that was based on Bitcoin Core needs to be deprecated,

and they've been trying to keep it up for years.

So they really need everybody who's using the old Zcash D to move off of it and move to a more modern wallet.

So as kind of leaders in interchange formats on Bitcoin, we've been working with them to create infrastructure for that.

And while we're at it, as long as we're being able to import from Zcash D to other wallets and support migration and portability,

which is one of the Gordian principles of our architecture, we want that they're also supporting that.

So we've begun working on it. The project is called ZWIF for Zcash Wallet Import Format Project.

We've released a report on our initial work, which was basically analyzing all of the major wallets that do Zcash.

And we've got a start on a migration tool.

If you're using Zcash, especially Zcash D, we would really appreciate it if you could test ZMigrate on it.

And then when we have better tools in the next month or two, please do take a look at that.

But I really want to kind of come back to the fundamental principle.

Interchange allows people to freely move their funds.

We don't want people to be locked into a single wallet.

We want to encourage cooperation between wallets so that they can do things.

But we also want to make sure that users don't get locked in.

They don't get lost and lose funds.

These are fundamental Gordian principles of openness, independence and resilience.

These are fundamental to self-sovereign management of digital assets.

And we really would like to see more people, wallets, even on the Bitcoin community, to begin to really look at, you know, okay, what are these issues?

Where do we need to be compatible?

How do we support user choice, openness, independence, resilience on any blockchain that cares about self-sovereign?

So if you're working with some other layer one technology and would like to start incorporating these,

or if you're on Bitcoin and would like to do more than animated QRs, you know, please talk with us.

We'd love to advance these forward.

Any questions on this and the general principles?

Or do we have any Zcash people here today?

I know there's a big Zcash event going on right now, so most of them are just going to get this in recording.

Ken, question for you.

If, you know, you guys are a Bitcoin-centric wallet, but you're going to allow apps,

do you think it's feasible for one of the Zcash people to be able to create an app for Zcash, on your advice?

Absolutely. We'd definitely be open to helping with that.

That would be great.

The point is that the SDK will be there, and you don't need to ask our permission to build an app.

It's a sovereign developer kit, basically.

So if you want a Zcash wallet, you build a Zcash wallet.

You don't ask us. With Ledger, you have to ask, get approvals. It's a completely different situation.

Exactly.

Okay.

Well, unless there is any other questions on any of the topics we've had,

on support of blockchain commons standards for interchange on other platforms,

on specific to Zcash, or about quantum and the quantum link protocol from foundation devices.

I would just quickly note that the ZWIFT format uses the same envelope format that is being used for Quantum Link's GSTP.

It's a very adaptable, privacy-focused format that we are offering as a specification,

because it can do a lot of different things.

ZWIFT, Quantum Link, and other stuff.

We've also been talking with various identity communities,

because one of the fundamental problems right now is that too much information is being shared.

We've seen this in everything from major things like KYC, where your security is up to whoever the lead is.

But also applies to pretty much any storage of private data at rest.

So in the same way that TLS is the default standard for transporting secure data,

we're hoping Envelope will become an international standard for storing data at rest in a secure way,

in ways that you can choose to allow the data, have it be offline, be entrusted elsewhere,

but still be able to rely on it rather than storing it.

I think it's one of my fundamental problems with some of the JWT, OAuth, web-based standards is

they basically store all the certificate information in every certificate.

There is no elision.

All the information is there, which is fine when it's just you and the one person you trust,

or the trusted service, but unfortunately this stuff gets shared around in a variety of different ways,

your employers, your educational credentials, etc.

And we want to be able to preserve the privacy.

And this comes back to the blockchain world.

Maybe you want to keep certain kinds of information to be able to give the details to your CPA,

or maybe in the future some form of auditor.

You can do that and not necessarily reveal the information that you don't need to reveal.

So again, we're just trying to create a structure that works for all of those.

Okay. I wanted to make sure that some of our regular supporters, Honk and Mike Warner, etc.,

don't have any other questions, or if Wolf, you have any last-minute questions.

No, I would just definitely encourage people to check out our YouTube channel,

where we have presentations about a lot of the technologies we've discussed here today.

And we'd always love to receive feedback, always love to have virtual sit-downs with you guys,

and help you understand our technologies or evaluate them for adoption for various purposes.

Great. And we'll also have this video on our YouTube channel in two or three hours.

I know some people were not able to get here on time because we had two different URLs we were using.

So if you missed the start, I will send out on Signal and send in blue to our email list our URL for this video when it goes up.

Okay. Well, thank you again, Ken, for your presentation, Wolf, everyone else.

And we will be having another meeting in a month on the first Wednesday. Thank you very much.

Thanks, guys. Thank you, everybody.

[end]


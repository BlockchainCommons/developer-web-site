---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Course & Demos Meeting Transcript"
tagline: "Learning FROST & the Hubert Demo"
hide_description: true
classes:
  - wide
permalink: /meetings/2025-12-frost/transcript/
sidebar:
  nav:
    - meetings
    - frost
---

**Participants:** Christopher Allen, Shannon Appelcline, Wolf McNally, Christoph Dorn, Henk van Cann, Leonardo Custodio, André Ferrière, Georgy Ishmaev, etc.

**Duration:** ~90 minutes

**AI Transcription:** Automated transcription (App: Mac Whisper 13.8, Model: Large v3 Turbo w/Speaker Recognition)

**AI Processing:** Medium cleanup (App: Claude Code, Model: Claude Sonnet 4.5)

*Note: This transcript has been moderately edited for readability - filler words removed, errors fixed, fragmented sentences cleaned up, paragraphs added, technical terms standardized. Original content and speaker voice preserved throughout.*

---

## Part 1: Learning FROST from the Command Line

**Christopher Allen:** Welcome everybody to our monthly Gordian meeting at Blockchain Commons. We are looking forward to a presentation today from Shannon on the new Learning FROST workbook and some new tooling to make that easier, as well as a demonstration at the end from Wolf McNally of a new kind of FROST coordinator. So Shannon, why don't you take over?

**Shannon Appelcline:** Hi, I'm Shannon Appelcline. I'm a technical writer here at Blockchain Commons, and we have all FROST all morning for you today. So I'm going to start out with a kind of introduction. Some of it's going to probably be stuff you already know, but hopefully there's a few little insights in here as well.

So here's our usual bit on Blockchain Commons. But seen through this lens, FROST is about letting people control their digital destiny through a signature system that can enable control of digital assets, group coordination, and more. And it's very much part of our collaborative work with the community because it's not something we created. It's something that was out there, that was being worked on, that was so cool that we really wanted to throw our support behind it as well. And I should say thanks to HRF, the Human Rights Foundation. They gave us a generous grant to implement this technology, the work that Wolf's been doing for much of the latter part of the year, and this course that I put together.

### What is FROST?

So, what is FROST? FROST stands for Flexible, Round-Optimized Schnorr Threshold, and that hits most of the key points. It's a threshold signing system that uses the Schnorr algorithm. There's a bit more than that, but the other particularly important point is that it's designed to integrate with distributed key generation. I'm going to go through those points one by one to give them all a little more detail.

So, signatures. Digital signatures allow you to verify messages. Those messages could be board minutes, a publication, or in order to move digital assets, which is a lot of where we've seen them used so far. With a signature, you know who signed the message, and you know the message hasn't changed since they signed it. Note that signature doesn't mean encryption. They're separate uses of cryptography, and FROST is all about signing.

So multi-sig—we all know that's a special type of signature that allows a group of people to all sign together. A threshold signature is a multi-sig where a subset of the group can sign for the group. So I have that labeled here as M of N, where a subset M of a group N can sign, not all the people.

Okay, so Schnorr. It's a particular signing algorithm. It's got a different foundation than some traditional signatures like RSA or ECDSA, but I'm not going to get into that. What's important to know are the advantages of Schnorr signatures, and the big one is that they're aggregatable. You can add together my signature and your signature for a multi-sig, and what you get out of it is totally indistinguishable from a single signature, which also means that it's the same size as a single signature.

Okay, so the last puzzle piece of FROST is distributed key generation, or DKG. FROST signing is done with what are called signing shares. They're essentially shares of keys, a threshold of which you bring together to sign. You could have one person create the key and shard it. That would be a kind of old-fashioned way to do it—it's called Trusted Dealer. But you can also use multi-party computing to engage in a ceremony where everyone exchanges information with each party getting their signing share and a shared public key, but the full private key is never created, which is some pretty powerful security.

### Why Use FROST?

Okay, you might already have some hints from those descriptions, but let's talk about why you would use FROST, especially over other signing systems out there.

Bitcoin, of course, has its classic multisig algorithm, which can be used to authenticate Bitcoin transactions. It uses pay-to-script hash with the check multisig opcode. Everyone can see that a Bitcoin multisig has multi-signers, and each signer increases the size of the transaction. In contrast, Schnorr signatures for Bitcoin transactions use Pay to Taproot (P2TR), and they look exactly like any other signatures. That's thanks to aggregatability. So people don't know it's a multi-signature. Its size never changes, no matter how many people sign it. As it happens, you also can't see which members of a FROST group signed, just that enough did, which is a nice benefit if privacy is important to you.

There's another Schnorr-based signature scheme that has quite a bit of usage called MuSig2. There's two big differences between the two. First, FROST is a threshold system, while MuSig2 is just an N-of-N multisig system. Thresholds are possible using Merkle trees, but it's a bit of additional work. Second, there's a contrast: FROST has the characteristic of privacy that I mentioned, while MuSig2 instead has accountability. You know exactly who's signed. This is one of those cases where you're going to probably pick your signature system based on the particular needs of your use case.

So here's a look at all three signature methods in a chart. I think the really important thing here is the size. You've got 64 bytes for Schnorr signatures, period. Classic ECDSA for Bitcoin, it's 72 bytes for every signature. Now, obviously, that's going to be a difference in what you have to pay for a transaction if two or three people are signing. But imagine a situation instead where you have a group of 150 people and you want a quorum of 100 of them to sign. There's no way that's possible in traditional signature systems, but it's totally doable with Schnorr.

So here's a quick summary of all the advantages of FROST. There's two of them that I haven't mentioned yet. One is that the FROST protocol is built to have efficient communication. I'm going to show you what that looks like in a second. The other is that it has refresh and repair capabilities. That last bit means that you can regenerate lost shares. You can increase the number of signers. You can change thresholds. And you can generally update your FROST group in a lot of different ways after the fact. In a classic multi-sig system, you would have to, say on Bitcoin, sweep your funds to do something like that. And you don't have to here. You can just redo the signatures. This is a bit more bleeding edge than some of the other things, but I've seen real working prototypes of it in an actual working wallet.

### How Does FROST Work?

Okay, so how does FROST work? This is the part that I'm not going to dwell on too much, but I wanted to briefly show you a little bit of what its communication mainly looks like, because it's good to have a handle on the overall system. It comes in two parts. You have the key generation, where you make your secret shares, and you have the signing where you use your secret shares. And as it happens, there are two different ways to do each of those, which is a total coincidence, but I'm going to run you through all four, and then I'm going to show you all four very quickly from the command line.

Okay, so here's Trusted Dealer. As I said earlier, a single person creates and distributes a key. It's a simple process, but you have to actually trust the person, and I mean really trust the person doing the work. The key exists in a single place during key generation, and it could continue to exist there if the trusted dealer doesn't delete it like they're supposed to. So it loses a lot of the advantages of FROST, but maybe it's the only way you have to do it. And you do get those advantages down the line if the trusted dealer does what they're supposed to.

All right, here is distributed key generation. It's the better way to generate keys. It's typically called a two-round process, but you can see the prep at the top, which means that you actually have to do some out-of-band setup to decide your group size and your threshold. In the actual ceremony, you create secrets and publicly commit to those secrets. That's round one. And then you calculate your signing shares, and then you privately, to each member, commit to those shares. Again, we're not going to dwell on the specifics. The most important thing to note here is that you have to have communication channels. There is a broadcast channel and there are private channels. And how those work are kind of beyond the scope of FROST itself. And you have to figure out how is this going to work. And we're going to show you later on how this can work in a very private, decentralized way.

Okay, so here's signing. Note that this time not only do we require communication channels, but we also have a centralized coordinator. They don't know any secrets, they don't learn any secrets, but they could block the whole process if they wanted to, so they're not exactly trusted but they're semi-trusted. As for the actual process here, it's the same type of thing: we're creating secrets, we're sharing commitments to those secrets, and eventually we're signing the message and then the individual signing shares are aggregated together.

Though FROST signing is fundamentally a two-round process as I just showed you, you can actually pre-process one of the rounds and you do that by creating a whole slew of secrets, by creating commitments to those secrets, and by maintaining all those commitments in some data store. A server is the way that the FROST paper suggests doing it. There's a lot of different methods for how you could do it. The end result here though is FROST signing can be made more efficient and sped up a bit.

### Learning FROST from the Command Line

Okay, so where can you learn more about FROST and get some hands-on experience to really see how it works? The answer to that is actually the foundation of what I presented here, which is the Learning FROST from the Command Line course. It's a brand new course funded by HRF. We've just released it on GitHub and we now have it on a nice book-oriented web page as well, which is what learningfrost.blockchaincommons.com is. It's fully edited, it's fully drafted, we're seeking peer review particularly to make sure that our descriptions of FROST and its processes are entirely accurate. We have a previous course that Christopher and I did called Learning Bitcoin from the Command Line. It's been very popular. We know that it's gotten people into the industry because we've worked with them.

Okay, so working from the command line means that we needed command line tools and those came about due to Zcash's ZF FROST project. They have a repo on GitHub called frost-tools and it contains tools for trusted dealer generation, distributed key generation. It's got two different types of clients and servers, one of which is used for distributed key generation, and each of them can be used for signing in different ways. So a lot of tools and a lot of ways that you can really check out how FROST works and see all of the particulars.

We—and by we I mean Wolf—have done a bit of work to expand their tools. We created a tool called frost-verify-rust which checks the particular file format of ZF FROST signatures so you can see that the signatures are actually working, which I thought was important for a hands-on course to make sure you knew that you could do a signature and that it then checked out. The biggest thing though was we wanted to show how could you use FROST for Bitcoin transactions, and so Wolf made several tools, a couple of helper tools and then some PRs for the actual ZF FROST tools to allow you to actually do a signature. And he's got a demo of that that he's done that we have videos of, and it also appears in the Learning FROST course.

So we suggest you try out the course. Our "From the Command Line" courses, they've always been about providing hands-on experience because we think that's a really visceral way to see how things work and to get it into your memory. But before I close out, I'm going to just quickly run you through what does this look like? How does this work from the command line?

So this is the ZF FROST trusted dealer tool here. It's very easy. You tell it a group number, you tell it a threshold, and it generates a whole bunch of packages, each of which has an individual signing share. So here's what one of those packages actually looks like. You can see it's got a bunch of related information. And again, this is why I like a hands-on course, because you can create this type of thing and then see, oh, what do they need to do this work? But most importantly, it's got that signing share. So this is what people will use to actually sign when they're using FROST.

So distributed key generation is, like I said, the alternative. And there's two different ways in the ZF FROST tools to do distributed key generation. This is their straight command line one. You, again, tell it your group size, your threshold, who you are—that's the identifier—and then it starts generating packets and you have to by some out-of-band means pass these packets around to other people and enter them in. And you have to correctly identify where each packet came from, which round it's from, who it's from exactly. If you identify a packet with the wrong member, the whole process breaks. In other words, it's just begging for a better UI and it's prone to errors without it.

And I say this knowing that it's prone to error because this is an example of Stack Wallet that actually does all of the cool stuff with FROST, and this is an example of its initial DKG. And as you can see, it's set up exactly like that command line setup where you're getting commitments from various members and you're entering each person's commitment in. And the first time I tried this, I screwed it up. I put in the wrong person's commitment or the wrong phase—I don't know—but I got all the way to the end of the process and it didn't work. So that really shows how this type of thing where you're trying to do this by hand is not just prone to error but it's one of the big stumbling blocks for using FROST. So we need to make it a lot easier.

So how do you make things easier? Fortunately the ZF FROST tools have examples of that too in two different servers. You actually have to set up secure servers on the internet to use them, so they're a little more challenging. So this example shows the frostd server which is the easier of the ones to set up. It routes the correct information to the correct people, it logs everything correctly. You could see the other—I was putting in all this information here—it just does it all for you when you enter the initial information. Again, the server is semi-trusted. It could block the process, but everything's end-to-end encrypted so it doesn't see any special information, and the signing key still never exists in one place. So this is increasingly what we want to see for the UI of FROST to make it easy for people to use.

So I should stop and note here that though we can start to see solutions for the UI of FROST, we're still going to have all of the typical issues with key management that we've always faced and are still looking for great solutions. And you have to secure your share. You have to record its purpose. You have to make sure you don't lose it. If you're using a threshold signature, though, which FROST allows, you're also going to get advantages. It's going to be more resilient, meaning you're not going to necessarily suffer loss if you just lose one share. It's going to be more secure, meaning you're not necessarily going to allow someone to steal it if they just get one share. So yes, we have the typical key management issues, but by using a threshold signature we hopefully are offsetting some of them.

Okay, just quickly to show signing here. Signing—again, this is using the ZF FROST frostd server—you need a coordinator, which frostd is, else the process is just too complex to manage. But again, we can see there continue to be UI shortcomings because that's always been kind of one of our issues in the industry—how to make this really approachable to people. The one that I find here is that message to be signed. If you see there's a hex encoded blob here, and there's no necessarily easy way to see what it is. Maybe it's a message I could unencode, but maybe it's actually bits for a Bitcoin transaction or something. We need to figure out how to make that more obvious too so that people can sign in a rational way.

### Bitcoin Signing with FROST

Okay, speaking of Bitcoin, as I said we have an example that we've put together of how to actually sign Bitcoin transactions. And this shows what's involved, and it's somewhat complex. It's a nested process. On the very outside, which is marked in gold here, we have the Bitcoin methodology, which is we need to create a PSBT at the start and we need to transmit a PSBT at the end. And inside that, which is in dark blue, are what we added to the process because there weren't tools for these—which is we need to extract a sighash from the PSBT because that's what we actually sign, the sighash. And at the end we're going to need to put the signature back into the PSBT. And in the middle of that, in the light blue, here we have what ZF FROST actually does: we initiate that coordinator, probably frostd that I showed you, we actually have some number M people sign it, we aggregate those signatures, and then as I said we put it into the PSBT and we transmit it. But that's what the whole process is and that's why we needed to create tools to make this all work.

Okay, so that's FROST in a nutshell. We've spent the last two years evangelizing FROST primarily through developer and implementer meetings. The Learning FROST course with the tools we've created for it marks the next phase. We're now more directly supporting FROST developers with reference tools, demos, and a hands-on course that show how FROST works. This is a lot of what we try to offer for a lot of the work we're doing. We think the advantages of FROST are powerful, and we hope you do too.

### Future Work

And we've got more that we're planning to do. So we want to show more use cases for FROST signing, and we're doing that with updates to two of our CLI tools. We have Gordian Envelope, and we have our new Gordian Clubs, and we're going to be able to show signing with FROST with both of them.

But I think much more exciting are coordination examples. We want to resolve those coordination issues we highlighted here. How can we offer the UI benefits of coordination without requiring some level of trust in a centralized server? Our answer for that is Hubert, our dead-drop hub, which allows content to be dropped and then recovered using decentralized storage. We're already doing work on that, and we're just going to have a live demo of using it to do FROST DKG and signing next. We feel like this is really innovative work that is really not like anything else that we've seen there, and so we're hoping it's a great advancement for FROST.

So that's it. Anyone have any questions?

---

## Discussion: Edge Identifiers and Identity

**Christopher Allen:** I wanted to raise up one particular point. I see we have some people in the identity side of the community. I have written about the opportunities to do something called edge identifiers and cliques in a couple of articles. And the basic premise is that our single signature paradigm limits how we approach and think about identity. One person, one key that they very, very carefully protect, et cetera. But with FROST and some of these other techniques, MuSig2, et cetera, there are real opportunities to be thinking about things in a different way, including edge identifiers and group identity.

And then for the wallet developers that are our guests today, there's a lot of opportunities around sort of hybrid things. We tend to think about multi-sig from the point of view of multi-person—Alice, Bob, Carol, Dan, et cetera. But any one of those could be a device, a smart card, could be a service, could be a social colleague, or it could be a bank or some other trusted institution. And we can take advantages of the heterogeneity of these different types of things. So I think there's also a lot of interesting things where we begin to think about security and identity and how we have no single points of failure, no single points of compromise or design with tools like FROST that we really never had before.

**Shannon Appelcline:** Yeah, I think maybe edge identifiers, we should talk just a tiny bit more. The idea behind edge identifiers is that identity isn't really just about me on the internet. It's about us. It's about relationships. And so Christopher and I have a relationship. And an edge identifier would be a kind of link that shows that. And we could have a FROST group that is the two of us that would allow us as our relationship to sign things on that.

But it becomes even more powerful with clique identifiers, where you could have a whole bunch of people in a group that together form the identity for that group, organization, club, whatever you want to call it. And with FROST groups, we can have a whole bunch of people sign. We can have a threshold of people sign. And no one who looks at that signature is going to know that it is any different from any single person's signature. So that means that the clique or the edge, the relationship, it looks just like a single person on the internet, despite it being our collaborative power. So it's another great use case that should have been on my slide there on it.

**Christopher Allen:** Any other questions about the basics of FROST or what we're trying to do with our courses and why we're doing it on the command line to start?

**Shannon Appelcline:** Great. Thank you.

---

## Part 2: FROST with Hubert Demo

**Christopher Allen:** Thank you, Shannon. So now we're going to be moving to Wolf, who is going to be demonstrating using some of these same tools, but integrated with some early proof of concepts of Hubert. Wolf, go ahead. Hold on one second. Christoph?

**Christoph Dorn:** I'm sorry. I didn't realize we're moving on to the next topic. Just one question. If you have any pointers around—you mentioned that identifying that blob of ASCII characters for the purpose of the wallet so you know what you're signing. I'm just looking for pointers there, people working on that because I'm interested in that, identifying identifiers and then what you're signing.

**Wolf McNally:** Yeah, that's something I will also be touching on in my talk, but go ahead, Christopher.

**Christopher Allen:** Okay. Yeah, so this has been an ongoing thing for Blockchain Commons. So these Gordian meetings are largely for wallet developers, and we have begun trying to create some standards and best practices for how and when users make choices about things and what is the trusted UX for those. And then we've had discussions, but we don't actually have implementations of other kinds of issues in this area.

So for instance, Foundation Devices, which is a hardware wallet company, they basically have a trusted relationship between your iPhone app and the hardware wallet. And you really want to be sure that information the iPhone app has about, say, the current price or other different types of things is given to the hardware wallet, which has no internet access because it's an air-gapped communication. You want to know that that's trusted. So they've also been experimenting with some of these types of issues so that the hardware wallet, when it presents a trusted interface to the user of, "oh, this is what you're approving..."

So we have—our ideal is we'll have some standards for this type of stuff and that it'll be available for the entire community to adopt. But I will say right now it's really early. I think this is one of the areas of trusted UX is underfunded at this point. We have a tendency to just slop the bits into a text field as hex, and that's not acceptable. So I hope that answers your question. Wolf, let me say a little bit more.

**Shannon Appelcline:** In this particular case, I think it's really difficult, and it's not an easy problem. And that's because we're just using a general signing tool here, and clearly we could send it anything to sign. And so it has no way of knowing how to turn what it's signing, which is going to be hex code fundamentally, into anything rational.

But I think if someone's building a specific tool, they are more likely to have a specific use case. And so what I'm signing in this example, it was just a text message. It was board minutes, theoretically. And so that was just turned into hex and it can be pulled out of hex with XXD, which section 2.3 of Learning FROST shows how to do that. If it was a Bitcoin transaction and you know it's a Bitcoin transaction, then you could similarly look at the hex and say, "you're sending this money to this address."

And so I think besides the best practices that Christopher mentioned, part of those best practices have to be: you should know what you're signing and you should be able to break it down for the person. I think that fundamentally is what has to happen. And it means that you probably can't have a general signing tool like what we're using for real pragmatic use cases.

**Christoph Dorn:** Yeah, I think that that's getting at my question. What can you embed in the message that you're signing? And if there's standards evolving for referencing schemas or pointers or anything within that message. But yeah, okay.

**Wolf McNally:** Yeah, we're going to be talking a little bit about that.

**Shannon Appelcline:** Because a lot of the work we've been doing is on creating standard ways with URs to look up—hey, I can see what this is. If you were able to pull apart a message and see it's a UR, then you could, from the UR, know what type of UR it is and know how to decode it from there. Any thoughts on that, Wolf?

**Wolf McNally:** Yeah, absolutely. I mean, transparency is a huge part of security and sovereignty and all that. And so especially a lot of the work we've been doing with Gordian Envelope is to make messages that are semantically meaningful, but also support security and privacy. And if you want to introduce my talk—introduce myself—we can separate this out as a separate video if you want. And then I'll dive into that because I'll be touching on these things.

### Object Information Block

**Christopher Allen:** Okay. Yeah. So we also have begun to recommend best practices for even what the UX looks like. So we have something called the Object Information Block, or OIB, which is right now focused on cryptocurrency transactions. But we're trying to basically say, hey, since users have to do a variety of different choices when they're looking at a Bitcoin transaction or looking at a key or looking at various kinds of objects, how do we make those very clearly different and such that users can actually make some decisions?

So we've begun to make examples of this available, mostly right now around key management. So we have things like LifeHash, which is a visual identifier so that you're not just looking at the last four digits of a hex value, but also looking at the security requirements because even a LifeHash by itself is not sufficient. You want to have things behind it and you want to have things beside it in parallel. And then we basically have to render this all on a screen.

I mean, for instance, with Foundation Devices, they're very concerned about trusted hardware. So they actually had to find a screen for their hardware device that let the CPU—that has the keys anyhow and has to be trusted because it's got the keys in it—let the CPU run the screen rather than the screen having a little chip in it that could maybe then change something if it was compromised. I mean, that's the kind of things we have to be thinking about when we're talking about trusted UX.

So we're all puzzling around this area. The point of Blockchain Commons is to be a coordination point for a lot of these types of discussions. And then we do a lot of the basic research and then work with various parties to basically go, "Hey, are two of you interested in interoperating? Yes. Okay, well, let's all work together to create a specification for that." I don't want to call it a standard. We're not a standards organization, but at least a well-defined specification. And then as that emerges, grows, the idea is we might submit it to a standards organization like we have with DCBOR in the IETF and some other things. So I think that explains our approach here. Yes, thanks very much for taking the time. It's great.

**Wolf McNally:** And just to add one more point to what Christopher was just saying, the whole idea of the Object Identity Block is an arrangement of elements in the UI that allow people to recognize digital objects as easily as they recognize human faces. So if it's your key, if it's your transaction, if it's your anything, it's distinct enough from anything else that you don't have to parse with your mind a huge string of hex digits to see which ones are different or whatever.

And in fact, I'll touch on that a little bit as I show you our Uniform Resource or UR format a few times in the upcoming demo. So with that, shall I jump in, Christopher?

**Christopher Allen:** Yeah. Yeah. Do the intro as usual.

---

## Wolf McNally: FROST with Hubert Presentation

**Wolf McNally:** Okay. So my name is Wolf McNally. I am lead researcher for Blockchain Commons. And this is going to be a demonstration of FROST with Hubert. Now, the previous part of this demonstration might be part of this video, might not. It was about FROST specifically—Flexible Round-Optimized Schnorr Threshold signatures. This is going to be about how we are using a new coordination layer called Hubert to do FROST ceremonies, threshold signing ceremonies.

### What is Hubert?

So let's talk a little bit about what Hubert is because it's really kind of novel. It is what would be called a dead drop protocol that facilitates secure multi-party transactions. So let's break this apart.

Participants in any multi-party protocol write once to a storage layer using random keys. And the messages contain random keys for expected responses. And I'll talk a little about how this relates to dead drops more in a moment here. But what this does is: if I tell you where to leave the next message when I send you a message, and you tell me where to leave the next message when you send me a message, then we can continue that conversation indefinitely, never revisiting the same place twice.

So this part of what Hubert provides is complete opacity to outsiders through both steganography as well as end-to-end encryption. Well, Hubert actually provides the steganography, as I'll talk about in a moment, and then we have other layers of our stack that provide both authentication and end-to-end encryption. The important thing about this is there's no central server required for coordination.

In our previous FROST demo, we showed that we used a coordination server to coordinate the FROST signature. That is not necessary in this case. You still have an actor whose role is coordinator, but there is no central server to take down, to censor. This enables trustless operation using public distributed networks that are already in place. And basically to bring down these networks, you'd have to bring down the whole Internet.

### The Dead Drop Metaphor

Part of my inspiration for this was traditional spycraft, the idea of a dead drop. And a dead drop is where two people want to communicate and they agree on the time and place, or they agree at least on a place and method in advance. A person leaves a message, in this case, say in a hollow rock, and then they or somebody else leaves a signal, which indicates that the drop has been loaded and is ready to be serviced. The phase two is the retrieval where the agent in the field sees that the signal has been sent. And then at some time later, visits the drop site and services the drop—servicing the drop is what it's called. And this provides secure one-way information exchange without direct meeting.

Now, there is usually—well, there has to be a phase before this, which is often called the brush pass in spycraft, which is where people exchange this other information. What is the time and location? What is the method of drop? Things like that have to be known in advance. And you'll see how that manifests itself in what we're doing here.

### ARIDs: Apparently Random Identifiers

So as I mentioned, you need a location. And so our location is one of our proposals—what I call proposal not a standard, because as Christopher was saying, we're not a standards organization—called an ARID. An ARID is an Apparently Random Identifier. And this is distinguished from, say, UUIDs. UUIDs are 128 bits, and they have certain bit fields in them, like the version of the UUID, sometimes date, or sometimes embedded IP addresses, things like that. Our ARIDs basically are 256 bits of pure entropy, or apparently so. That's what Apparently Random Identifiers means. And they can refer to anything. So they're generally designed to be identifiers for some referent, but they cannot be correlated to anything because they're basically statistically random related to that. And so with Hubert, ARIDs are addresses of cryptographic dead drops.

So just a little thought about the scale of ARIDs—256 bits. I'm sure if you're familiar with UUIDs, you're familiar with the idea that if two people generate UUIDs, or in this case ARIDs, there's basically an astronomically small possibility of them ever colliding, and so they're generally considered collision-proof.

Just to demonstrate the scale of an ARID, which is twice the size of a UUID: You can think about the size of the universe, which is about 3.57 times 10 to the 80 cubic meters. And you can divide that by the number of possible bit combinations, which is all 256 bits. Then you get into cubical volumes. You get basically volumes that cover the entire universe that are about the size of a house or a four-story building. So the idea is that your dead drop could be anywhere in the universe in volumes the size of a house. So that's not very useful information, but I think it's kind of useful to think about the kind of scale we're talking about here and the chance of collision.

### How Hubert Works

So relate that now to Hubert. So the initial out-of-band exchange, or as I called the brush pass, is where a sender puts something into the Hubert storage layer, which we'll discuss in a moment, and uses an ARID to do that. And they also exchange that ARID as well as a set of keys with the receiver. So basically the sender and the receiver are known to each other.

Now, we have two actors here, the sender and receiver, but this can be any number of actors, as you'll see with the FROST demo in a moment. So the sender loads the drop, which creates the next message and also includes in that encrypted message the next ARID. Then they pass it into the public distributed network.

And we support two public distributed networks: what is called the Mainline DHT, which is part of the BitTorrent network, and the other is the InterPlanetary File System, or IPFS. And in spycraft, this is what would be called "living off the land" or using whatever tools you have available. This is not required to spin up any separate servers. Hubert is a protocol, not a server.

We do include with our Hubert command line tool a server which accepts HTTP posts. It's not necessarily encrypted, but that's primarily for debugging. And if you just have a local system you entirely trust. But in deployment, you would use DHT or IPFS or our hybrid system, because DHT has strong limits on the size of messages you can put into the network—about a thousand bytes—which is pretty small for a lot of purposes. So you can use IPFS, which has higher latency but also has a much larger message size.

Both these networks basically let you store things in them for free for limited times. That basically means if you want them to be stored longer, you either have to refresh the message, or in the case of IPFS, you have to do what's called pinning the file. In which case, as long as it's pinned on one or more servers, it will stay available to the entire network. These things tend to time out after anywhere from 4 to 12 hours, depending on the protocol and the nature of network congestion and so on. But generally speaking, if you're talking about real-time exchanges of information through Hubert, there's no real limit here.

### Steganography and Encryption

As you can see, we're mentioning steganography here. Everything that Hubert places into the network has been obfuscated, which means if you know the ARID at which it was stored, then you can deobfuscate it. But even then, it's encrypted because we're using a layer called GSTP, the Gordian Sealed Transaction Protocol, to actually both authenticate, sign, and encrypt the message to the receiver or receivers. And we can do both multicast as well as singlecast messages.

So once a message is in there, a person can be polling the ARID they were given in the brush pass. And then when a message appears there in Hubert, which is in any of these networks, then they can retrieve it, deobfuscate it, decrypt it, verify the signature, create their own message, and then send it back through Hubert. Because the message contained the next ARID, they'll know where they're looking for the response. And that can continue back and forth among as many parties as you want, as many times as you want.

As I mentioned, we support the BitTorrent Mainline DHT distributed hash table, IPFS. And again, our server supports both SQLite as well as just ephemeral in-memory RAM table, essentially. And our other video on Hubert, I go into much more detail about the various kind of parameters of these storage layers. So if you want more detail about Hubert itself, you can check that out.

### The Complete Stack

So just to kind of summarize the whole stack: at the top is the application layer, any secure multi-party app—not just FROST, but FROST is what we're going to be demonstrating here. Then Hubert threads coordination routing through linked dead drops and ARID-based routing. Then it also threads the steganography layer, which is why it's called the "Gray Man Strategy," which basically means everything, every packet that Hubert places into these networks is indistinguishable from noise.

The storage backends themselves are Mainline or IPFS or a hybrid system, which decides which one to use based on the message size. And so if the message exceeds a thousand bytes, it only places a pointer in IPFS—also obfuscated—but places the actual message, which can be obfuscated up to 10 megabytes, in the Mainline DHT. And so everybody refers to IPFS first. And if you're using the hybrid method, if you discover a pointer, then that refers you to a second ARID, which is in the Mainline DHT. Without the ARID, everything here looks like noise.

Under that is the Gordian Sealed Transaction Protocol, which the actual messages themselves are both encrypted and authenticated using this protocol. And the messages themselves are built on Gordian Envelope, which is a smart document format, which has structured data, is designed for easy use with semantic triplets, and supports security and privacy-minded features like holder-based elision through a Merkle tree format.

And we have, again, videos about all these things, and everything is open source, and so you're welcome to explore it. Reach out to us. We'll be happy to meet with you, help you explore these things. But definitely, for Gordian Envelope, which is one of our major contributions, we have a whole playlist of videos on YouTube that I can't recommend highly enough.

And then this is built on DCBOR, Deterministic CBOR. CBOR is the Concise Binary Object Representation. You can think of it at a high level like JSON, but binary. But CBOR itself is used for quite a few different protocols, especially Internet of Things and cryptography, because it's binary. It doesn't have any extra kind of weight to it. We've refined that further through a series of narrowing rules we published as an IETF internet draft called Deterministic CBOR or DCBOR that basically kind of eliminates some of the foot guns when it comes to actually deterministic consensus-based protocols, which are important for not just cryptocurrency, but other kinds of things as well.

### Uniform Resources (URs)

All right. So one other thing that you'll be seeing a little bit in this demo is our URs, Uniform Resources. UR is a form of URI, or Universal Resource Identifier, but it basically encodes a binary object, and that binary object is always tagged DCBOR. But it encodes it in a way that's easy to handle as text, and particularly originally designed for air-gapped protocols like QR codes.

So it's not case sensitive. It's designed to use a very efficient ASCII method—or close, near ASCII method—that QR codes support. So it's not very heavyweight at all, especially when you compare to, for example, Base64 encoding in QR codes. URs are always recognized by their scheme, which is `ur:` followed by type and a slash, and then a series of letters called bytewords. And I don't really need to go into those any more deeply right now, but now let's get onto the demo.

---

## Live Demo: FROST with Hubert

**Wolf McNally:** Which screens here. Now you should be seeing my Visual Studio Code view here. What you see here is a Mermaid diagram, a sequence diagram. And we're going to quickly kind of cover the phases of the protocol of what we're dealing with here when it comes to actually implementing FROST using Hubert.

### The Brush Pass

I mentioned that spies talk about the brush pass. The brush pass is where you decide who you trust and you basically learn how to authenticate each other and send encrypted messages to each other. One layer I could have shown in the previous layers diagram is called the XID document. XID stands for Extensible Identifiers. We've done a presentation about that as well on YouTube.

But each person generates—you can think of it as a set of keys as well as other permissions and contact methods and things like that. It's your calling card, but it includes both your encryption key and your signing key. So if somebody has your XID document, they can send messages to you that are encrypted just to you, and they can also verify your signatures. So you can think of it as kind of a little keychain, and you can include even your private keys in XID documents, but elide those before you send them to other people.

So this assumes you have some kind of secure channel that you trust. We use Signal as an example here. And since these XID documents can be encoded as URs, they're just text. So you can generate your XID document, paste it in Signal, say, "Hi, I'm Alice, here's my XID document." Other people—maybe who know to trust you are already invited into the group—they also send their XID documents into it: Bob, Carol, and Dan send their XID documents. And then each person collects the XID documents they don't have. This is the brush pass, this is the introductory phase. Then you basically have what we call, with our FROST tool, the registry of people that you trust and that you communicate with.

### DKG Process

And again, we have basically a demo log here which goes through using our tools step by step. Each of these code fences here are actual commands you can actually execute in order in the shell. This is where Alice generates her private keys using `envelope generate prv-keys`. This is where she generates her public keys from the private keys. This is where she creates her XID document. Then she creates her public version of her XID document which has the private key omitted. These are all the keys. And then we do that for Bob, we do that for Carol, we do that for Dan. And then Alice builds her registry using the other participants' public XID documents as you can see.

And you can see these are all URs—this is `ur:xid`. These are the bytewords. Now I spoke a little bit earlier of identifying digital objects. With the UR, the last eight letters are a CRC32 of the rest of the message at the binary level. And so you can tell if you're looking at the same UR by always just checking out the last few letters. If they are the same, it's got to be the same XID or the same UR in general with basically an extraordinarily small chance of collision.

So you'll be able to see later on, I'll show you that certain messages might be very long in URs, but by just looking at the last few letters, you can determine that the UR is the same or different. So Bob builds his registry, Dan builds his registry, and then the next step goes into the distributed key generation.

Let's go back to the sequence diagram here. The distributed key generation involves, again, Signal. This is where you have to basically publish something—to publish your initial invite ARID. And that's so Alice, the coordinator in this case—and there's no reason why Alice can't also be a participant here, we're just keeping them separate for here—but she calls `frost dkg coordinator invite`, which produces the invite and sends it into Hubert and then gives back the ARID at which it's been published. At which point she can publish the ARID—just the ARID by itself, not a whole big document—to Signal.

The other participants get that. And then from then on, it's mostly automated. They have to receive the invite. They grab the invite, which is actually a multicast invite. So this—you can see it's BCD—it's the same message. That way they can all agree that they're doing the same thing. However, the return ARIDs they are going to respond on are separately encrypted to each of them. They can't even see each other's ARID. And that basically keeps a separation, a security wall between them. So they don't even know where the others are communicating back to Alice with.

So then Alice basically collects those responses, which include their round one responses, or they can affirmatively reject—say, "No, I'm not interested in doing this." Alice collects the responses, distributes round two. They respond back. And again, ARIDs are being passed both ways here, so they know where to respond. Alice runs round two, then the participants finalize, and Alice finalizes.

And this is all—notice that after the actual decision point here, where they're deciding in round one whether to respond—everything here is automated. So how does that actually play out in reality?

### Demo Execution

So I'm going to show my terminal here, where I've already run the FROST tool to do the distributed key generation here. This is a bit of "Julia Child pulling it out of the oven" thing, but I'll show you the actual signing ceremony running live in a moment here.

In this case, I have a demo scripts folder, which is going to be part—and by the way, all this code will be released by the end of the day. At the end of the day, you will find the FROST crate that will be published to crates.io, where if you do an install on that, you'll basically have this. You also want the envelope crate, which is our envelope command line tool and so on to experiment with that.

But so basically we run this test script, which is `coordinator dkg`, and then it creates the invite and posts it to Hubert. And we're using the hybrid layer of Hubert, which basically means short messages go to—I'm sorry, shorter messages go to Mainline DHT to 1,000 bytes. If it was longer than 1,000 bytes, which the invite always is if it contains like three other participants, there'll be a pointer placed in Mainline DHT with the actual message in IPFS.

You can see that actually took 50 seconds to do it. And you can see the elapsed time is listed on all these here. And it's not instant. If you run our local server and do this, it is instant. This all happens like instantaneously. But if you're using these distributed kind of consensus network kind of things that are global, it can take a few minutes to run these things, which is why I don't want you bogged down the demo by having too much of this run right now. We may or may not let the actual signing demo run to completion here.

### Signing Ceremony

Let's quickly review what the signing process looks like. It looks very similar to the DKG. It's actually a little bit shorter. So in signing, Alice decides on the envelope that she's going to compose to sign.

If we go back to our demo log here, this is the preview of the DKG invite. This is what an envelope looks like when it's been kind of put in the human readable format. It's a binary format. But as you can see, it's hierarchical. It uses this subject-predicate-object format. So this is the response ARID, and this is an assertion on it. It has a receipt continuation. I'm not going to get into encrypted state continuation. We talk about that in our GSTP talks.

But this is the actual sender, including their keys. And so the idea is everything you need—not to trust this, but to know that it is in fact sent by the person that is in your registry and so on—is here, as well as the actual data you need, the round one package. And this is actually embedded JSON. There's no reason why you can't embed JSON inside CBOR, inside an envelope, or embed an envelope inside other things. So the envelope is a binary format. So if you were to embed it in CBOR, then in JSON, then you'd probably Base64 encode it.

Okay, so I'm going through this here, down to signing. That's the DKG. And so as you can see here, at the end of the DKG, verify group key across all participants. Once all the messages have been exchanged, Alice, Bob, Carol, and Dan have the public signing key, which is part of the FROST requirement that you have a public signing key—the private key never exists in any one place. But you can see that these are all the same. Even though it's kind of easy to see that in my inspection, all you need is look at the last few letters and you can see these are all the same. They all have the same public signing key.

Then when it comes to signing, you have to compose the target envelope for signing. And so there's a few invocations here of our envelope tool. `Envelope subject type string`—and then there's the string. Then `envelope subject type`—they basically add an assertion to it, which is an assertion where the predicate is the purpose, and the object is another string, "threshold signing demo." So this is basically a complete envelope. And then we wrap it, which is these outer braces. I think I'm not going to get too much into the detail of envelope. It's a very simple and very elegant structure. Just encoding a string as an envelope takes like five more bytes. I encourage you again to watch our envelope playlist on YouTube.

But once you have that, this is ready for signing. And then that gets placed into the invite. Back to the sequence diagram. Alice, or the coordinator, composes the envelope or receives the envelope to be signed, creates that sign invite, which again is a multicast, puts the ARID in the Signal group. And I should say, these Signal groups can also be ephemeral themselves. You can do this initial work in the Signal group and then basically delete the Signal group. So there doesn't even have to be a record of that as long as you trust the people that you've invited in and trust their keys they're giving you as well. Your bootstrapping trust is not an issue we're solving here. That's always been a problem with cryptography.

So the participants receive it at that point. That's their only decision point. They actually decide whether—because they can, because the invite contains the whole target envelope to be signed—they can actually audit it. And this gets back to the question I was asked earlier. How do you know what you're signing? You'll actually see that as part of what we're doing here.

Then it goes to round one, collection, round two, collection, and then finalize. And by the time this is all finalized, everybody has the same signature and the same signed envelope.

### Running the Live Demo

So how does that work out in real life here? So let's bring up our terminals here. I'm going to clear the terminal here. I'm going to run the coordinator script with `sign`. When I start this, it's immediately going to create that target envelope you saw, and then it's going to start posting it to Hubert. Now, it's already doing that right now. It's actually sending the target envelope into Hubert. And these people aren't listening for it yet.

So I'm going to clear that, clear that, clear that. All right, now I'm going to run Bob. These are just basically shorthand scripts. They all run the same basic participant script underneath. I'm going to say Bob sign, Carol sign, and Dan sign.

Now I'm waiting for the ARID. The ARID hasn't been delivered yet because it hasn't been posted to Hubert. If something went wrong at this phase, then whatever ARID was returned would be useless. But so I'm waiting for it to finish that part of that path, essentially. Which in this case, because the invite is probably more than a thousand bytes, it's actually determined that putting the pointer to another ARID in Mainline DHT.

Now it's basically published the ARID to us. So we would publish this back to Signal. Now notice it's waiting for the responses already. And it's got a timeout here of 1,000 seconds. That's more than enough for our purposes here.

So now I'm going to just paste this. You can do the same order. I'll paste Dan's next, Carol's, Bob's. There we go. All right. So now they're all running.

So you see that they already retrieved their invites. These two retrieved their invites. This guy's behind that. Now notice that it's actually printed the same target envelope here. It's given us—it said, "Here's the group." This is a group that was formed. This is an ARID of the group. So you can determine if this is the group you joined earlier. You can see who the coordinator is. This is your pet name that you put in your registry for these participants. These pet names here could be unique for each reference. It could be an email address and so on, but they're the names you know these people by.

The invite declares the signer, the minimum number of signers, and who the participants are—highlights you as one of these people. So she's Bob, Carol, and Dan. It shows the target envelope. This also happens to be showing you the return address, which you don't really need to know as a participant, but it's there.

And then now you can see that Carol's has already been received by the coordinator here. She got that out. And again, because latency varies, it can take some time to go back and forth. But the coordinator is still waiting for Bob and Dan to reply. This is still uploading. This is still uploading. This is waiting for the round two request currently.

Now, Dan has completed his upload. This has immediately registered the download here. It's just waiting for Bob. The coordinator messages here that we're waiting for more than one—it's asynchronous. Now, basically, now it's uploading the round two requests because round one have all been—they've all confirmed basically.

Now, there are ways of doing this such that if you get just a quorum, you can abort because two is really all you need. But we're letting everybody respond. Even though this is two of three, we're just letting everybody respond. We could technically shut people out once we have two, but we're not bothering at this point.

Anyway, so you can see that these are counting down now because they have a timeout. This is counting up to show you the elapsed time of the amount it took to get these messages up. Since it's doing it in parallel, it's going to be the maximum of these. So now we're waiting for the dispatch of the round two messages to go up from this.

There we go. Two of them have now confirmed, which means they should—you should see them over here. Here's Carol's, here's Dan's. Now they're uploading their signature assertions.

**Christopher Allen:** Note that as each of these go around, you're not having to pass the UR because the state is actually encrypted by each party for itself when it's returned. So that's what allows these round trips to be all automated without more ARIDs being sent back and forth. Because in fact, it doesn't even need to know what the ARID is that it sent to them because it'll get back.

**Wolf McNally:** Well, actually, that's not quite true yet at this version because we're not using the encrypted state continuations for that at this point. We are actually storing local JSON files for each person. But Christopher's not wrong. You could basically take all the state that we're currently storing locally for each user, which includes their own XIDs and so on, and put those in what's called encrypted state continuations, which is basically where you encrypt something to yourself, you send it to the other parties, and then they are required to send it back to you, still encrypted. They can't read it. It's a black box.

But if you don't receive it back, then you reject the packet. And GSTP, that's one of the features it supports—is encrypted state continuations. So in this case, we are storing local JSON files with some of that state in it, but that doesn't necessarily need to be the case. It could be entirely self-contained. And because you basically self-signed and self-encrypted the encrypted state continuation, other people get it. It's just a black box. They have to send it back with their next message or you reject the message. And that actually could do that function.

**Christopher Allen:** So some of the advantages of doing that is you might have a hardware device that can't store a lot of state, or you may have a network device that is either a microservice or is a high-load service. And again, state becomes a problem. So we're really trying to think long term here into some of the different kinds of things that will make this a powerful protocol for the future.

### Results

**Wolf McNally:** Right. And so you can see it's almost done. In fact, the participants have already received back both the signature and the signed envelope. You can see these two are the same.

And now that this is finished, this is waiting for its finalized package. Now they're all synced up. You can see both the signature, like DALR, DLR, so on, they're all the same. Signed envelopes, EINZS, EINZS, and so on, they're all the same.

Just to kind of double check that, let's actually copy this UR from the signed envelope. And we're going to run `envelope format`, and then paste that UR. What you see here is the original target envelope that is now—because it's wrapped—the whole thing has been signed. And then an assertion on that wrapped envelope, which is `signed with` an ED25519 signature.

Currently, we're primarily supporting the ED25519 suite, but we can also substitute in secp256k1 Taproot and so on. But for the first release of this, we're supporting ED25519. But this signature was generated using FROST without the private key that generated the signature ever actually existing anywhere.

And that concludes the demo. I'm definitely open to questions.

---

## Discussion and Q&A

**Christopher Allen:** So this, in many ways, is the culmination of not just the last two years of work on FROST, but also of the last seven years of Blockchain Commons when we started. It's the ability to do this type of tooling that allows for you to be able to interact in adversarial environments is very powerful.

That's not to say that every transaction needs to use this at the bottom. As we demonstrated, you can use a REST protocol to do all of the different functionality here that would be like any traditional service, and it has some efficiency. But I feel like in order to truly decentralize and protect a lot of the different types of things that we're doing, we need to be able to have the option of being able to go down another level further and get a greater level of protection. When we're under siege in various ways—where it's efficient, I don't mind efficient and safe, I don't mind using a less decentralized or even potentially a centralized tooling, provided that there are options to be able to move into these fully decentralized type of approaches.

### Question on Key Compromise

**Wolf McNally:** Christopher, there's a question in chat from Henk van Cann. He said, "Great work, got a question. Most probably because of lack of knowledge—as far as I know, dead drops, so signatures do not solve the problem of key compromise. How does Hubert trace the key compromise?" And so I'll respond to it.

Well, I'm not quite sure about "trace the key compromise," but FROST itself is designed to deal with the issue of key compromise because the actual signing key for the group never exists—because it's multi-party computation. It never exists anywhere, literally. It's just that the key shares being combined for a particular signature in a particular session create the signature without the private key ever existing.

Henk, do you want to kind of... Well, he may not be talking about compromise.

**Christopher Allen:** You're correct on the compromise thing. But there is—if you have one of your participants who is in fact an adversary, so a Byzantine-type situation, they could send bad data to basically deny the signature from happening. And there is an extension of FROST called ROAST, which I think adds one more round per participant. I don't remember exactly, but it's still FROST. It's just an additional stage in there that allows for the coordinator to basically go, "Oh, wait a second, one of our participants has been taken over by an adversary and is trying to deny this from happening, so we're going to use the other ones instead."

So this has been well thought out and covered. There's a number of things that we would like to be able to do as these become more mature, but at least the feasibility of ROAST and such. I mean, we're also right now largely using the Zcash library because it's the one that has the most security reviews at this point. It's also in Rust, and our stack is in Rust.

There's a very good library by the folks behind Square that has been evolving, but it's Bitcoin curve only. But it has some interesting optimizations around some of these types of things. I think we could very easily use the C bindings into Rust and use that library when it's appropriate. And then there are other people that are using other kinds of DKGs that have other kinds of interesting properties.

Because again, once you have the DKG done, the signing operation is done on those DKGs. But there are multiple ways you can create that DKG. So there's already one wallet that's using a completely different way of creating the DKG. They're using it for Bitcoin mining for like a thousand miners, where the thousand miners are cooperating in one of these ceremonies. And their DKG has some different properties. But because it's an independent company organization, they don't have the security reviews that say the Zcash Foundation funded to have a third party review of the library we're using or the financial resources of Square to do the review of their C-Stack.

But I think the main point that I'm making, the whole point of all of this is that it's possible. We just demonstrated it works. You can do this. It is feasible. And what we need is for you to go out and communicate to the rest of the world that, yes, this is possible. Let's invest in doing these types of technologies and taking them from these proof of concepts into actual pilots, get them reviewed, get them security reviewed, etc.

Because I think they really offer some very powerful functionality—not just for security, no single point of compromise, no single point of denial, no single point of correlation. But I think they also have some real opportunities for new kinds of ways of working together, for collaborating together, whether or not it's through edge identifiers or different kinds of thresholds, various kinds of decision making type of things that can be adjudicated by cryptographic policies, as I talk about in my Exodus protocols paper and things of that nature.

**Henk van Cann:** No, I'm fine with the answer because obviously I don't have the right knowledge about FROST that upfront makes it impossible to do the key compromise. I didn't know that.

**Wolf McNally:** Yeah, that's the main point of FROST. Of course, in terms of the XID documents that contain your encryption and signing keys and all that, you have to protect those like any other private keys because people can spoof identities if they have them.

### Question on Context and Adoption

**Christoph Dorn:** Yeah, great. Thank you. That's amazing. I'm wondering a bit about the context. Just bigger scope. Why is it until now that something like this exists? Why haven't people worked on this sooner? And what do you see the challenges for adoption here, right? Because the concept could have been proven maybe five years ago, potentially. I don't know about the detailed technologies and algorithms involved, but overall as a concept. So I was just wondering what your thoughts are on that. And this is very timely for me.

**Wolf McNally:** Before you answer, Christopher, let me just say, I'm the lead researcher. I've been doing the technical kind of design and implementation. But I would say the short answer I would give to your question, Christoph, is Christopher's vision. And he's thinking actually farther ahead than a lot of people. And so what we're presenting to you now is a result of Christopher articulating his vision to me and others and advocating for that, that has basically gotten the rest of us off our butts to actually show that what Christopher has been envisioning all these years is actually viable.

**Christopher Allen:** Thank you. Yeah, I mean, some of this—I think I did a talk in 2016 at Stanford on how our assumptions around single key cryptography were limiting us. And then later in 2017, did a talk on smarter signatures about what were the opportunities of smarter signatures. I really think our single key paradigms have limited us.

And I don't want to call it a conspiracy theory, but there's really been very little incentive for centralized organizations and governments to break out of these paradigms because they basically—oh, well, trust us. We're going to do this trusted engine chip. We're going to do the next generation of the clipper chip, but we're not going to tell you it's a clipper chip. We're going to standardize that everybody has to use this particular method of doing things that we've been doing for 10 years.

I can't tell how many times in the community group or in working groups at W3C or IETF, I'm told, "Oh, well, you can't do that because it'll take 10 years for NIST to standardize it." Or then the 10 years happen, they said, "Oh, no, you have to work on quantum resistance schemes." And we've already added quantum resistance to our envelope stack.

So there's an element of a lack of vision here and a lack of thinking about it architecturally that has been suffering. And so I just keep on pushing the edge here to say, I know it's possible. Let's do it. And it's just taken a while for it to happen.

Specifically for FROST, I've always loved the aggregatable ability of Schnorr, but it turned out that doing it naively, there are some interesting attacks that people learned in the teens that really hurt it. And I know for instance when I was at Blockstream in 2016, we presented a version of MuSig that somebody basically pointed out didn't completely solve it. So that's why it's called MuSig2. FROST didn't come out until 2019, and it really was the first one that had the right formal proof all the way through.

And it just took a while for FROST to get through all the various kinds of reviews. It's been very, very well received. There's been really no—there are hundreds of deep cryptographic papers analyzing it, looking at it, extending it, and there have been really no fundamental issues with it. And then now with the Zcash review of the code, which I think was a fairly large contract with a third party to review it, we finally have code that I feel like is sufficiently reviewed.

So there's an element of that as well. But that probably could have been another five years earlier if we thought it was a priority, if the community had thought it was a priority. And it wasn't.

**Wolf McNally:** Christopher, as far back as the 70s, you were working as part of the Xanadu Project. Well, I think you spoke about this in our last Hubert talk, which is on YouTube, where you were talking with Ted Nelson and other people about what they were doing and liking what they were doing, but wanting it to be distributed. But even a number of things were just starting to emerge at that time, like public key cryptography. RSA and stuff like that was brand new and under patent and couldn't be used by anybody in a reasonable kind of open way.

And so part of what this has been going on is we've had to wait for these things to age out. Schnorr—I'm not sure when that left patent, but that was it.

**Christopher Allen:** Yeah. Merkle tree was under patent, right?

**Wolf McNally:** Yeah. So all these things that we're relying on now, we had to wait for the 20-year patent to expire before we could actually start using them widely and in an open way. But we reached that kind of critical threshold of available technologies now. And at least those patent periods have allowed them to prove themselves as valuable. And now they're available publicly, which is the whole purpose of the patent system. Why the patent system was originally designed was for a time to give creators exclusive rights to their inventions. But after that, they entered the public domain. And that is the system working as it was intended. And now we can do these things.

**Christopher Allen:** Yeah. And also I'll say, we pretty much had from the beginning when Blockchain Commons was founded that all of our work is not just open source. It's also under a BSD plus patent license, which we're basically saying we're making no patent claims on any of this. We're part of a couple of patent alliances to prevent these things.

So I will say there's an element of me getting this particular product out and doing this video today such that if somebody comes along and says, "Well, no, this is patented by our patent troll organization," I can say, "Nope, we've been talking about this for years. And here it's actually implemented and it's got real working code. Your patent's invalid."

And there was literally—I didn't even realize—yesterday was the end of a comment period. They're talking about changing some of the patent rules for how smaller organizations can get patents overturned, that was very much not helpful for—or it supported the patent troll way of doing things. And I and EFF and a number of others have been trying to offer some counter testimony to it because that's going to be the next emergent technology problem—is that some patent trolls basically get a hold of this and try to sue everybody who's doing anything interesting out of business.

I know it was a big factor in the origin of Bitcoin. I'm fairly certain that Satoshi was very well aware of the patent things going on at the time and knew that by the time he or she released Bitcoin, that at least everything that was in Bitcoin was no longer under patent. And it might have been able to come out a few years earlier if it hadn't been. So that's my little diatribe on that. That helped, Christoph?

**Christoph Dorn:** Yeah, very interesting. I came across your work 10 years ago with the SSI principles and I was working on—I always work, I'm self-taught. I just love software. I play with it. I build for myself. And one of my problems was always, how do you maintain complex systems by yourself? If you want more control over boundaries in your system and how things work, how does that actually work.

And then over the years, that's to me as a sovereign system. If you have control over the boundaries of the system, it's sovereign. And if that's private and you have control over the internals of that system, how it's provisioned and so on, that's then an extension of you that is secure.

So I've been looking at these patterns for a decade now, and I've been waiting for implementations. And it's very exciting to see these things work in a way where if I deploy them, they're going to be secure because I'm leveraging pieces, like you said, that have been well vetted and are ready for this now. So, yeah, it's a fascinating time.

### Infrastructure and Living Off the Land

**Wolf McNally:** One thing, Christoph—when Christopher and I were talking about what eventually became Hubert, I had proposed a coordination point, which would be a server. Which of course Hubert has a server, but that's not going to be its primary modality. And Christopher said, "Well, what about these distributed hash tables and other public networks like that?"

And I looked into it and realized—we also talked about how we've set up a number of servers for Blockchain Commons use, but running a public infrastructure is a big deal. You have to get a lot of people on board. And when Christopher proposed, "What about Mainline DHT?" I scoffed at first. I said, "Well, it's only a thousand bytes, and what can you do with that?"

And then I realized, oh, but if you pair it up with IPFS, then you can actually live off the land, as they put it, and basically go with the public infrastructure that's already there for certain things. So obviously, if you want a big thing pinned inside Hubert, you can do that, but you do it through IPFS and you pin it in one or more servers, and then it stays forever. And that expires after a few hours unless you proactively refresh it, both in DHT and/or Mainline.

But the idea is that that whole idea of—wow, now Blockchain Commons has to set up a server so people can do this kind of distributed FROST transactions through it that runs our Hubert protocol—goes away because the infrastructure is already actually there and being run by tens of thousands of people around the globe.

**Christoph Dorn:** I love it because it's a pattern and I think patterns are needed now and those patterns can be implemented in different ways. And the problem you're solving with announcing data online and coordinating—that's a very difficult problem to solve to keep private, right? Secure it for exchange. And yeah, I think this has a lot of potential. So we've seen a lot of evolution in end-to-end encryption.

**Christopher Allen:** And there's still some things that we want to add to our own standards in the area of perfect forward secrecy. I mean, I was the co-author of TLS and very much pushed that we supported that, even though it was not technically feasible. It took another decade before servers began to do it.

But there's been a lot of evolution in that for end-to-end encryption. But then we have the problem of all the end-to-end encryptions are correlatable to the endpoints. You can know that somebody is using one of these. And it's difficult but feasible to be able to even identify that somebody at this location in Iran is talking to somebody in this location in the United States.

And I wanted with this to even take it a little bit further and say no. There may be a good reason why an organization like Human Rights Foundation is trying to give some funds to an advocacy organization in Iran that may be at risk if they know Human Rights Foundation is giving them some money to defend people in Iran. So how do I support those types of things is very much a part of the here.

And to one other point you were talking about—the membrane, the container—that was one of the primary inspirations for choosing a lot of the principles and the names for self-sovereignty, which came from living systems theory, which was, I think, in the 80s. This is when I first ran into it. I don't know how old it actually is—where it basically said that in order for systems to function, living systems needed to have membranes.

**Wolf McNally:** I'm talking about Christopher Alexander's work on architectural and city design stuff, where he designed 15 essential properties of living systems, or basically systems of cities, towns, governments, under which people would actually want to live and thrive.

**Christopher Allen:** So I've been very much thinking—even going back to the 2016 principles—how do we create those membranes that have selective permeability so that you can interact with others and yet have the integrity to be able to do what you need to do at any level? So this has just taken a long time to get it all done.

### Vehicle Networks Question

So we're at an hour and a half, and I did want an opportunity for other people to speak. And one question I have for you is—this is the end of, a capstone of a lot of work for Blockchain Commons. How do we take this to the next step next year? I mean, I'm doing the first principles of self-sovereign identity—we're revisiting them is one of the projects—but where do we take this technology side of things? How do we get it funded? How do we get people to start piloting it, etc.?

In the coming year, what other kinds of priorities? I mean, Christoph you brought up the UX problems. Right now, the organization that is helping fund us that is trying to do some things here is Foundation Devices, but they're the only one who is using our full stack at this point. We have 18 plus Bitcoin wallets and a few Zcash wallets that are beginning to play around with our stuff, but not in the UX. And I'm really using UX a lot bigger than UI—the whole user experience, secure thing.

We have a thing that Shannon did earlier this year where he was able to do a deep dive into a particular thing and show by using GSTP how much better it would be for the user—how many decision points could be automated between heterogeneous wallets and how much easier it would make people allow people to do things securely. But we haven't had anybody adopt that yet. So what would you like to see us do next year?

**Leonardo Custodio:** Okay guys, nice to meet you all. It's the first meeting I'm participating and I would just like to share my vision. I'm not as a crypto guy as you all. I don't have this deep understanding about cryptography and I way more focus on products UI, UX.

And the reason why I am here is exactly because I saw the need for using a few specifications in one of our products. We have other companies developing protocols, but that led our company—for people to be very difficult to onboard people from other companies, from other chains, because we had our own thing. So it was not a specification shared to the public. It was our own specification.

And I found out about Blockchain Commons actually two months ago because one of my managers said to take a look on the website and see what I think. And I think that seemed to me—at least that was exactly what we needed to start to incorporate a few public specifications in our products.

So I'm saying this because obviously I cannot guarantee. I'm not a manager. I'm just a developer in my company. But right now I'm trying to bring those specifications into our products. And at least for the UX and the UI parts, I know we do something great—some products that have a nice UI, UX.

**Wolf McNally:** Leonardo, I believe that you're actually working on a TypeScript implementation of our DCBOR spec and maybe Envelope as well. Is that correct?

**Leonardo Custodio:** Yes, exactly.

So my vision is for the next year, I'm hoping to bring some UI and some UX products to those specifications that Blockchain Commons is making.

**Wolf McNally:** You're doing exactly what we would hope the community would do—is pick up our specifications and our reference implementations and then port them to other platforms and expand on them and build on them. So I want to applaud what you're doing.

**Leonardo Custodio:** Exactly. And that's—so at least for my side, I would expect that next year we'll have some progress on that part. So I wanted to answer.

### Standards and Adoption

**Christopher Allen:** So Henk asked in the chat, what are the next steps in the IETF/ISO standardization dance? Well, it really is a dance. There's a lot of stuff going on there right now.

Some of it is that it's been captured by Apple and Google in the ISO, especially in the EU emerging EU universal wallet efforts. And there's very little ability for independents to have influence on that. Which is part of the reason why I've been doing some things in Switzerland to help them be better with what they're doing—they're not doing the ISO standard—so that companies—not countries—inside Europe might go, "Do we really—like Germany in particular is one of my targets—how do we have Germany basically go, 'Oh, wait a second, ISO guys, do we really want to do it this way? We can do better.'"

Which then comes back to why there are a couple people in this group that are part of the revisiting SSI principles because we do have to play this dance where there's geopolitical things going on that are slowing this work from being done. And we have to play that game as well. But for me, it's like I also want to get the technology right.

I'm curious, from one of the people who is here from like Georgy or André—coming from the identity side of this question, in the SSI principles discussion, where does this fit in?

**André Ferrière:** What brought me—curiosity brought me. And I do have a question and it might be a bit of an unfair one, but I'll ask it anyway. For short communication context—and maybe just because of the demo itself it was slow—that problem I solved it, but I don't expect performance in something as new as this. But can you tell me a bit about what's usage for short communication context? And I'm thinking vehicle or contexts, things like this.

**Wolf McNally:** Yeah, so what you saw was a demonstration of using the Hubert protocol and the storage layers—in that case we're using our Mainline DHT, which is part of the BitTorrent protocol, and IPFS—no central point of contact, no single point of failure, no single point of compromise. And so therefore, what you give up to gain that is some speed.

So if you need very fast, real-time almost coordination, then you'd want to use something that has a center, like Signal, where you can do real-time chat, or Zoom where we're doing chat. Presumably some of it's encrypted. But the idea is that what you give up for that kind of privacy and global sovereignty is some speed.

Because you have to reach out and you have to find a node near you, or at least near you topographically based on their networks. It has to have time to distribute the key far enough so that any other node can discover what you put into these systems. They use an algorithm called Kademlia, which you can look up.

That requires figuring out—if you want to place your message, like in an ARID, you stretch that into a key for the system you're using. And then you have to make a certain number of hops. You have to talk to a certain number of servers that are successively closer to where you either need to put your data, ask it to be put, or where they need to retrieve it from. This all happens behind the scenes transparently, but it takes time. It's not as quick as doing a quick DNS resolution and then accessing a server and getting your answer. So that's what you're paying. That's the price you're paying for that.

**Christopher Allen:** I'll also add that there's maybe a higher level principle here that you haven't seen because it's your first time at this meeting—is that at every layer of our Blockchain Commons stack, there are lots of different choices. And our goal is to be somewhat agnostic and have all the different layers separate so you can make different choices at each of the layers.

So for instance, there is nothing that stops this from being done over Tor. We have some experience with Tor. It also has latency issues, but it is smaller, but also has other difficult issues. You could do it over an air gap with QR codes. We have this animated QR code technique, which we've got maybe 20 Bitcoin wallets that are using to communicate between Bitcoin wallets. Or you can just use a REST protocol and TLS, which is the first international standard that I'm the co-author of.

So you want to be able to have the appropriate level, the appropriate transport, etc., for the individual use cases. And this applies—this kind of pattern applies—at even a bigger level, which is that when we start talking about various kinds of coordination for various kinds of human activities, we also want to not lock people into one model.

And so some of the work in the initial principles work in the first years of Rebooting Web of Trust and the DID standard was—how do we prevent new ways of locking people into choices that may not be best for us as humanity? And that's always been part of the pattern, the architecture patterns that I have tried to embed in everything we do.

**Wolf McNally:** Just to put a cap on your question, André—as I mentioned briefly, and it may have gotten lost, that the same demo you saw that took several minutes to run, if you run it using our local server, it just appears. It's also all done because the delay isn't introduced by our stack in any way. It's introduced by the fact that we're using these public, open, decentralized networks that use consensus algorithms. Similar to when you do a Bitcoin transaction, it takes 10 minutes plus times six for your confirmation, stuff like that. That's slow. That's not fast. That's not instantaneous. But then for many, many applications, it doesn't need to be.

**Christopher Allen:** Also, I have this concept more recently of systems under siege. When it's peacetime, you can do the fast routes. You can do things that are a little less safe, etc. But you want the system to be able to very transparently go, "Well, wait a second. Things are happening. Things are going on. These are more risky. Let's move to more secure methods, more secure approaches, less correlatable methods, etc."

And just simply, as things become more difficult—we're under siege of some particular fashion—we can move to these slower protocols as we need. And then when the world is safe, we move back up.

**André Ferrière:** So my question came because seven, eight years ago, I did some research on VANETs—Vehicle Ad-hoc Networks—and some of the threats and problems that existed back then, which I haven't looked at since, to be honest. I could actually, while I was hearing Wolf talking during the presentation, I was like, "But that would solve something that I recall."

So towards the future, you guys were—what could you do next? I would look into it, but I would have to dissect and look at all of the things that were presented today because I do need to dive deep to better understand it. But as a naive suggestion right now is to see if that could actually be applied—because I understand if you take IPFS or some other components out, you will probably be able to have key aggregation of FROST in situations where short communication would—the security of that communication is vital for, say, a car exchanging messages with another. "There was an accident ahead, and you need to be careful." But in order that it can't be forged. So that might be something interesting to explore. Really enjoyed it anyway, but it is the first time I see this.

**Christopher Allen:** Well, thank you. Any last comments, Georgy?

### Byzantine Committees and Academic Research

**Georgy Ishmaev:** Yes, thank you. Thank you, everybody, for sharing your work. I listened with a lot of interest. My initial curiosity was quite simple. In fact, my colleagues are working on FROST signatures too, but in a slightly different context.

So I was curious to learn more about what you do with that. So what we work on is an academic experiment trying to see how FROST signatures could be used for committees. So we want to build a system with Byzantine assumptions. And if we want to increase guarantees without introducing too much overhead, we could use FROST-based committee to guarantee a certain threshold of honesty of participants. That's a high-level idea. And we were working around it, using it for a decentralized reputation system.

So I don't know how much of that overlaps with what you do, but I stayed also for Hubert, and I think it's very interesting on its own. It's much more than I expected to learn today.

**Christopher Allen:** Yeah, there's a whole other side of things. I put into chat "Polis Play," which is one of—but Shannon and I wrote a book on the design of cooperative games, tabletop games, that came out in 2019. We also have an early start on a book on cooperative play.

One of the things there is this Polis Play link that I gave you in the chat, which is—let's create a game around exploring what are the different kinds of ways we can organize ourselves. And then I have this long, exhaustive slide deck that I've given at a couple of places on over 100 patterns of cooperation around play with the sort of long-term goal of, okay, let's play around with some of these different patterns and how we organize, how we make decisions, and how we collaborate in a playful way.

So that we can explore the design space before we try to have somebody basically say, "Well, no, this is the way you have to do collaboration." Because I don't think we have good answers for some of this stuff.

And so one of my pet projects, which comes out of my pocket until we have a sponsor, is creating some tools for people to be able to create small, intentional collaborative groups using various kinds of FROST committees. I think you can, in my document on cliques, there's a little bit of discussion about that. Shannon, maybe you can put the cliques URL in there.

Being able to use groups of groups, being able to—because you can, in fact, have a FROST threshold group do a collaboration with another FROST threshold group, because in the end, they're just signatures. So you could do some various kinds of elaborate things there.

And on the policy side, one of the interesting things about the Wyoming DAO Act is that you can now register one of these FROST groups as a legal entity and thus have the limited liability protections under Wyoming law. So that also gives some opportunities for experimentation for how different groups can collaborate together to accomplish goals.

**Wolf McNally:** I'd just like to also just leave you with the thought that, as you pointed out, Georgy, that Hubert itself is a completely separate thing from FROST. We demonstrated FROST over Hubert because Hubert is a protocol. You can basically build all kinds of distributed coordination, control systems, things like that on it where there is no center and there is as much privacy as you need or want. And I'm really looking forward to seeing what applications people come up with for it.

**Georgy Ishmaev:** Yeah, I will definitely look into that. Thank you for sharing.

---

## Closing

**Christopher Allen:** Okay, well, thank you very much for those people who stayed around a little bit longer. And we do these meetings every month right now. I don't know that we have a definite plan for the January—I think it's 7th. What's the...

**Shannon Appelcline:** We were talking to Leonardo, weren't we, about...

**Christopher Allen:** Oh, right, right. Whether or not Leonardo and we could talk about TypeScript and DCBOR tooling and anybody who's interested in DCBOR tooling. That's right.

And all the tools that we demonstrated today will be up in release form—

**Wolf McNally:** In their first release form by the end of the day. So we will link to that both in our Signal Chat, our Gordian Developer Signal Chat, as well as probably the show notes for this particular video when it's released. And so, look for that. It's all open source, BSD available for everybody to use, and we want your feedback. So we're looking forward to that.

**Christopher Allen:** Great. Thank you.

So André, you are going to share your X id?

**André Ferrière:** Okay, well thank you—apologies for that—just to add I used to be on that platform but I made the decision not to, identity related, when they started to demand—yeah, I did share it with a third party and I didn't like that so I left. So that's the reason I'm not there. I'm not trying to be weird or funny.

**Christopher Allen:** I understand. Yeah, there are lots of interesting choices that need to be made these days to different levels of security and—

**Wolf McNally:** And I have what I call a "real ID" on X. I use my real name, my real photo, @WolfMcNally on X. So if you want to follow me there, you can do that. So yeah, that's a choice I made—very security minded choice, but not the highest security level in terms of knowing who I actually am. So that's me.

**Christopher Allen:** Okay. Well, thank you, everybody. And we'll be talking more either in this channel, the Signal channel and the Gordian group. And then obviously we have other activities like Rebooting Web of Trust—no, excuse me—revisiting SSI. And other ways of communicating.

So thank you. Thanks everybody. Bye.

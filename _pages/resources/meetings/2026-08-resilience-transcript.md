---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/otherresources.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "Bitcoin Resilience & Interoperability"
title: "August 2026 Gordian Developer Meeting Transcript"
hide_description: true
classes:
  - wide
permalink: /meetings/2026-08-resilience/transcript/
sidebar:
  nav:
    - resources
    - meetings
---

## Blockchain Commons Quarterly Review

**Christopher:** Welcome to Blockchain Commons, our monthly Gordian developer meeting. Our particular topic this month is Bitcoin resilience and interoperability.

So what is Blockchain Commons? We are a community interested in the self-sovereign control of digital assets. We bring together stakeholders to collaboratively develop interoperable infrastructure. We design decentralized solutions where everyone wins, and we are a neutral not-for-profit to enable people to control their own digital destiny.

In particular this week, I want to thank our Learning Bitcoin sponsor for 2026, Human Rights Foundation. If you're interested in sponsoring us, or particular projects that we do, please let us know.

For today, I'm going to do a very brief review of our last quarter. Then Shannon is going to be talking about sharding secrets, in particular with Bitcoin Core. And Stan is going to be talking about KeyLay.

We have a blog post that gives details on every quarter — there is a blog post with what has happened in the last quarter that gives more of what you need to know to understand what we do.

We've continued to have meetings on revisiting SSI. SSI is self-sovereign identity. The principles for it were written in 2016, 10 years ago, and have been widely adopted by the decentralized identity community. We're now in the process of revising the principles, taking the lessons that we've learned from the last 10 years and trying to make it good for the next 10 years. If you're interested in this project, you can go to revisitingssi.com.

We've been doing a lot of work on our developer pages. We have a lot of technologies — we have a fairly deep stack of tools and libraries, and that makes it a bit harder to understand what's going on, where to fit in, and where to start. So we have new sections. The first one is on technology, which organizes it by technology. Then a separate one organizes it by resources, and we're working now on the architecture one. So you can approach it from a variety of different entry points. And then we have a brand new set of pages about issues related to identity.

We published a new chapter of Learning XIDs. XIDs are our decentralized identifier proposal — what I would consider to be DID 3.0, if they ever move that far along, as a standard for identifiers. The new section is about managing keys, and it's very closely related to the discussion we're having today: how do you back up a wide variety of different kinds of secrets in a reliable way, and be able to share them with others, so that you can have resilience but without enabling compromise?

We also have a human and civil rights oriented use case called Amira, that was started in I think 2017 and finished in the W3C in 2019. We've been implementing that use case as a proof of concept, so we have a progress report on that.

As I said earlier, the Human Rights Foundation has been funding an update to Learning Bitcoin. Learning Bitcoin is probably one of the most popular online intros to learning how Bitcoin Core works and how Bitcoin works, starting off with things that anybody can do from the command line through more advanced features. We know a wide number of people, especially in the third world, that got their start doing the Learning Bitcoin course.

So we've been updating it for newer versions of Bitcoin since the last time we did it. It fully integrates pay-to-witness-public-key-hash and the pay-to-witness variations, how to use descriptors better, and of course Signet, where we used to be using Regtest. There's a new chapter, which is what we're going to be talking about today, about working with secrets, and a new chapter on Miniscript that is available. Next we're working on more details on Taproot. So hopefully this will make it a lot easier for people to learn about how modern Bitcoin works.

We've been doing a lot of work around trust architectures — kind of a review of the last 10 years of self-sovereign identity, various issues of AI agency and intelligence, and how that fits into self-sovereignty. We have been talking about what we call Exodus protocols, and what the Ethereum community calls sanctuary technologies. And then we talk about why we created Envelope and why that's a powerful tool that we think more people should be using.

So anyhow, the quarterly is at blockchaincommons.com. My regular posts on various musings on security and decentralization are at the Musings blog, and of course our developer pages have lots of new changes.

And now we will move on to our feature presentation from Shannon on making Bitcoin resilient. 

## Sharding Secrets to Protect Your Bitcoin

**Shannon:** Yep. Okay. Today I'm going to be talking about how to shard secrets and how to protect your Bitcoin, particularly your Bitcoin Core. But we're going to have some best practices and lessons for how this should be working overall in the ecosystem, we'd hope.

Chris has already talked a little bit about who Blockchain Commons is. And as he said, this is drawn from work on the new Learning Bitcoin from the Command Line that I've been doing this year. If you go and look there, it's at learningbitcoin.blockchaincommons.com. What I'm talking about today is basically a synopsis summary — top info from Chapter 10, which is called Working with Secrets.

Here's the link to the overall Learning Bitcoin course. It's something that Christopher and I have been developing since about 2017 or so, so it's had almost 10 years too. And every once in a while we need to do a big update, because Bitcoin is constantly changing. So this is 3.0, that I've been working on since the start of the year. As he said earlier, we've got most of it updated, and Taproot's the big, big addition that we still have.

Okay, so today we're basically focusing on two themes, and the first of those is resilience. It's very important if you have self-sovereign control of your digital assets. Bad things are going to happen, and you don't want to lose your funds or digital identities or whatever else you're protecting with keys. So that's what we're going to be talking about today: how do you protect your keys?

But we're also going to look at the theme of interoperability. It's an important part of everything because it gives you independence. It lets you move your assets around as you see fit, and it also supports resilience. Without interoperability, you're probably not going to get to use these other resilient protective functions that you want to use. As we're going to see with Bitcoin Core, we have to get things out of Bitcoin Core to offer the best resilience, and that requires interoperability. So they're really closely intertwined.

And this is a very real need. Many of us have heard the story about the guy in Wales who sent a hard drive to the dump and realized it had what's now hundreds of millions of dollars in Bitcoin. It's not the only such story — I have a couple others here.

In our opinion, when you're talking about self-sovereign control of your digital assets, when you are holding them on a hardware device or in Bitcoin Core or something like this, and they're not on an exchange, the big problem you're going to have is loss. It's not that theft you have to not worry about at all, but loss is what you have to really keep the focus on.

So our best practice to protect Bitcoin is to use multisigs. We have this whole scenario, which we have a QR for here, where you have two hardware devices and then you print out the codes for a third key and you shard those. That way you always have yourself protected. You're on different devices, you're on different operating systems — and it's a huge pain in the neck. We put it out there, and we know that not many people use it, because it is real work, especially if you're not talking about huge amounts of funds.

So we need easier solutions. And Shamir's secret sharing is our alternative. It has some issues that multisigs don't have. In particular, it lets you shard your key so that you can protect it, and it lets you get your key back from a threshold of those shares that you've put out there. But when you reconstruct, it's very dangerous, because you suddenly have your whole key in one place, and it may not be the regular place that's secure. So it could be stolen then. But we've already said that we think losing your key is much more dangerous than having your key stolen. So that's why we suggest Shamir's secret sharing if you're not going to use multisig.

**Shannon:** Going back to Bitcoin Core, we saw that the first problem with keeping your digital assets safe was that you can lose your key. And the second is that there are resilience features, but Bitcoin Core doesn't necessarily support them.

We're talking about Bitcoin Core here because we've done work there. We offer it as a great way to learn about it, and we know some people use it as their wallet. But there are going to be other wallets out there that don't use resilience features either.

So in particular with Bitcoin Core, there are two factors. There's the fact that it doesn't support BIP-39 mnemonics or entropy. BIP-39, that's where you have mnemonic words, usually a set of 12 words that lets you reconstruct your seed. And it turns out that it's the whole first stage that you can use to produce keys. Bitcoin Core skips it entirely.

And the other thing is Bitcoin Core, like many wallets, doesn't have any access to Shamir's secret sharing. So the only way to really be interoperable when you're using Bitcoin Core is to use their descriptors. They're a descriptor wallet, and that's all kept natively on Bitcoin Core in an SQL file. It's a very specific format that they have. You can back it up, and you obviously want to back it up. But you're dependent on a file format that someone might not understand in 10 or 15 or 20 years. And you're also dependent on a singular database that isn't sharded — that could be stolen if people grab that one thing, or that could be lost if you lose that one thing. So it doesn't have any of the advantages of secret sharing.

I mentioned that BIP-39 is kind of the first stage, so I wanted to lay out all of the stages of how keys are generated on most wallets.

Most of the time you generate entropy, and you turn that entropy into the mnemonics. It's 128 bits of entropy, it's checksummed, it generates 12 mnemonic words. There's then a one-way hash which uses a password stretching function that gets you to what's actually called a seed, which is the BIP-32 definition.

But the funny thing is, this entropy back here — that's what actually is used by the whole interoperable Bitcoin ecosystem as seeds, not the BIP-32. And so that causes a lot of confusion, and is one of the reasons it becomes much more difficult for something like Bitcoin Core to share, because no one else recognizes what that BIP-32 seed is.

Then again there's another hashing function and it goes into the actual keys here — extended keys, chain code, and regular keys — and then they're usually encoded in descriptors right now.

So we can see that whole system, and then we can see what it looks like here in Bitcoin Core, where the BIP-39 is gone. The BIP-32 actually is not accessible, it's never exported. And so when you're working on Bitcoin Core, what you have are these extended keys, and more so descriptors. Descriptors are what you really have, but you can extract the extended keys from them, as we're going to see.

So this is kind of our challenge. How do we correlate the extended ecosystem here, where we have everything, with Bitcoin Core, where we only have our descriptors and functionally our extended private keys? That's what we're going to talk about today: how to get both in and out of Bitcoin Core.

**Shannon:** So here's our game plan. Bitcoin Core will allow us to export our master key, our descriptor. We can take advantage of that, and then we can secure those secrets with sharding once we've got them out. We also can go the opposite direction. We can create a seed externally and then shard it, and simultaneously bring it into Bitcoin Core by converting it into an account descriptor. Both methods allow us to make Bitcoin Core assets much more resilient, which is of course our goal.

In order to carry out this game plan, we're going to use a handful of reference tools that we've created at Blockchain Commons. SeedTool CLI is a command line app that creates seeds. Keytool CLI converts among a variety of cryptographic formats. And Envelope CLI is a smart document system for secure storage — and more than secure storage, very regularized storage. It's really part of the magic sauce of all of this. And SSKR, which we have here, is our version of Shamir's secret sharing. It's integrated into these CLIs, and it does a little bit more than standard, as I'm going to talk about in a minute.

SeedTool and Envelope are both really easy to install — Rust crates. Keytool is a Python program; you have to clone it and compile it. It's available on the GitHub repo, and I've been working with it recently to make sure that the compilation is clean, so there are instructions there that tell you how to do it all.

If you've got Bitcoin to secure, you can just use these tools directly. More generally though, we're offering best practices. We encourage developers to consider the interoperability that we're displaying here, to consider the tools we're displaying here, and hopefully take advantage of some of these same tools to offer interoperability for their own wallets.

Our SSKR, which is our version of Shamir, is a security-reviewed library. It supports not just thresholds of people, which is what Shamir usually does, but also thresholds of groups, which each have met the criteria of a threshold of people. So you can say, for example, that we have to have at least two out of three people from two out of three groups, or something much more complex than that. When I talk about SSKR in this demo, I'm talking about this version of Shamir's secret sharing — but a wallet or someone else could easily use a different one, though ours is available in libraries and hopefully easy to add.

### Exporting Secrets: Envelope Metadata, Sharding, and Verified Restoration

**Shannon:** So we're going to start out with the idea that you already have your assets within Bitcoin Core. We're going to talk about how you would export these secrets that are actually exposed to you, and then how you'd back them up afterwards — since that's the point of getting them out of Bitcoin Core, to make good, resilient, solid backups.

So here's that game plan I showed earlier, hopefully actually readable now. It's really simple. We just either export a master key or a descriptor — I'm going to do both. We add metadata to it, and then we shard the envelope with Shamir's secret sharing. And like I said, the envelope, and the fact that we can add metadata, that's a lot of the secret sauce here. It's relatively easy, once you can figure out how everything works, to get the secrets in or out. But actually recording them in a way that you can come back to later, that's very important for resilience.

All right, so I've screenshotted a bunch of command lines beforehand, which hopefully will be clear and obvious here. All of your coins in Bitcoin Core are protected by descriptor addresses. It's a descriptor-based wallet. Here we can see a listing that I've made of all of the descriptors in a brand new wallet that I set up. There are eight of them, which is typical. They all include the master private key — it's the xprv right here. And since it's the master private key, it's all the same private key.

You can see it's the private key because our derivation path here is all listed after the key. If this were a key that was derived lower down in the path, we'd have part of the derivation path at the start and part at the end. But the fact that it's all at the end tells us this is the master key. That's what we want.

So this is just a bit of command line code that I've used to extract the various descriptors into an array. I've got all of this on a web page — I'm going to make it available after the meeting is over — but you can see I've just put it all in so I have it all accessible. And here I'm also using a bit more command line code to actually extract the xprv out, the extended private key. So at this point I have both the descriptors and the private key.

I want to demonstrate how you might store the one or store the other. In real life, you'd probably only do one, but they're both valid targets. You might choose either.

So we use Gordian Envelope to store things. It's a smart document system. It lets you record semantic triples — subjects, and then assertions about them, where an assertion is a predicate and an object. You can see predicate, object here.

I'm creating an envelope where the subject is the master private key, and the assertions are all meant to describe it. What is it, what software created it, what software used it, what derivation paths are in use. The metadata is intended to help with the future recovery of the digital assets. It ensures that this key isn't just a couple of digits on a piece of paper that no one might have any idea what they are.

This shows it all much clearer than the instructions for how to create it. This is what a Gordian Envelope looks like. You can see at the top, the subject, this is my private key. You can see I have an `isA` that says it's a master key. You can see I have what created it, which I extracted from the precise version that I was using. And you can see that I also have what it's used by. And then I have my derivation path, so that I know all of the derivation paths that were being used.

So this week unfortunately showed us exactly why we need to have that "created by." It's because seed key creation can be buggy. And in an interoperable world like the one we are hoping to see more of, you can move your Bitcoin around, so you don't necessarily know where it originated unless you carefully record it like this.

Coldcard proved this last week, why this is really important. But it's not the first case. Here I have another news headline from 2014 about Blockchain.info, where a white hat rescued a bunch of funds and then returned them — again because of a vulnerability in how they produced keys. And a few years after that there was this big issue in JavaScript where secure random ended up not actually being secure random, and so that put a lot of things at risk.

And so when we look at a Coldcard problem like this one — this is one of the ways that we solve it, by actually saying where did our seeds come from? The other way is multisigs, and again, that's not something many people are willing to do. This is a great start, though.

Okay, so I showed you how we store our private key. This is the flip side, where we store a descriptor. Here's what this looks like in an envelope. I just gave this a subject of "descriptors for Bitcoin" as the subject. I said it was a collection of descriptors. Again, I showed where it was created, where it was used, and I listed all of the script descriptors. So this is exactly what someone would need to restore all of these private keys — and thus the accounts, addresses, etc. — to Bitcoin Core. That's the kind of thing that we want to see for restoring this in the future.

So we want to shard these envelopes, so that they're not just these private keys sitting on our hard drive. And we do that with Envelope, which has SSKR built into it. We can see here just a couple of lines that shard them, and we drop them all in an array, and here is all of our shards in an array.

That's the first part of checking it works. The second part of checking that it works is: can we actually restore it? So here I use SSKR to put back two of the three shards, because I had a two-of-three threshold. And I look at my restored envelope and I can see it is exactly like the original one.

A best practice would be to test out restoration via various means — i.e., I'd try share zero and one, zero and two, and one and two, each of the combinations that should restore things. Because before I put this off in storage, I want to make really sure I can get back to the original.

So that is basically the methodology of how you start with Bitcoin Core, extract the important information, and store it in a resilient way that is also private and secure.

### Importing a Seed Into Bitcoin Core with Keytool

**Shannon:** So the opposite is importing a secret into Bitcoin Core. And in many ways, I like this one better, because it allows you to start with an actual seed — technically BIP-39 entropy — which is the standard of interoperability for the majority of the Bitcoin ecosystem. You can store that seed, you can shard it so that it's protected, and then you can use something to import into Bitcoin Core, a descriptor as it happens.

Here's the game plan here. It's a little more complex and it comes in two parts. We use SeedTool CLI to generate our seed. We store it in Envelope CLI and shard it exactly as we showed when we exported something. And meanwhile we use another tool that I talked about earlier but we haven't seen yet, called Keytool. Keytool is a magic little tool that imports and exports from, I don't know, 20, 30, 40 different cryptographic formats. And so it allows, among other things, us to turn that seed into the account descriptors that we're going to need. And those we're going to then import into Bitcoin Core.

So here's how SeedTool works. We type `seedtool` and it generates a seed. In this case, I demonstrate how it works and then I save that into a variable. And of course the second one that I generated, that I saved into the variable, was different from the first, because it generates a new one each time. That's why we keep it in a variable, so that I'll have that static one that I'm using.

We're going to first do the import into Bitcoin Core here, and that's via Keytool. The way that Keytool tends to work is you give it an input, in this case a seed, and you tell it what type of output you would like to see.

So I'm going to need to generate two bits of information to be able to import into Bitcoin Core. Technically I only need to generate one, because this fingerprint you don't have to have. But it's metadata just like everything else — it'll tell us what seed this came from, so it's important to have if you can get it. In this case, I'm just generating that fingerprint and I'm saving it to a variable.

The other thing you need is an account key. I could choose to generate a master private key like the one we exported. But one of our general philosophies of architecture is what we call the least necessary. We always want to give the least permissions, the least information, the least access that we can — we're looking from the flip side at what's needed. And so we don't have to offer a master private key. We can offer an account key. And if just this one descriptor happens to be stolen in some way, it doesn't compromise all of the others.

So here, again, Keytool: we just feed in the seed, and we tell it with this account derivation path — which is SegWit — we want to have the account key in Base58, which is the xprv format. Again, we save that.

Then we create the descriptor. The way I make sure I understand the descriptor format is I usually go and look at Bitcoin Core and I tell it to list descriptors, and I make sure that the format is right. You can also go to the BIPs, of course, to get the format.

In this case we mark it as `wpkh`, so we know it's a witness public key hash. We put in the fingerprint and the derivation path that was used to generate the account key. Then we put in the account key, and we tell it that it is an external address, which is zero, and that it is in a range of addresses.

You have to, when you're working with Bitcoin Core and working with descriptors, have a checksum. Fortunately, Bitcoin Core has a way to just use `getdescriptorinfo` and you can generate a checksum. I use a program here called `jq`, which lets you extract individual bits of data from JSON output, and I get the checksum from that.

And then I put together my descriptor. Here's what it looks like. You can see there's the `wpkh`, there's the fingerprint, there's the derivation path, there is the actual private key, there's the rest of the derivation path, and there's my checksum. That's what I'm going to need to import it.

And importing is simple. I did create a new wallet here that did not have any private keys of its own, so I know anything in here is actually from the descriptor that I imported. Then I imported it with the first 100 addresses, with all of the account information checked back to a specific time. And Bitcoin Core has told me it worked.

You always want to check your work again and again, since you're working with self-sovereign assets where you don't have backups. And in this case I list my descriptors and make sure that they're there.

If I was working with a real wallet, I would import at least one more descriptor — I would import the internal addresses, so that I could make change to my same descriptor address. But I might also want to import the other six that are standard: legacy addresses, pay-to-witness-script-hash, and pay-to-Taproot.

**Shannon:** So going back outside of Bitcoin Core, because I've already done all the work I needed to import, I also need to back this up. Here I wanted to show that SeedTool actually can do backups on its own.

Here I took my seed and I output it to BIP-39 words. Those can be engraved in metal plates, or output in some other very resilient form. And I can also just create straight UR SSKR shares from SeedTool. This is just the seed without metadata, which isn't necessarily as good — I mean, it's absolutely not as good. And here, as usual, I check two of them together, and I should do this for all of them. And I can look and see, is this the same seed? It is.

So I said it's not as good, because again, we'd much prefer to back up with Envelope, because we can put in more metadata. So here, instead of the master key, I put in the seed and I say this is a seed. And again, I put extra metadata in. I want to make sure I know it was created by SeedTool, in which version, in case we ever have a problem. I want to know who it is used by, so that I or someone in the future can go back and put this in and use the exact same software I was. And right now I just have the one derivation path, so that's all I record. I might go back and expand it in the future.

Once I have it in an envelope, I can shard it. There's actually a technical difference between how SeedTool shards and how Envelope shards. Envelope creates a new secret, it shards that secret, and then that's what you get back to unlock the envelope, which has been cryptographically sealed with the secret. Whereas the seed we're just sharding normally. But it's the same basic idea — it's all Shamir's secret sharing.

Okay, one other thing I wanted to mention, which is: you've been seeing these URs, which are textual representations of the various codes. They can actually all be stored as QR codes. In fact, UR was built so that it could very efficiently store itself as QR codes.

Here is this seed, and here is this three sets of SSKR shares. And these you can now store. There are things that allow you to print them in various ways. You could also send them to a friend if you're in person, by flashing them the QR and then them recording it in some type of QR or storage software.

One of the problems with QRs is the question of how you transmit them if you want to give them to someone further away over the net — which I think is very relevant for, say, SSKR shares. If I'm occasionally making these, occasionally updating them, I don't necessarily want to have to go out and visit every one of my friends, my attorney, whoever I have them stored with, every time I do new shares.

So the question arises: how do you safely secure these over the internet? And that, as it happens, is what Stan's going to talk about when he looks at KeyLay for the second presentation.

Okay, so I just wanted to show a couple more QRs. These ones lead to the tools, so if you want to go grab SeedTool CLI, Keytool CLI, and Envelope CLI, you can just click these QRs and they will open — I think our repo pages is how I set them.

`jq` is the other tool that I did not link on here, because it's available from jqlang.org, and it's also easy to install, say on Debian — it's a package that you can install. But these are all of our tools. They're all available on GitHub at Blockchain Commons. The code is all open and open source, so you can look at them, you can review them before you use them.

## Q&A on Seed Tool, and Handoff to Stan

**Christopher:** I also put into chat the Gordian Seed Tool iOS app, which does all of this with a nice UX, and supports QRs, URs — you can test all these different formats. And it can even sign air-gapped PSBTs.

**Shannon:** Yeah, Seed Tool iOS is very nice. The only reason it's not used here is just because I'm showing how it all works on a command line, to give a solid pipeline. But Seed Tool is great. Once you have a seed, you can import it into Seed Tool, among other ways, through QR codes. And then you can shard it from there exactly like we did on the command line, and then send those out as QRs.

So that's my whole presentation. It looked at how to import and export secrets so that you can better secure your Bitcoin Core. Are there any questions, comments? Alrighty, Chris, I will hand it back to you. One last QR here — that is Chapter 10 of Learning Bitcoin, which goes over much of what I discussed here today in much greater length.

**Dan Pape:** That's great. I like seeing the use of all the command line tools like that.

**Shannon:** Great, thank you, Dan. I have loved working with command line stuff for a long time, so it's really great to be able to show how you can just sit down and work through it all.

**Stan:** So you're going to hand this over directly to me, or how are we doing this?

**Shannon:** Chris, you're muted right now. But yeah, I think so, Stan.

**Christopher:** Yes. So next is a presentation by Stan Reeves on KeyLay. He is going to be talking about how to use QRs, including some of the ones that we're talking about, and how do you securely transmit them across the net? So I will let you go ahead and share.

## KeyLay: Remote Multisig Setup and the Untrusted Relay

**Stan:** All right, great. Well, I appreciate everyone who's stopped in to hear about this.

This is a project that I began working on about a year ago, after I did a deep dive on multisig setups, both for myself and also helping some others onboard into multisig. Anybody who's done multisig knows that there's a lot of friction involved, and I was really surprised at just how tedious and difficult it was. So I guess I was inspired to think about how I could build a tool that would take away some of that friction and some of the privacy concerns with that. That's how it arose out of my mind.

And there couldn't possibly have been a better foundation laid this past week, with everything going on with the Coldcard situation, to just underscore how important multisig is. I actually have a multisig setup that includes a Coldcard, and I was on vacation away from my Coldcard when all this news broke. But I slept easily and slept well, because my setup was in a multisig, so it wasn't dependent entirely on that one key.

So multisig is really important, and particularly what this addresses is being able to set up a multisig remotely — without using tools for communicating the information that might reveal something that you don't want to reveal, in particular and most importantly your descriptor. That's kind of where I went with this.

And also to be able to do this without getting people in the same room, particularly in a case where you have, say, political activists or something, where it's risky to be in the same room, or to be tracked to be in the same room, or anything like that. It also creates a single point of failure. So being able to set up a multisig with participants in remote distributed locations, I think, is really important.

So the coordination of the setup is the leaky step. I'm just going to go through the cryptographic reasoning on how this thing works, and then I'll do a demo near the end of it.

First of all, in order to get the multisig set up, everyone has to share key material and put together a wallet descriptor. So you have to have communication among all the participants to do that. Again, you could put everybody in the same room, but that has its own risks. And so if you want to do it remotely, then that means there's a temptation to send that information over email, or over a messaging app that you may or may not have full control over the privacy of. And then also, once you get it set up, just to be able to spend from the multisig, especially if you've got remote participants, it's way better if you can do that over some kind of private channel.

The descriptor is particularly important, because once you have the descriptor, you have a pretty significant privacy leak — you can then determine every address that will issue from that wallet, and the balance of the wallet, as long as you continue to use that setup. Also, you have metadata leaks that would reveal possibly who you're coordinating with, when you're coordinating, and so forth. So again, for activists and people like this, this is really important, to be able to have a privacy solution for this. It also helps to take away some of the friction, because as we've seen, multisig is an important solution for lots of different risk factors in wallets.

So I'm starting off with the threat model that the relay is the adversary. We're assuming we cannot trust the relay, and that pretty much dictates how I set up everything. So we are going to assume that the relay could potentially record everything, try to read the contents, try to substitute handshake keys, possibly try a replay attack — and we need to be able to function even under those kinds of threats.

So there are four guarantees in KeyLay. First of all, the relay never learns plaintext — all the cryptography happens in the browsers of the participants. Second, there's no key substitution without already knowing the code; the code is the thing that sets up the initial channel and handshake. Third, a code that's compromised later does not unlock past sessions — in other words, we have forward privacy. And then finally, private signing keys never enter KeyLay at all. KeyLay is a coordination layer, it's not a wallet.

### The Session Code, Handshake, and Replay Defense

**Stan:** So to begin with, we start with a session code — and a lot of this will become clear when I do a demo. The session code is a minimum of ten characters. It uses, by default, a pruned 31-character alphabet, so that there's no confusion about things like zero and O, or pronunciations of certain letters. And this requirement produces a code of about 50 bits of entropy. That would be shared out of band, either through a phone call, a piece of paper, Signal, something like that. That will be shared among the participants.

Right now the way it works is it assumes just two participants at a time. My goal is to expand that out to be N participants, but for now it's just two.

So that session code is used in two ways. First of all, it's used to derive a name for a WebSocket channel, through a hashing process. And secondly, it's used to authenticate a handshake. The session code derives — or sorry, authenticates — an HMAC key, and uses expansion of the key, so you do 300,000 iterations of HMAC to make it difficult to do a brute-force search attack.

So then with that we go to creating fresh keys. Both participants will create a public key and a private key, and then will authenticate their own public key, because they have a shared code. And now both parties have the other one's public key and their own private key, and it's been authenticated through the code.

So there's no way you can have a man-in-the-middle attack with this particular approach, because that code at the beginning is the authentication layer that makes sure that no one else can just substitute a key into the process. It's not a signature, it's basically an authentication. The authentication applies equally to both parties, so there's no distinction with the code among parties — you don't have an individual signature, but you do have authentication that everyone that is in the channel belongs there.

So from there we use an X25519 process. We multiply, or combine, your own private key with the other person's public key that they have shared with you and authenticated, and from there you can derive a session key that is shared. Both parties know it. Both parties have this shared set of bits, because they each have their own private key and the other one's public key, and by combining those properly, you derive the same shared bits. So from that you can derive a session key that then can be used for encryption in both directions.

Once you have that, then you basically just throw away the private key. That way no one has ever shared a private key, and it didn't have to cross the wire. So it's going to be an ephemeral key, but that guarantees you forward secrecy. That way, if let's say somebody wrote the initial connecting code down on a piece of paper and shared it, and then somebody intercepted that code, they would not be able to go forward and reconstruct this whole process going forward.

So basically you can kind of see the process here. You start with the code. From there you generate an HMAC key. That key authenticates the communication specifically of the public keys that correspond to each participant's private key. Then with the other participant's public key you multiply with the X25519 process, and then we sort those and create a hash-based derivation function, and then finally we land at the ability to do AES-256-GCM encryption, and we have a shared key for that process. The private keys that generated these public keys are nulled out, so that there's no risk of those somehow being revealed at some point.

So there are three possible attacks that could happen here, and this might be getting into the weeds a little more than we want to. We want to be able to send PSBTs, BSMS files, various blobs of data under AES-256-GCM encryption. So we want to be able to detect if there's any kind of attack there.

A random 12-byte nonce is sent along with plaintext, so that an identical plaintext does not give you the same ciphertext anymore. We also add in a counter, so that you can't do a replay attack. Both parties would expect the counter to have incremented each time. If the counter has not incremented, then that means that there's a potential replay attack underway.

So with all these things combined, an attacker can't read the messages that are being passed, because that's encrypted. They can't tamper with it, because that's blocked by the GCM portion of the AES-256 algorithm here. And they can't replay, because there's a counter in there that keeps it from being the same thing over and over.

All right, so this kind of shows you the process from end to end. Let's say you have two participants, Alice and Bob, and a relay in between. So you share this session code out of band. Alice derives the channel name from the session code plus HMAC. Bob has the same code, so he does the same thing. And that way they can connect through a WebSocket connection — they derive the same channel name through that same process. So they're able to talk to each other over a WebSocket connection.

I should mention at this point that my goal ultimately is to be able to do this over Nostr, so that you're not dependent on a single point of failure with the WebSocket connection. But even now, anyone can run the WebSocket server. The code is there on GitHub, so anybody can download that and run that themselves. They don't have to trust my implementation of that relay.

So from there they generate these ephemeral key pairs, public and private. Bob does the same thing. Then they swap the public keys with authentication through the code, and make sure that they're actually talking to the person that has that code. That guarantees forward secrecy.

Once they've both done that, they can verify that those are real. They use X25519 to generate the shared secret for AES. And again, they can do that on both sides, because both of them have that same information once they combine the public and private keys together. So now you have forward secrecy at this point. Now you can communicate in an encrypted fashion — both parties have the key to do that. As far as the relay is concerned, it's completely opaque, and the other party can decrypt. There's a counter in there, they check that, and so there's no replay attack there.

So the relay itself — the only thing the relay knows is a channel hash, two public keys, HMAC tags, and they know the size of the ciphertext and the timing of the communication. That's all they know.

### Live Demo: Sessions, Roles, and QR Transfer

**Stan:** All right, so at this point I want to try to do a little bit of a demo. I discovered at the last moment, when I was thinking of using my browser to scan a QR code, that Zoom takes over my camera, so I can't scan anything with my laptop. But I managed to scramble around and get another laptop going, and my phone, so I'm going to do it that way. So first of all let me change the share that I'm doing, if I can do that. I guess I have to — no, I think I can do it this way.

All right, so start with this. Okay, so if you go to keylay.org, that's just a website explaining all the details of how KeyLay works. There's a button in the upper right corner that'll take you to the app. This is just running the same exact code that's on GitHub.

So this gives you an opportunity to start a session. You can enter a code yourself — you can make up a code if you want to — but it's preferable to just generate a random code, because the algorithm is going to do a better job of getting higher entropy that way. So once you've created a random code in there, that becomes the code upon which the channel is built, and then you join session.

Okay, so now it went through that whole process that we saw, but now it's waiting. It says "waiting for peer handshake to complete." There's no one to talk to on the other end at this point. So if you wanted to send this through Signal or something, you can click copy there and copy it and paste it into a Signal app.

What I'm going to do is, I've got another laptop here that's off screen that I'm going to type in that same code. So let's say I get on the phone — or typing into Signal, but let's suppose I just get on the phone with my partner — and I say the channel code is W8EQFMUXJ5. So I just typed that into another KeyLay session on another browser, and I click join session.

Okay, so now it detected that another party has joined the session. Now we have two parties in the session. Currently KeyLay limits it to two parties, so no one else can join, and no one else can take over the channel either, to prevent a DoS attack.

You'll also notice that it says "you are the sender" there. The other side says "you are a receiver," and I'll just show you what that looks like, because you need to go both ways with it, but one at a time. So the receiver can claim the sender role, and when they claim the sender role, it automatically shifts the other party to the receiver role. So now you can see what it looks like when you're a receiver. And if you need to send something back the other way, you claim the sender role again.

Okay, so I'm going to claim the sender role on the other laptop so that I can scan a code. What I've done is I've just pulled up a Bitcoin receive address. Let me go back the other way so you can see what's going on. So on the other side, once I'm in the sender role, I'm going to click start scan, and then I'll be able to scan a QR code. So I'm going to do that on the other side. And so basically instantly it picks up the QR code and sends it here.

By the way, if anybody wants to do a screen grab and send me some Bitcoin, I'm happy for you to do that. But anyway, that's a receive Bitcoin address.

So that's just a single QR code, and that's a little easier — but that's after you've set things up and you want to be able to coordinate spending. We also want to be able to set up the descriptor, so we need to be able to export a wallet setup, or even a preliminary setup, from a wallet that we've started working on.

I'm doing this from a Nunchuk wallet on my phone, and I'm going to export the wallet configuration with a QR code option. It uses BC-UR2, and so this was a little bit more difficult, because it's a video. Sometimes it takes a minute. What I wish you could see, but I can't do it on the browser, is it shows you which frames have been read already and which ones are remaining.

So now it's done, and now it's created a BC-UR2 on the other end. So on this end, now you're using this computer as a coordinator. You could use a hardware wallet to scan that QR code, without violating the air gap, and you could use that process to set up the multisig.

You can also download it, depending on the type of QR code. In this case I haven't yet implemented decoding in the browser for the BC-UR. So it assumes that the wallet that's reading this is going to read the raw BC-UR and then decode it in the wallet rather than in the browser. One of the next things I have to do is implement that in the browser.

At some point, with feature creep, I kept adding one more thing, one more thing, and I finally was like, I need to draw a line just so I can get this thing out there at some point. So anyway, you can click download and you can download something, but it's not going to be the decoded version, so it won't be particularly useful unless you have a software decoder or something.

So again, once you get all that assembled, if you want to go back the other direction and claim the sender role and then send it back, scan it and send it back to the other parties, you can do that. So that's basically what it does right now. Like I said, I want to be able to do multi-party joins at the same time, and also to be able to do this where you don't have a WebSocket relay in the middle, but it uses Nostr in the middle. So one thing at a time, but that's where things are right now.

All right, so a couple of things. Well, go ahead.

### KeyLay's Known Gaps and Design Constraints

**Christopher:** Oh, I was just — if you have a couple more things, go ahead and share, and then we'll go to some Q&A.

**Stan:** Yeah, so just a couple of quick things. One of the things is, I was going back through this — and this is one of about 12 projects I've been working on, so it's not like I'm working on this all the time. I had to kind of refresh my memory before I sat down to talk about it again.

Revisiting this, I realized I need to use the same code expansion process in the channel name as I do for the code itself, for the authentication side of that. Currently it doesn't do that. So what that means is that the relay itself, instead of taking 58 years to decode the underlying code, it might take GPU-days to do that. If a state-level attacker was trying to do that, then they probably could. So I need to shore that up. That's one of the first fixes that I'm going to do next.

And this one is — I'm not using a standard AKE algorithm for the handshake. It'd be nice to do that, but what I've done, and I probably should have said this at the beginning, is my design philosophy was only to use cryptographic primitives that are already implemented in standard browsers, in their libraries. So I didn't want to go to the step of either inlining code or writing my own cryptography code. I wanted stuff that was very well vetted and already being relied upon in browsers. So the problem is that this is not implemented in any browser that I know of, and from what I understand it's not even really seriously under discussion in the immediate future. So at this point I'll just leave it as kind of a makeshift alternative that I've already implemented, that uses the cryptographic primitives of the browser.

Another thing I'll have to go back and think a little bit more about is, right now everyone ends up with the same authentication, so it doesn't distinguish one party from another, because we're using a common code and the way it's being handled there's really no distinction. So it's not a signature, it's just an authentication that you belong in the channel. That's another thing I want to look at.

So just to mention here — as much as possible, I'm trying to use off-the-shelf functions that other people have developed and done the deep dive on that I have not. So I've tried to use standards and implementations that already exist out there. I should have mentioned that this handles BC-UR as well as BBQr, and then uses the standard PSBTs, BSMS, and descriptors.

So yeah, these are just a few things to maybe talk about, but feel free to jump right in now. That's the end.

**Christopher:** Anybody have any questions? Dan, Tugume, Shahzad, Shannon?

**Dan:** No questions, but thank you, Stan, for the demo. That was cool.

**Shannon:** Thanks. Yeah, I really love the way that it is able to read the QRs, and even the animated QRs, and reprocess them to make them clean and something recognized. Neat setup. Thank you.

## Connecting the Two Talks: Smart Custody, Multisig Friction, and Automation

**Christopher:** So I wanted to connect these two presentations to some of the larger picture — not just here at Blockchain Commons, but what's been going on in the last few weeks.

Shannon and I wrote the original Smart Custody book, which was about how to think about single points of failure, how to do cold storage, etc., with the focus on being able to work with single sigs. That book is available, and it's still considered to be one of the definitive methodologies — very careful and methodical. But we found that people still wouldn't do it. It might take an hour or more to do, but people wouldn't do it.

And so we started working on multisig. Obviously this week has really proven the value of multisig, and as Stan said, if you have heterogeneity in your keys, then you have some additional resilience. So we wrote this multisig self-custody scenario — gosh, it's been a few years — using a lot of the stuff that's available in Sparrow Wallet, in Gordian Seed Tool iOS, and other tools out there. Passport can do some of this. The goal was to develop a multisig that wasn't that much longer to do, step by step, than the original single-sig one.

That being said, it's still an hour. And now you've got multiple devices, each with their separate interfaces. So one of the things we worked on is we have this thing called request-response protocols, through one of our technologies I'll talk about in a second, which basically demonstrates the various steps that you would have to do between the multiple wallets to be able to recover, and how it would work.

And then we went in and went, hey, how does this really help the overall system? Turns out there are a bunch of places in here where we can make the whole process simpler than the original one. So in the classic multisig, the one I showed you originally, it went from five decision points to two. It has six confirmation points, which are basically yes/no. The original has kind of 11 research points, because of all your different wallets, but if you can automate some of these things with request-response QRs, it turns to one. It goes from 31 human actions to 14 human actions, and automates 33 actions.

So you can see that the ecosystem could really benefit if we started puzzling out and refining these. At this point, as far as I know, Foundation Devices is the only one that's actually implementing the relatively full stack. Given that they're the only one, there hasn't been a whole lot of interoperability, because other wallets have not quite gone as far in that. But I think it's really becoming important, especially given this last week.

### The Gordian Stack: GSTP, Envelope Tooling, FROST, and Hubert

**Christopher:** So we have a wide variety of technologies — this is on the developer page — that offer things to make various aspects of this easier. In particular GSTP, which is this box right here, allows for some of those interactions between devices to be secure.

And then we also have this encrypted state continuation, which is a particular advantage where we're also putting into the envelope extra information, so that when we're doing a multi-party protocol, a party will send back my state information that was encrypted by me, only for me. So I can forget it and then continue on at another point. This uses ChaCha20-Poly1305 and also works in the browser.

There's some interesting work done more recently. There is now a JavaScript/TypeScript playground for doing all of the Gordian Envelope operations and understanding how they work. There's some good code there if you're working on the browser, so I encourage you to take a look at that. Right now there's a monorepo that has all of our standards, and he's very carefully creating idiomatic libraries for it, because all of our native code is in Rust. So I encourage people to take a look at that.

But again, the bigger issue is we've got to figure out how to make more wallets interoperate, allow for and encourage heterogeneity, encourage the different parties to do cooperation in looking at code and such.

But there are also protocols that do things. So we have here Learning FROST from the Command Line — you saw some stuff from the Learning Bitcoin from the Command Line.

Some advantages of FROST: it isn't using Bitcoin multisig. Instead it's using Taproot and using a multi-party protocol, but it is effectively a multisig. What's really interesting is that as part of it, it is able to do something called multi-party secret creation. So in that particular case, as long as at least one of those parties is honest, your key is going to be strong and it's going to have the entropy that you need. That just happens to be a small part of FROST.

So that would be potentially another solution space to the kind of problem that we're seeing with Coldcard, where you literally do not have to rely on a single machine, a single computer, to do something. Instead, your machines cooperate — and as long as one of those parties is doing good randomness, everybody's safe. So we need to be doing more work together, collaborating together, to do this type of stuff.

Other things — oh, this is the thing on the Sealed Transaction Protocol. But we have a whole bunch of articles on FROST.

And we also were playing around some in the same area that Stan was working in, with this prototype called Hubert. In this particular case, the coordinator — you're not streaming to it. Instead you're dropping objects into IPFS or into a Bitcoin DHT, and the coordinator needs to know a lot less about the various parties. And then there is no point-to-point connection that's required. It's slower than what Stan was showing, but I think we did show that we could do FROST coordination using that technique.

So I just kind of wanted to discuss this larger picture with everybody. Dan, and Tugume — what's your take on how do we get wallets to work better together, have some common standards, make this all easier for users? Dan, you've been working on some things in this category.

**Dan:** Yeah, and I wish I had a better opinion. I personally have been terrible about my one wallet that I've used here and there. So I was interested to see you guys' thoughts on how to back that stuff up, because I definitely do not have any kind of backup right now. But I am excited to see this kind of software put together. I know in our organization, Ryan has some ideas for what they want to work on in the future, but yeah, I don't have any concrete plans to share with you at the moment.

### Forward Secrecy, Bytewords, and Verification UX

**Christopher:** And I think Stan, you've talked about Nostr. There have been a number of people who have been talking about that as a peer network.

I was also intrigued — so our Gordian Sealed Transaction Protocol doesn't have forward compromise resistance, but it's designed to allow us to do so in the future. We just don't have the staff right now to do that properly. I would love it if you're interested in helping us make that more resilient.

For people who don't understand, there's a concept — it was originally in SSL/TLS, that I did in the nineties — where basically a compromise in the future doesn't compromise all of the historical information. It's sometimes called forward secrecy, but I prefer that it's future-compromise-resistant. It's a property that Signal and iMessage and a number of other protocols do through various techniques, ratcheting techniques, etc. But you also have to be very careful with them.

Gordian Sealed Transaction Protocol now supports post-quantum as well as the existing X25519 and ChaCha20-Poly1305, et cetera. So it just requires a little bit more work to puzzle out what is the right answer to offer forward secrecy. So Stan, if you know some people that are interested in contributing in that area, that would be great.

And then obviously, you've identified the coordination problem, and the fact that there is a leak of correlatable public keys when we use multisig descriptors and things of that nature, that we've got to solve.

**Stan:** Yeah, that sounds good. I wasn't aware of some of the things you guys have been working on, so I'm definitely going to go back and ransack your website after this meeting.

**Christopher:** Yeah, and we also have a lot of stupid little things. So for instance, you had some code that you were sharing and you were reading that thing out loud. Well, we basically have something specific for that called Bytewords. It generates — we would generate three words, or maybe four words in this particular case. We specifically designed it so that it would be readable by people who don't have English as their native language, so we found dictionaries of what the things are that, say, Chinese or other international people would have when speaking English words. It also has a bunch of other interesting internal checksum properties, and compression possibilities.

In fact you're kind of already using it if you're using URs, because every two letters in a UR actually becomes a Byteword. So you can actually look at a UR and a Byteword and basically go, oh yeah, those match. So you can use it for verbal checksums and things of that nature.

So we've been thinking a lot about the UX problems as well. Like, the first and last four digits of something is the classic comparison of two hashes, and that's not good enough. So we recommend a LifeHash in combination with the Bytewords. Between the two of them, you get a lot more strength. It's small, but it's part of an overall stack where we're trying to think about the whole thing. So yeah, I would love to have you look stuff over.

**Stan:** Sounds good.

### LetheKit: Resurrecting the Air-Gapped Entropy Device

**Dan:** About a year ago I did attempt to procure all the items on the parts list. I wanted to build one myself. I even had a friend 3D print me the little box, so it's all ready to go. But there were one or two things I couldn't find, and then, I don't know, life marched on. But you remind me, I have this box of parts on my shelf somewhere. So I'm going to see if I can resurrect that and finally get that thing built.

**Christopher:** Yeah. So the guy who designed that — I just posted a few minutes ago, or maybe it was yesterday, basically saying hey, we ought to be looking at LetheKit again.

So what is LetheKit? The idea is it's a very small, simple commodity processor. I suspect that's probably maybe what your problem is, is that the commodity processor isn't available anymore. But it could be that maybe the development environment is out of date, or something of that nature.

The idea is that you can enter in your dice directly onto this little keyboard, and it will generate a UR on this little display. It works quite well — even if it's just a 128-by-128 display. No, it's a 200-by-200 display. But it works fine. And then he also has methods to put these onto dog tags as Shamir shares.

This was done — this is what, five years old, six years old? So we've had the capability of doing this stuff for a long time. It's just that we have underprioritized it. So yeah, talk to Ken Sedgwick. His name is in here, and like I said, he's still on the Gordian channel. With whatever obstacles that you have, I'm sure he'll be glad to help.

**Shannon:** And if you end up with some substitutions, please let us know so we can get them in there.

**Dan:** Oh yeah, yeah.

## Closing: Why Even Bitcoin Core Engineers Don't Back Up

**Christopher:** Okay, any last comments before we call it for the day? Okay, well thank you Stan, thank you everybody. We'll have a recording and transcript up later today.

And yeah, Tugume was putting into the chat that few people he has seen use multisig. That's just true. I'll be honest — part of what started this whole process was I interviewed some of the core developers, and people that had been involved in Bitcoin since like 2013. And the number who had really crappy backup strategies, or had lost what maybe then was not a whole lot of Bitcoin but today would be a million plus in Bitcoin, because of bad practices.

And then I asked them, okay, here's something that's better that would have addressed these. I recently was talking to one of them — it was six years ago that we came out with the book, seven years ago that we came out with the book — and he says, "Oh, I've been planning to do it." And he's a Bitcoin Core engineer. So clearly we failed if we can't even get Bitcoin Core engineers to do the right thing.

So we've got to find out how can we remove some of the steps, automate some of the steps, make it such that the whole UX is clean and of course secure.

**Shannon:** Yeah, I think automation is going to be the big answer to that, because as we've seen, a lot of us are just lazy about it. But if you don't use multisig, backing up with the shares like we showed today is a big way to make sure you don't lose your Bitcoin.

**Christopher:** Okay, well thank you, Tugume, glad you were able to join us. And as I said, we'll have a video of what you missed in the presentation later today.

**Stan:** Have a good week, everybody. Thanks for letting me participate.

**Christopher:** Thank you, Stan. Appreciate it.

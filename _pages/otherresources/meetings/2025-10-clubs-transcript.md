---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "Gordian Clubs"
title: "Gordian Developer Meeting: October 2025 Transcript"
hide_description: true
classes:
  - wide
permalink: /meetings/2025-10-clubs/transcript/
sidebar:
  nav: meetings
---

* **Participants:** Christopher Allen, Wolf McNally, Mark S. Miller, Alan Karp, Ryan Grant, Gordon Mohr, etc.
* **Duration:** ~90 minutes
* **AI Transcription:** Whisper Large v3 Turbo (with speaker diarization)
* **AI Processing:** Claude Opus 4 medium cleanup + standard summary

*Note: This transcript has been moderately edited for readability - filler words removed, errors fixed, fragmented sentences cleaned up,paragraphs added, technical terms standardized. Original content and speaker voice preserved throughout.*

---

## Pre-Presentation Discussion

**Christopher:** Good morning. Good morning, Alan. Thank you for making it.

**Alan:** I have to leave in an hour, but I didn't want to miss it entirely.

**Christopher:** I think we're going to get through the core of everything within that. I have more on some of the cryptography specifics around object capabilities at the end of the presentation. We're going to talk about it in general during it. But some of the actual cryptographic constructions and stuff, I think, depends on who the audience is.

**Alan:** Yeah. Well, I'm not a crypto guy. So I just get the general concepts, but the details are typically beyond me. If you want to solve a partial differential equation, I'm your guy. But if it's discrete math or that kind of thing, integers, I don't have the background.

**Christopher:** I mean, some of it is fairly straightforward. I don't think you'd have a problem with it, but then there's some of the more advanced constructions that are not necessarily intuitive.

So, what I love, what's one of the things I love about Schnorr is that the math is effectively finite field. So you're just adding very large absolute values. And so the constructions there are fairly easy. I think what's challenging is the, I call a lot of the constructions naive, meaning, yes, it absolutely works, except if you have an attacker who takes advantage of it.

**Alan:** Well, yeah, I attend some of Dan Boneh's crypto seminars, and I can usually understand the first 15 minutes of the talks.

**Christopher:** Yeah, I don't think we're going to go quite that far. And again, that'll be at the end of the talk.

**Alan:** Okay. Well, I'm going to have to miss that.

**Christopher:** I think we may get to it if you're here for the first hour. So we usually leave the last half hour for the broader discussion.

**Alan:** And if it's being recorded, just post the recording and then I'll get to the end.

**Christopher:** Yeah. Hi, welcome Mark. Thank you. You are the inspiration of this. I still can't exactly place when our, you know, when I first shared this with you what, 32 or 33 years ago. I remember at the time Xanadu was kind of in its, hmm, Autodesk is pulling out, what are we going to do now days.

**Mark:** Yeah, yeah. So that would have been around '91.

**Christopher:** Oh, okay, yep. So I think one of the reasons why I shared was, hmm, I'd like to try to do something with this. Or is Autodesk gonna stop me? And you said nope.

**Mark:** That's correct. Autodesk, well the Xanadu, all the Xanadu stuff was still proprietary and Autodesk did block us from freeing up the one thing that was not covered under the Ted proprietary arrangement, which was the trustee scheme. But I think I made the right call that Autodesk would not think to block a separate release of the club system.

**Christopher:** Especially implemented with a separate mechanism. Yeah. But anyhow, you were absolutely encouraging me then. And this is in some ways a tribute to that. So thank you for being here today.

**Mark:** You're welcome. Thank you for keeping at it.

**Christopher:** Well, I'm not the only one who's been working on some of this stuff for a while. But yes, sometimes it just takes time.

---

## Introduction and Welcome

**Christopher:** So I wanted to welcome everybody to the Blockchain Commons Gordian meeting, where each month we talk about different aspects of open, interoperable, secure, and compassionate digital infrastructure.

Today's topic is on Gordian Clubs. For those of you who are not familiar with Blockchain Commons, we're a community interested in self-sovereign control of digital assets and identity. We bring together stakeholders to collaboratively develop interoperable infrastructure. We design decentralized solutions where everyone wins, and we are a neutral not-for-profit to enable people to control their digital destiny.

I have to give a great appreciation to my sponsors over the last couple of years who have enabled this research to happen. In particular, lately, the Human Rights Foundation and the Zcash Community Grants have contributed to sponsor some of our work. If you're interested in sponsoring us, please let us know so that we can keep these activities moving forward.

At our last meeting, we had a demonstration of FROST signing on the command line, in particular using Bitcoin Development Kit and the ZF FROST tools. If you're interested in seeing that demonstration or trying out our code, it's available on our meetings page.

Today's topic is going to be about Project Xanadu, which was, if you're not familiar with it, way before its time. And then we're going to talk about some ideas from that, in particular around Gordian Clubs. We're going to give you an introduction, a demo, and then I'm hoping to talk a little bit more about the longer term opportunities.

---

## Project Xanadu Background

**Christopher:** So first off, Project Xanadu, obviously inspired by the Samuel Taylor Coleridge poem. The visionary was Ted Nelson, and I believe he's credited for coining the term hypertext.

**Alan:** Yes.

**Christopher:** The goal was a digital repository scheme for worldwide electronic publishing. And you have to recognize that he first started talking about this in the '60s and did a major influential book in the '70s. And then Autodesk basically picked it up in the '80s and hired a number of people, including Mark, who is here today, to basically make it happen. And I believe for not quite a decade, they had you working on different aspects of it. I became involved in the early 1990s, kind of in its trying to get their first public version out. And that was where a lot of this discussion started.

So I do have to explain a little bit about Project Xanadu versus the World Wide Web. It definitely was developed well before the Internet, and in some ways, it had some superior qualities. For instance, it had the idea of two-way links. It had some very interesting ideas about uniquely identifying users and documents and dynamically changing storage. And it had some early thoughts on micropayments.

In fact, that's how I got involved. I went to a conference and basically said, hey, Ted, how can you be saying that a byte is worth a byte is worth a byte? And somebody kind of pulled me aside afterward and said, who are you? Why are you asking these questions?

**Mark:** Was that Phil Salin?

**Christopher:** Yes, it was. And he was great. Miss him very much.

---

## The Xanadu Club System

**Christopher:** But in particular, the Xanadu Club system fascinated me. One of the aspects of it was it was public by default. The root of the whole system, you could kind of think of it in their terminology, would be kind of the root of the World Wide Web was public. Openness was default. And then privacy was partitioning within that.

They had this concept of a person as being a club of one. So there really wasn't a distinction between people and groups in the same way that other kinds of systems did. But probably the most important power was that clubs could be members of clubs. So a club as a group could hold rights and be members of other clubs. And this allowed for a lot of interesting recursive opportunities to grant different kinds of access and how groups would function internally. And then when you combine that with a number of locksmith options, clubs could grant access through a variety of different kinds of mechanisms. And I took all of these to heart.

I'd also, RSA was actually fairly new at that point. I became fascinated by it. And I thought, hmm, we could actually do this with RSA.

I want to say a little bit more about why it's revolutionary. So I really like that it had these sort of natural hierarchies without a central administration. And these nested permission structures allowed for flexible governance. And through the access control mechanisms, today we would probably say, oh, those are smart contracts. But they really were in a sense. And the team at Phil Salin over at Agorics did early smart contracts work.

And ultimately, it clicked because it modeled how human trust hierarchies actually work. And I think that was the thing that particularly grabbed me.

Here is a diagram from one of the documents back then. Basically we have Bob who is a group, can self-edit his club and he can self-read his club, but then he has privileges to be able to edit this poem, the Coleridge poem, but he also has the ability to edit a blind copy group who can read the poem but don't have the ability to edit it. And various parties, Chuck and Alice, have the ability to also participate in this BC Club and thus be able to read Bob's edition.

And that's from those old documents. And here is the key quote from the old documents that explains a little bit more about the meta issues, who can read what. And if you're trying to do this all in one particular type of thing, it could become infinite and big and lots of regression. But the internal loops and self-modification things made it much more comprehensible.

**Mark:** Could you back up to the club slide? I just want to point out one thing, which is, when a club is created, it's self-read and self-edit, which means it sort of ties off the meta levels. So you don't start off with an infinite hierarchy of meta levels, but you grow meta levels on demand. I think that was one of the most important things about the club system.

So for example, when the BCC club was created, it may have been created at that moment, disconnected from everything, but created by Bob, who had ability to, well, and could have been self-read and self-edit, but then Bob turns it into something that only he can read and edit. And you can, so for anything that's just a self-read and self-edit, sort of tying off the meta levels, when you find you need those distinctions, you can grow upward as well as by inclusion of other groups growing downward.

**Christopher:** Yeah. And that's exactly the thing that inspired me and I wanted to recreate for coming on 34 years, because this actually happened two years before I thought.

---

## The Problem with Xanadu Clubs

**Christopher:** But there is one problem with Xanadu clubs, given sort of today's thoughts on decentralization, is that it's a "mother may I." You're still going back to a centralized computer who is looking at this and, as has been discussed in the years since, it really still is sort of an access control list.

What I really wanted to do and what the inspiration that I shared with Mark back in '92 was, oh, I can prove I belong. And that is what will give me access.

And so, but, you know, again, we're talking 1991, 1992, but RSA was too slow. RSA was export restricted. So you had lots of problems if you wanted to sell outside the United States. And probably the most important was that good code didn't exist.

I mean, I basically, in '93, '94, did RSA Ref, which was really the first implementation of RSA that was kind of publicly accessible. And I wouldn't recommend anybody use RSA Ref anymore. But it was used by Netscape, by PGP, by DigiCash, all the early cryptography people did that. But until RSA Ref was available, good code wasn't.

Later, I wanted to do this in 2001 and discovered that a lot of the things that in order to make it work, the patents were either too restrictive or just absolutely too expensive. The timestamp for what we would today call hash chains and hash trees patent, they wanted 5% of global revenues, and they were just sticking on that. And I don't think they ever did anything with it, but that was their story.

So this is, for instance, one of the reasons why Bitcoin couldn't have happened until 2008, 2009, after the hash chain, hash tree patents expired.

Later I wanted to do this in 2014 but the problem was that the tool I wanted to use, which was Schnorr, Schnorr aggregation was not safe. And in a naive sense it worked, but it was able to be attacked. And this is what led to MuSig2 and MuSig-DN and FROST and other different types of techniques.

But now we're in 2025. The technology has finally caught up to the vision. With Gordian Clubs, we use Schnorr signatures, FROST, our own technology Gordian Envelopes, and XIDs. And the result is a pure mathematical object that encodes its permissions in their structure.

---

## Gordian Clubs: Recursive Cryptographic Capabilities

**Christopher:** So I need to explain a little bit more about this. So the key here is recursive cryptographic capabilities where the mathematics delegates. This can happen anywhere. Like the original Xanadu system, clubs grant capabilities to other clubs. The difference is there's no administrative intermediary. The chains are pure cryptography.

The recursive permission structure allows Club A to grant read access, to delegate subsets of it, all enforced by mathematics, not code. And thus, I feel like it really fits into the original Xanadu realization. It allows for hypertext objects to be shared with cryptographic enforcement. And you are what you can prove you can access.

So thus, rather than being a simple document, a club is an autonomous data object. With a Gordian Club, you...

**Alan:** Chris, does that include a way to audit?

**Christopher:** I will talk a little bit about that. So, you know, with autonomy comes some limitations. So, you know, definitely in the purest form, no, because it's completely autonomous and you can use these clubs completely offline. You could have received this on a USB key and have no active connection or the connection is extremely latent.

That being said, there are a variety of constructions within autonomy that can allow you to do some of this type of stuff. And then obviously when you're online, which you're going to need for certain kinds of operations, you can have constructions where you can't operate without an authority. But we'll talk a little bit more about that later.

---

## Autonomous Access

**Christopher:** So in the autonomous case, you receive an edition and the contents of this edition is encrypted and verifiable with a signature and a provenance mark. But how you got that edition is completely transport agnostic. Obviously the internet, it could have been peer-to-peer, could have been through Tor, maybe through an NFC card or Blockstream satellite, or as I said before, a sneaker net.

So you have cryptographic proofs and you use those to obtain your read privileges on the edition content. This decrypted edition may reveal that, in fact, there are other editions from the same or other clubs inside it, which you may or may not have access to. So you have as much access as you need, given the authority that has been granted to you by your keys.

If you are a member of the right club, you now have the ability to update the edition by authoring a new edition that can be accepted as authoritative due to your cryptographic proofs. So thus, there is no server, there is no database, there is no requirement to phone home.

---

## Cryptographic OCaps

**Christopher:** So in order to do this, we have to do access control. And this is what I call cryptographic OCaps. It is different than the classic OCaps that Mark and others here have been working on for years. But it is also very similar. We're using mathematics to enforce capabilities and delegation.

**Mark:** Can I just want to interrupt for a moment? It says Xanadu OCaps with cryptographic OCaps. What you're doing right now is cryptographic OCaps. But the move of a club system to an OCAP basis is your invention, not ours. The Xanadu club system was an ACL system. It was not an OCAP system.

**Christopher:** Correct. Okay, I see. Oh, I get your distinction there. But I think that my point is that you were replacing the traditional system, which in the years since we've gone, oh, that actually is still just an ACL. And the OCAP community has continued to work for these last three decades to really puzzle out a lot of the issues and opportunities and the security for OCaps.

And I just want to be clear, we can't do everything that the traditional OCAP community does. There are limitations on what we can do. That being said, it is very much inspired by what they've been implementing in the last 30 years. And if you're not familiar with it, I definitely encourage you to study how object capabilities work.

So how do we do this with cryptographic OCaps? So what we start at the bottom level is with permits. So these are cryptographic capabilities for read access that allow you to decrypt things. And we've made the decision, at least for this version, to focus on Schnorr signatures. And the reason why is that there are some very interesting opportunities for composability in what I call smart signature protocols.

In particular, there is a concept called adaptor signatures. Note that is A-D-A-P-T-O-R if you're searching for it. Scriptless Scripts will also give you a lot of archive and other cryptography documents that show some of the things going on there. But basically, they allow you to leverage signatures to give conditional authorization. And that is what really is the heart of an object capability.

We've added to that our work on FROST to allow for group decision capabilities. And so this is fundamentally what we're trying to do at this point.

For the existing OCAP community, it is the same principles, except for the fact that it's cryptographic enforcement, that these are mathematical objects. And I believe that there is no confused deputy problem because the math doesn't lie. But clearly it is different than classic OCaps. And there may be specific things that we would love that community to look at and contribute back.

**Mark:** I'm confused about the phrase "math doesn't lie." I did not spot a confused deputy problem in your design. So it might very well not have one. But what is "math doesn't lie"? How does that relate to that?

**Christopher:** Sure. And maybe that should be a different point. What I mean by "math doesn't lie" is that this is not a computational test. In other words, it isn't, you know, if A, then B in some kind of programming language. Instead, it is the math either works and you end up with a result of a 32-byte number that allows you to have access to it or it doesn't. Any corruption of the code isn't going to help you. You can corrupt the code all you want, but in the end, you can't get that answer unless you have the keys.

**Mark:** Confused Deputy has nothing to do with corrupting code. Norm made a terrible, terrible mistake in naming it confused deputy, because the phrase confused deputy suggests that the problem is a bug in the deputy. And if the deputy were better, he wouldn't be confused. And the real problem is the framework in which the deputy finds himself is one in which the deputy can neither ascertain nor express the distinctions needed to keep authority separate.

So the phrase I suggested to Norm, which he accepted, but never became popular, was commingled authority. It's a problem of losing the separation between different sets of authority, very much the way commingled bank accounts lose the distinction between money for this purpose and money for that purpose.

**Christopher:** Right, and we'll definitely be talking more about that in the future in the sense that we're going to be demonstrating in a few minutes a single user version of cryptographic clubs. And we have some solutions that allow for group delegation using FROST that I think avoid the confused deputy problems. So we'll be talking more about that in the future.

---

## The Schnorr Foundation

**Christopher:** So again, our choices for clubs all have to do with Schnorr. And I think there are a couple of properties of Schnorr that are powerful that we're leveraging. First off is that signatures are indistinguishable. A single-party signature is exactly the same as a multi-party signature. In all of our stack, there's no way for you to know did one person sign it or was this a multi-party signature.

Thus, simple workflows are really pretty similar to complex workflows because in the end you're just verifying a Schnorr signature, same mathematical structure. Another aspect of Schnorr is that it has mathematical linearity, and that allows for seamless composition.

So this allows us to start with basic cryptographic objects, which we are going to be demonstrating today, and these can evolve to very sophisticated multi-party protocols using some of the rest of our toolbox. And thus, the external systems to verify these objects don't really need to change because they all use the same verification logic and allows for infinite internal complexity for how these objects were created. But the verification is very simple.

Specifically, as far as composability, the primitives allow us to layer the complexity without breaking compatibility. They all use the same mathematical foundation. They all produce indistinguishable signatures. All the primitives can be combined and layered. Thus, I really want to consider them to be cryptographic Lego blocks that can fit together in a variety of ways.

With Gordian Clubs, we're not using all the Lego blocks. And there are also new emerging Lego blocks that may offer incredible opportunities. But what are the Lego blocks we're using? Obviously, we're using adaptor signatures. And we'll talk a little bit about that.

If you're familiar with Bitcoin, the Bitcoin tweak that is used for Taproot is a form of adaptor signature. Basically, the signature is modified such that if you don't have knowledge of the hash tree, you can't verify the signature. Thus, when a pay-to-script-hash style script is out there, and now you're actually going to publish the transaction, you have to prove that you have knowledge of the hidden values in order for that signature to be valid. But you don't have to actually reveal those hidden values to do that. You're just proving it. That's an adaptor signature.

And then the other area that we're leveraging is FROST. As I said earlier in our call, we had a demonstration of FROST with Bitcoin at our last meeting.

But there are a lot of other future Lego blocks. MuSig2 offers a sort of trusted key aggregation and has some properties that are different than FROST that might be useful in certain situations, especially when you want to have accountability for decisions. FROST is very anonymous in the sense that even if you were a party, say it was a 4-of-100, okay, and the signature passed, which meant at least four people cooperated to sign something, you, as one of those four, cannot prove that you are one of those four. That's how anonymous it is. In some cases, that's wonderful. In some cases, it's not.

MuSig2 offers opportunities to be accountable. There is a technology called Schnorr Blind Signatures, which offers interesting privacy opportunities. There's some papers I wrote at the first Rebooting and then subsequently presented at a variety of cryptographic conferences for predicate logic languages that can allow for more complex conditional authorization. And they can also work in here because they can produce 32-byte random numbers or apparently random numbers that can be used to do more complex conditional authorization.

We are not implementing any of that in this first implementation. But the point is we can build complex systems from simple interoperable parts and then we can add more kinds of tools to create them over time.

---

## Lego Block #1: Scriptless Scripts (Adaptor Signatures)

**Christopher:** So our Lego block number one sometimes is called scriptless scripts. This was Andrew Poelstra who called it that. And basically, if condition X, then unlock Y. This is the mathematical proof. And it's being used all over the place for atomic operations across systems.

Besides the way Bitcoin Taproot uses it, Lightning uses it extensively. There are lots of interesting opportunities with Lightning that have been demonstrated. For instance, you have the ability to create a delegatable, attenuated subscription to, say, the New York Times, which allows you to have access for one day of somebody else's year-long subscription. And the act of using that will actually reveal the payment, the micropayment, that allows the Times to continue to publish these things.

So these are already actively implemented in the Lightning Network. And I'm sure Mark and other people here can talk about how other kinds of atomic operations in other systems are being used with some of the workflows they're doing with traditional OCaps. But I think the point is with adaptor signatures, we have this elegant cryptographic composition.

And this allows us infrastructure-free logic where we can embed conditions directly into the Schnorr signatures, we can chain them, and we can build workflows that are pure mathematics. I think we have demonstrated in the basics that we can offer conditional access, delegation. I think attenuation is still a work in progress, but is definitely feasible. And in fact, I'm hoping some of this community might help us extend that.

And of course, atomic swaps is already happening on other uses of adaptor signatures. It allows for cross-system coordination and different kinds of delegation chains to other systems. And all the logic is expressible in math, no servers required. So you can have highly distributed systems.

**Alan:** You didn't mention revocation.

**Christopher:** I didn't, and we'll talk about that.

---

## Limitations of Adaptor Signatures

**Christopher:** So what are the limitations? Well, first off, it's limited to signature mathematics. So it is bounded by what elliptic curves can express. So in the case of Bitcoin, it's 2 to the 256 minus 766, I think, is the integer bounds there.

It doesn't allow for loops or iterations. So the mathematics are finite, not computational. You can't conditionally branch on external events. The logic must be deterministic. Specific to your revocation question, what that means is you can absolutely use an external oracle who publishes a secret in a variety of different ways that allows you to revoke a particular item if nobody can get access to that secret. But then that means that you do have to check that oracle. And that introduces other kinds of denial of service.

But then there are interesting technologies of decentralized and distributed oracles that you can leverage. So for instance, I could set this up that it leverages a time-locked transaction on Bitcoin and basically can only function as long as that time lock has not been spent. So there are opportunities here. And in fact, I think Ryan is here on the call, Ryan Grant is using this with a DID method that can leverage this.

But it is, basically the logic must be deterministic. Also, in the adaptor signature Lego block, we do not have the ability to have mutable state. Each operation creates new mathematical objects. You don't have state here.

These limitations do offer freedoms, which means if you're willing to live with these limitations, you don't have platform lock-in, you can't be censored, and you can't be surveilled. So this means that at least with the base set, mathematical certainty replaces administrative whim.

---

## Lego Block #2: FROST Threshold Operations

**Christopher:** The second Lego block is FROST, which basically offers threshold operations. If you're not familiar with FROST, I think it's a very important future for the next decade. It basically allows you to create M-of-N signatures that look like a regular single signature. They're privacy preserving. You can't tell who participated. And the verification logic is basically a simple Schnorr signature.

The two core applications that we are doing is threshold signing, which is group authorization for updates. We're using the Zcash libraries for this, which have been well vetted both in the cryptography and the code. But we're also using FROST groups for group-managed provenance, which gives you shared custody of version history.

And we'll talk about this later, but we do not have cryptographic proofs for this particular construction and the code has not been audited, but it basically works. So this is one of the things we're hoping the community will help us move forward on.

But you don't really need to use FROST right now. You can do single-party authorization. You can do multi-party but non-FROST threshold operations and then seamlessly upgrade to group governance when you need to. And external systems just never need to know the difference.

So, you know, FROST in practice, you have to have a signing ceremony. You coordinate off-chain or offline to generate the signatures. Wait, I'm a little tangled here. So what we're also talking here about, we also have something SSKR, which we're going to talk about in a minute. That's what's off-chain, sorry.

So you have to have these signing ceremonies with FROST. The provenance chain allows for shared custody of the version history, and thus the use cases for FROST in group governance include governance without servers, community decisions by cryptographic proofs, and you can actually design succession planning into the mathematics.

But the limitations of FROST thresholds are that it does require secure multi-party communications. If you are going to do a signing rapidly, you're going to need some form of network availability. I mean, if you're going to do it with sneakernet, you absolutely can. But it might take a while for you to move all those USB keys into all of those places.

There is a key management burden, not quite the same as private key/public key, because you do have some resilience because you don't have to have all the shares available. But it is a key secret management burden on each of the parties. And you can't compose mid-ceremony.

So let's say we're in the middle of a ceremony of 4-of-9, and you've got three of the parties, and you're kind of desperately flailing to be able to get the fourth party to sign, you're kind of stuck if you're in the midst of a much larger complex system. You have to wait until you have that signature.

But the advantage is that you do have the freedom from single points of failure and control, and they do allow for democratic governance through different forms of cryptographic consensus.

---

## Future Lego Blocks

**Christopher:** Again, this is an expanding toolkit. Each builds on the same Schnorr foundation. I've mentioned MuSig2, which allows for key aggregation with accountability. We have some initial thoughts on how to do delegation of read and write rights, but attenuation is a little more complex. I think there's some interesting opportunities in blind signatures, which preserves privacy, prevents signature correlation, etc. And then, of course, predicate logic. Think Bitcoin script, but simpler, where the operations are on 32-byte numbers.

So for developers, I think the key point is we're building on stable foundations already being adopted by Bitcoin, Zcash, and other projects. And these allow us to bring mathematical certainty into an uncertain world.

---

## Discussion: Caveats and Attenuation

**Christopher:** So now we're going to move more toward the demo. So we have some terms that you're probably not familiar with if you've never been... Go ahead.

**Alan:** Before you go on with the demo, a lot of capability systems want to have something that I guess ZCAP calls caveats. And those are explicitly ruled out, I think you said. Right? The most common one is time, you know, timeout.

**Mark:** It's not valid. I'm sorry, Alan, is caveats distinct from attenuation?

**Alan:** Well, for example, an expiration time would be a caveat.

**Mark:** That's also an attenuation. I give you a read permission, but it expires at noon. That's an attenuation. How is that distinct from attenuation?

**Alan:** All right. You can view it, I guess you can view it as an attenuation.

**Mark:** But I mean, attenuation is abstractly what you're doing. And a caveat is a particular implementation technique for expressing it.

**Alan:** Okay, fair enough. But it seems that since there's no conditionals based on external events, that they're not expressible here. That's the question I have.

**Christopher:** So to be clear, with Gordian Clubs, we're choosing all of the autonomous parts of our toolbox. But that being said, if you're willing to go outside of that toolbox to an oracle, you can go to a timestamp server. You could basically say, this is not valid until block such-and-such has been proven on a blockchain. So there are things to do there, but then you lose some of the autonomy.

But there are actually some opportunities within the autonomy. So for instance, there are these verifiable delay functions. Now, it's not useful for the specific thing you mentioned, but it's useful for "it is valid when." But the "when" is not deterministic by a clock. It's deterministic by a verifiable delay function where you have to prove that you've done the computation, which will require time to do.

So you could say, "Yeah, this is valid only when somebody is willing to expend the time in a verifiable delay function that represents 10 days of computation." And when that computation is complete, you now can basically end up with a 32-byte number that you can prove that, yes, I did spend 10 days doing this. And thus, I'm entitled to do a rotation or something of that nature.

**Alan:** Yeah, Ryan. I just want to finish the thought here. The concept is called risk-adaptive access control. The example I always use is you can read this document unless the US is at war with Canada. That used to be funny, but that's the kind of thing I was looking for.

**Christopher:** Yeah. I would love to be able to do it with prediction markets, for instance, where you can basically say, given this prediction market and cryptographic result, when the prediction market is finalized, such-and-such can happen. Ryan, go ahead.

**Ryan:** Yeah, I just wanted to check, are there verifiable delay functions that are not proof of work? Or is it, I guess, maybe you could also use like a secure element, but are there other styles?

**Christopher:** Yes. So they're basically operations. And again, look up VDF, verifiable delay functions. And there are a number of oddball blockchains that are leveraging this to try to get around having massive numbers of computers doing hash calculations. And basically, they're specifically designed so that they can't function in a distributed manner and that they rely on various properties of chips to do their functions. How secure they are is emerging. But some of them are more than 10 years old. And some of them, like I said, they're already blockchains that are using those in lieu of the Bitcoin method of doing this.

**Ryan:** So basically anti-parallel, hardware aware, computer architecture aware proof of work, but kind of still in that class.

**Christopher:** Correct. And then obviously you can do this with TEEs. You can do that with other different types of things. There's a lot of interesting things in sort of the multi-party computation with homomorphic computation where you can also have some of these types of delay functions with a truly homomorphic protocol. Again, all emerging. But I think my point is, ultimately, at the end, it's an apparently random 32-byte number. And you can use it in our protocol.

---

## Gordian Stack Technologies

**Christopher:** So, anyhow, let's... if you haven't been to our Gordian meetings, or have looked at our stack, we're going to be using some terminology that you may not be familiar with. I wanted to go over it real quickly so that when Wolf does his demo, you'll have some idea when he says dCBOR, what the heck is that?

So we've selected data structures. In particular, dCBOR is our underlying structure and Gordian Envelope on top of that. Our Lego blocks that we're going to be talking about today are FROST and Adaptor Signatures. We have our own identity layer called XIDs. It is inspired by the DID standard that I'm the co-author of, but it is not quite the same. We secure our data with something called an envelope permit. We'll be talking more about that. And we have two coordination protocols, provenance marks for offering provenance, and Gordian Seal Transport Protocol, which basically allows for these objects to be communicated and the protocols to happen over any kind of connection, including sneakernet.

dCBOR is a binary encoding. It is basically a more constrained version of the IETF CBOR construction specifically designed to produce the identical bytes in all situations. And it is our foundation for cryptographic verification and content addressing. And it is an internet draft and being discussed actively by an IETF working group.

Gordian Envelope, we have a whole bunch of videos on this. We have an hour introduction that is really very accessible. Trying to condense it all is hard, but it is kind of a hash tree of hash trees. And it offers a subject-predicate-object semantic structure. And this allows us to do selective disclosure through both elision or encryption. We can reveal only what is needed and even exclude it.

So we have some situations where, for instance, we're limited to 1K if we're going to distribute a XID or an envelope or a club through the mainline DHT used by BitTorrent. So we can basically say, well, the only important bits you need to know are this, but we're going to make a commitment to everything else. And so we're going to give you the 1K that you need to be able to move forward from here. But we've committed to all the rest of the pieces which you can collect in other places. So this is an important concept for how clubs work.

And then finally, really the most important part, and this is really how we do some of the club features and such, is that it's radically recursive. Everything is an envelope. In fact, the subject is an envelope. The predicate is an envelope. The object is an envelope. And all of the subparts of that are envelopes.

So you can do these fairly radical things with this. And you might go, why would I want a predicate to be an envelope? Well, you might want to hide the predicate, elide it. But you also might want to have, well, this is the JSON-LD semantic information about this predicate. No, this is the OWL information about this predicate. Oh, no, we're using this particular zero-knowledge proof technique. All of those things can be in a tree of objects inside the predicate.

We've talked about the Schnorr Lego blocks. We have libraries for doing FROST. We're largely leveraging the Zcash libraries. And in order to make them function, we have some simple adaptor signature things. Mostly we use that right now with the Bitcoin Taproot adaptor signature.

---

## XIDs (eXtensible IDentifiers)

**Christopher:** What is a XID? It's a 32-byte cryptographic identifier that is derived from a key. In some ways, this is similar to DID Key, except for the fact that one of the first things you will probably do is rotate that key out, because what we're really wanting is an apparently random identifier that we can prove.

We have a variety of mechanisms to resolve them to XID documents that can offer communication keys, endpoints, permissions, but most importantly support key rotation while maintaining a stable identity. It's basically kind of what I would call a DID 3.0 technology.

---

## Permits: Multiple Ways to Access Content

**Christopher:** Permits are multiple ways to access the same content. So underlying any particular object that we are encrypting, we're doing ChaCha-Poly, the IETF standard for symmetric key encryption. And that basically uses a secret, a symmetric key, to encrypt it. That is what we're encrypting in a variety of different ways.

So we have XID permits that allow you to do identity-based access with key rotation support. We have simple public key permits, which allows direct cryptographic access to a specific public key. We have SSKR thresholds. It's basically Shamir. You could set it up that, hey, you have to get things from 3-of-5 different places. And if you get all three of those, you basically can recreate what is necessary to get the symmetric key for the object at its heart. And then, of course, simple password permits. You can just stretch a password and use that and just basically share the passwords offline.

But the point is you have multiple kinds of permits. I believe, like a lot of our other toolboxes, these are extendable as long as they basically reveal the base symmetric key in some kind of fashion.

**Mark:** Before you advance, I have a question. Sure. The SSKR thresholds, that would be the mechanism you would use for social recovery, correct?

**Christopher:** Correct. Yes, it basically uses Shamir, except for the fact that it also has a second layer, a Shamir of a Shamir. So you can do things, you can obviously do 4-of-9. That's one of my favorites because it has a particular property that it is very similar to 2, 3-of-3s. So, excuse me, 2-of-3, 2-of-3s. Does that make sense? We still have nine shares, but we're saying that they're segregated so that 3, 3, and 3, you have to cross the groups. Does that make sense?

So you're gonna have these three keys be offline keys, these three be friends, and these three be family. And somebody who colludes and gets all three of your family members doesn't matter because you still need something from one of the other two.

**Mark:** A quorum from one of the other two. It's basically a multi-cameral voting rule.

**Christopher:** Correct. And it is, FROST can do some similar types of things, but the difference is that FROST never reveals the private key. With SSKR, because it's Shamir, once you've got your shares, you have the key to be able to do it. You have all the information once you have the number of shares. But that does mean it allows you to do offline key reconstruction. So pros and cons.

---

## Provenance Marks

**Christopher:** The final two kind of terminology from our Gordian stack are provenance marks and GSTP. So provenance marks are, it's basically a hash chain. Our goal is how do we do sequential tamper-evident chains of document versions? But we're not trying to do a lot of the other things that other kinds of hash chains try to do. All it does is cryptographic link the content digest to the previous mark.

So you're starting with a genesis mark, the zero mark. You're progressing to number one. You're progressing to number two in a continuous chain. But we are not trying to do like what Git does when it does its own provenance of also saying, oh, it has to be a diff of the previous version. You could literally have completely discontinuous objects that you are basically saying, I'm making a statement about this. And it is provably after this statement. But it's not making a proof about time. It's making a proof about ordering.

**Wolf:** I think one of the things we forgot to mention on this slide is that one thing that does make provenance marks unique is that each provenance mark includes a pre-commitment to a secret that has to be revealed in the next mark. So it's not just a backwards looking chain. In fact, the backwards looking hash or whatever, which we use in clubs, is optional, whereas the forward looking commitment is intrinsic to it.

**Christopher:** Right. And we've done a lot of going on.

**Mark:** Does the forward looking thing... I hadn't heard about this before. Does the forward-looking thing prevent branching or inhibit branching?

**Wolf:** Not necessarily. It depends on... it's possible to branch it. But then the question is, how do you decide which is authoritative? Part of what makes, if you're going to use simple forward branching, we discussed this in our previous talk, so I don't want to go over too much of that, because we did do a recent talk on provenance marks. But part of what makes provenance marks secure is a number of people who are replicating a chain in a kind of distributed way. So you can actually cross-reference them.

And so if whoever holds the seed that generates the provenance mark or even a FROST quorum, which is what we're developing now, tries to branch it and then tries to publish branches of it, then that gets flagged by the community of whoever's archiving the chains as not allowed or allowed, depending on what the protocols are you built on.

**Mark:** Okay. It's just a building block. So if you have disjoint audiences that can't compare notes, then you can still branch.

**Wolf:** Correct. Yes. Well, in fact, if I issue a provenance mark and nobody has ever read it, I can issue another one on top of it that's different. So it's just that when it becomes a subtle consensus that it becomes very tamper evident and nobody else, other third party who does not have access to the seed that generates the whole chain or in the case of FROST, the FROST quorum that generates the whole chain, can actually create the next mark.

**Christopher:** And there are other Lego blocks that use Schnorr that we could use in the future that might allow for the provable reconciliation of branches and things of that nature. None of them are solid enough or easy enough that we want to use them right now.

That being said, within the context of the FROST provenance mark, a lot of it has to do with the nature of the quorum. If you have a quorum that is a majority or a supermajority, then it's very hard to fork. Because if you're going to require 66-of-100 people to cooperate to advance the mark, well, clearly that has the majority. And you can make statements about that majority in a variety of different ways such that you know that it requires 66% or 67 votes to advance and thus can give that one priority over a different quorum.

But clearly there are sometimes advantages to forking. I mean, you could have a 4-of-100 where it's basically, as far as we're concerned, when we set this up, only four people need to agree to advance this provenance mark. Clearly there are lots of sets of four people in and out of 100 that could advance it. And it's not our responsibility, the responsibility of the underlying cryptographic club object, to reconcile that.

We also have a variety of different user interface tricks and other different types of things to make it human readable, to use it with some of our QR and UR, you know, UX stuff to allow for provenance marks to be used in a variety of other circumstances. But in the case of clubs, we're using the full version of it.

---

## GSTP (Gordian Seal Transport Protocol)

**Christopher:** Finally, we have GSTP, which we're not going to be demonstrating today, but basically it's secure multi-party coordination that's transport agnostic. And this is what allows us to do the sneakernet type of things. The particular feature of it is that in a multi-part protocol, it's stateless in the sense that if you have state, you basically encrypt your state with your keys, encapsulate it inside what you're passing to the next party such that when they pass it, they can't decrypt it. But it's their responsibility, if you're going to continue the protocol, that they pass it back to you so that you can decrypt it and get the state you need to proceed in the protocol.

**Wolf:** That state is self-encrypted and self-signed. So you know it's what you sent yourself into the future, essentially.

**Christopher:** Yeah. So it basically offers a lot of interesting secure coordination without infrastructure opportunities.

---

## How Gordian Clubs Work

**Christopher:** So you need to understand these to understand Wolf's demo. Basically, he's going to create a club using a XID. The XID identifies the publisher across updates. The content of that XID is stored in a Gordian Envelope using dCBOR. He's going to demonstrate multiple permits that allow different access methods.

Later, if members make decisions, the FROST Threshold Signatures can authorize additional updates. Provenance marks create tamper-evident history of the changes in the new editions. And GSTP coordinates secure communication between the members.

The result: an autonomous cryptographic object. No servers, no databases, no central authority. Mathematical proofs replace administrative control. Unstoppable, private, censorship-resistant collaboration.

Again, it's a layered onion. The envelope is the dCBOR encoded structure. The public metadata is the visible information that is non-encrypted, the club ID, the version, the provenance details. The encrypted payload, which is the actual club content and member data. The access layer is the collection of permits. And the governance layer inside there is what are the rules for the signatures and provenance chains.

Right now, we are supporting read and write access. There are various kinds of other permissions that could be granted. We are not really trying to explore that right now, but the OCAP community may have additional ideas here.

Basically, read access gives you the ability to decrypt and view the current club content. The multiple permit types allow for different access methods. One of my favorite access methods of Mark's at Xanadu was the knock club, which is basically you knock and you get in. It's just all you have to do is ask. So you can have something as simple as that. And you can have one time or ongoing access depending on the permit type. And then the next step up from that is write access.

**Mark:** This is what you...

**Christopher:** Go ahead.

**Mark:** When you say one time, that sounds like mutable state that once you've used it, you can no longer use it.

**Christopher:** Sorry, one per edition. So I could basically, by how I encrypt the internal object, when I do the next edition, I will, the best practice is you're actually going to change the symmetric key for that particular edition. You're going to make a commitment to the content.

But you can choose not to, which means the people who had access to the previous edition will still have access or privileges on the new edition because the symmetric key that is revealed is the same in both. Also with public key, there's some public key things you can do. But the point is it's not saying one or the other. It is a design choice per club.

For write access, obviously you have to be able to sign, but you also need to be able to do a provenance mark. So you're really having to know two secrets: one which is the seed of what you're using to generate the keys or the XID details, and the provenance genesis seed or the shares in a FROST quorum that allow you to participate in generating the next value.

So the point is you have to ensure ordering in order to prove that you had write access.

Keep concept, flexible governance while maintaining security. Again, unblockable, perfect privacy. It's usable in disaster resilience scenarios, internet outages, infrastructure failures. No authority can revoke these mathematical proofs. If you have the secrets, you can do this. Thus, the control rests with the key holders, not any platform operator.

Can't be algorithmically suppressed. Can't be deplatformed. Mathematical certainty replaces platform dependency.

Why might you use this? Infrastructure can't be trusted. Dissidents organizing without surveillance. Long-term archival that outlives companies. Emergency coordination during outages. Mathematical proofs that can endure when servers don't.

---

## Demo Introduction

**Christopher:** So Wolf's going to do a demo. He's going to show you identity setup. He's going to show how the Genesis edition is created for the provenance mark. He's going to demonstrate some access methods, some edition updates, and then the verification. What you'll want to look for is these self-contained artifacts working without any infrastructure.

He is going to be showing SSKR, which is the 2-of-3 threshold. Again, anybody who can get the two shares can reconstruct the decryption key. But once you have it, that's not revocable. You have it and you have that now forever, which is different than in the case of FROST.

Ultimately, this is what the encrypted object looks like when it's encrypted, which you can see almost all...

**Mark:** Could you make the font larger on that?

**Wolf:** Sure. There will be other versions of this during the demo as well where it will be larger. Yeah. And this is just to be real clear, this is the text representation. It's called envelope notation. So it's not JSON. It's not CBOR diagnostic notation. It's envelope notation. And like I said, I go deeply into this in our other videos and talks and papers on envelope, but this is basically showing the hierarchy of the subject, which is encrypted, and then a set of predicate-object assertions on that object. And that's all wrapped and then you see a predicate-object assertion for the signature. So all of this is using our command line interface.

**Christopher:** So the clubs edition verify is what proves the authenticity and then you can do sequences. He's going to show the content decryption and the provenance progression. This is in the clubs CLI Rust demo in our repository.

You have the foundations, I hope, to understand the demo. And I'll turn it over to Wolf.

---

## Wolf's Demo: clubs-cli-rust

**Wolf:** Great. Thank you, Christopher. So I'm going to share my screen here. Okay, so you should be seeing my Visual Studio Code window now.

**Alan:** And what you're seeing at the top is a markdown file called demolog.md.

**Wolf:** At the bottom, you see my terminal. That's all we really need for this. There are several tools that I've installed, which are all in our stack and all available. If you're familiar with Rust, our tools are written in Rust. And so our tools are all installable using the Cargo Package Manager from crates.io.

I'll be using ZSH for this demo. So I'm going to set up, basically I'm hitting control enter on these lines here. And that just tells VS Code to retype the line and execute it in the terminal. So I'm going to be doing a lot of that in this.

But basically in this log, you'll see in this case, for example, I'm just looping over the names of the required tools here: Cargo, which is obviously installed with Rust, Envelope, which is our Gordian Envelope Manager command line tool, Provenance, which is our Provenance Mark command line tool, and Clubs, which is the new tool that we're going to be mainly talking about here.

Like I say, we've done previous talks, extensive talks on envelope and provenance marks, and I urge you to check those out. I'm happy to answer specific questions about them as we go.

### Setting Up Cryptographic Material

**Wolf:** The first thing this little log does is it just shows you the versions that are installed, so we can double check that you have the versions installed that we have published. Then we have a directory here, which is the code of the Club CLI directory. This is the Club CLI tool. This is the demo directory.

It's only used to store the state of the provenance mark generator because you need a private seed for that, as well as it keeps the history of your marks. But the generator, which is storage JSON here, the seed is a randomly chosen number that's encrypted here, and you never give it out. So it has the same kind of risk profile as a private key, which of course part of what using FROST to do this is to spread that across several people in a governable way and less risky way.

So, but I'm not going to dwell on FROST in this demo. This is a single publisher, many receiver. So this basically deletes that directory and recreates it. So we have a place to store that.

So the next thing we're going to do is the publisher is going to spin up its cryptographic material. And this is done in several steps. We use the envelope tool here. We use the sub command generate private keys. And then we're going to echo each of these to the terminal.

*(demonstrates creating publisher, Alice, and Bob XIDs)*

What you're seeing is when you see a UR, this is our, basically, our ASCII armored binary, this sequence here. And if you go into our demos and talks and so on, the baseline is actually human readable words, called byte words, each two letters is basically first and last letter of an English word. There's 256 of them. That's what they're called bite words.

And so UR is basically a way of specifying tagged CBOR essentially. So if you're familiar with CBOR, this is just a tagged CBOR object. This is a specific kind of tag, and this is the crypto private key base, which is basically just key material. And this is the actual XID document in this case.

### Creating the Genesis Edition

**Wolf:** So now we have three participants: the publisher, Alice, and Bob, and they're fully kitted out now. So the next thing we're going to do is the publisher wants to create a new edition, the first edition in their club. To do that, they have to have the content of the edition, which is going to be a Gordian Envelope.

And as you see here, they're going to take the actual payload, which is a string: "Welcome to Gordian Club!" Now, a Gordian Envelope can be as simple as a string, which is just a tag and the string itself is just the whole Gordian Envelope. The nice thing about Gordian Envelopes is it's a very elegant, very minimal structure that allows branching.

**Mark:** I'm sorry, could you, in terms of which parts are the subject, the predicate and the object, could you point out, expose that again?

**Wolf:** So when you see a string by itself, that is the subject of the envelope. It has no assertions. There's no predicates or objects. And that's what we do the first one here. Envelope subject type string. And then we give it the string and that creates this first thing. And then we format it. So you can actually see that this is the UR form. This is the transportable form. This is the human readable form.

The next thing we do is we say envelope assertion add predicate object string title. This is the predicate and string genesis edition. So we're basically adding a single assertion to this subject title genesis edition.

The third thing we're going to do is we're going to wrap that envelope. That's because when you add a signature to this, it only signs the subject. So if we signed this right now by adding a signature, we'll only be signing the subject, which means this assertion wouldn't be covered by that signature. So we're going to wrap it and now it's wrapped. You see the outer curly braces. And by doing that we can add assertions to this like the signature or basically take its digest, which is the most important thing we're going to do with this right now.

**Christopher:** Yeah, also everything, all of these pieces underneath themselves are envelopes. So for instance, that title and Genesis Edition themselves can be a Gordian Envelope.

**Wolf:** Yeah, exactly. Well, in fact, they are Gordian Envelopes, but they have no assertions on them. If you saw assertions on them, then you'd see additional pairs of brackets or curly brackets.

### Envelope Structure Discussion

**Mark:** Actually, could you do that? Just show us an example of what it's like when it's nested in that way in this readable notation.

**Wolf:** Well, here the XID document itself, the XID which is just a 32-byte number, a tagged CBOR 32-byte number, has one assertion, a key. The key assertion, that key assertion is a public key object. That public key object itself has one assertion which is the private key object, which itself has an assertion which is the salt. Actually two assertions: the first one is private key assertion, which has an assertion, and the other one is the ALL assertion. So it's two assertions. This assertion as a whole has another assertion on it, which is the salt. So when you elide a branch, you basically, the digest takes everything into account because it basically uses the serialized dCBOR, which is one of the reasons why we use dCBOR, which is deterministic CBOR, because we want the same semantics to always encode the same byte stream. So when we take digests, we can be assured that if you put the same semantics in, you get the same digest out.

**Mark:** Where's the closed curly?

**Wolf:** Oh, the closed curly here, there's two of them.

**Mark:** That's the one I was looking for.

### Semantic Structure Discussion

**Mark:** The textual notation you're using here, with the square brackets and the angle brackets and the colons, it's clearly not JSON, but it seems to be conveying information that's sort of similar to JSON. Is it YAML or is it...

**Wolf:** It's what we call envelope notation. It was developed for Gordian Envelopes. We discussed extensively in both our videos and so on, but basically it's designed to be a form of diagnostic notation. It's not designed to be round-trippable. Like I say, this is a 32-byte number, but you only see the first eight bytes for recognition because it's designed to be diagnostic.

**Ryan:** I don't know yet why assertions and predicates are a valuable distinction. I think that theoretically it makes a lot of sense, but I'm looking at the objects and thinking, well, you know, I could just kind of ask for one predicate or another predicate, right?

**Wolf:** Yes. I mean, well, the idea is you have, it basically goes back to the semantic web idea of subject-predicate-object assertions. But the idea is that you're going to have a subject about which you make a number of assertions. The subject can stand alone with no assertions on it. You can also make a number of assertions on what's called the unit object, which the unit subject, which has no, carries no content.

So it's up to you to decide how to use these things. We have kind of conventions we've developed as to how we use them. For example, in this case, this is like the title of the edition in this case, or maybe this could be like a newsletter and this could be metadata about the edition, which is the title. And so our APIs, they'll let you query these by various criteria. And in fact, we have a regex-like pattern matching language that you can also use in one of our crates that lets you actually match on various parts of the hierarchy of envelopes.

**Christopher:** But also to be clear, we are not doing JSON-LD here. There is not any semantic content, semantic details are optional. And you would basically, you can do those by turning a predicate into adding additional predicates and objects to the predicate if you need to.

**Wolf:** Also, I should note that rather than have to do that, we also have noticed that some of these things are double-quoted, like these literal strings, "Welcome to Gordian Club." Other things, sometimes you'll see single quotes. These are what we call known values. We have a registry of known values, which are essentially, they substitute for like a long URL you might want to include from an ontology of some kind, because even though it's produced here in the envelope notation as a string, a human-readable string, it's actually just an encoded up to 64-bit number.

So you don't have to encode all these strings. When you see a single quoted string, it's just a number, but it's a known number, so people can register them. Eventually, we'd like to see all the major values from various ontologies in our registry. And that way, you can basically represent all the things you can do with JSON-LD and OWL and all that in Gordian Envelopes without taking much space at all.

### Creating and Publishing the Edition

**Wolf:** *(continues demo, creating provenance marks, composing editions with permits)*

So now we're going to actually create the content that we're going to publish as part of our edition. And there you see, there's the wrapped subject with a single assertion on it. And it's been wrapped. That means when we take the digest of this, it'll be a unitary digest and that'll be important.

So now we're going to capture the content digest... And there it is. And you notice that it's actually identical here and here, because there's no variable data in this. It's all deterministic. This is just a known set of values here. So it's always deterministic.

So now we're going to start the provenance marks chain. The first mark in a chain is called the genesis mark after the genesis block in Bitcoin. And it has certain attributes which are checkable. You can always recognize a genesis mark. It has no predecessor, but it also commits, it has a hash which commits to a key, which is a number which will be revealed in the next mark. That's the pre-commitment.

*(demonstrates provenance mark creation with human-readable representations)*

This is using what we call byte emojis, where we have 256 curated emojis that can represent bytes. And again, this is just for human recognition and searching. This is the actual full provenance mark, which includes the hash, the key from the previous mark, if any, the sequence number, the date, all those kinds of things.

### Recognition and Verification

**Mark:** I'm sorry, could you explain again the thing that's highlighted there, the sequence of icons?

**Wolf:** Yeah. So there's four bytes that when you actually read the paper on the provenance mark, you'll see that we actually obfuscate the actual, even though the provenance mark is not encrypted, it is obfuscated. So it basically becomes a sequence of random bytes. And we take four of those characteristic random bytes and use those as a recognition key.

In this case, each of those bytes is being transformed to the corresponding byte word. Again, there's 256 common four-letter English words. And they have certain attributes. Like the first three letters are always unique. The last three letters are always unique. And the first and last letter are always unique.

**Christopher:** These are not cryptographic values, these are for the user experience. Like how do you look up something? How do you compare two things side by side? We also have a thing called LifeHash that basically allows you to have a visual identifier. We're trying to solve the problem of everybody carefully reading the last four digits of the hash and trying to compare it against this. And it's kind of hard to do.

What we do is by having heterogeneous methods by which to compare these, you can go, whoa, wait a second, that is not the same LifeHash or provenance mark that this other thing says it should be. Maybe I should investigate further.

**Mark:** So there's been attacks that I'm sure you're familiar with on the user interface side of these systems of maliciously creating a key that has the same first four letters and the same last four letters.

**Wolf:** Yes, yes. And that's very difficult to do with our case because you can use any of these things together. You can use for example a LifeHash and a byte words hash, a byte emoji hash as well. So the idea that you could create one that maybe gets close visually to one or gets close in terms of the actual words of one, but getting them all, impossible.

**Christopher:** We're big believers in heterogeneity in these types of things. We still show things like the first and last four bytes. And we have this whole user interface convention called an object identifier block. But we should probably move on because we're now at 11:30 and I know some people had to go.

### Completing the Demo

**Wolf:** I'll try to speed it up a little bit. So now we've got our provenance mark that we're going to embed in our edition. So now this is actually what we're going to call the clubs tool. We're going to create the first edition. We're going to pass our publisher's XID in, which includes the private key. We're going to pass the wrapped content envelope. We're going to pass in the provenance mark we just generated. We're going to permit this to Alice through her XID. We're going to encrypt the symmetric key to her public key. In fact, this is Alice's whole XID document. This is Bob's just public key. And then we're going to actually also generate a 2-of-3 SSKR split. This is Sharded Secret Key Reconstruction, the Shamir that Chris was talking about.

And then we're going to basically capture that in a shell variable. Again, we're not using stored files or anything with the provenance marks here. We're capturing everything in shell variables here.

*(continues demonstrating edition creation, permits, SSKR shares)*

So this is a wrapped envelope with it which is signed by the publisher. Inside the envelope, the envelope is an encrypted subject, which is the content that we showed you earlier. It has a type identifier. This is an edition. This has an assertion on it, which is the has recipient, which this is the actual encrypted symmetric key encrypted to Alice's public key. And here's another one. In this case, because it was given a XID document, it actually is identifying the XID, but that's optional.

And then the club is publishing this, and this is what you would dereference if you wanted to, for example, get the club's public key, because there's no club public key here. And so part of what we are presuming is a way of dereferencing the pure XID to get the XID document.

And we have the provenance mark, which is basically the provenance mark we generated previously. In its info field also has the hash of the encrypted content.

And then we're seeing basically the SSKR shares are also simple envelopes that are the encrypted symmetric key along with an SSKR share which is just an object that includes what's needed to combine through the Shamir algorithm, our multi-level Shamir algorithm. So again, 2-of-3 of these will retrieve the symmetric key you need to decrypt the content.

### Verification and Decryption

**Wolf:** So now we're going to actually verify the composed edition. So if you receive an edition and you know the publisher's public keys, which are included in the XID document here, then you can verify that in fact, it is a valid edition by that publisher. And basically no output basically means success in this case.

So now we're going to basically extract the two permits because we want Alice or Bob to be able to decrypt the content. Alice basically wants to decrypt it because she's part of the audience. But so she has to use her private key, but she doesn't know which of these sealed editions she can use. So she's going to try for each of these two permits, it's going to try to decrypt the actual content.

*(demonstrates successful decryption)*

As you can see, it found the envelope and basically this is the actual decrypted content that was decrypted by Alice using her permit.

And then the SSKR shares, we can show that a quorum of shares can recover the same plain text without any permit or rather using the SSKR permit. So in this case, we are using clubs content decrypt. We're passing 2-of-3 SSKR shares. And then we're basically printing that out both in UR form and envelope form. And there it is.

### Creating a Second Edition

**Wolf:** And then we're going to basically create a second edition that is a new thing. And this could have a different set of permits. Somebody asked about revocation earlier. This is what you do with revocation. You drop somebody from your permits or not distribute your SSKR shares to that person in the future or whatever. So that's what... Reading is basically having a permit that reads the edition. Writing is basically having the authority to actually create the provenance mark and signatures.

So in this case, we're going to create a new subject. We're basically doing the same thing we did, but creating a new content envelope: "Second Edition." We're basically going through the same dance we did before. We're going to retrieve this digest. We're going to create the second provenance mark. And this, again, necessarily follows the first one cryptographically.

*(demonstrates second edition creation and verification)*

And then the new step we're going to do after this is that we're going to confirm that both editions belong to the same club and form a continuous chain. Editions may be provided in any order. No output indicates success. In this case, we're passing both editions and passing them in fact out of order from the order that they were published, because the provenance marks are used to order them.

And so no output here, being success, is that they are in fact from the same club and that they form a continuous chain. It would warn you if, for example, the chain is broken or any of the things didn't validate or it didn't start at the very beginning, which you may or may not care about. But it definitely in this case indicates that these are both the entire chain from the same club with the provenance marks unbroken.

That's the demo.

**Mark:** Thank you.

---

## Post-Demo Discussion and Future Work

**Christopher:** I'm going to go ahead and go to the slides. And so that was the demo from clubs CLI Rust, which uses clubs Rust and our other repositories at Blockchain Commons on GitHub. The other thing particularly to note is that his testing methodology, basically, you can run a single Python script and it will basically run that entire sequence of things. And thus, you can create your own demos of this workflow in a variety of ways by modifying that object. Is that correct, Wolf?

**Wolf:** Yeah, absolutely. What you saw there as a series of command lines was actually generated by a Python script that meta generates that, and then you run that Python script that actually generates that markdown output that I just showed you. So you can tweak that and that's included with the repo, with the clubs CLI repo as well.

**Christopher:** So future work, obviously using FROST for editions. So basically in this case, we're using the symmetric key to encrypt the edition content and then make a commitment to the next provenance mark in the chain without having to have that genesis secret seed. It's collaboratively created and then sign the edition Gordian Envelope using FROST. So we do have a proof of concept code working for this.

**Wolf:** There's three FROST rounds needed: one to generate the symmetric encryption key for the content, one to generate the provenance mark without a seed, and one to generate the signature for the entire envelope.

**Christopher:** Once we have that more fully integrated into our libraries, this will allow FROST groups to publish editions in addition to the technique that we just showed you where a single user publishes an edition.

And then in order to do that, we need to create a server right now we call Hubert, which is a dumb message passing hub for GSTP messages. It basically receives the Gordian Seal Transport messages and supports multi-cast, single-cast, store-and-forward, etc., without actually having to have any knowledge of what's going on. And this will simplify the communication needs for FROST to produce new editions.

So we basically already have working libraries for FROST and for GSTP. We just need to integrate them into code for Gordian Clubs. So it's not a technology achievement as much as it is just integrating some of the pieces we already have.

### Future Work: Cryptographic OCaps

**Christopher:** Now, what you saw was static permissions via permits and signatures. A number of you have come here because you're interested in the object capabilities opportunities where we can delegate authority without sharing keys, do attenuation, composition, etc. So these are research phases. And really, before we can go to any kind of deployment, these novel cryptographic constructions are going to require formal proofs. So we're really hoping that this will attract interest in people, not just engineers, but cryptographers to take a deeper dive into what we're doing.

So specific for OCAP delegation without key sharing, Alice wants to delegate her read or write authority to Bob, but she wants to do it without sharing keys and without creating a new edition. What I'm going to give you in a second is what I call the naive version of it.

What do I mean by naive? We've known that Schnorr aggregation, back when it was first created, that was one of the things that was wonderful about Schnorr signatures was they could be aggregated. But if you do it in the naive way, it can leak keys. Thus, it took another decade for MuSig2 and MuSig-DN and FROST and other protocols to do these without some of those attacks. Thus, we're going to need cryptographic proofs before production use.

But the naive version: basically, Alice creates an incomplete signature. She generates a random secret and commits to it, uses the symmetric key for that to create an adaptor signature that only Bob can complete. Bob has to have his private key, he's published his public key which Alice has used. He can use his private key to complete the signature. Completion of the secret reveals the secret that he can then use to have the read authority to decrypt this edition. So no key sharing, no symmetric key sharing is involved, just the public keys. Alice retains her initial access.

Basically, the incomplete signature hides the secret that unlocks the decryption. So it's a cryptographic lockbox that only Bob's key can open.

A little bit more sophisticated is Alice, with write access, wants to delegate to Bob without sharing keys, symmetric keys, or updating the current edition. Obviously, anybody could basically share the symmetric key of the object with the future party, or share the genesis keys and the commitments and things of that nature. I mean, there's nothing that stops people from doing that. But this is about how you use public key operations to do this.

So we're basically doing a standard Schnorr, and then we're tweaking it for the adaptor signature that is locked to Bob's public key, which means only Bob can use this with his private key. And it proves both that it was used with Alice's authorization and with Bob's identity. This results in delegated write authority. Bob can create a new edition, but it shows Alice's valid signature. No key sharing is required. And Alice still retains her original authority. So basically this gives Bob a blank check that only Bob can fill out.

So those are what I call the naive constructions. The harder one here is the FROST provenance mark.

### FROST Provenance Mark VRF

**Christopher:** So FROST came out in 2019 and at this point has hundreds of papers examining it and building on it. So we are very confident with the underlying FROST method for doing group signatures.

That being said, we want to use those same signatures to allow for a new edition of a provenance mark, which is a unique, unpredictable symmetric key that everybody can verify came from this group. We have a particular construction using a VRF where the group shares a secret that no individual knows. Each document creates a unique mathematical challenge. The group collaboratively solves it, producing an apparently random number that is unique for this group, but is also publicly verifiable, unpredictable, and deterministic.

And this basically is your cryptographic proof of group consensus for the provenance mark, an unbreakable chain of documents. So thus it becomes a combination lock where the group collectively knows the combination, but no individual does in order to move editions forward.

This is a lot more sophisticated, and I share this slide with you. I'm not going to walk you through it *(shows cryptographic equations)*, but it is using VRFs and Chaum-Pedersen proofs and ratchet states to do all of this.

So before we can do FROST provenance marks, we're basically using FROST beyond what it's been already proven for. So we're going to need help to do that. That being said, at least this does work in its naive case. So we can basically implement it and show that it is functional vis-à-vis the naive use case.

### Call for Involvement

**Christopher:** And then, of course, I would love for you to get involved with Gordian Clubs. In particular, what are the use cases that you're interested in for this technology? But in particular, where there are practical problems that true autonomous data objects could solve today, maybe in combination with other things.

For instance, it could be that some OCAP technology that Agorics and other people are working on, maybe some subpart of it could be solved. Some problem with it could be solved with an autonomous data object.

Obviously, as I just mentioned, we have these novel constructions that need cryptographic proofs. At this point, we do not have peer review of the club's protocol and code. Obviously, we're looking for companies that are interested in partnering on this tech.

And we always need financial support for a variety of things, including what's been going on with government funding of projects has reduced everybody in the decentralized identities community's ability to fund these types of things. So we're really needing support. And that support is not necessarily just individual support of companies, but just the social proof that you are offering us $10 a month on GitHub is something that we can use when we go to Human Rights Foundation and to other funders to basically say, hey, this is a technology that people are willing to commit to.

So if you're interested in any of this, please email me at team@blockchaincommons.com.

And I think in summary, some dreams just need the right tools to become real. I think that Ryan and others will agree that sometimes it just takes time. Today, we have these tools. Tomorrow, developers like you will use them to build systems to preserve human dignity. Are you ready to build uncapturable infrastructure?

And I'm Christopher Allen, and I'm at @ChristopherA on almost all of the major social media sites.

---

## Q&A and Closing

**Christopher:** I'm going to stop sharing this now and give an opportunity for Q&A. Ryan? Gordon?

**Ryan:** No questions for me right now.

**Gordon:** I'm gonna have to watch the demo a little with more attention to step through and make sure I understand all the steps. But the thoughts that keep coming back to me going through this is how to communicate it to other people via analogies to other things, especially the way that the clubs advance being related to Git versioning, especially the way that the clubs state is a lot like a proof-of-state blockchain.

And possibly also clearly it's at the state where the command line tools are the key, but starting to imagine how this would look even if the interface isn't built. Like what would the objects look like to somebody who was doing this in a different look? It could be helpful in reifying it in many people's minds. So those are just my thoughts from what I saw today.

**Wolf:** One way of approaching that is one of the earliest things we've worked on is how to deal with cryptographic seeds. And so we have, for example, our iOS app, Gordian Seed Tool, which you can get on the App Store, is basically an example of how to use that part of our stack, seeds, to generate keys, to represent, to recognize seeds and keys derived from them and things like that using our LifeHash and other techniques and so on. So that's basically kind of a very UX-driven form of that.

And yeah, so we're basically working not just at the command line level, but we're also at the UX level, resources and time permitting.

**Gordon:** And I'm definitely going to check that out and look at some of the other videos to make sure I'm caught up.

**Wolf:** So thanks, yeah, we have a whole playlist on Gordian Envelope for instance, which I think is definitely essential watching to grok what we're doing here. But yeah, and also please reach out to me personally. I'm happy to set up a calendar link and set some time to talk to anybody who wants to ask more detailed questions about our stack or dive into particular details that maybe aren't covered by our videos. So you know, feel free to reach out to me directly, wolf@wolfmcnally.com. Thanks.

**Ryan:** One thing that comes to mind is that since you're serverless, this might be a great application for CRDTs, conflict-free replicated data types.

**Wolf:** In fact, I've thought for a long time about how I would do CRDTs using Gordian Envelopes. I think that would be a great carrier for that kind of thing. And it's not something we've done much experimentation with at this point, but I would love, I would definitely welcome that opportunity.

**Ryan:** We've done a little bit of experimentation with it over here and I'm super excited about it. Maybe we could collaborate on that.

**Wolf:** That sounds great. Yeah.

**Christopher:** Yeah. I mean, I think that's why I have the call out for use cases. We have some sponsorship in the past from the Human Rights Foundation, which protects journalists and activists of various kinds.

Provenance marks are, in particular, are apparently random numbers. There are other aspects of understanding the threats that we'd like to understand to address specific problems that, say, a journalist or an activist in an adversarial environment needs to be able to do. That would be an interesting use case.

A lot of the inspiration for this is, I want to enable the AMIRA engagement model. So back at the second Rebooting at the United Nations, there was a YORAM use case for refugees that inspired in the third rebooting something called the AMIRA engagement model, which was an example of a developer trying to develop activism software without being vulnerable herself.

So a lot of what's in my mind is how do we protect software developers so that when they go through the Istanbul airport like that computer scientist did a few weeks ago, they don't get pulled aside and say, hey, you're potentially a bad actor. So we're going to, we don't care about your free speech rights, we're going to haul you off. That did happen a few weeks ago. And it is in the Istanbul airport. Fortunately, he was released. But that's the kind of stuff that's happening these days. So that's another use case. But I need to know more about the use cases.

**Christopher:** Okay, so last call. Anybody else? I don't see any hands raised. Thank you everybody for participating in the session today. We'll have a recording up later in the day and transcripts and other information. And then I'm working on a detailed article to talk about the history and where this all comes from. That'll probably be up later in the week or next week.

**Wolf:** Thanks, everyone.

---

*End of Transcript*

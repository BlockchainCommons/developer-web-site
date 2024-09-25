---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Round Table II (2024): Transcript"
hide_description: true
classes:
  - wide
permalink: /frost/meeting2/transcript/
sidebar:
  nav: frost
---

_This is a raw, unedited transcript of the FROST Implementer's Round Table on September 18, 2024._

Welcome everybody to the Frost implementers roundtable.

This is our second roundtable on this topic and we're hoping to do more in the future.

What is blockchain commons?

We are a community interested in the self sovereign control of digital assets.

This isn't just money but also digital identity, the types of things that we feel are needed to create an interoperable infrastructure.

So our goal is to bring together stakeholders to develop this infrastructure and to that end we design decentralized solutions where everyone wins and we are organized as a neutral not-for-profit that enables people to control their digital destiny.

So this particular event is also sponsored by the Human Rights Foundation.

So thank you very much for that community for supporting our work.

Personal introductions.

I'd like to have everybody briefly introduce yourself and your particular role with Frost and we'll talk about status and the various presentations in a minute.

Jesse, why don't you begin?

Sure.

My name is Jesse.

I work for Block.

I work on the self custody wallet called BitKey.

And I'm also working on a Frost implementation in the SecP-CKP repo and I also spent a lot of time looking at how we might take Frost and different ways we could potentially integrate it into a self custody product.

So that's great.

Armin, do you want to go next?

Sure.

Thanks.

Armin here.

I've been working in the Bitcoin self custody space for the last couple years.

I've been working with some of the developers on the Zcash Frost crate helping test and added some of my own customizations to it and I've been developing with Frost for the last year.

Thanks.

Great.

Conduition.

We're not hearing you.

Yes, no, Mike.

He's over in chat.

He says, "Hi, I'm Conduition.

I'm a freelance developer working on a BIP 340/341 compliant implementation of Frost based on the Zcash Foundation's Frost crate."

Great.

Conrado?

Yeah.

Hi, everyone.

I'm Conrado.

I work at Zcash Foundation.

Our main work with Frost is a set of Rust crates which offer a presentation of the Frost protocol and we're also working on some tools to help people actually use Frost or I mean our goal is to help wallets, Zcash wallets or any wallets that need Frost to use it.

So tools to help communication, stuff like that that I'll talk more about later.

Great.

Francisco?

Hey.

Hi.

I guess that Zoom just docked me.

Okay.

You know my first name.

I'm Pakwu.

I'm a developer.

I'm currently CCASH Community Grants Grantee and for a Developer Relations Engineer Grant and I'm developing various things on CCASH and specifically on this topic.

I'm working on a Frost UniFFI SDK that is now a proof of concept, but it's basically trying to bring the Frost crates to other other languages without the need of porting them like using Mozilla UniFi as a way to have a more gentle form and function interface with Golang, Swift, and Kotlin initially and it could be expanded to other languages that that tool supports.

Okay.

Jonas, you're next.

Yes.

Hello everyone.

I work at the Blockstream Research Group where I'm mainly focused on the various cryptographic aspects of the Bitcoin protocol, Schnorr signatures music and also Frost and we recently published a draft of a specification for a Frost distributed key generation protocol protocol that I'm going to present hopefully today.

Thank you.

Callpreet.

I hope everybody hears me.

I haven't used Zoom in a long time.

So I'm probably the most naive and ambitious here because I'm not working on an implementation of Frost.

However, I want to kind of use it in an online federation.

Right.

So I probably have some very silly and naive questions and hopefully which will be useful to the implementers here.

I'm kind of most curious at the moment about being able to make the Frost key gen work given the weaknesses that have been discussed on GitHub recently.

So I'll talk about it later in the roundtable.

Thank you.

Nat.

Hi.

Yeah, Natalie, I'm working.

I work at the Zcash Foundation as well.

Kenardo, we're both on the Frost team.

So working on the Zcash Foundation's Frost library which is built in Rust.

We have various Rust crates.

And yeah, and supporting tools.

Thank you.

Tim.

Hey.

Like Jonas, I work in the research team of Blockstream.

I'm an applied photographer there and I have been doing research on Frost in court of two papers related to Frost and I'm also part of this project that Jonas will talk about later at EKG.

Great.

Sivram.

Hi, I'm Spiral Grantee working on the signing BIP and also reviewing a few other ZKP Frost projects.

Okay, I'm trying to think who have I missed?

Luke?

Jack.

Yeah, Jack.

Shall I go first or shall Jack?

Why don't you go first?

Sure.

Hi, I'm Luke.

I'm representing Sarai Decentralized Exchange and with that the kind of decentralized network approach in view on Frost.

Thank you.

Jack.

Hi, I'm Jack Kavigan.

I also work at the Zcash Foundation but on the non-technical side and just joining in to see how other folks are using Frost and thinking about using Frost.

Okay.

Anybody else other than the Blockchain Commons team will go last?

Sounds like we have everybody.

Shannon, why don't you introduce yourself?

Hi, my name is Shannon Napo-Kline.

I'm a technical writer and I work for Blockchain Commons.

Wolf?

Hi, everybody.

I'm Wolf McNally.

I'm the Lead Researcher for Blockchain Commons.

I'm also the designer and implementer for the envelope which is close to the top of our stack of technology that we're working on integrating a lot of the technology you guys are working on into.

And my job is basically to make things much easier to use for higher-level implementers, engineers and things like that.

So ergonomics is huge for me in code and UI and my work is made possible by the generous support of our sponsors and patrons.

So thank you for your support.

Thank you.

And I'm Christopher Allen.

I'm the Executive Director and Principal Architect of Blockchain Commons.

I used to work at Blockchain when having two Blockchain people, former colleagues here is really nice.

And we're really passionate about the whole aspect of how do we allow for greater security, greater resilience and we have long identified Frost as being key to this.

So that is why we've been started hosting these events.

So we're going to walk through a number of the major repos.

I don't know if this is the exact correct order.

They'll be in the upcoming slides.

Our goal is to understand the progress and what is your roadmap and in particular if there are security reviews that have been finished or are planned and then other kinds of standardization efforts and then of course unique features of your particular project.

So Jesse, do you want to go first?

Sure.

Let's see.

I'll pull up my slides.

So yeah, so I'm working on two Frost implementations.

I'm not sure you've stopped my...

Yeah, you need to take over the screen.

Sorry, here we go.

Great.

I see your screen.

Okay.

So given a few updates about my Frost implementation work.

And just some research I've been doing.

So one of the things I've been looking at recently is can we take proactive and dynamic Secret sharing protocols and these are protocols where you can refresh shares, repair shares, add and remove participants and increase and lower the threshold without changing the underlying secret.

And this is pretty interesting for a number of perspectives.

One, like in a self-custody product, we can do recovery flows without having to move funds with these protocols and it can be quite expensive to pull in all the UTXOs for a wallet and to a single sweep transaction.

Whenever we need to, like if in a two or three multisig if we lose a component and a new one is added without these kinds of protocols, we have to do these sweep transactions that are expensive and they're bad for privacy because they link all the UTXOs together.

So we can use these protocols potentially with Frost.

However, what I have found is the schemes that I've looked at in the existing academic literature when the protocols that defend against active adversaries rely on a bivariate symmetric polynomial as a different type of VSS than what we see in Frost where the bivariate symmetric polynomial can be used to verify the shares being distributed and the data being distributed by the participants.

So we can assign blame in order to defend against an active adversary.

However, in some of the, in at least one of the papers, it alluded to that we could use a different VSS instantiation and it should just plug into the protocol.

So I've been experimenting with what that might look like because in Frost we have a univariate polynomial.

So unless there's a way to like convert the Frost shares in the univariate polynomial shares into bivariate shares and then back again, I think we need a different type of VSS.

So I've been looking at kind of the Feldman style VSS where we commit to the coefficients or we commit to whatever kind of data we need to as a in the point domain as you know by multiplying our secret value by G and having this kind of homomorphic encryption of it.

And in I have this Python repo that I use is sort of like experimental testing grounds.

And so I've implemented all these protocols refresh repair add and remove participants and changing the threshold with this kind of experimental VSS approach where all the data can be verified and blame can be assigned and I'm planning on publishing a detailed description of this protocol soon and hoping to get some review on that to see if it if it seems safe and workable.

So that's that stuff.

Something else that I just came across and implemented is I was looking at the silent payments BIP 352 and there was a mention of oh you can do Diffie-Hellman with Frost.

I was like that's pretty cool and interesting.

We can have this kind of like threshold decryption or encryption stuff.

So I have implemented that as well in this repo and I'll just go through a brief overview of the math.

We have the Frost group private key X.

This is the private key that's never actually constructed.

We have a Frost key share Si Lagrange coefficient.

And let's say we're doing a Diffie-Hellman with some counterparty and the Republic key is P.

Well, you know, according to the Diffie-Hellman protocol the shared secret would be P to the X.

You can think of a partial shared secret as P to the share times Lagrange coefficient.

So each participant computes this they take the counterparty public key raise it to the power of their share times Lagrange.

And then we can see when we get a threshold of these points we multiply them together.

It interpolates the private key and the result is the Diffie-Hellman shared secret.

So I thought that was kind of neat and again, I've implemented that in Python.

Another thing that I've come across recently.

So when we're implementing Frost for Bitcoin, we are using the BIP 340 Schnorr spec and as part of that spec we use X only public keys.

And so my initial approach to handling X only public keys was to have the DKG do the share negation and output and X only public key.

So the output of the DKG would be directly able to be used on chain and you don't need share negation logic in the signing part unless you're dealing with tweaked keys, but otherwise it's just like ready to go.

But I realized this is actually like a pretty bad idea.

And one of the reasons is that unlike music to the Frost group public key is not randomized and that means the malicious party could add an undetectable script path during the DKG.

So the DKG would output a key that also embed some script path that lets the participant just spend all the funds or sign for the key without anybody else.

So in the BIP 340, in BIP 341 it recommends to add before you use like a key on chain and taproot to always if you don't want a script path to add an unspendable script path to disallow this kind of like hidden script path that the participants don't want.

You don't need to do this in music too because it has this coefficient re-randomization that takes care of that.

But in Frost we definitely need to do that.

And so I'm thinking because of that it's better not to output an X only public key from the DKG because we really don't want the API callers to think it's safe to just use that directly on chain without adding this unspendable script path.

And for that reason there's really no, there's no, if you're not going to output an X only public key and if you can't use that output directly on chain, there's no reason to do any negation logic during the DKG because you're going to have to do the negation logic again when you add the tweaked key.

So it doesn't really save you any work.

It gives like potentially a confusing impression that the key is safe to use on chain.

And so I think it's much better that the the DKG not necessarily output a public key at all, but if it is going to output a public key to not have it be an X only public key, but like it might be better for it to just output public verification shares and leave the derivation of the public key that will be used for an address or for signing as part of something that happens outside of the DKG when you like tweak the key or or go to sign with the key.

So I've raised that in the Frost DKG BIP and also in my implementations in SecPZKP, I've updated them to not no longer output the XL output any public key from the DKG and I think it works pretty nicely.

So last thing steps for next steps for the ZKP implementation.

So the plan is I have this trusted dealer PR and the plan is to get that reviewed and merged first because we can kind of put the thorny and complex questions or implementation details of the DKG to the side and just focus on the signing and nonce code and have this kind of simple trusted dealer stuff just to set it up.

And the focus is to implement the Frost signing BIP that Siviram has drafted and then what's cool is the DKG code will be additive and it will only affect the the key gen part of the code.

So once the trusted dealer PR is in the signing and the nonce code shouldn't have to change at all and we can just drop some additional APIs in the key gen part of the code to support a full DKG and and we can still have the trusted dealer API in there for those who would prefer to use that.

So and then then there's another PR that implements a DKG that will then follow but that's still very much a work in progress as the DKG BIP is is getting finalized as well.

And this is like C implementation, correct?

What's that?

This is all C.

This is all in C.

This is all built on the Bitcoin sec P cryptographic primitives implementations and stuff and the trusted dealer PR is ready for review.

If anybody wants to come in and take a look at that that would be much appreciated.

Any questions?

Raise your hand in the in using the Zoom tool on ratto.

About that negation issue.

It's good to know.

I'm interested in it.

I need to look into it a little bit more to understand it.

But my question is if the signing tool or whatever code that does the sign check it if the if the X coordinate has the right sign would that be enough to block this?

I understand it's a bit riskier but not sure if the question makes sense.

Well, we want we definitely want to do some negation logic during signing and I've implemented all that and we use the same technique in music where when this when the when everyone goes to sign they check the there's there's logic that specified in the music BIP and the Frostbip for looking at the tweaked keys and how they were added together and then your final result and then you can you can determine each signer can determine for themselves.

Do I need to negate the share before signing it?

So definitely we want to do all that when signing the only question was should any of that happen during key generation because you could during key generation you could check the public key that you generate and if it has an odd Y value everyone could you could negate all the shares when you average you could you can negate the aggregate shares and then your final DKG output.

If you just use that directly on chain that would that all the share the it already has the even Y value and no further negation is needed.

So that's the only thing I was speaking to is like I don't think that's a good idea.

But definitely when signing we absolutely need to do negation logic and we have algorithms to to be able to do that.

Right.

I guess my question is if we are trying to find if there's another approach to this, right?

So if we do the negation logic on DKG, right?

We make sure is it outputs with even Y.

This so from what from what I understand this is not enough.

You still need to do the negation somehow during the signing.

Oh, I see what you're saying.

Yeah.

So the thing is the the DKG output shouldn't be used on chain because there might be this unspendable strip path.

And so to use it safely, we're going to like the typical usage would actually be you're going to have a bit 32 derived key from the DKG output that's going to tweak that output once and then you're going to have a taproot script tweak that's going to be the typical usage.

There's going to be like two key tweaks.

And so in typical usage, you're always even if the DKG output is fine.

You're going to have to recheck everything with these new keys.

Yeah.

And even if you're not using BIP32 and even if you're not using a taproot script at the very least you should add an unspendable script path.

And so that means you're going to still have to do the negation logic to check.

Okay, did the unspendable script path now require the chairs to be negated or not?

So you're not really saving any work by doing it in the DKG.

Yeah, got it.

Thanks.

I ask it because in Zcash we have something similar.

We also have to ensure the excoordinate is even but you don't have this process of tweaking and so on.

The key that generate is the key that you use.

So in our case, I think it's fine to do the logic in DKG which is what we're doing.

But I understand that in the Bitcoin scenario you need to do that.

Thank you.

Tim, you're next.

Hey, yeah, it's all the question but two remarks.

So first of all, I'm sorry for not looking at the PR, the trusted DLL PR.

I know it was me who suggested that if you have first trusted DLL implementation then you could make progress faster but that's only true if people actually look at the PR.

So you should do this.

And the second thing is, as I said, it's really just a minor remark that the easy H thing of course is cool and it's threshold decryption or threshold key exchange or however you want to call this.

I just don't think we should relate it to frost or we should call it frost because frost is specific for signing and then dimensional signatures and threshold key exchange isn't earlier or just a different idea at least following the standard crypto terminology.

Yeah, yeah, totally.

I mean the I guess it's just interesting because it's compatible with a frost public key and frost quorum.

I mean, I guess the maybe the better way to put it is it works with any set of Shamir shares whether they're generated by frost or otherwise, but but yeah, that's if that captures the nuance you're mentioning.

I think that's a good way to phrase it.

Yeah, I'll follow up on that one.

Well, then we'll go to Jonas because it minds directly related.

Yeah, I think that there is a whole once we have really some solid DKG's there is a lot of opportunities to do things that are compatible with frost on top of them, but don't actually use frost and of course, all of these are going to have to be cryptographically reviewed because like in the same way that you know, naive, you know, signature aggregation required, you know, years of work to come up with things like frost and music to do it actually securely.

I think we need a similar review of a Diffie-Hellman that being said, I really need it because this is exactly one of the problems when we're talking outside of you know, the coin type digital assets.

We have a lot of different situations where one public key needs to you know, have key agreement with another public key and if a frost Federation and other parties can't do that, that's good, you know, that's limits them.

It also might identify that that's a frost foundation as opposed to a you know, relatively trivial operation on a public key.

So how we name them I think is maybe one of Tim's points, you know, these sort of frost adjacent protocols, you know, or even for that matter music adjacent, similar problem and we as a community should maybe start puzzling out what is the right way to describe those things.

Jonas, you're next.

Yes, I'm Jesse.

I have a question on your first project that you mentioned what are currently the main considerations, the main problems that you're encountering.

So for example, I'm imagining if you would want to use bivariate polynomials, you probably need to have sort of change frost in a very significant way such that you would also need a separate security proof even just for signing and stuff like that.

So what are the main questions right now and and blockers?

Yeah, so I kind of abandoned the approach of trying to work with the bivariate like polynomial because I couldn't think of a way to make it work with frost.

So the approach I've taken is to instead commit to the values.

So like in share refresh, it's the same as we do for DKG where everyone just commits to the polynomial coefficients and you can verify that the share the refresh shares are valid.

But like let's say for share repair where everybody like takes a share and multiplies it by Lagrange coefficient and adds it to some other shares like everyone can commit to those values as well.

Like you can you can the person who has having their share repaired they can check by virtue of the commitments that okay, these are the additions of these people shares with their Lagrange coefficients and if somebody gives me bad data, I can verify and check like oh that this is the misbehaving participant.

And so it's been pretty straightforward to just go through each of these protocols and just be like let's just multiply stuff by G and and verify it and it's kind of just worked.

I think the only thing that is a little bit of a different emphasis with this mode is that with frost DKG we output the public verification shares and we kind of throw away the VSS as something that's not needed after the DKG.

But in this reality we kind of want to with this with this protocol we kind of want to keep the VSS.

We want a committed version of the polynomial at all times as it's undergoing these various changes and we want to be able to refer back to the VSS or update the VSS whenever the polynomial changes.

So we can use that to verify shares and verify data and then from the VSS you can derive public verification shares.

There has been in one case.

I believe it's the threshold decrease where I had to go in the other direction because there wasn't a straightforward way to update the VSS from the old one to the new one after you decrease the threshold.

So I actually have to generate the public verification shares for the new polynomial and then go from using a Vandermonde matrix go from the public verification shares to the committed polynomial VSS.

And maybe there's a more efficient way of doing that than having to do like matrix inversion and all this stuff.

So that's something I'm curious about is kind of just that new way of thinking about this stuff of making VSS primary whether it actually whether we think it actually works to verify all this data.

And is there a more efficient way to go from public verification shares to VSS?

The other thing is there's a lot of these protocols make a deletion assumption where everybody has to delete old shares.

And so there's security nuances we need to think through there of okay, what if a participant doesn't delete their old share?

What are the consequences?

How should we view the new polynomial?

So there's definitely some tricky important nuances to think through with that.

And there was one other thing.

Oh, yeah, the there's a there's this thing where when you're doing oh, yeah, there's something called an adjacent assumption in the proactive secret sharing protocols where it assumes that like the same set of nodes are compromised in time period t minus 1 and t plus 1 and there's a couple of papers that identify that assumption as problematic and that there's a issue with some of the reef with these protocols where when you're commit to a zero constant term, which is how the refresh stuff works.

You give everybody a free point which is for the new for the refresh polynomial everybody knows that 00 is a point that the polynomial evaluates and because everybody gets a free point then there's some things that they can do with t minus 1 shares where it should take t but they can do t minus 1 because they get this free point and there's some papers that there's a paper that has a proposal for how to address that were basically during refresh you create a one higher degree polynomial to kind of negate the free point and then after all everything's added together you do a protocol to then lower the threshold back down.

But it's a little bit confusing to me like whether that's actually needed and also the how to implement it.

So that's something else that's come up as a as a potential challenge but there hasn't been any showstoppers.

It's all been like pretty straightforward.

So if you can share with us some of those papers and we'll add it to the transcript and I'm hoping that you know, we can have a you know, at our next meeting of you know, a further review and maybe some cryptographers who can say aha you weren't thinking about X or Y or whatever and or there's an easier way to do things.

But yeah, I think these are important for reliability and they're all good, but we should move on to Conrado you're next want you want to share your screen.

Yeah, just a second.

So yeah, thank you.

I'm not showing up something here.

This is Yeah, I recently updated my Ubuntu and I think it broke sharing with them.

Sorry about that.

But we should we you want to take a couple of minutes to work on that and go to the next.

Yeah, I'm not sure if I'll be able to fix it in a short time, but I don't have much of content slides just a guide for myself.

I can just sure speak it.

Okay.

Okay.

I will share the link for the presentation later to for reference.

I think maybe Natalie can help.

Could you try sharing these lines please?

Yeah, one minute.

One second.

Thank you.

I'll just start talking on the so just give some context.

So we have a bunch of crates of rust that implement Frost.

This crate having audited we launched the one zero zero release while back and since then we very close to releasing to zero zero.

We've got a release date and we're doing the final release soon.

And the only change is just some API adjustments and always the support.

There you go.

I think you can you try to yeah.

Just like show maybe.

You're a little more to the right.

You should have slideshow button.

Yeah, just click.

Oh, yeah.

All right.

Somebody's nice to can see.

Thank you.

Thank you.

Yeah, I can go to the next.

Thank you.

Yeah, so that's a repo.

So because release to zero zero.

We're working with fresh ice functionality.

We already support it for the trusted dealer and we're working on the DKG support for it.

So I also interested in what Jesse has just talked about and it look into it.

And the only additional thing about this crate is that we have a PR open but to write support for the type root.

So we support the cypher suites that are specifying the RFC but does for the sick it set be 256 K1 curve, but doesn't work out of the box with taproot due to the whole sign.

Sign adjustments things.

So we have a pair open by contributors.

Composition just suggest a quick way to PR to do some small fixes.

So don't block it as far as to review it.

So which is we hoping to do that soon and this information that has been just discussed it will be useful to help review that.

But yeah, I think for the main crates, that's it.

Can you go back to the.

Thank you.

So the other part of what we're working on.

It's a demo a demo code and the thing called for several.

So the goal of this frost server is to help people communicate because frost itself is straightforward, right?

The algorithm.

So the hard part is to use it.

You need to talk with other people and this is hard.

Unfortunately, this sounds like should be a solid problem.

It's not.

So we're working on a frost server to help people communicate in this server basically just forwards message back and forth.

It doesn't handle any of the false logic.

And we currently working on it to add encryption to the occasion which of course needed.

We currently we have like a user registration process to the server what we're looking into just having since we as each participant needs to have a key pair for communication for to encrypt and not educate communication.

We're thinking of just using that key to authenticate also.

So we're working on that component.

We're also working on the first client to which is basically a common line to that.

Helps you manage contacts like the public you need the public keys of the people you're talking with.

So you have like some contact management to address that and you also interface with the server.

And eventually this is also become a library.

So people if you want to do something equivalent to the frost client in your wallet, maybe you can impart this frost client library and you have a bunch of code that will help you do that.

And we have some other tools in this repo.

The trust dealer, the community participant coordinator.

These are like the older versions of the demo.

They work.

You can use them to to run frost with either by copy and paste things between participants.

But they already supports the frost server to you just doesn't have the encrypted education.

It is stuff that we are working on.

So it's not ready for for like traditional usage, but you can take a look to see or to see how it works and try to use frost is for in the samples.

So yeah, our biggest next step is getting the first ever done by adding equipment education and hopefully doing some audits and then you can go to the next slides.

Thank you.

Our final goal here is to have frost or libraries working with wallets to support the cache or maybe anything else that uses frost and possibly using your frost server.

We think it is our frost servers like a stepping stone into a better solution that uses full peer to peer.

I think it's not good to rely on a server, but I think it's a good first step to helping get frost to play it.

So this is our first main goal and you're looking for wallet developers.

We already have come some conversation with some wallet developers from the cache ecosystem.

But if you are a wallet developer, you know, wallet developers which are interested in integrating frost.

You can reach out and you can see if you can cooperate somehow.

I think that's it.

Okay.

Thank you.

Packou you're next because you're using these crates in your work.

So why don't you share your screen on that?

Hi.

Yep.

Sure.

Just one second.

Okay.

You.

You.

We can see it.

Just do slideshow.

Pop right round button.

Is it not showing?

We're still seeing the.

Is it what opening on another screen?

Is that why it's not showing?

Right.

Yeah.

Okay.

Pocky you're muted.

Okay.

Yeah, it's on the another screen, but the same thing as the.

Oh, there we go.

Let's see.

Do you see that now?

Um, says that you've started screen sharing, but we're not seeing it.

Just share the old one and just go through the pages.

The one you just had was showing it.

It just wasn't showing the full screen and that's okay.

Okay.

Let's see.

All right.

Let's stop sharing and let's try again.

There was a charm they say, right?

Okay.

Okay.

Let's do this.

Not fancy, but it'll work.

Right.

Okay.

Okay.

All right.

So I'm talking about said that.

Currently as second see cash community grants grantee.

Working as a debt row engineer and doing ecosystem outreach and other developments doing doing tooling and specifically on this topic.

I'm doing a frost uni FFI SDK for those who haven't.

Met the tool uni FFI is or unify as Mozilla wants to want us to call it is a an FFI wrapper for Rust that.

Allows you to have a unique you FFI approach where there are a single FFI I guess bridge or interface that allows you to generate bindings for different languages.

Currently it supports like Python Swift Ruby and Kotlin and on another third-party extensions allowed other languages such as start or go man and I'm using this Mozilla tool to create an SDK the biggest of the word because it's still a PLC but like a developer tool so that we can bring the T-cash Foundation Rust crates to other languages without the need of porting the whole crates and the protocols and all and using them in other platforms such as go Lang.

Kotlin Swift for mobile for example.

Go so currently.

I'm only focusing on randomized for C cache but there's a possibility since we I'm using Frost core and then just using the traits to indicate which curve I want to use it is possible to just use the same primitives and generate an interface that can accommodate more or curves.

This is a little bit more of heavy lifting because of how Mozilla unify works but it is possible.

So the architecture is very is not really complicated.

It's just using unify to call the Frost primitives to generate trusted or like trusted dealer or DKG and then the signing all everything that is documented in the papers or in the Frostbook.

And then just with the unified tool generate the bindings for Kotlin Swift and go Lang the bindings are not the best looking let's say but they're functional so the approach I'm taking is to you know generate the bindings as they are and then wrap the bindings into a more ergonomic API that also allows me to generate some interface that I can easily test on without relying on the real add file for example.

So that's that's kind of the the architecture that I'm envisioning for this tool and I'm actually using one of the use case these cases I'm proposing as a demo and as an idea.

I don't want to go any much longer but is based on presentation from the CF team or that proposed a for example a two of three self custody scheme using Frost and I thought oh that would be a nice idea to kick this off and maybe have some Frost companion application that I can create a mobile that can use this SDK to create a trusted setup or maybe a DKG if you're going to sign stuff with people that you not necessarily want to trust or have a spin or some spin authority like the justice setup may enable in some way.

So the SDK currently supports all the primitives for DKG and trusted setup and then you can like participate and in a signature in on either of the setups there of the schemes and that also this is that you can also back up your own share to a secure storage on your device if your device allows that or help others restore their shares.

This mobile app is in development at the moment and just as a way of like that including the primitives but I have like created some integration tests that actually emulate the Frost demo that's that prove that the SDK fully functional.

This is like really in a really early PLC stage.

So I haven't asked the C-CASH ecosystem security team to do audits yet, but it's on the roadmap but the scope of the project is to have this base like support randomized Frost primitives on Red Palace.

That's the initial scope and doing DKG, trusted dealer signing aggregation validation and also key theorization for sharing purposes.

Since I'm working on a self-custody scheme, I'm not worrying too much on how things are transmitted.

But the idea is that we can in the future start connecting to the server that Colorado mentioned here in the architecture.

You can see that there's some kind of like secure channel SDK that is needed to like remotely do, you know, frosting, having fun with frost so in a secure way.

So in progress, we have the demo app and we have a to-do of adopting frost 2.0, supporting multiple curves, and in a cuddling API and integrating with the frost server and all of this like in I'm a single person having like many hats.

So any contributions are very welcome.

So I'm leaving here a GitHub link, a QR or of the link here below in this presentation or you can ping me, email me at haku@zekdab.org and we can or if you want start doing something, just open an issue and GitHub, whatever is you feel more comfortable.

So thanks in the chat.

I'm just looking at our time and we should move a little bit on if people have questions about this particular.

Yeah, I'm yeah, I don't have any more slides.

That's what I wanted to present.

So I don't know if anyone has questions or we can move on.

Okay, Jonas and Tim, why don't you go next?

Yes, one second.

Let me finish my setup.

Christopher, can you remind me how much time we have for the presentation?

Well, let's you know, I think the we really would not want to go more than like 15 minutes right now to get everybody in and get some Q&A in so, you know, focus less on the the generics of frost.

Everybody knows that here and on what your stuff is, what is unique on you.

All right.

I will try to do that.

And can you let me know when I have maybe five minutes left or something?

Sure.

All right.

Cool.

It's not sharing.

It's shared briefly and then it flashed and went away.

So that is strange.

So let me retry.

It's time a little bit differently share entire screen.

Right now we're seeing it.

Okay.

All right.

It's not really full screen.

Good enough.

But now it is.

Okay.

All right.

So I guess I'll start first of all, thanks to the Blockchain Commons team for giving us the opportunity to present our work.

I'm going to present ChillDkg, Distributed Key Generation for Frost, which is joint work with Tim, who's also here and introduced himself earlier.

I'm going to start with motivation and where this all started and it actually started with Jesse opening a pull request for Frost to our repository.

The repository is called secp256k1zkp to be precise and it contains cryptographic primitives and protocols, I should say, for the Bitcoin, wider Bitcoin ecosystem.

So this was a good fit and we started reviewing this on and off for a couple of months, but we did not really make progress.

So I copied some of the discussion into the slides to illustrate what happened.

So the sentence, I'm not sure occurred a couple of times, but isn't it dangerous right now?

It's really hard to convince yourself that it works.

Couldn't it be a problem that there's no randomness risks inventing complicated machinery that turns out to be broken?

Sorry, I entirely forgot what we're trying to do.

Sorry, I'm doing a lot of hand waving and the ultimate classic.

This can be mitigated by another communication round.

So what were we trying to do?

We were trying to implement the Frost protocol in particular key generation is what gave us problems and on the right side, you see the specification of the Frost key Gen algorithm of key Gen protocol.

But it's not complete because it requires you to have first of all secure channels between the participants and secondly and more or harder difficult problem is the broadcast channel between the participants and we're going to talk more about that in this presentation.

So what is distributed key generation?

I'm told I should go quickly over this.

It's an interactive protocol.

It outputs the threshold public key also known as group public key and the secret share that a signer will use for signing and the properties that we want is that T out of n signers can actually use their shared designs as sort of a completeness property and also no less than T signers can produce a signature and in particular this excludes a trusted dealer model.

We want distributed key generation.

So there is an RFC for Frost, but I'm saying famously because brought up a couple people bring it up regularly is that it does not specify a DKG.

It sort of specifies a trusted dealer model.

So we had no DKG that we could use off the shelf.

So our thought was well, then we should probably write a specification of a key generation protocol ourselves.

And luckily sort of in parallel of that whole discussion.

Tim published a paper with his co-authors.

It's called practical Schnorr threshold signatures without the algebraic group model and that presents an alternative key generation algorithm to the original Frost.

It is not extremely different from the original key generation protocol, but there are two sort of main differences.

First of all, it replaces this broadcast channel abstraction with an equality check protocol that is called EQ.

We're going to talk about that and also a bunch of minor changes to improve the performance.

So we wanted to specify this protocol called simple padpop.

So here's a simplified presentation of simple padpop.

So every signer generates and shares and computes a VSS commitments to the shares.

So VSS is this verifiable secret sharing scheme that allows you to commit to the shares and a bunch of other things.

I'm going to mention in a moment.

Every signer sends the I generated share and the VSS commitment to the I signer.

Every signer computes the threshold public key from the received VSS commitments.

And now the question is how does the signer actually know that T signers can actually sign for the threshold public key?

The answer is that there is a fourth step where every signer verifies their received shares shares against the received VSS commitments and they use this verify operation that stems from the VSS scheme.

Now the property that VSS gives us is that if every signer receive the same VSS commitments, then the signers can indeed sign.

So in other words, what the signers need to ensure is that there is no malicious signer that send a different commitment to signer I than to some signer J.

And that is actually what the equality and the broadcast protocols are for in principle.

Okay, so the equality check protocol EQ is an interactive protocol that takes some input and then outputs true or false and in the simple pet pop DKG the input contains the VSS commitments.

And the property that we want from it is integrity.

So if some honest signer outputs true, that should be singular honest signer not signers.

If some honest signer outputs true, then the input of all honest signers are equal.

Okay, so this is the what we want to check what we want to ensure with the equality check protocol is that the inputs for honest signers are equal and what we get is that if an honest signer outputs true than these inputs are.

All right, so there is a simple equality check protocol.

If there is one party having three hardware wallets represented here by these calculators.

And they are all in the same room.

And you want to check the or you want to do the equality check protocol as part of the DKG what the hardware wallet could do is it could just hash the input and display the hash and the user would then check if would look at the screens and check if the screen the hashes on the screens match.

If they do well, the inputs are equal and the DKG can continue.

Of course in a network, which is potentially controlled by an adversary.

It's a bit harder and we're looking at one of the failure modes now.

So this is an example of a two or three setup where you have two honest signers and one malicious signer and the malicious signer participates in a protocol in a way where the EQ input for the honest participant for the honest signers is actually equal, but the equality check protocol which we haven't defined yet also has some dedicated messages signatures or whatever and the adversary sends a valid equality check message to one signer but invalid to the other honest signer.

So what happens is that for the one honest signer who received a valid equality check message is equality check returns true the DKG finishes.

So the honest signer thinks well we can send now money to this threshold public key.

However, for the other signer the DKG did not finish it actually failed and the problem now is is that there's just one signer left and we need to to actually spend the coins which means that the money is gone.

And this means that this integrity property is not enough.

We need an additional property and we call that agreement or to be more precise conditional agreement.

And it says that if some honest signer outputs true then eventually all honest signers will output.

True and we can see how that could have have prevented this previous scenario because this one honest signer whose DKG succeeded could have convinced the other honest signer the DKG actually succeeded and then they could have both signed for this threshold public key that had the coins.

And agree this agreement properties often an overlooked requirement in the in the Frost world and initially I didn't want to mention this but now that there are so many Zcash engineers looking at these crates here.

There is actually an open issue in the Zcash Frost repository by Tim which mentions that the API of the DKG Frost implementation said lists a bunch of requirements and they're all good.

I think it really helps the engineers but it misses this agreement property and we think that's an important one because it could lead to coin loss.

All right.

So now we went deeply into DKG.

Let's get back to what we actually want to do.

We want to specify this simple pet pop DKG.

And simple pet pop requires some equality check protocol and secure channels and we want to specify those as well.

So our design is sort of a wrapped design.

We start with simple pet pop and then we build a wrapper on top that we call a wrapper around I should say that we call ank pet pop and ank pet pop uses simple pet pop internally but adds secure channels and encryption.

And then we build another layer.

Chill DKG that uses ank pet pop and adds the equality check protocol as well as other desirable features.

For example, simpler backups.

All right.

Ank pet pop.

We try to go briefly over it.

It's not too interesting.

I would say every signer has a long-term ECDH key pair consisting of what we call here static pub static proof.

Our assumption is that everyone has a correct copy of every other signer static pub.

So we assume public key infrastructure and now encryption use a one-time pad which is created through an ephemeral static ECDH key exchange between some sender I and receiver J, which means that to the share from sender I to receiver J we add the output of the Diffie-Hellman of the ephemeral key of the sender and the static key of the receiver.

And to ensure authenticity the signers claimed static pub and ephemeral pub keys are added to EQ's input such that if someone would change the keys on the wire EQ would fail.

Let's get to finally to Chill DKG.

How does it work?

So in Chill DKG every signer has a long-term key pair that we call the host key pair and it's derived from AC.

And EQ is now instantiated with a concrete protocol that we call cert EQ.

How does it work?

It's extremely simple.

So everyone sends a signature of their EQ input to everyone else and then signer terminates successfully when it receives a valid signature from all N participants and we call this list of valid signatures a success certificate.

How does this ensure integrity?

Well, if the input is different of one if the signer succeeds and he has all the signatures from the other signers and integrity would fail that would mean that one of the signatures was forged.

How does this ensure agreement?

Well, if a signer terminates successfully the signer has a success certificate so it can convince every other signer using that success certificate that the equality check actually succeeded and thereby also the DKG succeeds.

All right, so this sounded very complicated but from an API perspective it's not.

So in Chilled DKG we have participants and we have an untrusted coordinator that is used mainly to relay messages but can also aggregate some messages to reduce the communication overhead.

So we start by generating host public keys on every participant.

We send that to the coordinator.

Coordinated distributes them or it's distributed in any other way.

It's just an example and then to start the session the signers they select or get obtain host public keys from somewhere.

And the threshold T they run this function that we specify participate a participant step one.

It outputs a participant message that is sent to the coordinator coordinator runs a step returns a coordinate a message that is sent to the participant another participant step another core then the coordinator finalized step and then the participant finalized step.

So that already includes the equality check protocol.

You wanted a time check that we should.

Yes.

Okay, sounds like I'm perfectly in time.

All right, I mentioned back ups and so a traditional problem in these DKG's are also frost is that in contrast to Schnorr signatures or music which we know at least in the Bitcoin world secret keys cannot be derived from the seat because they depend on contributions from other signers.

So the naive approach would be to back up new secret data for each DKG session.

In chill DKG we use a different approach where we have this seat secret seat that only needs to be backed up once and then we need to back up what we call recovery data for every DKG session and recovery data is an improvement over backing up secret data because recovery data is self-authenticating and contains secret data and encrypted form which means that it can be stored with an untrusted third party and moreover it's the same for all participants.

So if I lose my recovery data, I can request it from another honest signer which hopefully helps me or which hopefully works if the other signer is honest.

All right.

So summary, chill DKG is a specification in the form of a Bitcoin improvement proposal short BIP.

It's what we call standalone.

So it's fully specified.

It doesn't require external secure channels or consensus mechanism.

We have a specification or reference implementation in Python.

It provides this agreement property we mentioned.

It provides simpler backups because you can recover from a static seat and some recovery data that is basically public is public for for unforgeability, but it might be a privacy concern.

It supports any threshold and we have an untrusted coordinator that reduces communication overhead by aggregating some of the messages.

And we're working on one additional feature in particular.

So an issue is currently that is and probably this this will stay a property of chill DKG.

A single signer can cause chill DKG not to succeed.

For example, this malicious signer could just send nothing or could send inconsistent VSS commitments.

Which would mean that chill DKG does not succeed.

And actually in the setting that we're considering the signers they are not able to agree on which signer is misbehaving if the DKG does not succeed.

In order to achieve that we would either or for example require a majority of signers to be honest, which we don't want to.

We want the chill DKG to work for any number of honest or dishonest signers or we would require a synchronous network which is another assumption.

We don't want to make.

But we believe that we can modify chill DKG such that in case of a failure each honest signer can determine that either a certain participant or the coordinator are misbehaving.

So they don't know if it's a certain participant or the coordinator which is a weaker statement than determining the exact participant that is that is at fault.

All right.

Besides that we're also collecting feedback at the moment.

So if you're interested in this whole project will be really nice if you would have a look and give some feedback and any feedback is welcome.

We also addressing feedback currently that we've already received.

We also need to add test vectors to make sure that implementations that pass these test vectors are compatible with each other.

And I think that's all I had.

Okay.

Jesse you had a question.

Yeah, I'm curious about this kind of I guess ambiguous claim property of like coordinator or participant.

Like I'm curious how is that actually more useful than not knowing who to blame at all because like as a practical matter what should the participants do with that information or like okay, it's either Alice or Bob who's the who's malicious or misbehaving should they like eject both and like, you know, maybe some innocent participant gets kicked out.

But at least you have all the misbehaving participants out of the quorum or how are participants supposed to kind of use that information?

Right.

I'm not sure if there's a great answer to this question.

So first of all, I think what we want to address here is we want to give an ability to just debug a certain process because right now if something fails, you don't really know why even if you're just playing with a with a protocol.

So I think that that is a problem and then okay, you're right that the statement may not help much.

It might actually be so misleading that it decreases the security of what you actually want to set up because if you were to throw out the participant if you trust the coordinator then you give the coordinator the power to exclude any participants they want from your setup.

So that is a problem.

I think what our choice or what our design goal is there's a long section in our in our BIP actually where we say what we don't want to achieve is that we we don't need to achieve what we call robustness which is that we would automatically be able to throw out misbehaving participants exactly because of this issue which is that when you do a DKG with n signers and you set a threshold and something goes wrong.

Do you really want to continue with a different set of signers automatically or do you want to?

Debug and see what the actual problem is and we believe in most cases you want to do the ladder and this is where this statement it's either the participant or the coordinator helps you.

Yeah, but also we cannot get much better that we cannot get better than that.

Because it would violate these theorems and distributed computing.

Interesting.

I think one way to look at this is that I mean as long as you have a coordinator the coordinator can always.

Lame that the participants send some some other message or even omitted the message right so as long as you have a coordinator the coordinator is always able to assign lame to some honest participant and then if you if you think about this further, it's it's actually not possible to gather around this at all even if you even if you have a protocol without the coordinator.

Participants still need to talk to each other via the network right so if like if I'm the ISP of a honest participant I can always drop their messages and then all other participants will think that I'm too plain because just I or because this one but that this one honest participant is to blame just because it didn't send messages.

So you always have to trust your network and in some way to get some liveness or identifiable boards property even and in this case in our case is just that the coordinator plays the role of the network because it's it's doing some computations but it's also just therefore relying messages.

Yeah, really messages.

We should move to corporate.

I think it's hold on a second who I have next.

Yeah, corporate Frost Federation.

Hi guys, I'll try to share my screen.

Okay, see this.

Yes, we do.

Excellent.

So this is awesome to kind of just follow up after chill DKG because I've been learning a lot about reading from you know Frost keygen and also chill DKG is work and all the issues that you pointed out especially the lack of agreement at the end of Frost keygen.

So I mean I have a slightly different approach right like but exactly what you guys were talking about.

Sorry.

I'm just diving deep into it.

I'm good breathing.

I just want to talk about Frost Federation how we could build an online Federation which remains online and keeps making progress even if there are occasional failures or network outages of some of the members.

So now going back to you know, what you were talking about.

I want to kind of take a different approach and see even if some parties have outages which happens on an online setup.

How can we still keep the Federation alive, right?

But if the threshold is beaten then then the Federation dispense.

So I'm just going to take five minutes here just going to talk about what my use case is what we are motivated by the approach we're taking and what our plan is and essentially I'm looking for a you know, as much feedback as I can get in terms of if I'm going down the wrong track completely.

So the motivation of this is that we want to replace the centralized Bitcoin mining pool operator with the Federation of pool operators, right?

So to kind of not go completely peer to in the middle like you have a Federation of pool operators doing the job of a single entity and the question has to remain online and sign payouts for miners.

So, you know, it is going to be probably holding a decent amount of capital which is kind of the scary part here.

But the membership is decided because we have some Bitcoin contracts that kind of make the Federation do the payouts.

The membership of the Federation is decided by UTXOP on the Bitcoin blockchain.

So essentially we outsource the membership agreement to Bitcoin.

So the approach we're taking is we want to use the Frost TSS board which we are using the Zcash Foundation libraries there and we want to keep remaining as close to as Frost Keygen as possible just because you know, it has security proofs and it seems to be having the most eyes on it.

However, the problem that was pointed out that in the second round if we don't have an agreement then you know, we can end up with some parties thinking, okay, I can create a transaction and some other parties thinking I can't and then therefore, you know, disaster strikes.

So but in that context, we want to solve the problem in a way that you know, we remain robust as long as we have the threshold number of parties alive and you know contributing their shares correctly to the Keygen.

And the reason for that is given that we are replacing we kind of have a federation where we assume an honest majority we say that okay, if and it's an online service if one or two has a production issue and you know, the you know, they're unreachable or whatever happens which kind of can happen and we want to keep making progress as long as the threshold number of parties are still around.

So for us, robustness is important.

We don't want to completely disband the Federation and for obvious reasons reasons.

We can't use a coordinator.

So we're trying to take the Zcash examples and kind of make them work using the APIs in a in a in a point-to-point connected network setup.

And of course, as I said, we need if there is no honest majority we basically the system dies.

So we assume an honest majority at least the threshold as we talk.

So the current plan is essentially just focusing on the DKG.

So what what I want to do is take the Frost-Keegen protocol and in the second round where there was no agreement have a simplest broadcast a Byzantine fault tolerant broadcast which kind of make sure that all parties know that every every other party or at least a threshold number of parties have seen the same shares which is very similar to what EQ and the 30 Q are doing.

But I think your solution is super elegant in the sense that you don't need too many rounds.

You just look at your log of what you've seen and then share exchange exchange signatures of it.

Whereas what I'm thinking of doing is just run BFT broadcast our protocol.

The reason is I mean again robustness and then secondly, we are willing to sacrifice some latency to keep liveness around right.

So I remind if it takes 15 minutes for the Federation to change because we don't need to generate signatures like as frequently as possible just because of our use case being like that.

So as part of this the Federation work we basically started work with Rust implementation where we're doing simple things like, you know, reliable secure, authenticated channels for unicast which is just using simple noise framework.

We have the echo broadcast implemented with no failures assumption, which is basically we make sure that all the parties in the network have received the same secret share by just sharing the hashes and enacting it.

There are more elegant ways to kind of reduce it from you know, to I was thinking of using the Brahma's BFT broadcast protocol there as the most simpler solution to try things out with and see how they work.

Of course, there are much more elegant protocols, but I think they just complicate the implementation.

So just stay simple even if it costs us time at runtime.

That's fine.

If such a modification can work, which is a big question mark in my head, then we'd like to kind of use that.

Again, we can't assume a coordinator and then of course, we want to measure how long it takes with this kind of additional step in there, you know, which might have more than two rounds to see how long it takes to kind of run the keychain protocol in a setting of 20/30 members.

I would love any help with reviews and any feedback or any pointers in the right direction.

And you know, that's that's what I'm talking.

I mean, I haven't had, I haven't written anything down now.

So I should write something down so that we can then I can ask properly for reviews.

The GitHub repo is here and my email and signal are here.

And thanks a lot for giving me the five minutes here.

Any questions or suggestions?

Thank you.

So yeah, I wanted to include yours here because I just felt it was a different class of frost implementation and I wanted to make sure everybody was aware of it.

So thank you.

Thanks, Chris.

Similarly, the next one is the Sarai, which is Luke Parker also has kind of a different case.

You ready, Luke?

Yes, I am.

Let me just figure out exactly how to do the screen sharing and I will get started there.

And try to keep it short where I want to leave some time for Q&A and where to go from here.

So yeah.

I'm so sorry.

I'm just struggling to figure out the screen share for some reason.

It's only showing a few specific things to screen share instead of my actual presentation.

So just one minute while I try to sort this out, please.

I can also share it from my screen.

It works now, but thank you.

Let me just enter presentation mode.

Is this still working?

Is this all good?

We're seeing the overview, not the presentation mode.

Sorry, I was trying to screen share to the presentation mode.

I was just asking if it was full screen.

Yeah, we see it.

Okay, I'm just going to do it like this.

Sorry, we're not full screen.

We're only...

Let's keep it simple.

Is this full screen now?

We're not seeing the full screen.

We're seeing your thumbnails and such.

Oh, got it.

I'm so sorry.

I don't want to spend too much time debugging this.

So I'm just going to...

Sure, present from here.

Yeah, exactly.

Because this is still largely the full screen experience.

Sorry that it's such a mess.

Anyways, so Sarai, it's a decentralized exchange and I've done a lot of work with Frost over the past couple of years.

So today I'm just presenting on that.

Sarai, it's different from what most people are discussing in the context of Frost, which is why I'm...

It wasn't enough to be speaking here.

It's a decentralized autonomous protocol and we're looking at using very large signing sets.

So we're not looking at the self custody situation where you might have, you know, a three of five multi-sag or two of three, yet we're looking at doing 101 out of 150.

And then this is also considered an adversarial environment.

So not only can we not be at risk if you know the private key leaking, but we do have to consider that any one of our individual participants may actively try to halt the protocol via denial of services.

So that informs a lot of the design decisions and choices made with this work.

So to start with, Sarai does have an implementation of PetPop.

It's just the one from the Frost paper.

We do offer share encryption because encryption can be difficult and we needed to offer identifiable of boards in order to offer identifiable of boards.

If someone sends an invalid share, we have to have some degree of understanding of how shares are communicated and how to verify if an invalid share was sent.

So we do handle share encryption on our end.

That's using a Diffie-Hellman with a ephemeral point and a per participant point, but we don't attempt to handle authentication of who sent the messages.

We don't attempt to handle the communication and we don't attempt to handle the consensus on who sent what messages if they actually sent multiple messages, etc., etc., because as the ChillDKG talk pointed out earlier, that is quite a lengthy discussion and there's no great answer to it.

Well, obviously there's the ChillDKG work, but that is a non-trivial amount of work and there's choices to be made there.

So for Sarai, we did not intake that into our library.

We just offered the cryptography of it.

What Sarai has also done is a novel one-round DKG currently just being referred to as DKG 576, better names, more than welcome to hear them out.

DKG 576 solely relies on the hardness of the elliptic curve decisional Diffie-Hellman problem.

So it's not pulling in another crypto system such as Palier or class groups.

It does not require any consensus on the context or the messages.

If the participants disagree about the protocol being executed, they simply won't interact with each other.

It does achieve identification of faulty participants if participants agree on context.

If they agree on who else is participating and what public keys they're participating with, then it is able to identify valid messages and invalid messages.

It outputs an unbiased key if done in the N of N setting, but it can also be done robustly in the T of N setting.

Of course, if you do it with just T of N, that's no longer unbiased and you do need some degree of consensus to agree on which threshold of participants actually participated.

So there's a few different ways that it can be used, but if you use it in the full N of N mode and you don't care about identifiable of boards, it removes all requirements on communication and consensus.

It either works or it doesn't work.

The way we accomplished that is by using Exponent VRS.

That was from a paper that came out in March of this year.

Exponent VRS or Exponent Verifiable Random Functions.

They generate a verifiably random value, but instead of publicly revealing the random value, they only reveal a commitment to it.

So the EVRF work itself, they did posit a one-round DKG.

DKG 576 is the DKG from the EVRF paper, but extended with public verification of the secret shares.

I'll get to that in a bit, but just to kind of summarize the EVRF work, they premised in EVRF.

There's a couple different ways to do it.

They premised one based on the decisional Diffie-Hellman problem with the Tower of Elliptic Curves.

For anyone unaware, a Tower of Elliptic Curves is where you have two elliptic curves, say EP and EQ, and the scalar field of EP is the field of EQ.

And the great thing about this is that if you have a proof over EP, you can efficiently work with points and do scalar multiplications on the Tower of Elliptic Curves EQ.

So for the decisional Diffie-Hellman premised EVRF, they prove for a pair of Diffie-Hellman over the Tower curve within a bulletproof.

Of course, a bulletproof is not only a very well-reviewed proof at this point, but it also has a trustless setup.

The bulletproof itself is over the Towering Elliptic Curves EP, and it just defines the random value as the sum of the X coordinates of a pair of Diffie-Hellman.

If you use Elliptic Curved Advisors, which is its own long story of academia, which is too complicated for me to get into on this call today, but if you use Elliptic Curved Advisors, you can actually prove a scalar multiplication, or rather a Diffie-Hellman, using only seven rows in the resulting inner product proof.

So it creates a very, very concise bulletproof in the end.

The divisor technique that was not specified in the EVRF paper, it's something I actually reached out to the authors about after they published, but the technique of using divisors to prove for scalar multiplications is proven.

That does have the security proofs for it, and those security proofs have been reviewed.

But the exact interactive protocol and the associated R1CS gadget, that is still a work in progress.

So while there's a lot of interesting functionality here, the academia on all of this is still moving forward and still pending.

So the DKG that was in the EVRF paper, they defined the coefficients of the polynomial as the results of a variety of EVRFs, and because of defining the coefficients in the polynomial, that way you only have one possible polynomial for a key in context.

So even if a participant wants to send different people commitments to different polynomials, they will be unable to create the necessary zero-knowledge proof, and they are forced into sending everyone the same polynomial or not sending a polynomial at all.

Then my contribution was extending the idea of doing a one-round DKG, where the coefficients are EVRFs, by also saying that instead of the secret chairs being transmitted out of band with no further context, I defined the secret chairs as encrypted with the results of a pair of Diffie Hellmans again, just as the pair of Diffie Hellmans produce a random value, here they produce a mask that we use to encrypt secret chairs.

And this means that because the prover outputs a commitment to the mask that they're using, and of course the bulletproof inserts its integrity, and this lets anyone, even non-participants in the DKG, verify that the encrypted secret chairs are correct.

Beyond the implementation of PEPOP with identifiable reports and beyond the work on DKG 576, Sarai has also implemented Frost, it implements the IETF specification, it's also modular to the challenge function, so the library allows you to specify a custom way of hashing the nonce public key and message.

It's also modular to the signing protocol itself, so if you don't actually want to use Frost, but you want to use another protocol amenable to the same techniques, that is an option.

For our work with BIP340, we actually defined BIP340 signing as its own protocol because of the oddities around tweaking keys so that they're always even.

It was easier to define that as its own protocol that uses the same techniques rather than just as a custom challenge function.

The library also supports signing with offset keys, but one regret I have is that we did not implement the library with public verification.

The library is only coded to allow you to participate in a signing protocol, handle the commitments, handle the shares, if you're an active signer, and the reason I included this bullet point is because in hindsight, it's actually something I regret because public verification I've actually found very desirable.

So if anyone is still working on their Frost implementations, I greatly encourage adding public verification.

This is just a slide on achieving robustness.

Obviously, as I said, Sarai is meant to be this autonomous platform, so robust protocols are important to us.

Obviously, Roast has been out for years.

We did not go with Roast.

We went with BFT consensus.

We have a bespoke Tendermint blockchain to execute all of this.

So we also use that to see if people don't send a message in time or if they send an invalid message and we use this Tendermint blockchain to schedule reattempts.

It's not a solution that will work for everyone and it's certainly not the most efficient solution, but it was a simple solution that worked well with our use case.

I also wanted to share my personal opinions that protocols with N-squared complexity are too expensive to be regularly run.

In this use case, Sarai is looking at potentially signing signatures every minute or so on average, if not a bit faster.

So when we're discussing, you know, doing 150 squared operations every minute, especially with regards to the bandwidth usage, that's not something I can personally express interest in, but I am very interested in robust protocols that have linear complexity even if those do pull in alternative cryptographic assumptions, even though at this time, none of Sarai's work does rely on alternative cryptographic assumptions.

This is just a status slide.

The DKG crate that is published on crates.io that has the PEPPOP with identifiable of boards that was audited all the way back in March, 2023.

DKG 576 is fully implemented in Rust, but it's unaudited.

It will not be audited until it's proven.

The security proofs on that are still pending.

The frost implementation was also audited back in March, 2023.

And again, that does support the IETF specification, including all cipher suites.

Since then, we've applied modular frost to Bitcoin with, of course, BIP 340.

We also are able to sign "transactions" on Ethereum.

Technically, we use a smart contract to verify the signature.

We've also applied it to Monero, which uses a linkable ring signature, amenable to the same techniques, but not itself snore.

And then for our work on Bitcoin, we do offer a library of Bitcoin Sarai, which has a high-level API for sending Bitcoin transactions from a threshold multisig that was audited back in August of 2023.

And it has already been integrated into StackWallet to offer frost-based Bitcoin wallets.

One of the things is Bitcoin Sarai use the DKG 576 and the StackWallet use it.

Bitcoin Sarai is abstracted to the DKG used.

So the only other thing I wanted to note on this call here is that I would very much support a BIP for frost because Bitcoin Sarai kind of went its own way, but obviously I would love to standardize this and achieve interoperability.

And then of course, if anyone has any questions, I am happy to take them.

Any quick questions for Jesse?

Go ahead.

Yeah, so super interesting.

This is my first time hearing about the Exponent VRF paper, which I'm looking forward to digging too, but just curious if you have thoughts about that scheme in general and whether or not it looks like it maybe has some advantages over frost, like you can get accountability in the signing or determinism and stuff.

So curious if you just have an overall take on that scheme and how it compares to frost.

So the Exponent VRF work, we only are proposing the use of Exponent VRFs during the key generation process.

The EVRF paper did define a two round, simulators, Schnoor signing protocol.

I don't remember if they proved it secure in the concurrent model or not, though.

I believe they did define a two round Schnoor protocol.

I believe it has stronger security properties than frost just because of course, it's fully simulators.

But at the same time, I would not prefer it over frost because the EVRF is of non-trivial computational complexity, despite being only, you know, seven rows in a bulletproof, which is quite small.

That's the same cost as creating seven bit commitments.

So it's roughly, you know, 10% the size of a 64-bit range proof.

That doesn't change that actually creating the proof if you use the visors requires doing some polynomial multiplications, which can be accelerated if you use the fast furrier transform, but still means that you need to grab a fast furrier transform.

So because the signing protocol at best achieves some better security claims and it doesn't actually change the, it doesn't cause a functional improvement by reducing the complexity or adding robustness, I don't believe that the security is worth the computational expense.

Sam, go ahead.

Yeah, thanks for the talk.

The TKG 576 looks very interesting.

I also have played with some of the thoughts into that direction, but cool to see that someone has really worked on this.

Do you think with the property that BSS commitments are fully deterministic, would you still need, I wonder if he still needs broadcast for something else or would this be a protocol that doesn't require any external broadcast mechanism?

If you're talking about the end-of-end use case, all that you require is that everyone receives a message from everyone else and that message can be different, but it doesn't matter if it's different because if it's different, the only malleability is going to be in the proof itself because the commitments are deterministic.

So you can use broadcast communication if you have an efficient protocol for that.

You can use point-to-point communication and never obtain synchrony to ensure that you all receive the same messages.

And if you have a different context string, like for some reason you don't even agree on who the participants are, by including the context in the transcript of the proof, you just simply won't accept other people's messages as relevant to your TKG protocol.

So it really is quite flexible.

Okay, that makes sense.

You say end-of-end, that it means, and maybe you had this on the slides, you say in a T-of-end setting, you still need to agree on which contributions you take into account, I guess.

Yes, if you're only performing the TKG with three people, you don't want the participants to disagree on which three are actually participating in the TKG.

That makes sense.

Okay, cool.

But yeah, very cool work.

Quick question from me is, there was some discussion, gosh, five years ago, I put the link in the chat about doing curves where the P and the Q are swapped to be able to do different kinds of bulletproofs.

Is that effectively what this tower construction is?

Is that?

So in SecP, you're swapping the...

Yeah, so for the EVRF implementation, we use SecP and SecQ for that specific, for SecP specifically.

For ED25519, which is the other cipher suite we've defined internally, I just ran a script to find an amenable elliptic curve, and then I verified it had the security properties necessary.

That was my second question, because I wasn't sure how it worked on 25519.

Okay.

Yeah, there's been a couple of proposals for embedded elliptic curves or powered elliptic curves over the years.

I didn't use them because I wanted a prime order curve, just because it's resolved some headaches, especially when you're doing proofs and a bulletproof about scalar multiplications.

But yeah, elliptic curves aren't too difficult to find, and you do have to, of course, do review on them to make sure they're secure.

But the safe curves criteria are well documented, and then the disagreement with those criteria is also well documented.

Okay, so to keep things tight, I'm going to, let's see, first close this one, share.

We were going to share briefly on, you know, updates on frost signatures with Gordian envelope.

The key points are we are using the Zcash frost implementation for the BIP 340, 341 support, and basically adding that to Gordian envelope.

And this is important to us because we want to be sure that signatures are verifiable there.

So we've long had Schnoor support, but not the latest BIP 340 support.

Why is this relevant to this community?

A number of you are working on protocols and APIs over the net to do coordinators and other functions.

I would highly encourage you to take a look at our videos and documentation on Gordian envelope as a possible underlying data format for it.

And it has some definite advantages, which we can bring up in another meeting where I can share offline, but just in regards to time.

I did have some quick questions to try to wrap things up.

Are people able to spend a few more minutes and see if we can't get some some quick discussion on this or we were scheduled to stop on the hour.

People still available, worthwhile discussions.

I see one thumbs.

I need to leave.

Okay.

Sorry.

Yeah, I have another meeting.

Thank you for having me.

We look forward to hearing more from you.

Yep.

So, right.

Next time folks.

I know that NIST, some people are, you know, talking about submitting Frost, I assume under Ristrado, but I don't know what the NIST is.

I'm going to go back to the NIST competition for multi-party threshold cryptography.

Does anybody know if that has made any progress or is anybody here involved with that?

Doesn't sound like it.

Similarly, you know, ITF, the IESG published a Frost protocol, but as we've already discussed signing part of it, nothing specified on the DKG.

Is anybody working on any ITF or IESG documents in this group?

Not hearing anything there.

The obviously we've got the two BIPs, BIP Frost DKG and BIP Frost signing.

Are there any other BIPs that we didn't mention in our discussions that we should be aware of or pending or out there and unnumbered?

Not that I know.

Okay.

But I think probably or it is at least conceivable that there will be follow-up BIPs that deal with standardizing Frost in wallets similar to how it happened in music for descriptors and PSBT.

Yeah, so we're planning a meeting in December.

I think it's December 2nd or 3rd, Shannon, do you have the date off the top of your head?

But it's basically more for wallet developers who are now beginning to put Frost on their roadmaps and such.

So hopefully we'll get some updates from there.

And I also know that the Zcash ecosystem has a number of people that are also interested in incorporating Frost in into Zcash.

That will be December 4th.

December 4th.

Okay.

On DKG, Colpreet had raised the question on detecting failures.

I think we talked quite a bit about that today.

So I don't know if we need to discuss that anymore.

Yeah, you know, examples without coordinators during the signature generation, you know, peer-to-peer is kind of an open question.

There aren't a lot of examples.

I mean, we obviously have trusted dealer and then we have, you know, the distributed key generation with a coordinator involved, but there are a lot of discussions about how you do this in more of a peer-to-peer fashion, especially in small groups.

So for instance, self-sovereign keys with, you know, where your phone and an air-gapped device and some other device all cooperate for you to be able to not have to trust any one of those devices.

Has there been any other work on key generation without a coordinator, distributed key generation without a coordinator?

Okay.

Definitely some questions came up with the BIP32 derivation.

And, you know, what are the risks there?

And, you know, especially, you know, I would ask this of Jonas and Tim, you know, what are the complications with BIP32 keys and risks with ChillDDK?

I mean, you are creating as you, you have a, anyhow, I'll let you guys.

No comments there.

So, yeah, that's...

Sorry, I unmuted the wrong microphone.

I'm not sure if you're referring to the thing I mentioned during the presentation.

Yes.

Where, yeah, I just said that you cannot derive your frost keys from some seed because it depends on the contribution of other signers.

But once you have an output of the DKG, then it should be secure, at least I'm not aware of any reason why it wouldn't be to also derive additional keys from it using BIP32 or Taproot tweaks.

So those would have the same risks that BIP32 already has.

So you still have to be careful about, you know, your chain codes and all that type of stuff, but nothing that you know of that's at worst.

Yes.

Okay.

I mean, it may affect the the unforgeability of the signing scheme, but I don't think so.

But I also haven't seen a proof of that.

Go ahead.

Oh, I was just going to say, I think my main question would be if this was attempted to be standardized at BIP32 for frost, would the standardization interest be in, would there be interest in applying the unspendable script path to the post-BIP32 derived keys?

From a security standpoint, I don't believe it's necessary.

I believe the BIP32 derivation should invalidate any existing script path and not allow claiming there's a script path.

But if we want to have this practice of always applying the unspendable script path, once you do the BIP32 derivation, that would be the next step.

That's definitely right.

Yes.

We spoke, other key uses, but we spoke about Diffie-Hellman earlier with Jesse having an example of using the shares to create, to allow for a distributed Diffie-Hellman.

And we also talked about there's no proof there that there might be some naive assumptions there, etc.

But I'm also concerned there are a lot of other places where wallets and other parties are expecting that the keys are single key.

And if there are other operations that wallets and other parties do beyond BIP32 and Diffie-Hellman, that we need a distributed function for.

Is there anything, anybody have any thoughts or concerns there?

I think this will come up again when we have the wallet discussion.

I think there's, you know, just my somewhat naive talking with other developers of wallets that, you know, they're doing a lot more with the seeds and derived keys and things of that nature, sometimes internally for different things than we might be aware of.

Including, you know, encryption and such.

We've had a lot of discussion about X-only keys.

It sounds like there was some good discussions there in regards to, you know, should it be done at the DKG level or at the Frost level, etc.

You know, that might be worthy of a further discussion.

Obviously, you know, we've had some discussion about the Pettersson commitments that are used in Frost VSS.

Not really clear how much compatibility there is between the different versions.

And of course, we also have an example of a non-Pettersson commitment in one of the examples.

I mean, how much compatibility is there between the various Frost implementations?

Can I use the, you know, a VSS secret from a DKG, you know, on Rust with, say, the signing in C?

I don't believe so right now and don't know how feasible it is.

And then I also, you know, is there anything in common?

Is there sufficient in common between MuSig and Frost that, you know, for instance, the Chill DKG has a technique where each shareholder has a seed?

Can, you know, is that possible to be compatible with MuSig and such?

Any thoughts about compatibility of the VSSs?

Well, I don't think there are Pettersson commitments, because usually there are Pettersson commitments which have two generators like GNH, whereas the VSS is just the G generator.

But, and I don't think there are any commitments in MuSig, like there's nothing, there's no coefficients to commit to in MuSig.

So I think the, and also, if we're just talking about, like, if we're not bringing in proactive secret sharing and dynamic secret sharing, the VSS is only exists during the DKG, and when the DKG is done, the VSS is thrown away.

So I don't think you necessarily would want to use like one implementation for half your DKG and another implementation for the other half of the DKG.

Like, the DKG should always be, like, a single implementation should take care of the whole DKG.

But I do, but at the end of the day, they're just elliptic curve points that commit to the coefficient commitments in a very straightforward way.

So interoperability shouldn't be particularly difficult if that was desirable.

I just, I'm concerned that, you know, I mean, we have high standards for storage of keys, you know, where a number of parties are, of seeds, excuse me, where, you know, people use air gap devices, use ephemeral devices like the little seed box to do signing, and then it has no memory and you have to reenter in your seed every time you use it, that type of stuff.

But we don't quite know what is the risks of how we store our, you know, our shares, our verification data, things of that nature, you know, how, you know, clearly because it's on frost, we, there, you know, is some value in having it on multiple devices, you don't have to trust them all.

But then there's this heterogeneity value, whereas, you know, if something, say, is in a Rust implementation that runs on a, say, a foundation devices wallet versus something that's, you know, in C running on, you know, on a laptop, which is storing it in iCloud or Google or, you know, on a trust zone, you know, there's some value in heterogeneity.

So that's part of the reason why my question for the compatibility here.

And, you know, just wonder if there have been people thinking about any of that.

It doesn't sound like it's been a priority in these discussions.

There was some discussion about verifying, you know, offering the verification APIs.

Luke was talking about not having done that in his implementation and now going, yeah, maybe that would still be valuable.

But again, that's assuming you're keeping these around and not just using the public verification values.

Am I correct here or am I missing something?

I'd love to clarify that the reason I regret not offering public verification, not only because of course, it makes my library less appealing, you know, there are people who may want that and I am unable to service that for them.

Yet, also because in the Sarai use case where we specifically use Vicentine fault-toler consensus to execute our signing protocols, we kind of have to give everyone a copy of the full signing transcript.

Yet, despite that, they have to be notified out of band of the resulting signature and they can't simply rebuild it from the transcript.

So when I started working with this two years ago, I didn't realize how that would be a good and smart thing to do.

And then I kind of realized, yeah, I should have added that functionality because you realize it would benefit you after you've already started building stuff and it's kind of just the unfortunate situation there.

Yeah, I don't think we have anybody left from the Zcash team but they also I don't believe have a verification API per se once you've generated.

I'm going to skip that one.

Obviously a lot of the discussion today has been, you know, what are the trust channels?

You know, with ChillDkg, you guys have begun to define what the messages are.

I don't know if you've actually have, you know, are they encapsulated in JSON or whatever if you've gone to that level yet.

Is there anybody who's planning support for NFC and QR?

A number of wallets are using our standards for QR codes now for PSBTs.

We're planning on, well, we already support NFC and QR in the Gordian envelope libraries, but our roadmap is to use GSTP for Frost and Gordian Seal Transport Protocol if you've not seen the documentation or the video that Wolf did.

I think has some real power in the fact that it really makes it independent.

You can send something concisely over a QR, animated QR, NFC, Tor, other unsecure channels, and it has a peer-to-peer and Tofu capability currently which we can extend with Frost into other scenarios, especially if we can get some reliability on the Diffie-Hellman things that Jesse's been working on.

Are there any other trusted channel issues that people kind of feel like, gosh, I wish, you know, we had a chance to work on that?

You know, limitations, Jonas, any particular things in your, where you basically are punting, and I think you said you were punting on the public key distribution, is that correct?

Jonas, saw your mute go off.

Yes, again, wrong mute button.

Yeah, we're assuming the public keys are already distributed.

That's the assumption we're currently making.

They are either distributed or there are some out-of-bounds check out-of-band check to make sure that these distributed public keys are correct and no one has intercepted them and exchange them with something else.

Yeah, so that's definitely on our roadmap.

We don't have anything particularly for that, but that is a very important part of, you know, being able to create a group, for lack of a better word, community, whatever, that are, you know, maybe leveraging many DKGs for different purposes within that group.

So, okay.

I wanted to share, Shannon, could you put these two links into the chat?

So we have started a Signal chat if there's interest in the community.

You can sign up either by going to the tiny URL, which also will give you the sign up, but if you are using Signal and want to have some of these deeper discussions and ability to ask a question on there of the Frost developer community, you know, you're welcome to join that group.

I know that Jesse and I have had some interesting conversations on there in the past and if you're using Signal, it's a good place for that.

Our next meeting is going to be on December 4th.

Again, a focus on the developers needs for Frost and their questions and concerns and, you know, how do they begin to support either 341 or whatever the Zcash equivalent of it is.

So that'll be on December 4th, same time also from 10 Pacific to noon.

And if you know of any wallet developers that might be interested in talking about their challenges or concerns about moving to Frost, we'd love to have some presentations on that.

And if you have a presentation where you want to talk about it, not with the audience being other library developers, but the audience being wallet developers and sort of the practical side of these problems, or you want feedback from them on your libraries, we would welcome presentation from that.

Any other things that we can do better for and to help?

I mean, I think this was a good productive meeting today.

I hope it was for all of you.

What else can Blockchain Commons do as kind of a neutral party to help pull the community together?

Jesse Jonas, how can we better connect to the Bitcoin community?

I feel like sometimes it's, you know, I keep on finding little hidden edges.

Not an easy question.

Okay, but the invite was not public, right?

I didn't find anything on Twitter that I could have shared Yeah, for this meeting, we didn't want it to be too public because we really wanted, you know, I worked really hard to get a broad set of people and how many people are on the Frost list?

We have 23 people on the Send in.

Yeah, so it's partly just to keep the conversation a little bit higher level.

I think at our next meeting we can open up more.

But I didn't I mean, and maybe this was a mistake, but I didn't want it to be.

I wanted it to be a little bit more technical, which is part of why I opened with Jesse's, you know, talk because I knew he was going to jump into some stuff that other people go.

Oh, I haven't heard about that idea before or and also why I really appreciate you and Tim presenting on Chiltakg because I think the Zcash community, you know, hasn't really gotten to that point yet either.

So I appreciate you guys presenting the wallet one will be more open and then we should make a decision about, you know, do we want it to be wide open for the next one of these which would be early, you know, sometime winter not exactly sure the date for the next wallet.

This is going up on YouTube as well, right?

So all of this will be up on YouTube.

Okay, great for other people to it.

Yes.

So cool.

It'll be up to later today probably.

Okay, awesome.

And we do transcripts and notes and and some of that will come out over the next week or so.

It takes a bit more time, but you know annotate some of the links and things of that nature.

So those all be in our Frost pages.

So developer blockchain Commons and then we have Frost.

So it'll all be linked off of here and we have the videos from the previous presentations and as well as some links and papers and things of that nature.

That's all at developer blockchain Commons Frost.

Anything else that I'm missing?

Shannon.

Luke mentioned over in the chat that something that we could do is actively soliciting groups to do security proofs.

Yeah, we definitely need to go ahead.

I'm not against that idea.

I was correcting a typo.

I made my message about how I am working on that for my own work on DKG 576.

I still think that's a great suggestion for the group.

I just wasn't making it for the group.

Well, we I'm not sure why we had several cryptographers say that they couldn't present or couldn't come today, you know, all different reasons.

You know, we scheduled this in September to avoid the vacation madness of August, but apparently, you know, a number of people weren't able to participate.

So, you know, we need more cryptographers and more people that are able to, you know, to do the kinds of proof and proof review, especially as we're kind of pushing the edges of things in some of these areas.

So, question, would you like me to still do my short talk for the record for either clipping up on YouTube or for you know, just extended meeting.

Obviously, nobody has to stay for that.

I expect people want to, you know, this is just on what we're doing with, you know, for the record.

Anything else?

If you'd like to hear about Gordian envelope and how it applies to our BIP 340, 341 support and plans, hang around a bit longer and Wolf will do a quick overview.

So, you want to take over the screen or you just want me to go to that slide?

I'll take over the screen.

Okay.

Let's see.

So, you should be seeing my browser tab here.

Screen here.

Yep.

Okay.

Alright, so as I put already put in the chat, I want to mention our Gordian envelope playlist.

The first three videos are probably the most interesting if you're new to Gordian envelope, which this is basically a commercial teaser, which basically fast-forward through all my slides of this with music.

So, these next two pieces here are a pretty thorough technical introduction to it.

So, if you're not familiar with it, but obviously this gets into the question of what are we signing with Frost?

We already support several different kinds of signatures, including Schnorr signatures, which I'll talk about more, but also ECDSA and the various kinds of signatures that SSH supports.

But so, and the Gordian elements themselves can be used for everything from distributed identification documents, verifiable credentials, requests and responses, including our Gordian cell transaction protocol, which actually includes something that's been talked about a little bit or touched on, which is encrypted state continuations, which are self-encrypted continuations that come back to the senator, so they can actually not store local state, which is great for a number of reasons, both in servers and constrained environments.

So, our stack is based on the SECP-256-1 crate, which is a wrapper around the CD library.

And that stood us in good stead until we actually wanted to ensure that we had BIP-340 compatibility, in which case we realized that the crate, as it currently ships, only allows fixed length messages at 32 bytes, which is fine if you're going to pre-hash everything, which is what we're doing until just now.

And I'll show you how we worked around that in a moment.

The other crate that we're working on with Frost experiments is the CD cache implementation.

However, due to the fact that we want to also use BIP-340 compatibility, none of the crates currently shipping with this crate, the implementations actually support that.

So, we've gone directly to this PR, which is for the CIP-326-1 TR caproot crate, which a conduit mentioned earlier that he's working on, and we really appreciate that effort.

I hope that will launch soon.

And that's based on this PR by Zebelake.

So, we're basically, let's see, let me switch over to now.

My development environment here.

Let's see here.

Not yet.

Yeah, I know.

I'm finding the right one.

Here.

I can't share my whole screen.

Okay, one second.

Close a bunch of windows, maybe simplify this.

Should we skip?

No.

Okay.

All right.

Okay, now we're seeing it.

Okay, so you should be seeing my BC-REST crate.

Should say BC-REST at the top.

See that?

Yep.

Okay.

All right, so this is our, the crates you see here are our stack, essentially BC-Crypto is where we have our Schnorr signature implementation based on the SECP-226-K1 crate.

As you can see from the cargo, it was pinned to the 0.27 release, and now it's currently pinned to the actual master branch, which includes the ability to have variable length messages in it.

And that's all I need to say about that crate for the moment.

That's basically means that's a breaking change to our stack, but not going to be actually using our Schnorr signature scheme yet.

And then that, but that probably gets into our envelope crate, which is the main thing of interest at a higher level here.

So I have a BC-Frost crate here, which is currently using, it would be using the Z-CASH implementation for Frost, except for right now we're pinning it to the actual PR from Zebra Lucky.

So the actual interesting thing here is to actually convert between our stack, which includes sign public key or what you're calling verifier keys and our signatures, which are 64 bytes.

These are 32-byte X-only keys.

In fact, sign public keys can be several kinds of keys there in Enum, but they include our Schnorr type, which is a 32-byte X-only public key.

So, but we have these conversion things that basically give us every name Z-verifying key by imports import as for our purposes.

So anything you see with Z is essentially from Z-CASH crate.

Everything without it is our crates.

So all you need to sign envelopes and verify them in this case is to convert from the Z versions to our versions and you don't even need the other way, but we can run trip them.

No problem.

But the Z-verifying key that bins is actually still 33 bytes, but always has a prefix of 02, meaning it's an even parity.

So we just drop that off.

The frost signature is just a straight copy across.

So then we have this method called frost sign, which just takes a variable length message and that's again important because it can be pre-hashed if you want, but it can be according to 340, which takes variable length messages.

We now support that and it returns a tuple of a signing public key, which is our crates format and a signature, which is again, it's our crates format.

This is basically adapted from the example given for a trusted signer.

Sorry, sorry, trusted dealer, where it's doing a straight 35 split.

And so the most important thing here is that we don't care about actually how the frost signature is generated because all we care in this case is not is that it's compatible with our system completely transparently.

So I'll skip over most of this.

Here you can see at the end.

It's actually converting using these conversion methods to take the verifying key and the group signature and convert them to ours and then returns them as the tuple.

So and then in our tests here, we verify that by simply taking a simple message and then frost signing it and then we're basically done with the frost crate at that point.

Now this public key to verify is back to our our stack, which goes down to the secptu6 table one crate.

The branch of it that the master branch which currently allows for a balanced messages and it doesn't track verify for running this.

So now envelope.

Envelopes are a binary structure.

They're based on to turn a seabor.

They are basically they have a Merkle tree like structure and allows you to do whole order based deletion of each other really cool things.

I will go on out here because we've talked about elsewhere, but I really urge you to check it out because there's a lot of interesting things here that I think especially if you develop things like protocols or blockchain documents on the blockchain things like that.

I think envelope is might be of interest.

Anyway, all we're doing here is taking any envelope.

We're wrapping it because an envelope is the subject in several assertions.

So again, when you understand all the students know what wrapping is we get the wrapped envelopes digest because every branch of an envelope including the whole branch itself has a 32-byte digest associated with it, which is a shot of 256.

And then we call the same frost sign message with the with the digest now in this case the whole envelope but the digest itself because that's what we care about and we get back to public key in the signature and then we add that as an assertion to the envelope.

So we're not just returning it.

In fact here we're returning the signing public key the verify key and the sign envelope is what we're returning from this and then we pass that back as a tuple.

And so in our test here.

We're creating a new envelope with the subject Alice and this could be any seabor any binary object can go as any of these positions and they're adding a single assertion to it knows Bob and so that's the unsigned envelope.

Then when we call the frost sign envelope and we get back to public key and sign and sign envelope we print that out as well and I'll show that in just a second here and then we verify it and obviously we're starting that it's okay.

So when I run this you're going to see it's going to print out the text format of envelopes which is it what's called the envelope notation which is a non-run round trippable diagnostic format essentially.

So so there we go.

This is the original envelope Alice with one assertion knows Bob.

This is the wrapped envelope in the in the braces here and it contains one assertion verified by signature and as you can see it passed.

So this signature could be one of many signatures on the envelope the envelope can be encrypted the envelope could be alighted all kinds of things the signature will still verify.

That's one of the cool things about about envelope.

But the thing is also in this case is entirely agnostic as to how the signature was created in this case.

It was created with frost.

It could just be a one-off or one-on-one signature normally you don't know and you don't care and that's the whole point of this is to be entirely transparent about how the key was generated just that it verifies.

So that's our proof of concept at this point and the fact that it only takes this few lines of code actually successfully sign an envelope.

I think is you know part of what we're doing, you know, part of what we think is special here is all it takes is wrapping up signing it and adding it as an assertion and you're done.

So I'm open to questions.

I think the the key thing right now for this sort of the next, you know, I mean we need to support the the Schnoor signatures and the the the 340 and 341 test vectors.

But I feel like the next step is you know, can we add sufficient value to actually do some peer-to-peer coordination or coordination between, you know, self-sovereign devices that you know, you know either learns from the chill DKG or whatever.

What is your and Tim and Jonas what is how are you planning on encapsulating your data when you're in your in your format is it going to be you know, Jason with base 31 or base whatever the base 58 type of constructions of all the binary values or kind of where are you guys at?

So yeah, please Tim.

Okay, I mean I've talked enough today.

Yeah, right.

So at the moment, I think like the the honest answer is that basically out of scope currently of our group because we try to focus on the really on the cryptographic or the deep cryptographic details because I think makes sense to spend our time on this.

It's not premature for us to start looking at your your protocol and figuring out if we can you know, what we can do to do it with GST P.

That's the Gordian seal transport protocol and such as the using your you know, your API protocol.

You know, go ahead.

I mean, Yeah, sorry.

Good.

I'll just say so some one particular thing that I didn't that kind of rung a bell when you guys were talking there was there was something where you were talking about with the coordinator receiving some documents some things and being able to evaluate those.

One of the things that is a particular feature of the Gordian seal transport protocol is an option to do this thing called in an encrypted encapsulated state and this was required by parties like Foundation devices and others where the device can't necessarily keep track of all of the the state that is required.

And so the you know in a lot of the GST P protocols the you would encrypt it with your own your own private keys and the pro, you know, our protocol basically says hey when you're communicating to me, you need to communicate back to me the state that I you know gave you previously.

And there was something that you guys were talking about in yours that kind of reminded me of that and of course, you know, all of our objects are, you know, deeply nestable with different you know capabilities in there.

So you can do we already support a Schumier permit which we'd like to move to being a VSS permit and things of that nature.

So that would allow a coordinator or somebody like that to be able to verify an object that they received as well or a group of objects or a tree of objects.

So there's some interesting possibilities there, but it sounds like over the next few months.

We should be talking with you guys more and see where I mean right now.

There's the we're mostly using it for you know seed and key backup of using social recovery type techniques and self-sovereign techniques.

Basically what Ledger is doing with theirs except you get to choose who the parties are that are that you're storing keys with and the keys can also be stored on things like NFC cards.

You know and you know colleagues and things of that nature.

So that's the what it's being used for mostly right now.

Also backup of some hardware wallet all the additional data transaction data in the case of like Lightning wallets Lightning channel details and things of that nature can be backed up with the seed.

You know over QRs NFC, you know all securely.

So that's you know, so that's what so I mean, I couldn't follow all what you said, but I think at least for our proposals.

I can say that we believe it's in a state where we hope it's in which we hope it's quite readable already.

So we invite everyone to who's interested to have a look at it and in particular the API is already pretty well defined at least what we currently think the API should be and it seems like this could be an area where for example, you guys could be interested to yeah look at and provide feedback if it matches your use cases or if you want to see something else or how it could be improved.

I mean, I think you know, we are ideal is you know peer-to-peer type of use cases rather than coordinator use cases, but that being said, I mean, you know one of the parties that is currently using it is using GST P or working on GST P to do the initial stages of a multisig coordinator.

So, you know, you have a coordinator that wants to be able to talk with multiple kinds of devices, you know might be a foundation devices QR device.

It might be other devices that can sign and it's a very it's a allows you to do all of the key setup distribution of keys, etc. to be able to do a traditional multisig and that's you know, not a big jump from that to the kind of coordinator that you guys are working on.

So there's definitely a compatibility there.

I saw in from Conduition.

Yeah, so the I definitely encourage you to take a look at the first video of the of the set.

You may not be a video person, but it's been it's been really worked on to try to make and help people understand the you know, the you know, what Gordian envelope is for it is a it's it supports binary.

It supports Seabor which then that means it also allows for self-describing objects, but it's specifically designed for security through the ability to do things like a lesion and be able to make various statements about things where some of those statements can be hidden in different ways whether or not it's encrypted or compressed or alighted and you can still verify the whole object.

And so there's some real you know, it's designed from scratch as a as a format for doing that and then GSTP is on top of that that allows for a lot of these peer-to-peer connections and we're basically if you used any of the wallets that have QR code air gap communication between the the the the wallet UX and a hardware wallet.

There's over a dozen wallets now that use one of our protocols that is you know, feel to do signing over air gaps.

And so this is an extension on top of that.

That help.

Okay.

Thank you much everybody.

Thank you.

And like I said, we're going to be working on the video and hope to have that up at least in a rough form later today and go forward from there and I hope that some of you can make it to the December 4th meeting where you know, we're going to be talking to the developers who are going to be implementing these libraries.

Yep.

Thanks everyone.

Video with chapter marks should be up within four hours or so.

Okay.

Thanks.

Thank you.

Thanks y'all.

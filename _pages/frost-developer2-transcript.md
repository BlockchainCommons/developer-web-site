---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Developers II (2024): Transcript"
hide_description: true
classes:
  - wide
permalink: /frost/developer2s/transcript/
sidebar:
  nav: frost
---

_This is a raw, unedited transcript of the FROST Developer's Meeting on December 4, 2024._

Okay, welcome everyone to our Blockchain Commons Wallet Developer Meeting focused on Frost.

So this is the second one of a series of meetings focused on helping Frost developers begin to plan their activities over the coming year.

So first, what is Blockchain Commons?

Well, we're a community interested in self-sovereign control of digital assets.

We bring together stakeholders to collaboratively develop interoperable infrastructure.

We design decentralized solutions where everyone wins, and we are a neutral, not-for-profit to enable people to control their digital destiny.

I would like to thank our Frost sponsor from 2024, Human Rights Foundation, who sponsored our events this year to do this.

Again, our agenda today is really a focus on Frost for developers.

If you go to our website, developers, blockchaincommons.com/frost, you will also see videos among Frost's library implementers and cryptographers and things of that nature.

So we kind of alternate between these two different styles of audience.

I'm going to first begin with an intro to Frost and Diego, if you're all in queue, we'll do you first.

And then the Zcash Frost unifies SDK and then talk about Frost federations, and then we'll be open for Q&A.

So I want to begin with an intro on Frost for those who are less familiar with it.

Also, I think there's some details that people inconsistently talk about and want to kind of raise those to the surface.

And then why should you use Frost and how can you use Frost?

So I will start that in just one second.

Where did that window go?

There we go.

Okay, so who am I?

I've been involved with the cryptographic industry for a really long time.

I was just talking to somebody yesterday in regards to chow me in cryptography back in the early 90s with DigiCache and I was the co-editor of the TLS standard.

I've also been heavily involved with digital identity.

In the present, I am been host of the Rebooting Web of Trust design workshops.

I'm involved with very far more credentials and decentralized identifiers.

I'm the architect of the Gordian standards for DC-BOR and Gordian envelope.

And then obviously I'm hosting these meetings.

And again, thanks to our 2024 Frost sponsor, Human Rights Foundation.

So what is Frost?

So Frost means flexible, round, optimized, snore thresholds.

I think the key thing to understand is that it really is all about snore and it allows for quorums for thresholds.

So it is a snore itself is a digital signature format.

It's built on a simpler approach, I think a much more elegant approach than ECDSA is.

And one of the cool things about snore is its ability to aggregate and split keys.

The keys are short, they're scalable, etc.

So we're all building on top of snore infrastructure.

In particular, when it comes to Bitcoin, this is the taproot family of transactions.

Snore is not applicable to the older legacy signature formats.

Thresholds is that M of N people can sign, where M is less than N.

And this is in contrast to MUSIG that in its basic form can't do thresholds.

You can actually do thresholds by various constructions of MUSIG too, which we've talked about in other meetings on MUSIG too.

But FROST is inherently a quorum signature.

You cannot distinguish a quorum signature from an individual signature.

They look exactly the same.

So how does it work?

The FROST signing I consider to be relatively easy and well-defined.

The user has a secret share, which we're going to talk about in a minute, of a signing key.

It's generated with one of the Shamir variants of verifiable secret sharing, a threshold, a participant's sign.

The public verification share verifies the participant's partial signature.

This is a little bit different than say MUSIG 2, where you're actually using a random value to make commitments for the knots.

And then together, the joint public key verifies the aggregated signatures, and you've now signed something.

But I think that the key challenge here is really the first stage is how in heck do you get that secret share?

So if you think about classic private keys, obviously you can use Shamir, SSKR, a variety of different formats to split it into secret shares.

But of course, the challenge is that before you distribute these shares and when you bring them back, there is a risk because they're all combined intact to become a vulnerability.

So that's one of the things that we try to do.

But we also need a public key.

We need the public key to verify these aggregated signatures.

And the group verifying key is public, is intact, is shared.

But we have to generate the keys.

So there are really two ways to generate keys.

There's trusted dealer generation, sometimes called TDG, but often just trusted dealer.

And this is sort of the old style of things.

If you think about it in the terms of SSKR or Shamir, you have a trusted party that basically splits up the keys and sends them to all the parties.

And in many ways, it's a great transitional technology as we move toward the distributed key generation, which is the next approach.

So with the little bit again, trusted dealer generation, we're taking a key, we're splitting it.

We have to trust the dealer that does so because the key fully exists in memory.

But an advantage of it is that a fairly traditional method and we understand how to protect these to at least the best practices of today.

But with distributed key generation, DKG, it's a multi-party protocol to create the key.

And in fact, the private key is never in a memory of a single machine.

A challenge as we will be talking about actually implementing this stuff, there are a variety of ways to implement the distributed key generation.

For instance, you'll note that although there is now an RFC for Frost signing in RFC 9591, it does not define how to create the DKG.

So I think the ped-pop version of the protocol is the most common, which is basically Pedersen with the proof of possession.

However, there are other variants.

Chill, DKG uses a simple ped-pop plus an encrypted ped-pop.

Luke Parkers uses a new one round DKG.

And there are other ones emerging such as Storm and other stuff.

So I think this is probably one of the biggest challenges as wallet developers where this distributed key generation technology has got some variety here and there's some choices you need to make.

And that's kind of why I do mention the trusted dealer generation because this at least allows you to begin to do things with Frost without necessarily having to make a commitment to one of these variety of approaches, some of which are very specific to a particular form of cryptography, etc.

So that's why I wanted to basically make it clear.

And once you've got these, now you can do the signing protocol where I've made a secret, it won't change, I'm proving my participation, I'm signing and we have the final signature and verification, which is the whole point of all of this.

So why would you want to use Frost?

One of the important things, of course, is the key is split.

That means the risks of one single point of failure can be radically reduced.

So if we are successful as a community to have good standards for how we do Frost interoperability wise, we can even avoid implementation failures because we can actually, well, single implementation failures because you can actually have a heterogeneity of implementations such that it's much harder to attack something that is written in C, in Rust, and some other approach.

So I think that offers some real security.

But also important, especially for some of the use cases that we'll be talking about later today, because Frost signatures are aggregated into that single signature, that means all signatures are the same size, no matter how many people sign, which allows you to do really amazing things like 1 to 66 of 100 threshold, no problem.

And of course, even simple multisigs will be smaller.

And that, of course, means lower fees on Bitcoin and other blockchains.

Frost is private in the sense that since all signatures look the same, there is no difference between a single signature and a multisig of 100 signatures.

And you really can't even tell which members of a group signed a threshold unless they reveal a lot of secret info.

And that's really a powerful privacy component.

Frost also can be flexible.

There are implementations now where we can change the groups and thresholds without changing the public keys.

That means you started off with a 3 of 5, two of the people or two of the devices you had those shares on disappear or no longer part.

The remaining critical threshold of the 3 of 5 can be, oh, wait a second.

We need to start over.

Let's do this as a 4 of 9.

Basically restore a lost share or change the shares and roll a new member, remove a member, and change the threshold.

This actually is one of the areas where there's a lot of lack of standardization on this in the sense that some of the libraries that you use, can use today and start implementing with don't allow you to do all of these functions.

This is one of the big things you should be aware of as being a challenge in the coming year.

However, there are other things.

Frost itself is not robust.

What does that mean robust?

That means a misbehaving participant can denial of service as signature.

Basically they can pretend to send you something that's a valid part of the protocol.

They are a quorum member.

Then when you receive it, it corrupts the signature.

Because of those privacy things we were just talking about, it makes it harder to identify the misbehaving or non-participating member.

There does exist something called ROAST, which offers a wrapper to make frost more robust.

There are other approaches to robustness with frost-dried protocols that are emerging.

I just want to make sure you understand.

It's one thing when it's a 3 of 5 and maybe it's a social key recovery or a multi-device key recovery or key signing operation, I should say.

That is a very different potential for adversaries than you have 66 of 100 people and you don't know what one person has corrupted your effort.

We'll conclude this.

How do I use frost?

That really is the topic of today's meeting.

There are existing frost libraries that you can use.

Stack is a production wallet that you can use as an example that already offers frost.

Space wallet Frost Snap and others have announced or have prototypes of this.

We really want you to be part of the coming of frost in 2025.

There are a number of major libraries at this point.

The Zcash Foundation has funded a number of libraries around frost in a number of curves, including SecP.

I believe that the latest version 2.0 has fixed some compatibilities with BIP 340 signatures that are part of what Bitcoin uses with Taproot.

In addition to that, we have the Unify SDK, which is available in Golang, Kotlin, and Vue, which is using the underlying ZF Frost library from Zcash Foundation.

It basically is allowing you to use it in these other languages.

SeriDEX is another one also written in Rust.

It has a unique DKG and it has a Bitcoin Seri focused on Bitcoin signatures so it doesn't offer other curves.

There is a BIP 340 frost implementation for SecP256K1, ZKP, which supports aggregated signatures and things of that nature.

I mean, not aggregated adapter signatures, but then there's the Chill TKG, which doesn't.

I think there's some decisions that you have to make if you want to go with what C libraries you want to use to support.

And then there's a number of others, which I haven't really...

Please go ahead.

You had a comment?

I'm sorry.

I just coughed and I didn't realize I wasn't muted.

Okay, no problem.

So anyhow, I wanted to give everybody an overview of where we're at today, give you some of the highlights of some of the questions and decisions you'll need to make in the coming year as you make your implementation choices.

And then now we're going to go and have three presentations from people who are actually adding these.

Does anybody have any immediate questions for me before we move on to our presentations?

Okay.

So our first presentation is going to be about Stack Wallet, which is in production at stackwallet.com.

Were you able to get your version up to be able to share it?

Yeah, I believe so.

Just a second.

Clicking share.

There we go.

You guys see everything?

Sure do.

Thank you.

Okay.

Hello, everybody.

My name is Diego Salazar.

I'm going to be presenting on Frost for the Masses.

Let me give a brief introduction to myself here.

I own and lead cypherstack.com, which is a -- sorry, the word escapes me at the moment.

We basically do all sorts of stuff.

We do DevOps, infrastructure, research, cryptographic research, development, design.

I myself am a UI UX designer, as is my wife.

We do a lot of work with Monero, and other leading privacy coins.

But some time ago, we decided to throw our hat in the ring of free open source software wallets.

There wasn't a lot of options out there for primarily like multi coin, stuff that supports privacy type stuff like Monero.

There are stuff out there like Exodus, but they're all closed source or open core, meaning only the core parts of that are open source.

So we decided to make Stack Wallet.

And it's going pretty good.

We have Stack Wallet, which supports a number of coins.

And then we have Stack Duo, which supports just Bitcoin and Monero.

So if you're a bit of a snobby elitist, you can go ahead and try that as well.

You may prefer Stack Uno with just Bitcoin.

That's currently not in the cards right now, but that is where we are.

So that's kind of a very, very brief intro of me.

I've also got one of my developers here, Julian.

He was the one that helped, that not helped.

He was the one that did a primary implementation into Stack Wallet.

Our implementation of Frost is currently live.

You can use it.

You can do stuff with it.

It is for all platforms, Android, iOS, Windows, Mac OS, and Linux.

And it does do the DKG model where you generate things separately.

So you don't have one person.

You don't have any trust because we try to be very privacy focused, trust minimized on all aspects of our wallet.

So that's the way that we do things.

We do use Luke Parker's Sarai DKG library for that.

We worked with him to claim the HRF bounty, HRF being one of the sponsors of here, as Christopher was saying.

They had a bounty program where the first person to get frost in their wallet in such a way that you can modify participants, add people, take people away, and stuff works.

And all of that is in Stack Wallet right now.

There is a bit of a bug in Android at the moment where you cannot actually generate a stack or a frosting.

One thing we toggled on Manero affected that and we didn't quite catch it.

But I am literally making builds right before this.

And right now we have the fix that will go live in a couple of days.

But if you have any of the other desktop things or iOS, it will work.

And Android is expected to be working within the next day or two.

So my apologies on that.

As I mentioned before, very briefly, I am a UI/UX guy.

I've been doing this for five, six years.

My wife for even slightly longer than that.

And if I may toot my own horn here, we are some of the best in the biz, I believe.

And we do really, really excellent work.

If you don't believe me, download Stack Wallet and try it yourself.

Also on this presentation is going to be some very fun -- we have different themes in Stack Wallet.

And we have custom illustrations for each of these themes.

Themes like forest green, themes like ocean blue.

And so I have attached several of the fun Bitcoin frost-specific illustrations.

So you can enjoy those as well, hopefully.

Okay.

What is the UX of crypto and frost?

In one word, it's bad.

The user experience is bad.

The user experience of crypto as a whole is bad for cryptocurrency.

We can start there very briefly.

People are not used to backing up keys, seed phrases, any of this kind of stuff.

They're prone to getting scammed and putting it in websites that they shouldn't be putting it in.

They're prone to losing their seed and saying, "Who can help me get my money back?"

And the answer is no one.

They're prone to misplacing it or not writing it down in the first place or yada, yada, yada, yada.

So cryptocurrency itself has a bunch of issues.

I know that's not why we're here today, but the point is that when you build a system on top of a bad UX system, the UX gets -- I wouldn't say exponentially worse, but it definitely builds on itself.

The badness builds on top of the badness.

And that is, in my opinion, having done the work to design frost in a way that is as user-friendly as possible for the most trustless method being DKG, that is going to be the biggest issue with frost adoption that we face, period.

Because the features can be as good as they can possibly be.

They can be the best in the business.

But if it's so difficult to use, nobody's going to use it.

Multi-Sig in general is also kind of really frustrating.

This is not even frost multi-sig type stuff, which is what is implemented in Stack Wallet.

But Multi-Sig in general, because the more participants you have -- so for those of you that may not know, if you have five participants, then participant one has to scan things or, you know, get an alphanumeric string to put it in from participant two, from participant three, from participant four, and from participant five.

And participant two must do the same thing from participant one, participant three, participant four, and participant five.

So it really doesn't scale, because the more people you have, if you have 15 participants, then participant one needs to get all this stuff or scan the QR's or whatever of participants -- two, three, four, five, six, seven, eight, nine, ten, eleven, twelve, thirteen, fourteen, fifteen -- manually at its worst.

At its worst.

I'm going to be discussing some ways that we could potentially go around this.

But at its worst, you're going to have to kind of like scan QR codes or exchange alphanumeric strings with the amount of other people in your signing scheme.

And this -- I mean, the good news is that, like, once you do this once, it's like set up, except not really, because when you have to send, yes, you only need thresholds.

So if it's, you know, a five of ten, then you only need five.

But you know, we were talking about 66 out of 100.

If your threshold is 66, you got to do that with 65 other people.

And it's really frustrating.

And that is the most trust minimized version of this, I understand.

Like I said, there are kind of solutions I'll be talking about a little bit later in this presentation.

But I like to look at what is the most private, most trust minimized version of this that exists.

The user experience is not great.

Not saying that it can't be solved.

In fact, that's I think what a lot of the research should be looking into.

And Christopher had alluded to roasts in regards to robustness, which I'm really interested in looking into, because in Stack Wallet, Christopher was talking about robustness.

Basically, if somebody starts inputting bad data, it kind of corrupts the entire process.

This means that if you're trying to do a send transaction and you have a, let's just say a four out of nine threshold scheme, so you need four participants.

If somebody, the more likely scenario actually is not that some bad actor is trying to deliberately mess things up.

But a user screws up a part of this process because it is a little bit of a confusing process.

And once again, you could go to Stack Wallet and try this for yourself.

Just make it two of three and go through this.

We've made it as simple as we possibly can.

It's still really difficult.

And so if a user messes this up, you can't really go back a step.

There's no way to go back a step in this kind of four or five step process, because there's two rounds of key exchange, meaning everything that I just said of participant one having to do scan QRs or get alphanumeric strings from participants two, three, four, five, et cetera, you have to do that twice.

Not just once to do a send or to set up a wallet or whatever.

And if somebody screws up the process, then the non-robustness means that you have to start the process from the beginning, regardless of where the error occurred.

Everybody does.

You can't just go, okay, let's everyone go back to step three and make sure we're all on the same page here and we all gave each other the correct strength.

That's not possible.

The non-robustness means you have to start from the beginning, which is very, very frustrating.

And again, the more likely scenario is not a bad actor, but some user, which the chances of this increases exponentially, the more people are a part of your threshold.

Because you know, human error, it happens.

You have to start the whole thing from the beginning.

So every decision, the other frustrating thing is every decision to make UX easier that exists right now often has decentralization or trust trade-offs.

And Christopher alluded to one of those where, you know, instead of doing DKG, you have a central person create the keys and then split them up to everybody.

Well, that requires a lot of trust in that singular person from the beginning, because if they save that key, they could do whatever they want with all the money.

They kind of have a one-of-one basically, in addition to the three or four or whatever you have set up.

So there's trust and there's ways to try to minimize that trust, but everything...

And this is true with cryptocurrency.

In general, heck, this is true of life in general, right?

The easier we make things for everyone, oftentimes the less private, the more invasive, the less decentralized it is, just because it's easier to have one person be able to click a couple keystrokes and make something than 15 people, especially when if only one person messes up, everything falls apart and you have to start again.

But it's a worthwhile effort.

It really is because decentralization is very important in many respects.

And privacy is very important in many respects.

I think all of us agree to that, otherwise we wouldn't be here.

So let me get into some particular difficulties.

The two-round handshake.

Like I had alluded to before, you need to do two rounds of everyone sharing with everyone in order to make the wallet or send funds.

To make a wallet, you have to do it with everyone that's going to be a part of the wallet, not just the threshold people.

So if you have 15, 16, 17 participants, you need to do a round of sharing with all of those participants and then a second round of sharing with all of those participants.

Any time there is interactivity on the part of the user, especially one that requires a lot of inputs, there is a high, high likelihood of error.

We see errors with users trying to just make two of threes, right?

So imagine anything above that and it just gets much harder and much harder.

And like Christopher said, you got to find out, okay, who is the one that actually messed it up?

Not even maliciously, who's the one that accidentally messed this up?

Because then we have to talk with them and make sure they're doing the right thing.

And okay, actually, yeah, you pasted them in the wrong thing here.

Here's the right thing.

There you go.

And if you don't know who's the one that messed up, you got to figure that out.

And a user's not going to go, oh, maybe I think I'm the one that messed up.

They're like, I don't know.

I pasted everything in there.

Everything looked filled.

So that can be a little bit difficult.

In UX, there's kind of an adage that every step you have a user take, you will lose users, either because users will give up as they go, you know what, this is too confusing.

I was hoping this would be a one and done, click one thing and it works type thing, but it's not.

I have to click through four or five different things and I feel lost on step three.

I'll come back to this later.

You never see them again.

Or you lose the users because of error, right?

I have to paste in or scan QRs for three, four, five different people.

And I start feeling fatigued.

My eyes start glazing over.

I scan the wrong person for one of the inputs.

And now all of a sudden, everyone's got to start all over and everything's all frustrating.

And you know what, maybe we'll try frost in another couple of years when it's better and you don't see them ever again.

So that is one of the primary difficulties.

I am absolutely interested in any sort of thing that can bring this down to a one round handshake, which is still not great, but it's better than two.

So if those exist, let's get those out ASAP.

In fact, I think if Frost wants to actually have a successful upcoming year, that should be one of the primary focuses.

All these user experience things, they do add up.

So we want to get those solved as soon as possible.

Okay.

Secondly, accurate messaging.

This is also kind of the case with cryptocurrency in general.

This is also the kind of the case with multi-sig in general.

There's a lot of vocabulary here that the general public, general businesses, heck, even a lot of general cryptocurrency who I would consider well-versed in cryptocurrency, regular transaction type things, sending regular Bitcoin transactions, that they don't have.

Thresholds, what's a threshold?

A regular person who even uses Bitcoin consistently, just not any multi-sig schemes, just uses it to send money to their parents abroad or whatever.

They have no idea what a threshold is.

Signing means something slightly different.

It means the same thing, but your signature is not complete if it's just you.

You got to get signatures from all sorts of other people.

So explaining all of that, either in a common, easy to understand, intuitive UX language, like icons or whatever, is really difficult.

And when you start moving into words, "Oh man, okay.

Now the word threshold needs that little eye icon right next to it so the user can tap it.

The pop-up will show up.

It's like, what is a threshold?

And it'll explain it in hopefully less than two or three sentences.

Otherwise they'll go, "Ah, I'm actually not going to read this."

In fact, even if it's a single sentence, they're probably going to go, "I'm actually not going to read this."

So there's a lot of vocabulary issues here.

Again, a lot of this is shared with multi-sig in general and with cryptocurrency in general.

So it's not something that the Frost people need to solve in some sense, but the better we can make this.

And the more common we make the visual language, when we're talking about other wallets implementing something like Frost, it'd be really good for everyone to get on the same page.

Because if we use similar or the same icons for stack as, I forget one of the other wallets that was looking to implement Space Wallet or something, then if a user actually takes the time to learn how to do it in stack, they can go to another wallet and have a similar experience so they don't have to relearn everything from the ground up.

And the worst thing that can happen is everyone uses their own visual language for this.

Because then not only do you have to learn a very complicated process once, you have to learn a very complicated process multiple times.

And it gets really frustrating.

So it's really great that we're having these kinds of meetings so everyone can talk about these things.

We can maybe start establishing something like a common visual language for Frost and things can start rolling from there and we can make it just that much bit easier.

And once again, every little bit of making things easier counts just as every little bit of making things harder loses us exponentially more people.

The other issue in accurate messaging, and some of you may or may not know this depending on how well-versed you are in Frost and its intricacies.

Yes, you can reshare, meaning you can kick some people off, bring some people on, etc.

Just completely do the entire scheme.

But that doesn't invalidate old share schemes.

So if myself, Christopher, and Julian have a two of three and we decide that we no longer want Julian on there.

We're a business, we're kicking him off the board, whatever.

So Christopher and I, because we have threshold with the two of us, decide to kick Julian off and add Pakku.

We now have a new two of three scheme.

But the old two of three scheme with me, Christopher, and Julian is still correct.

It is still active.

It's not invalidated.

Meaning if I was to collude with Julian behind Christopher's back, we could actually see or not see, spend all of that money.

Because the blockchain itself doesn't know, it has no way of knowing which scheme is active and which scheme is not.

Secondly, resharing doesn't invalidate kicked off wallets from seeing full and continued history of that wallet.

So Julian, in that sense, would still be able to take a look.

Even though he's kicked off and without any, without collusion, he can't spend, he can still see inside the wallet because he has valid keys for that, for seeing.

And man, that's a messaging we got to get right.

Because if you've got nonprofits or businesses or whatever, and they switch to Frost and they start kicking people off and bringing people on based on CEOs being fired, board members switching out or whatever, they could still be leaking secrets and not knowing or potentially have their wallets strained if there's collusion and not know or not be able to see who did this.

This is potentially very dangerous and the messaging must be gotten right here.

Otherwise, yeah, people really don't know because they got to make the informed decision.

Do we just reshare or do we just make a new scheme?

And some of you might say, well, if you just have to make a new scheme so that way, old participants can't see in the wallet why use Frost at all?

Isn't it basically just old multi-sig at that point?

Well, no, because like Christopher said, there's privacy benefits to it.

It looks the same on the Bitcoin blockchain as other transactions.

So there's privacy elements to it.

So there's reasons to use Frost as opposed to other multi-sig schemes.

But it doesn't solve anything and everything with this resharing.

There are definite drawbacks that come as a result.

Okay, solutions.

What are some of the solutions here?

You can see our little Bitcoin Chan here.

We have a Chan's theme where we give every coin kind of a little anime Chan girl.

It was very requested and a lot of people love it in the wallets.

One of the things that people compliment on a lot.

This is Bitcoin Chan.

I didn't make her.

There was a Bitcoin Chan before us and we just put her in a parka.

So yeah, good.

Good.

We can all see that.

Solutions.

Grin and Epic Cash are MimbleWimble based coins.

Those of you who don't know much about MimbleWimble, it is a coin scheme that allows cut through.

If person A sends to person B sends to person C, we can cut out person B from the blockchain because the end result is person A money through person B does end up with person C.

And in that way, they are able to reduce bloat on the blockchain by cutting out a whole bunch of different intermediary transactions and in theory have some element of privacy because person B is no longer a part of that chain.

It's a little bit more complicated than that, but that's not why we're here.

The reason why I'm talking about this at all is because they don't have transactions like Bitcoin does.

With Bitcoin, all you need is the receiving address of somebody that you're sending to same with Monero and most other coins.

And you can just send and the person receives and it's that simple with a MimbleWimble based coins like Grin or Epic Cash.

They have a sort of three handshake type thing where the sender has to get an alphanumeric string, which is the configuration of the transaction they're trying to send and give that to the receiver out of band.

Either once again, we have QR codes via email, telegram, whatever the case, right?

The receiver has to get that, put it in his wallet, click a button to sign it and they will receive a new alphanumeric string, which they will be giving back to the sender once again out of band somehow.

The sender then gets that, signs it, and that is the quote unquote final transaction that they then upload to the miners for a block inclusion, right?

So it's a three step process.

Sender sends the receiver, receiver signs what they receive and sends it back to the sender who signs and then gives it to the miners.

It's kind of a three handshake process.

So this is actually kind of similar to multi-sick schemes and frost in that people, it's interactive, meaning it's not kind of set it and forget it.

You send it and it's gone.

You need everyone's interactivity on this to get it to work.

And they have several ways that they are trying to solve this.

One of them, which Epic Cache is included in Stack Wallet, by the way, if you wanted to give this a shot, one of them is called Grinbox or Epic Box.

And that is kind of, there is a central server up there in the cloud at, you know, for Stack Wallet, we're the ones that are hosting it that does the coordination of all of this.

So we assign everyone a more or less readable address, similar to a Bitcoin address, because by default, Grin and Epic and things, they don't have addresses in the same way that Bitcoin does along alphanumeric string.

But we kind of, with the server, assigns them an address.

And if both participants have each other's address, then one can send to the other.

And the server kind of does this three handshake process automatically, assuming both participants are online.

And if one participant is offline, it holds the message in the server until they come back online and check their messages, at which point it gives it to them.

The signature happens in the background, under the hood, automatically, and puts it back on there.

So if both participants online, this could be a process that takes literally two seconds, because the coordination happens immediately.

If one of the participants is not online, it's on hold until that participant is online, does the signing, gives it back to the server.

But at least it kicks all of this under the hood and it makes it pretty much non-interactive, the same level of interactivity as Bitcoin.

The receiver gives the sender their Epic Box address.

But obviously, there's a decentralization and privacy trade-off here, because the person who is running the server, even if you encrypt it in a bunch of clever ways and really neat ways and stuff, they at least have the IP addresses.

And yes, you know, workarounds, VPN, Tor, etc., etc., I'm sure you're all aware of such things.

But there is a definite decentralization and privacy trade-off that comes here.

And so, you know, the same could be true with Frost, where we build some sort of coordinator server that makes a lot of these things easier.

In fact, something similar to this will be a necessity when you start going into higher participant counts, because people, like I said, will screw this up.

And if it's just handled automatically under the hood, then that's probably the best way to go about things.

Will there be a big gigantic business that sets itself up saying, "We are a trusted third party.

We are Intel or whatever, and we are hosting said server.

So if your business uses our server, then you know that you're in good hands and we hopefully will not leak all of the information."

Great, you know, that's what we hope.

But something similar will be necessary.

And hopefully it is federated in the sense that if I am signed up to one such server and somebody else is signed up to a different server, those two servers can communicate with each other, similar to email or matrix chat.

Because otherwise, everyone's got to be on the same server.

You got to go into your settings, make sure everyone's on the same Frostbox server type thing.

One way of helping reduce this user experience issue.

The second one is kind of a guided tutorial type thing.

This one is -- this was actually suggested to me by a user from Stack Wallet, which is why it's included.

I think it's a very, very low reward type thing.

I don't see -- it will probably make things a little clearer.

And walking somebody through making their own wallet and sending potentially to themselves and stuff will definitely help if there's kind of a guided tutorial process.

But you got to make sure that everyone has seen it or everyone's on the same step or something.

So there's still kind of out of band issues that result.

Lastly, we can restrict use casing.

And what I mean by that is in a wallet, maybe you only allow two of threes.

So you don't allow them to custom edit their threshold or their whatever or the number of participants because our -- like we could say in Stack Wallet, we use Frost just for the UK use case of you getting a sort of cold wallet out there that is a multi-sig type thing.

And we're just restricting to that particular use case.

We're not looking for businesses and board of directors and stuff that have seven people.

For those people, we'll have to go to other software that is for power users.

So if you restrict your use case to a very particular thing, you can move a lot of the things under the hood or out of the thing entirely.

And that's fewer steps.

It doesn't solve everything, but every fewer step means you lose less people.

So those are a couple of solutions that we have seen and we have looked at in my kind of cryptocurrency foray and travels.

I'm excited to hear about things like Roast.

I'm excited to hear about single round stuff because all of those will absolutely help.

There's definitely a big tech portion that can be done to simplify a lot of things on the technological level, which would then in turn simplify things on the UX level.

And those should absolutely actively be pursued.

And yeah, it's been quite the journey.

We did receive that bounty.

You can go to HRF bounties.org and scroll down to bounty number nine to see that Cypherstack has claimed that bounty for stack wallet.

It was a lot of fun.

It was a lot of frustration.

It was probably one of the biggest challenges of my UX career, but I do enjoy a good challenge.

And I am reasonably proud of what we were able to put out.

Again, there's things I have no control over.

Like some people are like, Hey, I started this and I get this error that says I got to start over.

Like what's going on?

I'm like, messed up somewhere bro.

Sorry, I can't do anything.

Frost is not robust.

There are still things that I wish could be improved, but I'm reasonably proud of stack wallets implementation of it.

So please give it a shot.

Install stack wallet or stack duo.

It is present on both.

And again, with the exception of Android, which the fix should be on the next day or two and give it a go.

Tell us how we did.

And that is the end of my presentation.

Christopher, is there time for me to take questions right now or do you want to take questions?

Let me, we have a number of people who haven't been at a one of our Gordian developer meetings.

So I just wanted to connect a few dots to some previous things.

And then we'll go to Q and A for yours.

Okay.

So we've had a number of presentations and work on something called the Gordian Seal Transaction Protocol.

And I encourage you to look at some of our previous events and discussions regarding it.

And fundamentally, we're trying to solve some of these problems that Diego was just talking about.

And in '99, we did this best practices of smart custody.

And we then evolved it in 2022 with kind of the best practices for doing legacy multisig.

But the process of doing that, we wrote one, it's here, this scenario, which basically used four different wallets.

One of which was a coordinator and didn't hold keys and then three wallets for two of three.

And you can see here, there's this just huge setup problem and UX problems all along and inconsistencies with every different wallet doing things, calling things different.

So you can see here, people just even though this is best practices, people weren't going to do it.

So what we demonstrated in, we have a presentation on the details of this, is just simply having a request response protocol for this that is oriented.

We were able to reduce the decision points from five to two research points where people kind of had to go click on that I button and go, what is a threshold?

To one, human actions from 31 to 14 and automated actions instead of from five, we were able to go to automate 33 of the steps.

And so this is what we're really trying to do.

And this is part of the reason why we've been supporting, why we really focused on Frost.

We're also like looking at music to, he talked about the coordination problem.

This also exists for music to with especially when you have three or three or more.

This is kind of the whole setup and signing for music to which is essentially simpler than Frost is.

So and we're trying to come up with, what do they have in common?

Where are they different?

So for instance, there's the nonsense is not done the same way in Frost and such.

So how do we do this and how do we automate it?

And then one of the key things is we don't know how people are going to transfer these things.

Are they going to do it over the internet?

They can do it over Bluetooth?

Maybe they're using NFC cards.

Maybe they're doing offline things and it's not actually to a service.

So maybe they're going to use Tor or any one of a number of different protocols.

And of course, our probably most popular standard is our QR standards for transmitting data over an animated QR between two devices.

So Gordian Seal Transport works on all of those.

And for instance, one of the things that we've been playing around with, I don't know if people are aware of this, is that BitTorrent has the ability to have a 1K DHT that is massively distributed.

So basically you post it and for about two hours it will be available on the BitTorrent network.

And of course, because of the way envelope, which is what Gordian Seal Transport uses in its format, you can can align things, remove things and whatever to be able to fit within that function, within that capability.

And we also have this thing called encrypted state continuations, which allows for being able to not have to keep state on these servers or be able to federate state and things of that nature.

So I encourage you if some of these challenges that Diego talked about, this is what our goal is for 2025, is to help all of the wallet developers come up with language, come up with support for protocols to make all of these steps easier.

So I just wanted to connect the dots back to what we've been doing.

So we have monthly meetings.

This is our December monthly meeting and we'll be having more next year.

So general questions for Diego on Stack Wallet?

I'm not seeing anybody raising their hand right now.

Okay.

So there will still be an opportunity for questions.

There is a question here in the chat.

Oh, it must be in another window.

Go ahead.

It popped in the chat.

Question about the design of Stack Wallet.

Why do you end up going down the path of having users copy and paste inputs for the different rounds of DKG or signing?

I'd expect the reason the default would have been to have users and machines handle this communication over the internet automatically, perhaps after some initial pairing process.

Because as you've seen, the UX of manual communication is incredibly fault-prone.

Server coordination, like you described with Grinbox, is one way to do that.

But there are other options, too, e.g.

Tor hidden services, Noster, et cetera.

The thing is that there's several reasons.

First of all, I try, like I said, to make at least available the most decentralized private version of something to exist.

I think that is important to have out there in some capacity.

Because then the users can decide how they want to exchange information out of band or whatever.

And I understand Tor has benefits.

But doing things over the internet requires at least one participant to be able to know how to set up a Tor hidden service or something like that.

Because different devices like iOS and Android are capable of different things.

Not all of them are able to just tap a button and all of a sudden be running a Tor hidden service, especially since IP addresses change in regards to if you're on 5G data or something like that.

There are different server methods.

But one, they're inherently more less private, I guess.

And I wanted to get the most private version of this out from the get-go for one.

And for two, not everything is available to every platform.

And I try for StackWallet to keep feature parity across things because I don't necessarily want somebody like, okay, I've set this up for, oh, you're on iOS?

Oh, dang.

Okay.

So actually, that's probably not going to work.

Do you have a Linux machine kind of thing?

Again, we went through all of this with Epic Cache when we put Epic Cache in there and we were trying to figure out alongside them, how can we get this to work on a consistent basis?

And they do have things like, hey, our GUI allows you to press this button and it does a Tor hidden service and then you just give them the Tor address and it works automatically.

Or similarly, if you press this button, it spins up a mini HTTP server that, well, hopefully it's port forwarded.

If it is and another individual can access it, then you can put that in there and boom, it just automatically works.

And some of the exchanges that Epic Cache is supported on, such as Vitex, they do use that HTTP kind of protocol type thing.

And it makes sense for them because one, they're a business and two, they can put a server on all the time and have probably paid DevOps guys to make sure that it's up there and actually working.

And so, with a regular user, even if HTTP was possible from a mobile perspective, spinning up a mini web service, which it kind of isn't in most circumstances, if something goes wrong, the average user has zero understanding of how to actually fix it.

They press the button and they hope it works.

And if there's an error, they're like, well, I don't know what to do from here.

Whereas at least if you get things wrong from a manual perspective with QR stuff, you probably mess something up somewhere and you can actually try again and have a reasonable chance of success.

Whereas unless you are a techie person who understands HTTP or Tor, if you get any sort of error in trying to spin one of those things up, you have no idea how to troubleshoot in any way other than turn it off and on again, and hopefully that works.

And if it doesn't, you're kind of screwed.

So there's a lot of reasons going through this with Epic Cache and stumbling on Epic Box as the most reasonable solution that requires the least amount from the user and the least understanding from the user is the thing that it's the one that made the most sense.

And so looking at Frost when we were, and it was very clear to us very quickly that it was the very same kind of problem as the one that we were working on Epic Cache for.

And yeah, we kind of looked at the different things and we realized, okay, we would have to make kind of a Frostbox type thing.

And for purposes of trying to get this out, for one, and for purposes of trying to make this of the most decentralized and private that could possibly be, we think the out of band stuff is the best thing to do for now.

So I concur you need to be able to support as much variety and style because I think in our scenarios, we've been talking about users, but it may, as far as each signer being a user, but it may not always be that it may be an offline device.

It may be your digital ring and your phone are part of one part of a quorum SSKR, for instance, supports a two layer quorum thing.

So you can have all the hardware things.

But then you have to have one other person that's a real person also approve it.

And so there's these interesting problems of how do I communicate with it?

This is like, for instance, one of the, he was talking about the changing IP address problem.

This is where we thought this was really interesting opportunity of, because basically an iPhone, which basically shuts your processes down, can basically rely on something that'll be available for the next two hours, which is usually sufficient.

You can do all, talk to all the parties, at least in the same time zone in two hours.

And that, so we've been exploring all that.

I would really hope that you'll take a look at some of the Gordian standards and code base to see if you've got suggestions on what are we missing or what's missing, what do we need?

So we should get onto the other presentations.

There is one more question.

If you want to get back to the end, that's totally fine.

It's in the Zoom chat.

Yeah, I see.

Okay.

So Nick Farrow asks, what's the privacy loss of using a coordinator to orchestrate the DKG?

Every party needs to receive everything.

Anyway, using a coordinator means fewer connections, so patties may learn less about each other's IPs, et cetera.

Nick, there is the possibility when you are generating all of this stuff.

This may sound like the biggest edge case ever type thing, like only Edward Snowden would ever consider doing something like this.

But for example, you can get your QR's and never put them on any sort of internet.

You could just print them, right?

You can print the stuff.

Again, this would be the longest, most frustrating and infuriating process to ever exist.

But you could get these QR's or these alphanumeric strings, print them up and exchange them in person kind of thing.

So you actually never touch the internet.

And anything that inherently touches the internet is less private.

That's just the way that it goes.

Because now you've got some sort of metadata about it.

Do I suspect any person in the history in the next 15 years would ever use Stack Wallet in that way?

No, I don't.

But does that mean that nobody will ever require such things?

Like we in the first world have very little to worry about privacy when our privacy is lost.

The worst we face at these days is targeted ads.

But that's not the case in people all over the world living under totalitarian regimes.

And who am I to say that, okay, you know what?

No, this is good enough for you.

Tools, finances, freedom or lives may be at risk.

This is definitely something I take from my Monero days.

There is a huge, huge emphasis on privacy.

And I'm still very active in the Monero community.

And compromising on privacy can have very unintended consequences.

I personally believe that freedom can only exist with adequate privacy.

So such tools need to exist in some capacity.

Okay, thank you.

So I don't actually still quite see the privacy loss though.

Like if you're doing it on paper, you could still do that with a central coordinator.

And instead of having to give those paper QR codes to every participant in the DKG, you could just sort of funnel them through one person.

And that could actually mean sort of better privacy instead of having to learn, you know, those addresses or meet with these people to exchange these QR codes on paper.

You just sort of funnel them through a single party.

And you're trusting that single party, right?

That they're not going to be leaking any of this.

You're trusting in their competency that they will not accidentally leak things, loose things, get hacked.

And you're trusting in their morality and ethics that they will not purposefully do so.

I mean, you're trusting one party instead of n parties.

It seems strictly better to me.

You know, I mean, we should definitely talk about this more.

I think I'm kind of of the opinion that there is gradients of trust and you want to support as broad a gradient as possible.

I mean, like we discovered with this two of three prototype for MUSIG, obviously Alice can be the coordinator and since typically you need somebody to initiate the, "Oh, you know, we want to spend money this way."

You know, there is this inherent coordinator advantage that they can basically, you know, kind of pick and choose what they tell the different parties and such.

So there are even without dealing with correlation risks of the internet, there are still participation and, you know, initiation, trust questions.

And we don't necessarily have to address them all.

But I think we at least need to be aware of them.

And I would definitely encourage more discussion about this.

Yeah, Nick, Nick, happy to hook up with you after and we can discuss.

I'm very open to having my mind changed that maybe I have been overly cautious in this.

And you may be right, there may be fewer privacy implications than I am currently seeing right now.

I'm just looking for an enemy behind every rock and trying to plan for that.

Yeah, happy to meet up with you.

Thanks, Zia.

That's very admirable.

It's awesome.

Thanks.

Okay.

So we want to talk about the Unify SDK.

Basically it is a wrapper for the ZF Frost if you're not doing Rust directly.

You want to give us, can you, you're ready to share?

Yes.

I'll then ignore this warning from Zoom.

Okay.

So let's see if this works.

Because last time I might not.

I saw I did this, didn't work.

Okay.

Yes, we did.

Yes, we did.

Presentation.

Thank you.

So this is the CCASH Frost Unit by SDK.

CCASH prefix is important because it's the mostly the first, the CCASH curve is the mostly supported.

And on Pako, the developer relations engineer for CCASH community grants, I'm a CCG grantee.

You can reach me at Pako@Zekdep.org, which is the GitHub organization I use for this code that actually is funded by the CCASH community through the grant.

So the Frost Unit by SDK is currently more of a TFC that is like slowly growing up.

And its goal is to actually bring the Rust creates a randomized Frost from the CCASH foundation to other languages without porting or rewriting.

Yes, in my experience, I'm the initial author of the CCASH iOS SDK.

And in my experience, the FFI code, which is the code that binds or glues up together, a C library or a C binary of some sort, which is like maybe Rust using C bindings to Swift or to Kotlin, it actually becomes a project on its own and an ugly one.

So Unify is one of the approaches of trying to simplify that and in a way that the glue language, the glue part of connecting the C binary that is generated from, in this case, from Rust into Kotlin or Swift or any other language is just written once.

And it's less of a burden.

So that's kind of what motivated me to start exploring Unify as it's a project from Mozilla and which is kind of a reputable project and maintainer.

The languages available are Swift, Golang, Kotlin, Python.

Currently, the Frost, Unify SDK has Golang and Swift.

Kotlin, I have all the GitHub issues created for that work, but I hadn't been able to actually start with it.

But anybody can chip in if they're interested, super welcome to do so.

And Unify is this multi-language binding generator from Rust.

The expectation is kind of like it's going to be fascinating, awesome, and super useful.

But the reality is, as always, is a little bit more difficult than that.

Mozilla is much more pleasant than actually writing your own FFI code, like worrying about how memory looks in the Rust world and how memory will look like in the other world, that being Python, Swift, or Kotlin, or Java.

So even though it's not like a server bullet or something super easy, it's definitely better than nothing and doing it manually at one time per language that you want to support.

So the architecture is actually like this.

There's the Frost-Rusk rates that will be called from a Frost-Unify Rust-based code that then will be converted to a Swift, Kotlin, and Kotlin API, then the clients can actually connect to.

Or maybe you can have a wrapper lightweight SDK that maybe creates more ergonomic types, which it's also a little bit better if you want to be super compliant with the code styles of each language community.

Like for example, Swift and Kotlin have pretty specific kind of codes or API guidelines.

So unfortunately, the bindings won't be pretty with those high standards.

But you can wrap them up and have a decent, almost native API.

The companion application, there's a use case that this project is aiming to study that it's a companion app or similar, pretty similar to, or could be a section of the Gordian, for example, the Gordian C2 that could support this.

And it's just an application that it's teamed up with a wallet that can support a viewing key, for example, for the C cache case and help the user to deal with Frost things.

But it's very important to take into consideration what Diego has very vehemently remarked about, choosing your battles and scoping your use cases.

So then that's the introduction to the project.

And so we can start with a walkthrough.

I need to change my screen.

Let me see if I can share another screen just from here.

Let's try this out.

First time I do it, do you see Visual Studio code?

Do you?

Yep, can see it.

Okay.

So there is a, on the project, there is a readme that explains most of it.

It also refers to a lot of resources, like learning resources.

Frost has a learning curve about learning about Frost on its own and then learning about the curve you're going to be applying.

In this case, a Frost demo from the C cache foundation covers a lot of that material.

So that's referenced here.

So to get a grasp on Frost Unify, you should do the reading first.

Sorry, if you are a hands-on person, I actually recommend that.

You're reading first to make your life easier.

Then there are a bunch of building for requisites.

There are all explained here.

And then how you build the Frost curves available or how to build the Swift ones and the features.

So then here in the, there is the script section.

I try to, I always say that I will be much more lazy if I was not that lazy of learning Bash to be better at scripting.

I try to do a lot of scripts to actually help building CI on GitHub to do stuff for me like testing very fine builds, releasing code.

So there's a lot of scripts here that help you do stuff, like the basic.

For example, if you want to go and build, if you want to build the Go lang flavor of this, then you just go to buildgo.sh and you will see the steps here or you can just run the script and then build the Go bindings and then you can test the bindings here.

And you'll be, this will be like easier to grasp.

All of these scripts are actually used up here and the GitHub workflows that are available on the repo.

So the idea is that when you, if you want to jump on this code and we work together and collaborate, we have a lot of things that machines will do for us, which is like the spirit of this project.

Then the unify SDK is actually this module and the structure of the project tries and attempts to be as a monorepo as possible.

One of the issues with this kind of translators or FFI binding platforms is that distributing every platform or target language has to reconcile the different ways the languages work and with their dependency tools.

And for example, if we just abide by Kotlin and Swift, they are like super different worlds.

In Swift, you will have Swift package manager and this is just like putting up your code on a GitHub repo using this swifty package.swift that has the stuff that you need to know about the package and that's all.

But if you were going to do this and something is very similar with Go, you just have the mod file and specify what is the module and then there's a lot of convention over configuration of stuff that will let Go line devs or end goal line tools with dependency management to resolve to this very same repo and the tooling will know where to get the code from.

And when you go to Kotlin, then there's a different world because it's Java world and Java world has a past respect and all.

But it has a very enterpricy vibe and then you have Maven Central, which is a centralized repo where you put your built artifacts that are sort of binaries if you haven't heard of it.

So you have to put your code there to be distributed.

Otherwise, it's mostly a very, very cumbersome to actually get code from repos on Java or Kotlin projects.

So this repo architecture can seem a little bit convoluted at times, but when I started to do my math about how many repos would I need to actually deploy every language, I opted to say, hey, let's try to do a mod repo with all its goods and bads, but keep everything in one place and we will be more aware of the changes and it will be easier to work on, at least code wise.

So the Frost UniFi SDK is the Rust module, which has some sub modules that are about having the main Frost features like the trusted dealer module that will have the public API that will be using the trusted dealer methods and APIs to do that kind of key dealership.

And these functions are actually calling the a cypher suite of the Frostcore and then the binding code actually will expose them to the public kind of code.

For example, if I want to see how this looks in Swift, I will go to Frost Swift and this is the Swift package that has the sources and the tests.

What I ended up doing is that since the FFI generated code is kind of ugly, I did a predified wrapper on it.

So I have two modules here, which is the Frost Swift 3D code with all the linting and code conventions and all.

And then you have the Frost FFI, which is the generated code, which is it really obviously looked like machine generated code.

So nobody wants to deal with this kind of code.

And it actually I think the code, the comments built up on top of the file says, hey, do not touch it.

Trust me, you don't want to mess with it kind of thing.

So that's not like the messaging that we want to read when we're about to code something.

So I said, hey, let's try to do a nicer kind of thing.

And I did the same thing with the goal line.

And then there are a bunch of tests.

Then the tests, when I approach a code base, what I like to do is to go to tests first, because by reading the tests, you learn about code assumptions and what it is expected to happen and what it is not expected to happen.

So reading these tests help you out, understand things about how are things done in the Frost FFI world.

If you look at the tests, you will see redundant testing that these tests you see here are also replicated in Rust.

So you can have like an end to end kind of CI job that will be able to run and build every language.

And then you can kind of test that everything is working as expected in every layer of the Frost unify.

There's not much else to it.

And I invite you to start tinkering with it.

And just if you want to, if you feel like chipping some code, look at the issues of the repo and just jump in.

And that's about it.

The idea is to just do a small walkthrough and take it from there.

I hope that this is useful for anyone out there just wanting to build like a Frost FFI native app, like a native Kotlin or native Swift app or could be a Java app or a Go Align server, you name it.

And that is useful for you.

And I'm glad to collaborate with anyone out there.

So thank you.

I stopped sharing.

Thank you very much.

Thank you.

For your time.

I think I had just last one last light with the last thing here or not.

Yeah.

Thank you.

Controversions are welcome.

We get a repo and this is my zet.org email where you can reach out.

Thank you again.

Thank you very much.

So we had a discussion about a wallet actually using Frost for Bitcoin transactions and other chains.

We've had a library to help you develop it.

But I wanted to also demonstrate that Frost is very useful for even more things as a developer.

So this is Frost Federation.

Karpreet, you ready to?

Yep, I'm here.

Take over.

Excellent.

So I'm just going to share my screen here.

All right.

So everybody sees my screen, I hope.

Yes we do.

Yeah, sorry.

I was just figuring how to start the presentation mode.

Thanks a lot for this opportunity to talk about the work that we've been doing at Braid Pool.

I wanted to just report on the progress that we've made since the last time we talked about our intentions to use Frost Federation for managing a few things in the mining pool.

So my name is Karpreet and I've been working on Braid Pool for almost all this year.

And one of the things that we want to do is to run a federation to be able to generate, to be able to manage payouts for miners who are mining on this decentralized mining pool.

So what I'm going to go through today is what and why of Frost Federation.

I'm just going to spend like a slide or two on that, then quickly go over the progress that we've made since the last call.

And then we're going to talk about some of the challenges that are still ahead of us and what are our immediate next steps.

So just to kind of communicate the work that we've been doing.

So Frost is highly motivated, as we all know, for hardware or offline wallets, right, where people want to be able to distribute some of the trust to manage their assets or whatever.

However, there is another use case like Braid Pool where we want to kind of have a federation of entities who are managing the operation of the pool, right?

But we don't want to centralize the trust.

We essentially want to remove the centralized pool operator with a federation.

So this use case becomes slightly different because we are always online and we want to stay live.

So live-ness is very important to us.

We want to be able to keep making progress, even if one party is misbehaving or is just having a production hiccup.

And we don't want to stop and then get people on the phone and figure things out who is the lying, cheating person and then the rest of the party continues.

Instead we just want to kind of ignore such attacks and be able to keep moving forward.

So one thing important to note here is that this federation, because it is kind of managing some points, funds of some of the miners, the membership of the federation is decided by proof of work so that it's not that you just join an online network and you become part of a group who's managing a bunch of Bitcoin.

That you need to kind of get a kind of delegation from the miners who are sending proof of work through you to be able to say, "Okay, I'm going to be part of the party," or "I can be a part of the threshold signature party."

Now there are different models in how we do this and we are still playing with a few ideas around.

Hopefully we land on a model that works.

So then jumping on quickly to what is the progress that we made.

So first thing I wanted to just say thanks to all the, we are using Zcash's frost implementation and the documents were really clear.

So a big thanks to people working on that team.

The DKG example in the documents, first I saw that I was like, "Okay, this is not going to work when I try it," but it actually did.

I just replaced their hash maps with network calls and no hiccups from there and all the hiccups were in my network code.

Surprise, surprise.

Also I thought that the caveats were sufficiently highlighted.

So people been trying to use that library are aware of the potential pitfalls.

So thanks again for that.

So what we have done up to now is we have implemented the KGen algorithm or protocol described in the frost paper.

However, there was an issue highlighted with it on both the frost and I think in the chill DKG repos and the kind of, I kind of proposed using a BFT broadcast, a Braxha's broadcast to kind of circumvent that issue.

The issue was that we needed consensus.

We needed some way for all parties to know that none of the parties is equivocating as in telling a bunch of parties X and telling the other parties Y.

So instead if we have a BFT reliable broadcast, then all parties know that all others parties know that everybody has the same data.

So we can optimize this later, but for the moment we're using the simple Braxha's protocol because it's easy to get your head around and easy to implement.

With that, we have the key actually working and I'm very happy about that.

You can find the progress here on frost federation on GitHub.

I wanted to highlight this image because I'm a bit of a stickler for code coverage and we've got, in fact, yesterday it went up to 94 because I wrote some more integration tests.

It doesn't mean the code is solid.

There's a lot of holes, I'm sure.

We are using the network stack is built using Tokyo and tower services in Rust.

So everything is event driven.

Hopefully this will scale up as we try to grow the size of the party.

I was hoping to have some performance numbers here, but I didn't manage to get there for the moment.

I've only been able to run it with, I didn't try actually with more than three parties just to kind of make sure that everything was working for the moment.

So currently we only handle network failures.

So if a party does not send an echo or an AC for the broadcast protocol, then we say, okay, this party is failed and we terminate the session, et cetera, et cetera.

However, we are not using the culprits exception from the library, which helps us identify which parties have misbehaved in round one or round two of the DPG protocol so that we can remove those parties and proceed if we want to, if the threshold numbers still make sense.

And so that we have not done that yet.

So that's really one of the next steps.

Also on progress and signing, we haven't really gotten there.

We just wanted to stay focused on DPG, especially since it's a more demanding protocol from the networking side of things.

Signing seems quote unquote simpler as compared to DPG.

So if you've got the DPG working with our network stack, signing should be easier.

Famous last words.

Challenges ahead.

Okay.

So since we want an always online federation, robustness is important for us.

And the roast paper first we thought maybe we don't want to do a roast because it's going to be too heavy, et cetera.

But reading carefully, of course, they have suggested some optimization schemes at the end of the paper.

And I think one of them might be applicable to us.

So that is a big challenge ahead of us to kind of figure out which one and then describe it and then maybe get some feedback and then implement it.

So that is a big road ahead long still for us to get the frost regulation really working as well as we want.

I do want to add an aside here because there is a new scheme by Lindl from who's heading the cryptography at Coinbase.

And they have a snort scheme which does DPG and signing in three rounds, which is not good for the wallet situation.

I totally get that.

But for an online federation, this sounds pretty neat in the sense that even if it is, if you need to do one threshold signature every day or something, we can do three rounds.

That's not a problem as long as we do get robustness.

So that's an interesting trade off there.

As far as I understand, they are using it in production at Coinbase, which is awesome.

However, there is only plans to make it open source.

So who knows when that is coming.

We hope it's faster.

I did think about it for a day or two in wild exuberance as a means to try to implement it.

But then better sense prevailed and started thinking let's instead just use the rost optimization.

But just wanted to throw this aside that this scheme is pretty interesting for online services.

In accepts make keygen robust and pick and choose and implement and understand one of the rost optimizations.

The robustness of keygen is not that probably hard as the rost optimizations that we need to describe and pick.

That's it.

I kept it short.

Any questions?

Thank you very much, Gompreet.

Any questions?

Get that chat window which disappeared again.

There we go.

Okay.

So we've shared around the email addresses.

I do encourage you to join the Gordian's signal channel if you want to ask specific questions of the different participants or general questions about what is the future here.

I'd like to close the meeting if what will it take for you to implement frost in 2025?

What did you hear that excited you or concerned you today?

Anybody?

Anybody change their mind about what their frost priorities are in the coming next couple of quarters?

I have some thoughts and some questions.

I'm working on a library called SecP256K1JDK which wraps the main Bitcoin SecP256K1C library with a modern Java API that can be used on the JVM in Java, Kotlin, Groovy, all the Scala, etc.

It's currently in relatively early development.

In order for it to support frost, generally we're trying to follow the Bitcoin SecP256K1 library.

When is frost expected to end in there, if ever?

Well, I think it will.

Last time we had four presentations and went over 90 minutes and this time we cut it down to three.

We hope to have on our next meeting the chill DKG people again and then the BitKey people who are implementing frost as well can talk more about the work that they're doing.

I believe that the way things tend to work, it's not official.

They tend to be done in the blockstream SecP256K1ZKP first.

There is a PR that we've used on occasion initially for musig support and then later frost support there, recognizing that those are not official.

Then of course, music2 about a month ago was moved into initially a branch and now it's a tagged release of the real SecP library, minus a couple of features.

For instance, you can't do adapter signatures with the one that was ported from ZKP into the venerable libsec.

I believe it'll be moving forward and hopefully at the next meeting we might have a little bit more of a roadmap of is this going to be a Q1 or a Q4 type of thing.

They do tend to be very cautious.

I thought musig2, which is in some ways easier to understand, was going to move from ZKP to the main branch sooner than it did.

I don't understand all the review cycles and such that they go through to do that.

Then again, just like with musig2, they may decide, "Well, we're not going to move all the features over," which also means there could be some delays in the sense of they go, "Well, we're not going to support X and Y because Z is more important and essential," and they actually have to remove it from the code base.

Hopefully, we'll know more about that in our next Wallet meeting, which I would think would be about three to four months from now.

Of course, a lot of the members of these different library developers and such are on the Gordian channel or I can point you to specific things.

Any other questions?

That's helpful.

Thank you.

Sure.

Obviously, you can contact me at ChristopherA on Twitter, BlueSky, Facebook, etc.

Although I prefer Signal, and I think most of you have my Signal contact information.

A big thing right now for us is to understand what it is that you need.

Obviously, we'd really feel like some of these UX issues are important.

We have a pretty big stack of specifications that are available now, but there are also holes in the stack.

We have some user experience things in the area of things like LiveHash and the Object Information Block, which we think helps standardize some of the UX experience of working with objects like keys and such.

URs I think are a superior alternative to being able to have a text representation of complex data.

As Diego was talking about, you have this problem of you want to be able to send it via Signal or print it on a piece of paper.

We want to be able to support all of those types of things, and URs is working and functional.

Now, Mer is basically the multi-part UR.

He talked about putting QR's on paper or text on paper.

That means if it's too large, you need to be able to split it up and reconstruct it.

That's what Mer is.

Obviously, that allowed us to do animated QRs.

Envelope and all of that technology allows for GSTP and a number of these other protocols.

One of the big projects this year is to be able to offer recovery services for keys in the way that Ledger does, but in a more open fashion, but also support more user choice, not just services, offline seed recovery, etc.

Zid is trying to solve the problem of Noster only having a non-rotatable key, which we think is not a good practice for both privacy and for reliability.

It allows you to have an identifier that you can rotate the keys that you use or transition to new identifiers.

Obviously, we have some work on our crypto stack implemented today.

Security reviewed is our SSKR, which is a traditional shamir with a layer wrapped on it for a two-layer shamir that allows for some really powerful options.

Obviously, we want to move to the VSS, verifiable secret sharing that's compatible with Frost and other systems.

We'll be deploying more tooling and information around that.

But fundamentally, we're a developer organization.

We don't do things unless there's at least two of you who want to work together.

We need to hear if Diego says, "Hey, we really need to solve this problem in a standard way."

We need somebody else to say, "Yeah, this is something that we also want to support, and we'll join in on that."

That's the signal for us to devote resources to have these meetings, have smaller meetings, have coordinate hackathons or other projects to meet this.

Then, of course, in all of this, we really need your support.

The easiest way to support us is through GitHub.

We really can use your individual sponsorships.

This can be as little as $20 a month to basically share that.

When we go to people like HRF or to other organizations to fund this coordination work, they want to see this number to be larger than the 12.

We need your help there.

Then of course, as a sponsor of Blockchain Commons, we would like to have more parties at a variety of different levels for our sustaining sponsors.

As a sustaining sponsor, you get to help us decide where do we want to focus our priorities.

HRF made a clear thing saying, "Hey, we think Frost is important."

They supported Frost, and that allowed us to have these meetings today.

How else can we help you?

Then we'll call it for the day.

I don't know if this is something that Blockchain, these calls can help with specifically, but I really appreciated Diego's point on terminology and glossary for these kind of, for say, Frost multiseeks.

I think coming up with similar terms for these kind of things could really go a long way.

That's something I'm pretty interested in.

It's definitely a challenging issue.

The hold on one second.

For some reason, my screen has locked up.

There we go.

For instance, I've been puzzling around with even simple things.

It's one thing that when we're talking about users, we also have to talk about among ourselves in a consistent way.

I've been even trying to puzzle out what are ways to represent what is a quorum.

We refer to it in so many different ways.

Obviously this is not something you want to present to a user, but it does come up with, for instance, quorums using Taproot.

Even just doing this presentation today, when I was doing my keynote, I found that there were a lot of little things where people were using the terminology in slightly different ways.

Even the definition of what is around was kind of confusing.

What state that happens in some of these types of things, what is the state that is required for these?

What state does the user need to know about or is hidden behind an encapsulation or requires some kind of persistent identifier, I think is another issue just among us developers working on this stuff.

With the object information block type stuff that we've done for Smart Custody, we kind of need the equivalent of some standards for this is what you should expect to see at minimum when you're approving a transaction.

Every wallet that we've seen has a completely different kind of approach to how they present, "Okay, here is the information you need to know before you click the button to sign."

It's getting more complex and we need some minimum standards for that.

These are all things we hope to be able to support you in working together on it.

We need people like two wallet companies saying, "Yes, we want to exchange seeds."

We've been talking with the Zcash wallet community, which is dealing with having to deprecate one of their oldest wallets, the Zcash D based on Bitcoin D.

Now they're kind of thinking about, "Oh, what is the future of interoperability when our most venerable wallet needs to be deprecated?"

They're coming back and going, "Oh, Blockchain Commons might be able to help them address some of the interoperability issues, coordinate the different parties, and come up with a good solution for their community."

We need you to come to us with your problems and in particular the coordination problems and we'll help you come up with solutions.

Okay, well, I think that's it.

Sounds good.

I see value in these meetings and the ability to create that kind of common terminology and getting on the same page and stuff for getting this technology into the hands of as many people as possible in as easy to use a way as possible.

Absolutely.

Thank you much.

Okay, we're going to turn off recording now and I'll hang around for a couple more minutes if anybody has some questions they want to not be on the record.

[BLANK_AUDIO]

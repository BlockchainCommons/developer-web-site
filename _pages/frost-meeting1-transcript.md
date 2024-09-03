---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Round Table I: Transcript"
hide_description: true
classes:
  - wide
permalink: /frost/meeting1/transcript/
sidebar:
  nav: frost
---

_This is a raw, unedited transcript of the FROST Implementer's Round Table on November 8, 2023._

Thank you.

Good day, everybody.

Welcome to the Frost implementers roundtable.

It's November 8, 2023.

I'm the host, and I'm representing Blockchain Commons.

What is Blockchain Commons?

We're a community interested in self-sovereign control of digital assets.

We've put together stakeholders to collaboratively develop interoperable infrastructure.

We design decentralized solutions where everyone wins, and we are a neutral not-for-profit to enable people to control their digital destiny.

And we are, as a nonprofit, sponsored by a variety of different organizations.

These are some of the more significant ones.

We're going to begin with just some brief personal introductions.

I'd like for you to say who you are, what is your association with Frost, but save any project status things for later in the agenda.

And in particular, I would like you to answer one of these questions.

What do you love the most about Frost?

What's been the most difficult?

What's the biggest roadblock?

Or what's coming up?

What's your biggest concern?

Just pick one and just share it with us.

I will begin.

I'm Christopher Allen.

I'm principal architect and executive director of Blockchain Commons.

What I love the most about Frost is the opportunities to do interesting things with quorums and do it in a cryptographically trusted way rather than relying on some post that such and such was true to be able to do it cryptographically.

Jesse, why don't you go next?

Yeah, I'm Jesse Posner.

I've been working on a SecP256k1 implementation of Frost.

I've got a PR open in the SecP-CKP repo, and I've been working on it with Tim Ruffing and Jonas Nick.

And there's a lot to love about Frost.

I think one of the things that I'm most excited for right now is how it can impact recovery flows.

So, for example, if you have a 203 wallet, instead of having to pay a high transaction fee when you recover funds and need to sweep all the UTXOs, we have these really cool rotation mechanisms using proactive secret sharing.

We also have enrollment and disenrollment, share repairability.

And so I think it can make for these really flexible quorums and quorums that can easily recover funds without paying any fees and without having to go on chain at all.

Thank you.

Conrado?

Hi, everyone.

I'm Conrado.

I'm a photography engineer at the SecCash Foundation.

I've been working with Frost mostly on a monorail supplementation of other cipher suites that are specified in the RFC.

I've been also helping a little bit with the RFC.

You'll find some small issues in fixing some of them, but I'm not one of the RFC authors.

And now I'm also involved with the NIST submission of Frost for the threshold schemes.

I think the difficult part about Frost is not actually Frost.

Frost is kind of.

.

.

it's fairly straightforward to implement, but the hard part is getting deployed.

So we've been thinking about, in our case, our scenarios about SecCash.

So we would like wallets to provide some functionality for people to be able to protect an account using Frost.

So how you get deployed?

How do you make wallets talk to each other?

Stuff like that.

It's just engineering.

It's not even cryptography, but that's actually the hard part of getting Frost deployed.

That's it.

Shannon?

Hey, I'm Shannon Applecline.

I'm the technical writer here at Blockchain Commons.

And what I love about Frost is its improvements for resilience in wallets that we can deploy, not just the fact that we can use multi-sigs to allow people to not just lose a key and lose their wallet, but also VSS in particular so that we can make sure stuff is still out there without actually having to reconstruct keys.

So that's all very exciting.

And I think it'll be.

.

.

if we can get people to deploy it, a whole new frontier for wallets.

Thank you.

Ian?

I have to unmute, I guess.

Hi, I'm Ian Goldberg.

I'm one of the original developers of Frost.

And I've been involved in the IRTF RFC process and will be involved in the NIST process.

Thank you.

Oh, you have something you want to say about Frost in this list?

What coming up is the biggest concern, I guess, if you're not familiar with our next work, Storm, which is up on ePrint and we'll be submitting it soon, it's a new key generation scheme that is appropriate to use with Frost.

Oh, and this is Luna.

Luna is also involved.

Okay, Chelsea.

Well, yeah, I'm Chelsea.

So I've worked on Frost, as well as some of the standards efforts as well.

Yeah, Ian and I both work together.

And I'm excited to see the community coalescing and finding Frost to be useful.

And yeah, I'm just kind of curious to listen in today.

So yeah, today I'll mostly be listening and hearing about, I guess, some of the development efforts.

You want to share one of the icebreakers?

I guess, yeah, I think, yeah, mostly my, I guess, like what's difficult for me about Frost is just understanding, like, yeah, what's going on in the community and what people are doing.

So that's why I'm excited about this roundtable to hear what people are up to.

So yeah, thanks, Christopher.

Thanks.

Lucas.

Hello, Lucas.

I currently work for Penumbra.

I've been implementing Frost on and off since it was not an RFC back in 2021.

And currently we're sort of exploring implementing it as part of the city in Penumbra.

I'd say the thing I like most about Frost is that it's easy to implement.

I've done that a few times.

Great.

Thank you, Chris.

Hi, I'm Wilhel McNeilly.

I'm the lead researcher for Blockchain Commons.

I've been a working engineer for over 40 years.

I've written a lot of our published specifications and example code.

And my major concern for Frost, which is a lot of things, is making it very kind of human-oriented, easy to use, making it so that people find it natural to adopt.

And I'm not a cryptography expert, so I often need things like that explaining to me like I'm five.

But then I think that's actually good because I'm a good test case in that case.

But other than that, I have a lot of technical skills.

Thank you.

Lucas.

You already won.

Oh, sorry.

Tim.

Hi, I'm a crypto talkover slash crypto graphic engineer at Blockstream.

And I'm involved in, let's say, Jesse's implementation because I'm a maintainer of the libsecp256k1-ckp library, Blockstream fork of the secp library that's used in Bitcoin.

And also, I'm a co-author of a recent paper at Crop and for the icebreakers, I think this really, if you ask me what has been the most difficult part of Frost, I think the most difficult part is the DKG.

No one wants to talk about it.

And we all talk about how nice signing is, but DKG is not that easy sometimes.

Thank you.

Pilly.

I'm not sure if I'm pronouncing that right.

Yes, that's great.

I'm Pilly.

I'm from C-Cash Foundation as well.

I'm the engineering manager.

And I'm here mostly to listen and learn with a product hat on because we want to have wallets integrate Frost for C-Cash.

And I guess that leads into my biggest concern, which is the user experience of this and what the user workflows are going to be.

Things like DKG also like Tim and yeah, looking to learn today.

Thank you.

Nat.

Hi, sorry, my daughter walked in my room and had to shoot her out.

Yes, so I work at the C-Cash Foundation as well with Pilly, Chelsea and Conrado, an engineer working on the Rust library as well in terms of, yeah, I guess the biggest roadblock is less that we've overcome and more what we're like trying to overcome now, I guess is sort of what people have said, just like getting it into the hands of users, basically.

I think it's very versatile, but we just want to actually have people using it, finding it useful.

Thank you, everybody.

Did I miss anyone?

I don't think so.

Okay, so project introductions.

So we have, you know, and I think I may have misnamed some of these properly.

We have a, reminds me, we have a notes document here that is collaborative and any of you are able to edit.

We'll put that in the into the chat here in Zoom.

So I was trying to just capture, you know, what are the repos and other stuff that different people here are involved in.

And also, there's an opportunity for people who want to edit and have notes, they can add it that add them to the bottom.

So I wanted to basically walk through the different projects and have someone, you know, talk a little bit about what is your current progress, what's next on your roadmap, security review situation, and then sort of related standardization efforts we've heard, you know, about ITF and NIST and Bitcoin, etc.

So let's begin with the Vcash Frost.

Who wants to speak up to kind of share the status of that?

I can talk about it.

So basically, we have this repo that has a representation of our surface suites in the right seat.

It has been audited, the audit has been published two weeks ago, something like that.

And we're probably publishing our 1.

0 RC0 release this week, probably.

So kind of finishing up the main part of our work.

And what's coming up next, we have another repo that has like a demo for how to explain how to use Frost to show how to use Frost programmatically.

I'll put a link to the repo in the hacknd.

Right now it works like we have to copy and paste JSON blogs between terminals to fake communication.

And what we're working right now is actually doing socket communication to actually make work of the network.

But that's like it's still just like a proof of concept.

Our final goal is to have this a poet, you know, say cash wallet, hopefully.

But yeah, it's just we're just taking the first steps towards that.

What else we have the RFC.

It's kind of done.

We're just waiting for the ITF process to finish.

And I think that's it.

I see some questions in the hacknd about if you are our code was reviewed only with stretto.

We reviewed the authors reviewed all the cypher suites, but the implementation of the lib curve itself was out of school except at FIFI rate, which since there was a crate that we use a crate that wasn't super well known.

We also asked them to audit, but yeah, they audited all surface suites just with the exception of the lib curve code.

We'll talk a little bit more about those questions in a few minutes.

And obviously your current plan is to use it with the zcash wallet and you're supporting the ITF effort.

ITF is not doing secp and zcash and you also you are offering both.

So any any particular comments on the status of the differences between those or they parallel ones behind the other.

Yeah, that's a good point.

We're we're we're following the RFC.

Exactly right.

We just provide the services that are in the RFC.

So one important thing is that the RFC has a sec 256 K PK one cypher suite, but that's not compatible with the type of signatures from Bitcoin.

So yeah, we only provide we only provide this non compatible cypher suite, but it could kind of easily be adapted to work with that.

Thank you.

Very useful.

I don't know what the right name is for the the the square frost.

That piece 256 K one block stream, etc.

repo.

As far as what the team would call it.

Who wants to represent the status on that one.

I can take that.

It's not it's not affiliated with square block.

You know, we we are we'll probably use it in the bit key.

I'm on the bit key team at block and we're definitely eager to to use frost for our product, but it's not.

There's no IP associated with block.

There's no funding associated with block.

So I caught this the the sec P dash z k P frost.

I think is maybe the best name for it.

And in terms of the the current status, I've got a PR that I've been working on that is getting into a fairly mature state.

And the current plan is to split out the the split out the signing code into a separate PR because that's pretty much ready to go.

So we can kind of get that merged and just kind of assume like a trusted set up or you got your shares from some other way.

And then and then Tim.

Roughing Jonas Nick are actively working also with Lloyd for near on working on and I'm helping with as well working on a frost bit and specifically getting down the spec for how we want to do the DKG, which has gone through several iterations.

And once that finalizes, I will be updating the DKG portion of the code.

And the goal is to get all this merged by the end of the year.

And the bit will be a standardization effort in terms of how how we can implement frost in in the Bitcoin world.

Great.

Thank you.

So anybody else have another code base that that they're working on or, you know, going to be want to talk about the status.

You can raise your hand or.

Just speak if you like.

I know there are several others out there.

I just don't know how many people, how many of them are, you know, active and progressing.

So no reports on those right now.

Okay.

So I have a number of different questions on distributed key generation.

It's evident from the discussion that, you know, this is one of the most difficult parts that there are multiple approaches which may or may not be compatible.

And then there are new approaches like storm, etc.

That are, you know, in discussion.

I will also add to that that the, you know, several people have talked about, you know, rekeying and other different types of things that might be possible with distributed key generation.

So let's not limit this discussion to just the questions here.

But in particular, I at least wanted to get these done.

So starting off with, does anybody have any comments about, you know, some of the drawbacks of their current DKG or or, you know, concerns or advantages that others might not be aware of in their in your implementation of DKG.

And so in the work that Jesse just mentioned where we are looking into raviance of, of DKG, he said together with Jonas Nick and Lloyd Funier.

I think this is pretty much a work in progress and but I think people haven't really heard about it.

So our goal there is to come up with a rather simple DKG.

So we drop the robustness requirement that's bit in the spirit of frost where when you when you start to drop robustness, everything becomes a bit simpler.

So we did the same and but we want to have a DKG with kind of as I wrote in the document with batteries included means that there is a broadcast channel included in the sense that there is a explicit check in the end that everyone agrees.

Of course, we can't have robustness here without any further setup assumptions, but we can at least have to guarantee that if everyone agrees, then we can we know that everyone has has agreed.

So we may not terminate, but if you terminate, we know we're safe and also includes secure channels and a queue and includes backup facilities and so on.

So this is supposed to be something that engineers can pick up implement and hopefully they're safe without caring about all these setup assumptions, but we don't have any advanced features such as let's say robustness or things that I mentioned on the slide here, Chrome rotation.

I use the shares for other protocols is really built on simple path pop, which is DKG, the Peterson pop that we used in the Olaf paper, which is really only to be proven secure with or which is only proven secure together with frost.

So we need a couple to frost at the moment.

Thank you.

Does the other, does the Zcash team have any questions for him on their on the ZKP DKG?

Yeah, I think we might have discussed this, but we also have implementation of the KG, but our crates, they don't handle communication, right?

They just provide the functions that people need to to run the KG, but all the handling of communication needs to be done by the caller.

And that's the tricky part, right?

You need encrypted, authenticated, they also broadcast channel to the DKG.

So yeah, just curious on how people are implementing this in practice, like maybe the communication part.

Yeah, what's it what what secure channel are you using?

Yeah, is it based on?

So in ours, it's really, I mean, at the moment, we're just drafting a specification, but the idea is that we just draft the algorithms, but the algorithms include secure channel construction, which is super simple.

It's like you exchange keys in the beginning and then use.

I mean, the only thing you need to send encrypted, this is one share.

So it's not that we set up a full encrypted channel, we set up an ECD, a shared secret, but we just use it to encrypt one message.

So it's basically authenticated public key encryption, but because it's one message, it's much simpler than having a full secure channel where you need to care about message ordering and and other stateful stuff and replay attacks and so on.

Right, so basically, you have used public key encryption, to authenticate the participants.

So you just assume that everyone has everyone else's keys.

Yeah, I mean, we have to make some assumption.

Yeah, and this is one way you can do it in our protocol.

We also have a simplified version of this where you, but it's just more like a convenience where you basically hash all the public keys and you have a short string that you can compare with everyone.

Which is probably a bit more what people would do in practice.

On the other hand, if you couldn't use this, like Xavi, I checked your public key out of band.

But I couldn't use this then for another setup because the hash of the shared string would change.

So it's a bit of a trade off if you want to compare individual public keys, but it's more annoying for users and implementers, I think.

I wouldn't necessarily understand.

Yeah, I want to jump in with a question.

So if you have a broadcast channel, you shouldn't have to do another round at the end where you compare keys, right?

Because if everyone sends the same commitments, then they all should be able to derive the same keys at the end.

Yeah.

So.

Is this a question?

Right, but so it's like, oh, so I guess like, yes, I mean, there's different ways of going about it.

But so if you assume that there's this underlying broadcast channel in the first round, everyone sends commitments.

Everyone should be able to get the same consistent commitment.

And then, and then they should all be able to derive the same keys afterwards because you're consistent.

So.

So it's a question, why is he doing the confirmation at the end?

Yeah, like it doesn't need to do that.

Yeah, you're right.

I mean, when I said broadcast channel, maybe this was misleading.

It's more like a synchronization step.

So we basically, so what we assume as a setup is that everyone has checked this, has checked public key with public keys with each other or has compared to each other.

Or has compared this setup string.

That means that we all agree on the public keys of everyone.

Then we just go ahead and run the protocol without any special channel.

And then in the end, we use the public keys that are our trust anchors to compare that we actually saw the same thing.

So we run the protocol optimistically over a plain channel in the sense.

And then in the end, we use the fact that we have checked public keys before the protocol to establish that we all agree on what we have received.

Okay, let's, we're going to talk a little bit more about trusted channels later, maybe all the, maybe we've already talked about it enough.

But I wanted to maybe do the other way around.

Conrado, would you want to share a little bit about the status of your distributed key generation and these questions or related questions.

And then we'll let the ZKP team ask you questions.

Yeah, like I mentioned, we have implementation of the KG, but it doesn't have communication.

So it's kind of very straightforward.

Only now we are starting to implement some mechanisms for communication so I don't have much to share about this right now.

I think something important to mention, I think, in the context of the cache or other cryptocurrencies, I think this is what people want, right?

The KG, they don't want trusted dealer key generation, they want the KG.

So I think it's really important that we actually provide this to people because it's probably what they want.

But yeah, this is all I have.

I'm going to jump in as well.

So yeah, so just to kind of clarify, by DKG there's several different protocols.

So the ZCache implementation implements the DKG as specified in the Frost paper.

Tim and Jesse and others are sort of working on two round, or I don't know how many rounds, but kind of like variants thereof.

And Tim, correct me if I'm wrong, but the proof kind of still assumes frost under the hood.

So I think there's these DKGs which are proven with respect to frost or variants of frost.

There's then the question of like DKGs that can be used in any setting.

And these two round DKGs are not proven secure generally.

So the DKG that Ian mentioned, Storm, that him and I and Douglas Deblo worked on, is basically a DKG that can be a drop in to any setting that needs discrete log keys.

So the reason why we did that is because we had a bunch of questions around using PetPop for other things, like you said, for example, for like threshold encryption.

And right now there's not a proof that Pedersen is secure in that setting.

And basically you need like separate proofs for all those settings.

So that's why we have Storm where it's basically where you assume discrete log keys, you can use Storm and the proof just sort of holds.

So I think it's kind of important to separate out these types of things and more generally just know that, yeah, like which DKG you use for which setting is important.

And if you want kind of a generic DKG that can be used in any context, then you have to pick which DKG.

You have to pick that one carefully, basically.

Right.

That's exactly what I wanted to know a little bit more about.

So I'm going to move to the next one.

So I was particularly pleased to see that the Zcash project was offering Presto Dealer Generation in their code base, partly because I feel like this is a transitional form.

Blockchain Commons has been using a Shimir based with some extensions to support two layers in one of our wallet specifications.

And we've really been wanting to move to VSS.

And if we're going to move to VSS, why not move to one that's compatible with Frost?

So this was kind of like, am I going to be staffing up to do something here?

So Zcash did implement Presto Dealer Generation.

It does not sound like the ZKP team has done.

I'd be interested if someone who's been working on that on the Zcash side could talk about why they implemented it.

Just some other sort of related things.

Part of the reason why I was interested in Trusted Dealer Generation is we also have a similar type of thing to this for open silicon vendors called Silicon Salon, where we've been talking about generation of self-sovereign keys, better security on the hardware.

And so we feel like the Trusted Dealer Generation might be useful in that particular category.

And then just in general, especially if the cryptographers among you, are there risks of transitioning some of the Shimir protocols to leverage the VSS that results in Trusted Dealer, or am I misunderstanding something?

So anyone want to speak to that on the Zcash side?

Yeah, I think the main reason we did Trusted Dealer is just because much Z I have the impression that in practice, like I mentioned, people will want DKG because they don't want the keys to be present in memory at any point in time.

But it still might be useful in some specific scenarios.

Maybe you can generate a Rump Trusted Dealer in an offline computer or maybe in a hardware device.

And you would still keep security, you would still minimize the chain-self compromise.

But yeah, that's all I have about this.

Okay, anyone else have comments?

I think my take on Trusted Dealer Generation is that it's not really worth the effort.

I mean, if you're going to add so much complexity because you add a threshold scheme, then I think DKG really, really makes sense.

On the other hand, now we just discussed this with Jesse, as Jesse actually mentioned earlier, our plan for implementation is now to split the current pull request and move forward with signing where we can think more about DKG.

And this then actually needs some Trusted Dealer or Trusted Key Generation because otherwise you couldn't use this signing code.

And I think it's just a tool to get this out in the world and to let people experiment with it and also to maybe add test vectors that work for just a signing protocol.

I don't think that's useful, but in practice, it's always hard to consider all use cases, but I don't think it's a very useful use case to have a Trusted Dealer.

I know for me, I'm also co-author of the DID standard in the W3C, but I've been very frustrated with the single key architecture that has evolved as that standard moved on.

And I'm looking forward to it just to help that community consider the possibilities of what's possible with multi-sig and quorums.

But I don't think they're prepared to even begin to think about DKGs.

So this just for me allows for better demos, better proof of concepts.

It also becomes kind of a constriction point where we can basically say, hey, we're going to standardize on some specific objects and tests and data formats that in this, you know, everything before signing is clearly separated from using these to find.

So that's my interest in it.

Any other comments on this one?

Yeah, I think Trusted Dealers can be useful for some custodians.

For example, when I was at Coinbase, we had something like this because we already had a secure, trusted process for generating Shamir shares.

But then we could use special signing to sign with the shares without having to have a trusted reconstruction process to bring the shares back together.

So for certain custodial use cases, I think it can actually be quite useful.

Yeah, part of what SSKR is used for with the Blockchain Commons Gordian standards is for improving self-custody.

So the ability to do VSS and be able to have the shareholders sign different types of things without necessarily restoring is just really useful for wallet resilience of legacy keys, personal data, etc.

Which sort of leads us to resilience and trust.

So, you know, I don't think we've well defined, we kind of begun to understand the secrecy requirements of private keys.

Obviously, we'd love to have single keys somewhere deep in the silicon where it can never, ever be seen and has multiple layers of physical and other resistance, etc.

And we never quite achieved that.

We still have a lot of keys and browsers and things of that nature that are insufficiently timing attack, but at least we know the level of care that's required for a single, you know, a private key that is intended for single SIGs.

But I don't think we have a good understanding for the privacy and care that is needed for a generated share.

So, I mean, there's some simple things like reliability issues and does somebody have it a year later and etc.

But then there's also these concerns of correlation or DOS attacks or other things and also kind of reuse of the initial keys, the keys that are used to generate the shares.

I mean, you know, in theory, I believe they can be thrown away, but is there some utility to keeping those around, which leads to what are the real requirements for trust in the wallets and what changes to trust models for wallets are needed given the opportunities of a DKG and then sort of related to the other event I host, are there opportunities to increase trust by implementing parts of Frost in silicon?

Anybody want to kick off any thoughts on related to your projects or, you know, the future of Frost in this area?

I think I've asked this question a couple of times before.

Go ahead.

I don't have much to share exactly about this, but something related is we're wondering in the context of the cash or other cryptocurrencies people are used to, you know, they write down the seed phrase and that's it.

If they lose, if they see your phone stops working, they can restore from the seed phase and they're good.

In the case of Frost, it's a bit tricky because on one side, it's tricky because you don't need just to share.

You need to share in the group key and the identifier and maybe other people's identifiers.

So it's more information that you need to back up or save.

On the other hand, of course, if you lose your key, you can still, depending on the threshold, you can still sign.

And particularly something that we also added in your creates the ability to repair people share.

Right.

You can if you have a threshold of people come together, they can restore another person share.

There's a protocol for that.

So maybe we'll need to care that much about people losing shares.

But I think what this is asking is what about the safety aspects.

Yeah, I'm not sure it would be good to have something like, for example, a harder wallet that can sign, can do Frost, can do this, you know, to generate a similar share using Frost.

That would be great.

Well, there's definitely discussion if you go to Silicon salon info.

There is discussion by Silicon designers about supporting this type of thing at kind of different levels.

And they're really unclear on which ones are appropriate.

But it's everything from one of the vendors actually has three different CPUs on the same ship.

So one's an arm, one is a risk, and another one is an FPU.

And they're like going, well, you know, can we do kind of Frost internally so that no single chip CPU has the whole secret.

Can we do DKG down down at that level?

But then there's also discussions of, you know, OK, so can we have a chain, you know, a private trusted channel between two identical chips, such that neither of the chips individually have to be trusted, etc.

So there's some discussions in that in Silicon salon info.

So but clearly this is something we'd like to in particular, Blockchain Commons would like to to try to move forward some better, more clear answers to these questions in the future.

So one one related thing is that that was an idea that came up in the Bitcoin community is how could you say you have a payment channel over the Lightning Network and you as your side of the payment channel, you want to use Frost for your note because this makes key storage for you much more secure.

And it turns out so we looked into this a bit and it turns out it's it's much harder than than just Frost and I could imagine that the use case you mentioned is similar because.

And these applications is not just about usually not just about the cryptography in the sense that you need to secure threshold signing scheme, but you also need to agree on the on the state.

So in the Lightning channel, yeah, there's some state and you need to do the state management.

So it's more like a state machine replication or or consensus or BFT or whatever you want to call it, which is which is another hard problem.

Of course, there are also solutions for that.

But then Frost is only really part of the answer.

Thank you.

Also, I want to add for I think it would be great to have hardware wallet support or secure enclave support.

But I think one thing to really pay attention to there is since we can't use deterministic nonsense or maybe we could with like some zero knowledge proofs.

But assuming we're not doing that, there's like more inputs coming into the silicon with regard to the signature and the nonce components.

And so you have to be really careful that you can't trick the hardware wallet into reusing a nonce.

And especially if you don't want to have to make it depending on some kind of state or counter or something like that.

So that's something I think just to pay very close attention to.

Great.

Thank you.

Okay, moving on to curves.

So this, you know, so the Zcash team has implemented in the Ristretto family, which is, you know, a subset of the ED 25519.

Well, 25519 in general curve.

And but they've also done the SECP implementation, whereas the ZKP team is only doing SECP.

So this, you know, wanted to discuss the opportunity, you know, challenges and opportunities here.

There's a lot of ED2519 infrastructure out there.

I know blockchain comments has been kind of frustrated with the lack of some clarity even within the, you know, IETF standard for 25519 and the implementations and stuff.

And then obviously, there's the whole reason why we're moving to Ristretto.

So I just don't understand.

So this is more the question for the Zcash team.

You know, what are the limitations of leveraging existing 25519 infrastructure or do we need to be completely moving to Ristretto or is it, you know, oh, it's not a big deal.

You just convert the keys to Ristretto and do them in there and then move them back.

Any thoughts there?

Chelsea, I know you've had some interesting discussions about 25519 and some of your thoughts.

I don't actually know if I talked about 25519.

That's surprising.

But I guess Konradi can talk about this more, but the IETF draft covers a range of curves.

But yeah, Konradi, you should jump in.

Sure.

Yeah, to be clear, we have five cypher suites.

One of them is H25519.

So you can use frost and generate a signature that can be verified by any H25519 verifier and it will work.

And we have another cypher suite which is Ristretto 255.

So you can use one or both, depending if you need compatibility with H25519, you can just use that cypher suite and you're done.

It's a bit less efficient than Ristretto because frost needs a prime order group and H25519 is not a prime order.

So that basically entails that when you're inside the frost presentation, when you receive a public key, you need to validate if the point is actually inside the prime order group, which is a bit less efficient.

But it works, right?

The signatures that are generated by this process are compatible with H25519 verifiers.

I never thought about a possibility of converting between keys.

It doesn't sound like a good idea, but maybe it works.

I will need to think about it.

Deidre and her podcast was talking last month about, hey, if we can find out that the P variant is actually safe, should we all move back to P?

It was kind of like because a lot of the speedups that 25519 offers can now be implemented by various tricks to convert things in a fashion so that you get the speedup of 25519, but the standard for the P curve.

So I don't know.

Part of me just wants to just move completely here with Ristretto, but I just don't know what other people are, just prime order curves.

And if anybody has any thoughts on that.

So I have doubts about this, but is there the distributed key generation phase completely different for the different suites?

Or can the result of a distributed key generation be then used for signing with Ristretto and for SecP?

I didn't understand that.

Yeah, if you compare Ristretto and SecP, I don't think they are any compatible because they are different groups, right?

So different Rit curve groups with different orders.

So I don't see how you could do a single DKG for both.

But if you talk about Ristretto and H5-519, they're kind of related, so maybe there's some way to make it work, but I would need to also think about it.

Maybe my question isn't well phrased.

I think there's a, you know, obviously we have this distributed key generation phase, which results in a share that can then be used for this signing.

So I guess the question is, are those shares completely different in format because of the curves they use, such that they're, you know, basically if you want to do both, you're going to have to do it.

You're going to have to do a distributed key generation separately for each.

If you want a later sign.

Or is there some way that the result of the distributed key generation can be used for signing either with Ristretto or SecP?

The key generations are strongly tied to the group order.

Okay.

So, as was just said, Ristretto and SecP, no.

Ristretto and ED25519, almost certainly yes.

And if you're talking about two curves that are basically equivalent, but one is in Edwards form and one's in Montgomery form or something like that, then yes.

Okay.

Thank you.

We've had some discussion on trusted channels.

So we've already talked about the distributed key generation requires a trusted channel and a little bit on the progress and requirements for this.

But I also wanted to talk about the signing.

So signing doesn't require trusted channels, but there are obviously privacy correlation issues if signed over on trusted channels.

Anybody doing anything here?

I guess I maybe pushed this to the SecP team or, you know, once you do your generation, are you reusing any of the Diffie-Hellman that you used in previous parts of the VKG?

What's any thoughts here?

I think that's at least not something we currently considering for the spec.

I think this really depends on the application or on the caller.

So if your application, if your system anyway wants to have secure channels for privacy reasons, which is obvious one, for example, then I guess you would use them for you would run DKG over the secure channel.

You would also run signing over the secure channel.

The reason we are including secure channels in the DKG is really that we wanted to have something with batteries included where the caller or the user can't screw up.

Well, I mean, I guess there are a thousand other ways you can screw up, but at least we remove that one.

It's pretty cheap what we do there.

It's just one easy to use and we use it for encrypting one message.

So even if you already have a secure channel, I think you could just go ahead with our DKG and accept the minimal overhead there.

Okay.

Any more thoughts on that one from anybody?

Yeah, on our side, in the case of Zcash, you are using for us to sign a transaction.

The transaction details are private, so we need to encrypt the channels, authenticate channels.

Yeah, we can kind of get away from that.

We don't have any specific mechanism in mind for that.

Just a random comment about the last title, about identifiers.

Something that we added to our crates is a function to do like derive identifier from an avatar stream.

It just runs like hash to group with a specific domain separator.

So maybe that's something, just talk about this.

So maybe this is something that is useful to other people.

You can take a look at a repo and you like to use the same domain separator that is getting the reward for interoperability.

I would love to see that maybe detailed out as something we can share.

It's sort of related, this sort of we get to the next question, sort of, you know, there is both alligator transformations for SecP and for 25, I don't know if there is for 25519.

I know there is for Ristretto, but like, you know, should we be integrating that future of being able to have various keys be represented by random, unidentifiable from randomness, or is it just a completely separate layer?

We don't worry about it here.

Are there other anti-correlation things?

And then Jesse put in the notes that the ZKP version is using the X-only pubkeys and the BIP340-Schnor, whereas the ZKP team has been, I guess, using different public key and isn't necessarily compatible with 350-Schnor.

And what are the, you know, do we want to try to have more compatibility there?

Do we want, especially since the ZKP team is trying to push some things through ITF, which has not yet accepted a K1 curve in any of their things that I know of, you know, do we want to be, you know, is there interest in your team and having compatibility with X-only pubkeys and 340-Schnor?

So what kind of other alignment with crypto and other libraries and things like identifiers and etc.

are needed or people are interested in?

R You spoke earlier that you sounded like, you know, you were not doing BIP340-Schnor, but there was some interest.

I mean, what's the, what help do you need or what kind of, how can we coordinate this better?

What are you looking for?

Yes, we are focusing, because the Keshe Foundation, we are focusing on the Keshe, of course, so we don't have the bandwidth to work with that route, with Bitcoin.

But of course, this is like the probably the hugest application for us that is kind of obvious.

So yeah, we would love to see this implemented.

I think we need two things implementation is, of course, but also some kind of specification.

I don't know how exactly that works in the Bitcoin world if you need to write a BIP or something.

So yeah, I, it's something that we're interested in.

We can help with if possible, but we're not, you know, we're not planning to do anything concrete to address this.

Okay.

Well, this is something that blockchain Commons is definitely interested on at least the specification side, maybe some, some test vectors and things of that nature, because we do work both well, we work with w3c itf and we also are involved with the Bitcoin ecosystem.

So, you know, there may be something that we can do to kind of kick things off.

We kind of consider ourselves in the mode of incubating specifications, taking them up to a particular level and then, you know, moving those specs as appropriate to itf, which we did recently with deterministic seabor and, and or, you know, in the case of like Bitcoin, we're doing some things with QR codes and other stuff.

So we're definitely interested.

Any other alignment with other kinds of crypto that people would like to bring up here?

Maybe Ian or Chelsea.

Okay, I mean, just to comment on what can read as that.

Yeah, I think that totally makes sense.

I think it's, it's similar for us.

We're happy to talk and look into stuff, but we probably won't spend our time on, on, on too much on another ecosystem.

It's a bit of a shame that we with the web 340 we introduced x only keys.

I mean, of course, they're, they're shorter on the blockchain.

That's important, but it's kind of, yeah, put a distance between us and the rest of the world a bit in a sense.

Whereas I think if you if you ignore x only keys, and of course, they're only detailed, but if you if you ignore that detail, then the main difference between what it seems to here is probably the curve.

And but this is not a not a big difference in terms of ideas and specifications.

I think you could use our, you could look at our spec and just plug in another curve.

And I guess the same is true for, for RFC a draft on the ATF draft or, or any other implementation or specification of frost.

Okay.

Well, definitely that's will be on the agenda for blockchain commons this year, because I think this is important.

So, um, performance.

I suspect you know all of you are using like the latest computers and such, and maybe haven't even looked at performance issues.

We do have people that want to do frost on potentially very low power devices.

We've already determined for instance like one of the telecom devices that one of the designers wanted to do they want to be able to do it under near field communications power which is very low power, and they've determined they can't do it right now because they just isn't silicon accelerator for for their particular thing.

Has anybody looked at performance issues, minimal processor requirements.

See if anybody's been puzzling with this.

We haven't.

As I mean it is closely but we did have some.

There is like, it's not super novel but there's a Sorry, I presentation.

Improvement I can do the, the, this, the aggregate step of frost requires a mood scaler point multiplication.

And that's the, the, the part that takes most time, the type of call.

So we have, I don't know have 100 participants so we need to do 100 point equations and some them.

But you can do there's particular average average for spinning up this type of skill.

So if you use that, there's a lot like, depending on the amount of sign is in my cage percent something like that.

So this is the part of the thing to be aware of.

We have a blog post about this in your site.

And low power devices should should work right because it's the bottleneck of the protocol is the deep curve multiplication.

This is something that has been has been optimized a lot.

Because of the curve of course but could like with strato and HOF and I think I've been optimized a lot so yeah it should be shouldn't be too hard.

Okay, is the interest is, is the interest in doing the the court, the combining of the signature shares on a low power device or just doing this individual signing on a low power device.

Well, I think both, and not necessarily at this, you know, by the same class of devices or whatever but there's definitely interest in the sense of, you know, say in the digital identity field where you have very important secure keys, you know, which require, you know, special care and they're currently you know the bigger single key parties are doing all different kinds of custodial and VSS, excuse me, familiar backups and things of that nature, you know, wouldn't it be wonderful if they didn't have to do all of that, and could basically you know have devices that you know we're able to handle that in a fashion including things like, you know, rekeying quorums and things of that nature.

So I'm not sure.

I do know that's one of the obstacles that they're basically saying why they don't want to move to to multi secret.

The reconstruction is an entirely public operation, right, the shares aren't even secrets.

That's true.

So you can use a low power device to generate the shares.

And then anyone on any computer anywhere can combine them into the final signature.

There's no reason that has to be done on any particular device.

That makes sense.

Right.

So only the actual signature computation of which the most expensive part is the, the multi X to compute the group commitment.

If you're using for us one, if you're using for us to that's cheaper.

Okay.

Anyhow, that's again something that I wanted the community to be aware of that there is interest there.

We don't have any of the hardware vendors with us today but you know there's definitely been some discussion about that question.

Okay.

So, there obviously is roadmap going on in the IPF.

Currently as I understand it, it's an IRC, IACR with the target of becoming an informational draft, not an actual standard.

But maybe I don't understand that you know what your roadmap is in the IPF.

I also heard at the beginning of the call there were some things proposing this for NIST quorums.

I think I was vaguely aware of the call with the NIST and I just assumed it was another, that it was another quantum one that, and not a something that Frost would be suitable with quorum.

So I must have misunderstood that.

Any other things going on in international standards?

Yeah, I'll jump in and I'll speak to both of those.

So for the IPF, it is just an informational draft.

That's fine.

If there's interest in standardizing it within the IPF for something that will have to be a separate effort and I will not be involved with it.

But I think the main goal for that draft is to get out something that everyone could sort of implement something that's close to parity.

And then, yeah, we have a team right now and we're submitting it.

NIST is doing a call for threshold schemes, both quantum, post-quantum and classical security and Frost will be a submission to that.

Okay.

And I would love at some point in URL to be able to participate in that.

So, you know, in your, you know, reviewing your proposals or whatever.

So let me know as that progresses.

Anybody else doing anything?

I mean, obviously we've also talked to BIP.

It's not an international standard.

But, you know, we've also heard about moving some things forward in the BIP efforts.

Any other kind of shared specifications, collaborative standards going on?

I know that as some of these intermediary formats, you know, things like the identifiers that Konrata was talking about earlier, the various share formats, etc.

We would be interested in, you know, what was necessary to be able to have binary representations of them in CBOR for, you know, variety of reasons, including ability to back things up and it's up with various standards.

So if people are interested in that, that's something Blockchain Commons would like to do once these kind of get closer to final or at least, you know, usable for testing.

Yeah, there's something I'd like to add about this.

The RFC doesn't provide any data format.

I think it used to have one of the first regions, but it was decided it was out of scope.

But our create implementation does have a binary format for coding in the structs.

We use, we provide search so you can use anything but we choose one particular encoding as the default encoding for our create, we choose postcard, which is a very simple binary encoding format.

I hadn't considered CBOR, but I'm looking to it.

It's interesting.

But yeah, I'm interested in if people have opinions about how we should encode things.

We welcome these opinions because we would like to have a default search format and we would be good to make it standard.

Not sure how to make it standard, like if you do a idea type draft or just some document, whatever, but it would be good to have a standard.

Well, we've, watching Commons has been doing this a lot this year, and we've learned some of the challenges.

Wolf can speak to that.

But we've had some success and in particular deterministic CBOR, which is a subset of CBOR that addresses some of the cryptographic problems that so that you can have a single signable CBOR object that is consistent no matter who constructs it, is moving forward toward a track to become an RFC.

And we have Rust code for all of that.

We have Rust and Swift and TypeScript and a couple of others for DC-BOR, and then we also have this envelope format that depends on that.

So we are very interested in this and we think there are some advantages to the CBOR format and we also have succeeded in dealing with some of the snafus of the ITF, IANA, challenges of getting these things registered.

So I think we could help everybody avoid that on these.

So we're very interested.

I mean, for that matter, I'm very interested not just for, you know, frost things, but I'd like to see, you know, X only be a ITF CBOR format.

So, you know, that'll solve some problems.

Any comments on data formats or international standards?

Okay, so moving on to kind of, we're getting too close to the end, any wildcard uses of frost to the community, you think the community should be considering?

Like you go, well, we can't do that, but we'd love somebody to do it.

Or, you know, some edge cases.

I mean, obviously, Storm is, you know, separate because it's useful for other things, but, you know, could be other things like Storm or other ways to leverage the cryptography of frost.

So, Tim kind of alluded to this earlier about using frost with lightning and there's issues around like, man, getting consensus around state channels, but there's an interesting cryptographic piece of this that I don't think has been fully explored, which is the notion of nesting frost.

So for example, if you have a music construction, like in lightning with a two of two music for lightning channel, you know, to what extent can you nest frost inside of one or two of the music keys?

So one or two of the music keys would have been the output of a frost DKG, or could you nest, you know, frost in a, yeah, that can also be useful, for example, for let's say you want to, let's say a custodian has wants to have a quorum of humans and a quorum of servers that both participate to create a signature.

You can have a music two of two at the top level and each of those would be a frost T of N for the component keys.

And we don't really have any security proofs for that.

But there could be kind of interesting use cases there.

Another thing is using like a BIP32 style key tree hierarchy with frost, which definitely works, but I don't think we have like security proven for how for that specifically.

So those are some kind of interesting interactions with frost that that I've come across.

Thank you.

So that kind of is a good segue to the next slide.

Some I believe some of you were involved in roast.

Any progress on that.

I don't completely understand how much of it's an extension to frost as opposed to a pretty major change.

I think the design goal was that it really just an extension and there's no progress, I think, but it's also I think there's no one currently working on on it.

So I think from from our side, it's basically there.

I'm not sure if it would make sense to to write a standard for it.

The nice thing about roast is that it's a simple wrapper of frost so you can't really get it to wrong in a sense.

So you like if you I think even even if you if you implement it just looking at the paper and without looking at any specification, that's probably will be a safe implementation.

I'm not sure if people want to.

There's a rare version, then there's the medium rare version, then there's the fully roasted version.

Yeah, I mean, it's.

It's supposed to provide robustness so you can guarantee to get a signature and of course you if you if you implement it wrongly, then it won't work and you won't get a signature in the end.

But this is something that could be what it would easily be catch probably when when testing.

But as long as you just use frost in a black box way, whatever you do with it, you can't break it.

You can't screw up with unforgeability and because roast just uses frost in a black box way.

Yeah, frost should stay on fordable so nothing can can go wrong there.

Thank you.

Yeah, I'm interested in roast just simply because as you talk about larger quorums, which isn't typical, say, for the cryptocurrency world of the cash or Bitcoin, you know, we do want to be able to have, you know, maybe a, you know, a quorum signature for all the stockholders of a company to approve changes to articles and corporation.

In that case, you might have thousands and you don't want to be able to have, you know, parties that are, you know, smaller than the quorum being able to, you know, deny or cause problems for the quorum to legitimately be able to sign.

So that was what I like about frost.

I mean, about roast.

Oh, I hadn't thought about that.

Fettiment can also have large quorums.

Yeah, Jesse, weren't you talking about some point about moving Fettiment to use frost rather than the current multisig or something?

There was something.

I thought it was you mentioned that.

There's some folks working on that.

I haven't been involved, but I think there's like, there was like some kind of frosty mint exploration.

Frosty mint, I love it.

Something along those lines.

I think there are people interested in that use case, but I haven't worked on it specifically.

Great, thank you.

So what other proofs can a frost quorum do?

We talked earlier that there are some risks for using it for things that, you know, maybe Storm would be able to address.

But I'm just thinking more broadly.

I know that when I brought Frost Knorr forward to the digital identity community, they said, yeah, there's some value for what I think I've heard called non-accountable signatures.

I think that's a Greg Maxwell Peter Woolley term.

I'm not sure if it's a more broad term, but basically a non-accountable signatures.

You don't know who signed, but it's but it is.

The quorum has provably signed it, but they say there's just too many cases where they really need to know who signed it.

For instance, the board of directors or, you know, somebody who is accountable to their decision.

And I didn't know if there was any kind of variant or tricks or whatever that, you know, members of a frost quorum could also do an accountable signature.

It just covers the signature shares.

But I don't know if you can just.

That's enough.

OK, each signature share is independently verifiable against.

People would have to know the participants individual public keys, but that's unavoidable if you want accountability like this.

If you want individuals to have identities that can be accountable, they have to have public keys.

People know what does that still mean?

I mean, are these just, you know, just multiple single SIGs or is this an aggregated snore at the end?

So you get the aggregated snore at the end that verifies with your standard verification.

But in addition, if you care about who contributed to it, you look at the individual signature shares.

I mean, it's linear size and the number of signers.

But that seems hard to get around if you want to end up with a list of signers.

Chelsea, maybe you actually have like you have that paper on this similar topic.

Yeah, that's a little it's a little different because it also tries to preserve privacy of who signed.

I think if you just care about accountability, I don't see why you can like.

Yeah, if you want a threshold.

T out of N, but you want to also know which T signed.

The easiest thing to do would be you publish your frost signature and then you have some ledger or something where you persist information.

I think Big Go does this.

I'm pretty sure because they have threshold signatures, but they also say they have an accountability layer.

So yeah, so you just do frost.

You publish that and then you could either persist the T individual signature shares or we have this nice multi signature scheme called music.

So you can also aggregate them if you don't want T sized storage and then you just need the public keys.

So I think there's lots of ways around this.

It would be a T of T for the for the accountable.

You could only get away with that.

I can't see how you get away with that without having a combinatorially large number of music public keys, like one for each size T possible forum.

Correct.

Yeah, this is something you could do, but you can build a marketable whatever.

But yeah, but I think it really depends on the setting here.

I mean, or on the very fire.

So let's say this is for UTXO on the Bitcoin or the Seacash blockchain.

If you have T malicious signers, they can just reconstruct the key and sign and there is no accountability for this.

So then the money is gone, but you don't have records.

So it really depends on whether you know that you have these records somewhere else.

So there are the partial signatures.

So of course, if you did the signature, you can prove to others that you did the signature, but you.

I'm not sure how useful that is.

So you said there was something that had greater privacy that you were working on as well or something, some variant in this family.

Yeah, I mean, I had a paper a little while ago on private and accountable threshold signatures, but it's more complicated.

So, I mean, it's there, but I think regardless, you're not really going to be able to get around needing some kind of second persistence layer.

That's probably always you're always going to need something like that.

So OK.

I think if you want what?

Sorry, I don't know who was just speaking before that was it Tim.

Yeah, it was.

Yeah, it was me.

Yeah.

Yeah, I don't.

For some reason, Zoom's not showing me who's speaking.

So.

Yeah, if you want it to be the case that you can tell who signed even without their cooperation, you're basically just doing T of N like just T signatures.

Right.

That works.

You just check that you have T individual signatures and you're done, but then you don't get a single signature that also independently checkable.

And then anyway, this scheme is pretty much underrated, right?

It's not compact, but it has everything else.

It has accountability.

It's non-indirect.

Easy to set up, Keith.

No, it's a great.

Yeah.

Except for the linear size of signatures.

Yeah.

It's a perfectly reasonable scheme for T not ridiculously large.

OK.

Yeah.

And I definitely hear some have some interesting thoughts regarding different tricks of using new SIG to with Frost or Frost with you.

So, I don't know if you're familiar with Frost with you, Sigto, that Jesse was talking about earlier.

But is there any other kind of related cryptographic opportunities that we're not thinking about?

You know, adapter signatures and Schnorr, et cetera.

Any thoughts about any of these other variants of Schnorr with Frost?

Yeah.

So, I actually have in the test suite for my Frost implementation using adapter signatures to do multi-hop locks, kind of like PTLCs, where it is, I think, mostly useful if we were nesting Frost inside of music.

But you can definitely do adapter signatures with Frost to create these multi-hop locks, which is interesting.

I also wonder if you could have like, I mean, almost certainly you could also use like a music DN style zero-knowledge proof to get deterministic nonces for Frost is, I think, another thing we could do.

That's interesting.

Anybody?

Go ahead.

Putting Frost inside of MooSig, that would require you to use the trusted dealer variant, like, way to distribute the Frost keys.

I would.

.

.

Oh, no, I'm not.

.

.

Sorry, the other way around.

Correct.

If you have like a Frost inside of Frost or something, rather, then you would have to use the trusted dealer on the, at least on the inner Frost.

But not for.

.

.

Go ahead.

Not for music.

Each subgroup of Frost, if you use the DKG, would produce an independent random key.

And so the outer layer would have to be one that supported independent random keys.

Thank you.

I also want to make sure people are aware of the.

.

.

Not sure.

No?

I mean, I haven't written down the scheme, but I could imagine it works.

I think you claimed an email five years ago that works.

Maybe not five years ago.

So, okay.

Let's talk about it.

So, you want a Frost inside of Frost?

Let's say Frost inside of Frost.

Right.

So, if the inner Frost's all are DKG'd, then they're each going to output an independent random secret key.

But independent random secret keys can't directly be used as shares in a Frost.

It's for Frost, yeah.

Right.

So, you have to do something else there.

There are things you can do.

So, for example, there's this trick that if you.

.

.

Of converting between N of N shares and K of N shares, that involves a combinatorial blowup.

So, and then K can't be too big.

Where in one step, you just basically make a shared secret for every size K minus one group of the N and give that value to each of those people.

And then everybody just adds up their values and multiplies by some well-chosen Lagrange coefficients.

And then magically that turns out to be a Shamir sharing.

And I might have got the minus one and a plus one mixed up somewhere.

Whatever.

Anyway, there's this trick where you can convert independent random values into Shamir shares.

Interesting.

And that's useful because if you've set up your Shamir shares that way, you have this super awesome property that you can forward-secretify them just by hashing all the original independent random values and then adding them up with the Lagrange coefficients again.

And then you end up with a new completely forward-secretly independent sharing of a new random value with no interaction.

So the key update is completely non-interactive.

No messages at all.

It's all local computation, which is super cool at the expense of something like N choose K plus or minus one storage and computation.

So that they are too ridiculous.

That won't work.

But if it's not too ridiculous, that's a neat trick that people should be aware of that can be useful in various situations.

Yeah.

Well, especially guys.

This could be one way to make it work.

Now that I remember, I think we had this we had the discussion about nested frost or frost inside frost a while ago.

But I think, yeah, we didn't talk about DKGs.

We just talked about signing and there.

That's right.

Right.

So you can do it if you use a trusted dealer.

Right.

You just make up whatever keys for the other one.

Then you use that.

Yeah.

Or as you pointed out, if you do frost inside music, that also works because then you have a public right to like.

Interesting.

Well, thank you very much.

It's exactly.

That was exactly the kind of discussion that I wanted to say that there are some opportunities here.

So thank you.

So we're going to wrap up with sort of these kind of open questions.

Obviously, we need to convince more developers to use frost.

What I mean, I know Blockchain Commons wants to help with some of the tooling there because we already have a wallet developer community.

14, 15 wallet developers, different companies.

But obviously, for them to do it, we have to convince users to use frost.

You know, any particular tooling that is not on your immediate roadmap that you would like others to do.

I don't there aren't a lot of developers out there who are ready to do some of the cryptographic code, but they could help with other testing frameworks, CLI apps, etc.

That would help in deployment.

This is something I'm very concerned about.

So I want to congratulate the Zcash team for for doing code reviews.

But how do we collectively do more code reviews?

Whether or not, you know, maybe, you know, our mutual our different teams could code review each other's code in some kind of independent fashion.

How do we get a lot get the different developers and wallet companies, you know, other companies that depend on our code to fund code reviews, I think is an important thing we need to puzzle through.

And then specifically for the roadmap of Blockchain Commons in 2024, you know, what other activities can Blockchain Commons do in addition to hosting activities like this?

I mean, currently, we're hosting a variety of open specifications and references to those open specifications.

We're currently moving a number of those specifications into various standards organizations.

And we also have a variety of, you know, discussion forums and, you know, special meetings, etc.

that we can host.

So just kind of want to leave it open.

You know, did you like this meeting?

You want to do it again?

What what else can we do?

Ideas in the next steps.

You mind if I just call on the people who remain and see if there any, you know, sort of top things on your head.

And Rado, you know, what what what is a next step for this larger community of Frost developers?

Yeah, those are great questions.

Unfortunately, I don't have the answer to most of them.

I think the next step is just getting Frost deployed somehow, somewhere.

Right.

We're trying to do that in the cache by integrating with wallets.

But of course, everyone's working on their own projects.

And I think the next step is getting deployed in whatever form people are working with to help that happen.

I don't know.

I think I think this talk was really useful just for me.

I think it's good to have, you know, run ideas by other people.

So we could have something like this, maybe a couple of months.

But on our side, we just try to get deployed.

The thing that I need more is like I would love to have some super obvious way to do communication between applications.

Right.

Those are ways to do this.

But there's no single obvious ways to do it.

So yeah, we're just figuring out this and analyzing which options.

But yeah, that's not really for having Frost as a digital protocol.

So you need this, but it's not actually really good for us.

But yeah.

On that last one, I know that we're working now on something called collaborative recovery.

And we are hoping to have a server out soon that you can basically do a trust on first use with that can do various operations for you that were basically ledger currently locks you into who your care partners are.

We want you to be able to have choice, you know, put it on paper, store it on a server, share it with a friend, etc.

Now, we're only doing shmear right now.

But, you know, the goal is that this is an open standard and could be used for other things besides shmear shares.

So we'll share that with you.

Chelsea, any thoughts here?

Yeah, I think kind of bringing the yeah, so I think what would be great for blockchain commons to sort of keep doing is the outreach to people who are working on Frost.

That's very helpful.

So I know, like, there's been several kind of, at least I've seen like several things floating around about people who've implemented Frost.

So that kind of like outreach and bringing people together is very helpful.

So yeah, thank you.

I'm hoping that we can do this again in as appropriate in January, February, and maybe with the video of this, we'll get some of the other developers in as well.

I think I invited all the ones I could find.

But I'm glad you're here.

Thank you.

Ian.

All right.

I was just reading Tim's message and I want to talk more about that but no I found this this valuable and yeah having it again in a couple months.

Try to get more people out.

Sounds good.

We're of course, moving forward on to things like storm like I talked about some other things.

Yeah, hope to see good progress in this space.

You can.

I do want to talk about what Tim just posted.

Go ahead.

So, I'm pretty sure that's not the case.

What Tim just posted individual signature shares are in fact, and for J right so you can check.

Okay, this this may depend on on the difference between Frost three and Frost two, whereas music to has shared non segregation like like Frost three.

But for for music to at least I, we have a write up of the of the attack.

So, if you're a participant of the protocol you, you can forge signature shares of other participants.

The trick here is that the signature share includes the hash of or the challenge hash includes the combined nonce and you can do a rookie attack or rock nonce attack on the on the combined nonce.

So, like, it's not a compilation of future me a style protocol again because you don't hash your commitment, but you hash the combined commitment.

Do you see what I'm saying?

So if so I can in the first round I can send nonce for which I don't have to secret key for which I don't have to secret nonce but I can create partial signatures I think I just can't create and partial signatures of course because then oh because in so okay so this doesn't work against classic or Frost one because that's what the individual rows are for is to prevent that from happening.

But if you combine the row yes.

Yes.

Okay, I buy this maybe right this maybe true that this is actually really the difference that which or this was no that was exactly the result of the crypto paper right.

No, that the exact difference between the security of Frost one and Frost two is that in Frost two, you can a participant can make it look like some other participant signed exactly because of that the individual D's and E's aren't separated because you use a common row.

Yeah, that makes sense.

Yeah, that's right.

So for Frost, okay, if you aggregate the rows then that's true.

If you don't aggregate the rows on the individual shares are unfortunate.

Yeah.

Yes.

So, Tim, any next step thoughts?

Things you'd like to see.

Yeah, first of all, I agree with what Carado and Chelsea said I think was nice to have this to have this roundtable and that would make sense to have it again in a few months.

Yeah.

I would appreciate this and I think also the the markdown document that we have, we could use this just to add maybe a few more.

I don't know links to stuff we talked about I think we, there are a lot of things and ideas we work on the different groups here and most of them.

The implementation side are public because they happen on GitHub or another public areas but we often hear about the stuff only when we talk to each other.

So maybe that's an interesting thing to do with a document or we could have some other channels to exchange things.

We'll be glad to help.

Also, PRs are really impossible to find.

Right, for example, PRs are hard to find.

I mean, this TKG repo that, or spec repo that Jonas created two weeks ago is hard to find because we didn't share with anybody.

Of course, it's super work in progress but I mean it's public people can of course have a look.

Another thing, I mean, Lloyd, unfortunately, couldn't be here today, I guess, but his project is what came to my mind when I saw the second bullet on your slide, how do we convince users to use Frost, I think, because those guys have thoughts on, they really have a concrete application in mind with hardware wallets and signing and making it more secure to keep your coins and they might have thoughts on that point.

It's a very interesting project, I think I can share the link here in the chat.

Yeah, I guess that's all I have to say.

Okay, thanks for organizing this.

You're welcome.

Jesse, Nat, anybody else?

Shannon, Wolf?

Yeah, I can jump in.

Yeah, I'd just like to thank you, Christopher, for putting this together is great idea and it has been fantastic and would love to do this again like others have said.

In terms of how to convince developers to use Frost, I think to the extent that we can make our implementations, batteries included, as Tim put it, where we can kind of provide ways of getting these guarantees around broadcast channels and authenticated encryption.

So there's as few foot guns as possible and the implementation is just ready to be used without having to think much more about it, I think is going to help developers feel like they can actually use this stuff safely.

And the other thing is I think these Shamir schemes, these extensions of proactive secret sharing, repairability, enrollment and disenrollment, I think all those add a ton of value to Frost.

I think it adds a lot of cool use cases and I think to the extent that once we kind of have these base implementations in a good shape, I think moving on to how we can layer on these additional protocols, I think will really entice a lot of users to want to use these systems.

Thank you.

I'd like to chime in.

Yeah, from the blockchain comments perspective, as I said, I'm not a cryptography person.

So a lot of what you guys who are into cryptography, I respect a lot, but it goes over my head.

But what I do see is value proposition in various things.

And when we see, and as an engineer, when I see the value proposition in something and when it's being handed to me, you know, batteries included without, you know, with, you know, in general, and I'm either told how to or prevented from using foot guns.

I have a very good experience developing end user apps.

And so my role in blockchain commons is often to take the kind of more the best practices that have already been identified with the cryptographic community and then put them into a context where other engineers like me can say, oh, yeah, this is definitely worth picking up and just using as is right now.

And that's what we're striving to do.

We're not we're not big on so-called crypto agility, where we're trying to support every single cipher suite, every single variant.

We tend to like to establish, you know, one thing that the community has consensus on as this is, yeah, this is at least as good as anything else out there, and it offers unique value and then package it in such a way that people can just pick it up and use it.

So that's, that's what I would like to see the output of this kind of roundtable, this kind of community effort being and a second tangent, I'd say if you work for an organization that would find this that finds an ongoing kind of community development and development of specifications, reference code, and example apps, you know, because we're not, we don't publish final apps or or wallets or things like that, but we do do things that are completely open source that the entire community can access.

Then please consider sponsoring Blockchain Commons, because I think it's a fair amount of bang for your organization's buck.

Thank you.

Okay, well, I think everybody who has left has spoken up.

Again, I, Blockchain Commons.

com has, you know, all of our documents.

There is now a developer sub area which will soon include a new frost section.

I have kind of a snore for the layman article out recently and I'm working on a frost for the layman.

And obviously you can reach me a variety from places, Signal at Christopher A on a variety of channels.

If you have any questions, please reach out to me, and I hope that we'll have YouTube video and transcript and some other notes up, you know, over the today and over the next week.

So thank you very very much for joining us and look forward to meeting again.

Thanks everyone.

Thank you.

Thank you.

Thanks.

Bye.

Bye.



What I mean, I know Blockchain Commons wants to help with some of the tooling there because we already have a wallet developer community.

14, 15 wallet developers, different companies.

But obviously, for them to do it, we have to convince users to use frost.

You know, any particular tooling that is not on your immediate roadmap that you would like others to do.

I don't there aren't a lot of developers out there who are ready to do some of the cryptographic code, but they could help with other testing frameworks, CLI apps, etc.

That would help in deployment.

This is something I'm very concerned about. So I want to congratulate the Zcash team for for doing code reviews.

But how do we collectively do more code reviews?

Whether or not, you know, maybe, you know, our mutual our different teams could code review each other's code in some kind of independent fashion.

How do we get a lot get the different developers and wallet companies,

you know, other companies that depend on our code to fund code reviews, I think is an important thing we need to puzzle through.

And then specifically for the roadmap of Blockchain Commons in 2024, you know, what other activities can Blockchain Commons do in addition to hosting activities like this?

I mean, currently, we're hosting a variety of open specifications and references to those open specifications.

We're currently moving a number of those specifications into various standards organizations.

And we also have a variety of, you know, discussion forums and, you know, special meetings, etc. that we can host.

So just kind of want to leave it open. You know, did you like this meeting? You want to do it again?

What what else can we do? Ideas in the next steps.

You mind if I just call on the people who remain and see if there any, you know, sort of top things on your head.

And Rado, you know, what what what is a next step for this larger community of Frost developers?

Yeah, those are great questions. Unfortunately, I don't have the answer to most of them.

I think the next step is just getting Frost deployed somehow, somewhere. Right. We're trying to do that in the cache by integrating with wallets.

But of course, everyone's working on their own projects. And I think the next step is getting deployed in whatever form people are working with to help that happen.

I don't know. I think I think this talk was really useful just for me. I think it's good to have, you know, run ideas by other people.

So we could have something like this, maybe a couple of months. But on our side, we just try to get deployed.

The thing that I need more is like I would love to have some super obvious way to do communication between applications.

Right. Those are ways to do this. But there's no single obvious ways to do it.

So yeah, we're just figuring out this and analyzing which options.

But yeah, that's not really for having Frost as a digital protocol. So you need this, but it's not actually really good for us.

But yeah.

On that last one, I know that we're working now on something called collaborative recovery. And we are hoping to have a server out soon that you can basically do a trust on first use with that can do various operations for you that were basically ledger currently locks you into who your

care partners are. We want you to be able to have choice, you know, put it on paper, store it on a server, share it with a friend, etc. Now, we're only doing shmear right now.

But, you know, the goal is that this is an open standard and could be used for other things besides shmear shares. So we'll share that with you.

Chelsea, any thoughts here?

Yeah, I think kind of bringing the yeah, so I think what would be great for blockchain commons to sort of keep doing is the outreach to people who are working on Frost.

That's very helpful.

So I know, like, there's been several kind of, at least I've seen like several things floating around about people who've implemented Frost. So that kind of like outreach and bringing people together is very helpful. So yeah, thank you.

I'm hoping that we can do this again in as appropriate in January, February, and maybe with the video of this, we'll get some of the other developers in as well.

I think I invited all the ones I could find.

But I'm glad you're here. Thank you.

Ian.

All right. I was just reading Tim's message and I want to talk more about that but no I found this this valuable and yeah having it again in a couple months.

Try to get more people out.

Sounds good. We're of course, moving forward on to things like storm like I talked about some other things.

Yeah, hope to see good progress in this space.

You can.

I do want to talk about what Tim just posted.

Go ahead.

So, I'm pretty sure that's not the case. What Tim just posted individual signature shares are in fact, and for J

right so you can check.

Okay, this this may depend on on the difference between Frost three and Frost two, whereas music to has shared non segregation like like Frost three.

But for for music to at least I, we have a write up of the of the attack. So, if you're a participant of the protocol you, you can forge signature shares of other participants.

The trick here is that the signature share includes the hash of or the challenge hash includes the combined nonce and you can do a rookie attack or rock nonce attack on the on the combined nonce.

So, like, it's not a compilation of future me a style protocol again because you don't hash your commitment, but you hash the combined commitment.

Do you see what I'm saying?

So if

so I can in the first round I can send nonce

for which I don't have to secret key for which I don't have to secret nonce but I can create

partial signatures I think I just can't create and partial signatures of course because then oh because in

so okay so this doesn't work against classic or Frost one because that's what the individual rows are for is to prevent that from happening.

But if you combine the row yes. Yes. Okay, I buy this maybe right this maybe true that this is actually really the difference that which or this was no that was exactly the result of the crypto paper right.

No, that the exact difference between the security of Frost one and Frost two is that in Frost two, you can a participant can make it look like some other participant signed exactly because of

that the individual D's and E's aren't separated because you use a common row.

Yeah, that makes sense.

Yeah, that's right. So for Frost, okay, if you aggregate the rows then that's true. If you don't aggregate the rows on the individual shares are unfortunate.

Yeah.

Yes.

So, Tim, any next step thoughts?

Things you'd like to see.

Yeah, first of all, I agree with what Carado and Chelsea said I think was nice to have this to have this roundtable and that would make sense to have it again in a few months.

Yeah.

I would appreciate this and I think also the the markdown document that we have, we could use this just to add maybe a few more.

I don't know links to stuff we talked about I think we, there are a lot of things and ideas we work on the different groups here and most of them.

The implementation side are public because they happen on GitHub or another public areas but we often hear about the stuff only when we talk to each other.

So maybe that's an interesting thing to do with a document or we could have some other channels to exchange things.

We'll be glad to help. Also, PRs are really impossible to find.

Right, for example, PRs are hard to find. I mean, this TKG repo that, or spec repo that Jonas created two weeks ago is hard to find because we didn't share with anybody.

Of course, it's super work in progress but I mean it's public people can of course have a look.

Another thing, I mean, Lloyd, unfortunately, couldn't be here today, I guess, but his project is what came to my mind when I saw the second bullet on your slide, how do we convince users to use Frost, I think, because those guys have thoughts on,

they really have a concrete application in mind with hardware wallets and signing and making it more secure to keep your coins and they might have thoughts on that point.

It's a very interesting project, I think I can share the link here in the chat.

Yeah, I guess that's all I have to say. Okay, thanks for organizing this.

You're welcome.

Jesse, Nat, anybody else? Shannon, Wolf?

Yeah, I can jump in.

Yeah, I'd just like to thank you, Christopher, for putting this together is great idea and it has been fantastic and would love to do this again like others have said.

In terms of how to convince developers to use Frost, I think to the extent that we can make our implementations, batteries included, as Tim put it, where we can kind of provide ways of getting these guarantees around broadcast channels and authenticated encryption.

So there's as few foot guns as possible and the implementation is just ready to be used without having to think much more about it, I think is going to help developers feel like they can actually use this stuff safely.

And the other thing is I think these Shamir schemes, these extensions of proactive secret sharing, repairability, enrollment and disenrollment, I think all those add a ton of value to Frost.

I think it adds a lot of cool use cases and I think to the extent that once we kind of have these base implementations in a good shape, I think moving on to how we can layer on these additional protocols, I think will really entice a lot of users to want to use these systems.

Thank you.

I'd like to chime in. Yeah, from the blockchain comments perspective, as I said, I'm not a cryptography person. So a lot of what you guys who are into cryptography, I respect a lot, but it goes over my head.

But what I do see is value proposition in various things. And when we see, and as an engineer, when I see the value proposition in something and when it's being handed to me, you know, batteries included without, you know, with, you know, in general, and I'm either told how to or prevented from using foot guns.

I have a very good experience developing end user apps. And so my role in blockchain commons is often to take the kind of more the best practices that have already been identified with the cryptographic community and then put them into a context where other engineers like me can say, oh, yeah, this is definitely worth picking up and just using as is right now.

And that's what we're striving to do. We're not we're not big on so-called crypto agility, where we're trying to support every single cipher suite, every single variant.

We tend to like to establish, you know, one thing that the community has consensus on as this is, yeah, this is at least as good as anything else out there, and it offers unique value and then package it in such a way that people can just pick it up and use it.

So that's, that's what I would like to see the output of this kind of roundtable, this kind of community effort being and a second tangent, I'd say if you work for an organization that would find this that finds an ongoing kind of community development and development of specifications, reference code, and example apps, you know, because we're not, we don't publish final apps or

or wallets or things like that, but we do do things that are completely open source that the entire community can access. Then please consider sponsoring Blockchain Commons, because I think it's a fair amount of bang for your organization's buck.

Thank you.

Okay, well, I think everybody who has left has spoken up.

Again, I, Blockchain Commons.com has, you know, all of our documents. There is now a developer sub area which will soon include a new frost section.

I have kind of a snore for the layman article out recently and I'm working on a frost for the layman. And obviously you can reach me a variety from places, Signal at Christopher A on a variety of channels.

If you have any questions, please reach out to me, and I hope that we'll have YouTube video and transcript and some other notes up, you know, over the today and over the next week.

So thank you very very much for joining us and look forward to meeting again.

Thanks everyone.

Thank you.

Thank you.

Thanks.

Bye.

Bye.

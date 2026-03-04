---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "XIDs & Garner"
title: "March 2026 Gordian Developer Meeting Transcript"
hide_description: true
classes:
  - wide
permalink: /meetings/2026-03-gordian/transcript/
sidebar:
  nav:
    - meetings
---

**AI Transcription:** _Automated transcription (App: Mac Whisper 13.8, Model: Large v3 Turbo w/Speaker Recognition)_

**AI Processing:** _Medium cleanup (App: Claude Code, Model: Claude Opus 4.6)_

**Note:** _This transcript has been moderately edited for readability - filler words removed, errors fixed, fragmented sentences cleaned up, paragraphs added, technical terms standardized. Original content and speaker voice preserved throughout._

**Christopher Allen:** Welcome everybody to our meeting of the Gordian developer community. We hold these monthly on the first Wednesday of the month, despite that little typo in the bottom of this first page. Today is March 4th, 2026. Today, our main presentation is on the extensible identifier, or XID, and something new, which is the Garner server. These are both technologies that we've created to better support self-sovereign identity.

Who am I? Well, my name is Christopher Allen. In recent years, I've been deeply involved with the architecture of the W3C decentralized identifier, which is an international standard. But in many ways, I feel like the community has gone wrong as it's been deployed. So XIDs and Garner are really some of the tools that I've been working on to move, or at least inspire, the international standard of DIDs back to their first principles.

What is Blockchain Commons? We're a community to bring together stakeholders to collaboratively build open, interoperable, secure, and compassionate infrastructure. We design decentralized solutions where everybody wins. We're a neutral not-for-profit to enable people to control their own digital destiny. We've been broadly focused on creating independence, privacy, resilience, and openness for digital assets. And we've had good success in a variety of digital asset communities, in particular around wallets and standards.

But we really have not done a lot of work on identity until the last year or so, mostly because we didn't have an identity financial sponsor. But I felt like the needs were becoming so great that I had to talk about the problems of SSI more, even without funding on this topic. And that is what has led to some of this work. Now, you can of course help us by becoming an identity sponsor. Talk to me if you would like to become more involved in this work. And of course, many thanks to the organizations that have helped support the wider work that Blockchain Commons does.

Before we dive into some new tools for digital identity, I wanted to share with you about our new 2026 technology overview video. Over the last several years, Blockchain Commons has developed numerous technologies meant to improve the Gordian principles of independence, privacy, resilience, and openness on the internet. But it's getting quite complicated. So this brand new 2026 technology overview video describes each of 24 different technologies, applications, and references, each in only about a minute. It really is the most comprehensive and accessible look at our entire Gordian technology stack to date. So if you're trying to explain to a colleague what our tools enable, why they should come to these meetings, why they should become involved — this 22-minute video is a great place to start.

So let's talk about self-sovereign identity itself and how I'm trying to return to first principles with the design and architecture of XIDs. This is likely a review for many of you, but the modern idea of self-sovereign identity was largely founded in an article I wrote in 2016 called "The Path to Self-Sovereign Identity." This happened just before the second Rebooting Web of Trust, which we ran in conjunction with ID2020, which was the first United Nations conference on digital identity. So we had a great audience for these principles.

The idea was simple: digital identity is central to our online presence, thus we should control it, because we are not digital serfs. These 10 principles for self-sovereign identity basically say you, not a gatekeeper, should have total access and control over your identity. Your identity should be long-lasting, and you should have a choice in how it is used and be able to move it to different places. That doesn't mean you can control what other people say about you, just that you control what you say.

Unfortunately, self-sovereign identity, as I imagined it back in 2016, seems to be failing. And I think in large part, it's due to developers neglecting these 10 principles. Lately, I'm finding that issuers have way too much control. They're the only ways to create identities, and they get to decide what you must reveal about your identity. They've also built in other forms of centralization and even sometimes phone-home behaviors into their supposedly decentralized identifiers. In particular, KYC, age verification, and collection of your data because it, quote, "has a legitimate business purpose," has become normalized.

So I built XIDs to be a reference implementation for how self-sovereign identity can be done right. They're totally self-contained and totally controlled by the holder. No one else is involved, so there is no possibility for centralization or outside control. Looking back at the 10 principles of self-sovereign identity, the XID ensures that a holder has access, transparency, and control. They support data minimization, and they're portable and interoperable as long as our low-level specifications, URs, and envelopes are being supported. Thus, I think XIDs are very important, not just as an alternative to the DID spec, but as a promise of what the DID spec could be. I've jokingly called it DID 3.0 — and we're at like 1.1 now.

In order to explain how this works, we've been working on a whole course called Learning XIDs from the Command Line, which right now is at learningxids.blockchaincommons.com. It follows the format of our popular Learning Bitcoin from the Command Line course. So I encourage you, if you want to learn about it, that's a good start. We'll be adding more to it as the weeks go on.

So that's XIDs in a nutshell. But really, they're just a first step because there's a lot more that is necessary to ensure that self-sovereign identity truly remains self-sovereign. In regards to your agency and self-sovereign identity, you can control an identity, but you still need to be able to publish it when you might not be able to guarantee the infrastructure — because the infrastructure might be compromised, censored, or just absent. Assuring that your self-sovereign identity can enter into a self-sovereign network is what this is all about. This requires us to rethink the internet.

Our Learning XIDs course offers GitHub as a simple starting point. You can just publish an XID to your GitHub, update it as you see fit, and prove control of the GitHub account with registered signing keys. But it's still far from a perfect solution. GitHub is already censored in some fascist regimes. And even though it's trustworthy right now, we don't know that it'll always be the case in a year or 10 years.

This is where our newest technology comes in — Garner, which is a Tor Onion service specifically built for serving identity documents in a self-sovereign way. You control what's transmitted. You prove control with key pairs. And you can't be censored unless Tor is entirely blocked. And even if Tor is blocked, Garner is just one solution we have for transmitting identity information. Take a look at our fall 2025 videos on Hubert for another way to communicate to networks without centralized gatekeepers.

Garner is really easy to use for a developer. One of the reasons why we suggested options like GitHub is because creating websites is very hard for the average user. You need to buy a DNS record, manage SSL certs, and all of this before running Apache or some other complex program. Obviously, you can have some gatekeeper do this for you. But to be truly self-sovereign, that means you have to maintain it. So we really want to make that easy.

For Garner, there are just really simple steps to make it work. First, you place the files you want to publish in a public directory on your hard drive. You install Garner, which is done through a Rust crate with a single command, making it an automated process. Third, you generate a key pair — something you only have to do the first time — and then you can start the Garner server with your private key. Finally, you give the public key and the file name to other people so they can use Garner as a client to access your files.

There's no DNS to set up, no SSL cert, no complex Apache configuration. It's just a directory of files and a key pair. And you can create an index file to make things even simpler. It isn't a full web server. We're not trying to reproduce that. It's a very narrow surface of attack, keeping it really simple. The Onion address will be generated deterministically from the key that you generated. The advantage of giving out keys is that they can be used to prove ownership. But giving out addresses also makes things simpler. Other people can use Garner with the Onion address or with the public key from the command line, or use a Tor browser to download the files.

We can see how the self-sovereign principles are supported. You are in control of everything. As long as you have your keys, you can start up your Garner server anywhere. And anyone who has that public key or address can access it. There are no gatekeepers in your way. And thanks to the privacy of Tor, it's censorship and coercion resistant.

Thus, I feel Garner is a new step in self-sovereign identity. It allows for self-sovereign networking and self-sovereign publication. Here is the trilogy of articles that define how I've looked at self-sovereign identity over the years. First, my 2016 intro to self-sovereign identity, including the 10 principles of self-sovereign identity, and my more recent articles on how the ecosystem has failed to follow the principles of SSI, and then my most recent article on how I think XIDs can turn that around. Of course, we've got a lot more in our developer pages, including sublinks to other materials related to XIDs and Garner, our demos we did last year of Hubert for dead drop protocols, and lots of other details. So here's the QR code for the XID and the Garner pages for more info.

I'm happy to take questions on these technologies or discuss some of the underlying principles and goals of self-sovereign identity and the problems that it's facing today. Any questions? Anybody using Tor now?

**Dan Pape:** Fortunately, not really. I was going to ask — I haven't looked at this in detail since you've kind of come out with it, Christopher — but is this similar to, I don't know, I remember something called the magic wormhole. Is that kind of the idea where it's just a really easy way for two people to share data between each other? Or is the Garner thing supposed to be like, is it going to run for days and days in case somebody's looking for your key?

**Christopher:** Yeah. So it serves a particular space. First off, you can create an ephemeral key and it's a very lightweight service. You just basically turn it on, share your envelope information, and then send your public key on Signal or on various kinds of channels. And then when the server comes down, you do another ephemeral key. So in that sense, it can be used in a very ephemeral fashion.

It's not quite the same as the magic wormhole in the sense that I believe that's more of a peer-to-peer protocol. But part of the problem is, how do you bootstrap the peer-to-peer protocols? Because you really have to have keys from both parties. Garner allows you to basically have your public key in a fashion inside an envelope, which can have all the other kinds of keys for key agreement, for verifying that you actually are the controller of the @ChristopherA account on GitHub or other different types of things, which then you can bootstrap into peer-to-peer protocols, including Hubert, which is a dead drop protocol where you're not ever directly talking to your peer. Instead, you're meeting in the middle and dropping off small objects to share. I think that's the ultimate in security.

You can also, of course, save the ephemeral key that you created and start Garner again for a more persistent server, and use that as an alternative key inside your XID document. Again, XIDs are designed to be rotatable. They're designed to support key separation and all the best principles here. So that way you're not locked into a single root key.

Some of this is also in response to protocols that aren't rotatable, like Nostr and some of the other different types of things that kind of scare me with single keys. There are some really good things happening there, but you still have this problem of, "Well, I have to now choose a Nostr server to be my gateway. And yeah, I can choose other servers, but then how does this server know that I'm now no longer using them and I'm using this one?" So I wanted to have some self-sovereign bootstraps.

Another important point for those people who are doing the current DIDs is that although Garner was designed with envelopes in mind, there's no reason why you can't offer a DID controller document or a VC controller document or verifiable credentials through Garner. That basically allows you to have a distribution point for your things that you want to publish and do so in a difficult-to-censor way. So I think it has some useful capabilities outside of just the envelope and the Gordian ecosystem. That helped, Dan?

**Dan:** Yeah, yeah, thank you. That's pretty neat. So I guess I go back to what I asked. Could Garner be used as a persistent, like running 24/7 type of thing?

**Christopher:** Absolutely. It's more of a command line helper tool at this point. Right now as a command line tool, you would basically push it into the background and it'll persist until the next boot, and then you can use a startup script or whatever. It's very, very lightweight. My hope is that you could at some point even run it on an Apple Watch, which has very low persistence. But that may be long enough.

That's kind of the issue. Hubert's the same thing — with Hubert, you're leveraging at the base the BitTorrent DHT, which only has a liveness of about four hours. So you can do that on an Apple Watch. You basically post it, the Apple Watch stops knowing about the network when it's idle, and it's still around for four more hours. You need to have a pinning service in order to have it last longer than that. And then IPFS is kind of the next notch up where you can post something and it's up for three, four days, which serves a different set of purposes, and you can have slightly larger data.

You can have these be hybrid. You can have an association between them. And then all of these you can pin with a service that basically offers it continuously. I think they're just different scenarios for when, "Hey, I just want to for four hours make something available to my colleagues to be able to communicate with me" — say, to do a FROST Bitcoin coordinator between us — "and then I want it gone after that point." And the fact that with Hubert and these types of things, they're effectively random numbers as far as to the rest of the world. There's some real value to it.

**Shannon Appelcline:** I think maybe we should drop a diagram at some point that shows the difference between Hubert and Garner. But most generally, in response to what Dan said, Garner is really intended as the persistent one compared to Hubert, which, as you said, has shorter times between a couple hours and a couple of days. So I think what Dan was asking is explicitly what was intended with Garner — that it'd be more persistent, even running it all the time if you want.

**Christopher:** Yeah. I think the other aspect is that Garner is also one-to-many. You can absolutely, within Gordian Envelope, address different things to different people, elide different things. But it can be incredibly powerful just simply to have a root envelope that has nothing public in it — it's all elided. But then when you have other kinds of communications, you can basically just have the inclusion proof that, "Yes, I'm making a statement, I'm entering into an interaction with you." But you want to check on any of my bona fides? Go to Garner, and it can prove that I've made a commitment to that a long time ago. And I currently have the keys, because you literally cannot connect to an Onion site unless you have the keys.

So I think that just offers something a little bit different. Whereas Hubert tends to be, at least in the current incarnation, a peer-to-peer protocol. So you have to know something about your peer. And only then can the two of you communicate.

**Dan:** Yeah, thanks. That's helpful.

**Shannon:** And the Learning XIDs course that Chris mentioned earlier has a lot of what he just described. Chapter two talks about how to make claims. And then it talks about how to make claims, elide them, and make commitments to them and let people do inclusion proofs. And then the section Christopher and I are working on right now shows how to encrypt it instead. So that shows kind of how these XIDs can be used for all of these different possibilities that Christopher was talking about.

**Christopher:** Also sort of related to that is that you can obviously encrypt things to specific people. So if you have their public key, you can add it into an envelope so that only they see it.

Several of you are aware of the Amira use case. It became a W3C Credentials Community Group note, which talked about the use case and the flow of Amira, who is a Syrian immigrant living in Boston working for a bank as a software engineer. She wants to be able to participate in advocacy software, but putting her name out there is a risk, and her bank isn't particularly happy with her doing work on the side. So she needs a pseudonymous identity to be able to participate in the creation of software.

That was published in, I want to say, 2018. It's a use case — it doesn't have specific technology. A large part of the Learning XIDs from the Command Line is basically going, "We can do all 15 parts of that use case. And it works." Any other questions, Christian? I see a couple of people I haven't seen before. AFO. I got a quick question. Sure.

**Rich Streeter:** How does quantum threaten this?

**Christopher:** So there are a couple of different issues here. First off, the Gordian stack already supports post-quantum crypto. If you choose to use it, it's just an option and it works now. So that's one answer.

The problem is that when you move to post-quantum, you lose certain performance and size advantages, which means right now it's going to be really hard for Bitcoin and Ethereum and other protocols that rely on high performance and small size to function. So I think we're going to be using keys that are potentially breakable by quantum computers for a long time.

That being said, if keys are ephemeral and signed objects are only out there for a few hours, then the only possible attackers are major funded sovereign states that can do things like have yottabyte collection centers in the desert of all of everything that is trafficked. And even then, with perfect forward secrecy and other different types of things, they still have to do a lot of work.

So I think there's a continuum. Right now, we're trying to make things oblivious to easy attacks — anything has to be a future attack. We have the two ends. The other extreme is you can go ahead and use the quantum protocols now, at lower performance and larger size. And then between that is perfect forward secrecy, which is something on our roadmap where a compromise of a key now does not compromise things in the future. I think we know how to do it. It's just more of a roadmap and funding priorities. That help, Rich?

**Rich:** Yeah. So the other question I have is — and this is going back to what I was looking at with the ID2020 stated goals, which were a little bit different than yours — how does this play in scale to disadvantaged populations?

**Christopher:** Right. Before the use case that we published in Amira, there was another one called the Joram use case, J-O-R-U-M. The Joram use case was very much around the refugee problem, where they can't even necessarily afford a cell phone. So we've definitely been keeping that in mind. It's part of the reason why we made some early choices three, four years ago in the Gordian stack that we wanted everything to be very capable of being run on low-powered devices. We demonstrated with LetheKit and other different types of tools that this was functional and you could do that. So that means you can have devices that are priced in the expensive smart card category. And these will only get cheaper. But again, you have limitations. You may not be able to do perfect forward secrecy. You're definitely not going to be able to do quantum on these cheap devices. But it's definitely been part of our DNA of the Gordian stack to keep that in mind.

I think part of the reason why we focused on the Amira model was that right now we need more developers. We need more people working with these tools, trying them out, pushing back on my architecture designs and going, "Well, wait a second, KERI does witnesses this way." I love KERI. And I basically say, "Well, we do witnesses in a slightly different way. And here's why." But the architecture allows you to use either of them. It's those kinds of things that we need from the community, and having a developer-focused use case now really enables broader discussion by the community.

Hopefully, we'll be able to do things. There was one of the companies who was a longtime sponsor who got bought by the Oura Ring folk. They were basically putting a self-sovereign key device with biometric — when you slid your ring on your hand, it would read your fingerprint and then enable like a Touch ID on an iPhone. And their target was $100. So maybe we'll see these types of things be available in the coming years and we can address some of the 10 billion problem.

**Rich:** That help? Yep, thanks.

**Shannon:** I'd also say that, of course, one thing we should recognize is a lot of the work we do is reference. We're kind of trying to show how things could be done by people in the wider ecosystem. But one of the things we do with Envelope is we allow passwords to be stretched out to keys, which is not necessarily the most secure way to do things. But it's a great way to do things if a refugee just wanted to encrypt their documents in a way that they'll be able to recover them without necessarily having a key. I think when we wrote Joram, we talked about maybe them recalling song lyrics or something which could be used, which is the same theory of stretching that out into a key.

**Christopher:** Yeah. Well, the other thing you can do — we did at least a prototype, it needs more work and it just hasn't been a priority — but we wrote something that basically turns the BIP-39 style root seed into a memorizable iambic pentameter poem. I did some research on what makes things memorable, what kinds of tricks can we do to encode that. It's larger than seed words, but much more memorizable. And lots of little things like avoiding words that people who speak other languages, in particular the Asian languages, have a hard time pronouncing. So you could speak them over the phone or in a private Signal conversation. That would be more likely. Things of that nature.

So yeah, in theory you memorize the poem, you cross the river to the free side, enter it onto your device on the other side, and boom — you've recovered your self-sovereign identity. Anyhow, there's a Python prototype. Gosh, at this point, I think it's probably seven years old. And it's been on my very large to-do list to finalize that and make that a formal spec. Christian, any questions? Anybody else?

**Dan:** No, no, this time, thanks Christopher. Thank you.

**Christopher:** Well, again, please spread the word about what we're doing. And if you've got ideas about what you would like to see us doing in the coming year, where you'd like us to focus, let us know.

We're kind of in the roadmap stage right now. I think we worked really hard to end 2025 with really our whole stack — that was the whole point of that video. I think we finally can say from the bottom to the top that we have something representative in all the major categories as references and functional code. So this year, we really need to be looking at, "Okay, so where does the community want us to focus?" Do they want us to focus on FROST coordination? On more with Garner? More with Hubert? There are some proposals for figuring out how to enable the elision capabilities for things like endpoints and kind of backdooring it into the DID 1.1 standard. But it could be that we need to be focusing on what we're doing with GitHub and with the open integrity, using old-fashioned ED25519 keys for supply chain security and protecting developers.

Right now, there's a Linux Foundation initiative associated with the Linux kernel of requiring a lot more information from developers in order for them to write code. This could be something that is good for securing the code. But I think they're really not thinking carefully about developers and their issues of privacy in their work. And I'd like to maybe see that be addressed.

Okay, last call for questions. Okay, go ahead, Dan.

**Dan:** Oh, I was just unmuting to say thank you and see you next time.

**Christopher:** Yeah. See you guys in a month. Talk to you later. Great. Bye. Bye everybody.

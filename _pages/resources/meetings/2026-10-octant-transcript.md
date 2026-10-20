---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/otherresources.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "Privacy & Ethereum"
title: "Octant Epoch 13: Introductions Transcript"
hide_description: true
classes:
  - wide
permalink: /meetings/2026-10-octant/transcript/
sidebar:
  nav:
    - resources
    - meetings
---

- **Christopher Allen**: Blockchain Commons, Gordian Stack
- **Chris McCabe**: co-founder, Session
- **Zach**: founder and executive director, Unredacted (FreeSocks)
- **Graven** and **Nico**: Octant, hosts

The recording has no speaker labels. Speakers were attributed from cues in the audio, then each turn whose speaker the words did not settle was checked by voice: a speaker embedding (Resemblyzer) of the turn was compared with reference embeddings from unambiguous turns by each of the five speakers, and every attribution made that way matched its speaker at 0.87-0.99 cosine similarity with a margin of at least 0.1 over the next speaker. Turns of four seconds or less are too short for that check; those still marked **Host (Graven or Nico)** or **Speaker uncertain** are among them. `[unclear: ...]` marks a word the tool misheard where the intended word is not certain; the raw rendering is kept inside the brackets.

## Opening

[00:00:00] **Graven:** Hello everyone. Welcome back to our third livestream for Epoch 13. Have another exciting lineup with a couple of Chrises and a Zach. And we'll be discussing Session, Gordian Stack, and FreeSocks today. Epoch 13, for those not familiar yet, if you're just getting tuned in, is Octant's Privacy Epoch. We're focused on funding things that are helping advance privacy both on Ethereum and the open internet. And so we have some great projects all across the board. We're trying to intro, give you an idea of all the amazing work that they're doing in these intro live streams.

And then on this coming Wednesday, which is the last day of the Epoch, we'll be doing some more demos and like a huge run through of all 13 projects. So we, with that said, yeah, we're two days away from the round wrapping up. Nico, thanks for joining again.

[00:01:04] **Nico:** Yeah, thank you, Graven, for having me. Super excited to be chatting with a few more projects on this round.

[00:01:11] **Graven:** So yeah, I love how it's going so far. So I think we're off to a really good start in the momentum builds with quadratic funding, especially, I guess, with how ours is run. The matching multipliers now are, if you donate a dollar, probably going to get a 50x match. There is a minimum donation, but yeah. So the big matching multipliers, all these projects are doing great things. And so epoch.Octant.app is the URL where you can go build your ballot. You don't have to be a long time GLM locker or Octant community member.

You can vote with ETH or wrapped ETH in your wallet. It's definitely want you to all check that out. And with enough prelude, I guess, I think it's time to shift to the main show, our projects that are really fighting the good fight and bringing privacy to users and developers and throughout the stack, both on Ethereum and the open Internet.

## Project intros

[00:02:14] **Graven:** So let's start, I guess, at the top of my list. I had Session and Chris, I'll go, I'm going to call you Chris and then we'll go Christopher to keep it. But Chris, why don't you tell us a little bit about yourself and Session and how you ended up in the Octant round?

[00:02:31] **Chris McCabe:** Yeah. So hello, everybody. I'm Chris from the Session project. I'm one of the co-founders we've been building since 2018. Session has gone through just a huge endeavor to get to this point. And how do we get into the round? We just, someone reached out to us on Twitter, on X, and we thought, yeah, why not? This sounds like a perfect fit, very in the Web3, but also very in the privacy space. And we do need help. So perfect timing to try and raise funds to help the project thrive on.

[00:03:07] **Host (Graven or Nico):** Oh, no, you're on mute. I cannot hear you, good sir.

[00:03:11] **Speaker uncertain (the muted host, or Chris McCabe):** I try to mute the background, but yeah.

[00:03:14] **Graven:** Why don't you tell us just a little bit about Session, a little bit more about Session?

[00:03:18] **Chris McCabe:** Yeah, I have to. So Session is a decentralized private messaging application. It uses a public-private key encryption. All the messages are onion-routed and go through decentralized node networks. And all the messages and communications are stored temporarily for 14 days. So we kind of thought many years ago, what does the world need in terms of private communications? And it was building an app where not even the developers can read your messages. There is no way, shape or form, that we can get your metadata, get your messages, and use that against you.

And that's the fundamental difference between pretty much Session and everything else that's out there is that by design, we cannot, no matter if we got arrested in France, there's no data that we are holding that we can give over that will compromise your privacy. And I think the design is what sets it apart.

[00:04:09] **Graven:** Cool. Good stuff. And let's kick it to Christopher to do a little intro. I think he has a video, but why don't you tell us, give us your name and what you're doing and we'll go to the video.

[00:04:22] **Christopher Allen:** Sure. So my name is Christopher Allen. In 2016, I wrote the 10 principles of self-sovereign identity. And since then, at Blockchain Commons, we've been building open tools to make that real. In particular, this year, Vitalik and the Ethereum Foundation said that it was the year of self-sovereign identities. And that's part of the reason why they invited me to participate in the broader Ethereum community. And I appreciate the opportunity to participate in this Octant round. I've made a three-minute video to show a little bit of what this is about.

[00:05:04] **Video narration ("Keep the Promise", 3 minutes):** I wrote down 10 principles for a movement that came to be called self-sovereign identity. The ultimate moral authority on your identity is you, not a gatekeeper. You should control it and have a voice in what you reveal and to whom. We all have inherent dignity simply because we are human. We are not digital serfs. Ten years later, we have let that promise break. The issuers decide what you must reveal. Identity checks and data collection have become normal. And you can't take your credentials from one provider to another.

Each asks you to trust a platform that sees everything. At Blockchain Commons, we build the tools that keep the promise, our Gordian Stack. They make privacy a property of the data itself. These tools are for people like Amira, a developer in the United States whose family is still in Syria. She wants a reputation for her work, but using her real name could put them at risk. You should show only what a task requires. Gordian Envelope lets you elide, or provably redact, any part of a signed document, and the signature still checks.

Your identity shouldn't depend on anyone's registry. A XID is an identifier you make yourself from your own key. You decide how much of it anyone sees. A key kept in one place is a single point of failure. SSKR splits it into shares for people you trust. Any two of three bring it back. A key that never touches the network can't be stolen over it. Animated QR codes carry data across that air gap. These are only part of our Gordian Stack, but together they support what I call Exodus Protocols, rules in mathematics, not policy, and everything yours to take when you leave.

These tools already ship, and they work together. Our Uniform Resources and animated QRs are in more than a dozen wallets, and the Passport Prime hardware wallet is built on most of our stack. But open infrastructure has a problem. Nobody owns it, so nobody is obliged to pay for it. In 2014, OpenSSL, the encryption under much of the web, had one full-time developer. Then came Heartbleed. It keeps working only while the people who care keep it working. We maintain these libraries, write the documentation, and bring new tools like Garner, Hubert, and Gordian Clubs to their first users.

If you believe you should control your digital self as you do your physical self, scan the code and support Blockchain Commons. Help us keep the promise.

[00:07:58] **Christopher Allen:** So, what you just saw isn't an app, so we're not like Session or a service. It's a layer underneath apps. How keys are held, how data is shared, how wallets talk with each other. Most of our customers, you might say, are wallet developers that build on these tools, and their users thus get the privacy without ever knowing our name. It works on any blockchain. Right now, we have a lot of Bitcoin wallets, some Zcash wallets, Polkadot, and just beginning to be adopted in the Ethereum community.

So, that's infrastructure that nobody owns, which is why rounds like what Octant is doing now really matters. And I hope that you will participate in the allocation until 8 p.m. tomorrow. Or Wednesday, excuse me.

[00:08:51] **Graven:** Wednesday, yeah. But get it in by tomorrow, too. Great stuff, Christopher. Really appreciate it. And yeah, beyond just doing Ethereum and open internet, we also have, throughout the stack, we have different types of projects. All important, all touching different parts. So, definitely there should be something in the round that speaks to you. And we definitely recommend supporting multiple projects in your ballot. With quadratic funding, your small contribution can go a long way. We'll get back into some technical stuff, but let's give Zach an opportunity to introduce FreeSocks.

[00:09:28] **Zach:** Yeah, hi, I'm Zach. So, I'm the founder and executive director of Unredacted. We build internet infrastructure for privacy and anti-censorship purposes. So, we run a lot of different things like Tor relays, Tor bridges, and we try to do it at scale, at a large scale. So, it's kind of we're trying to do it like a little bit more than the average hobbyist could do. So, we're trying to push a lot of traffic to support those networks. And we also built our own open source VPN that we call FreeSocks.

So, what we're building really is a control plane where people can take the code and they can actually spin up their own VPN service. And essentially, any kind of, like, VPN backend that has an API, you could potentially connect that to FreeSocks and manage the nodes, manage the users. It can do the billing for memberships, it can do bandwidth allocations, that kind of thing. So, we're really excited about this being an open source control plane that really anybody can use. And primarily, our purpose really is to provide it as a free service where you can sign up very easily.

You don't have any kind of, you don't have to enter any kind of identifying information. You just kind of, you generate an account number and that's it. You don't put in your email, your phone number. Even the payment methods can be anonymous if you want. You can pay in different cryptocurrencies. And there's a lot of different features that it has and we're constantly building. So, it primarily has an anti-censorship focus where places like Iran, Russia, China, Myanmar, and lots more people from those countries can access the open internet.

And they can get online. And it can be very difficult for people in those countries to actually get on a VPN because many of the traditional VPNs, they don't work in those countries. The censors, the governments in those countries, they can fingerprint the different protocols like WireGuard, for example, or OpenVPN. Those don't really work well or at all. So, that's kind of what we're focusing on is really more advanced protocols that they look identical to normal web traffic and can punch holes in these governments' firewalls.

And for everybody else around the world that doesn't live in that kind of environment, it works as a VPN that can protect your privacy. You can mix in your traffic with other users and inherently that's enhancing your privacy in a lot of ways. So, yeah, that's primarily what we're working on there.

[00:12:37] **Graven:** Yeah, good stuff. Yeah, Christopher mentioned the EF this year and some of their focus and they have the CROPS kind of Ethereum commitment, censorship, resistance, and privacy. Two sides of the same coin are often complementary. So, great to see you guys pushing in to places that need privacy, even going beyond privacy. So, really good stuff.

## Stories from the field

[00:13:02] **Graven:** To get started, I guess, let's kind of, this might be hard sometimes with privacy. You don't get all the opportunity to talk to your users because they're inherently not trying to be found. Let's just go around and if you have a favorite use case or like a context where your product has been used and has made a really big impact or just something that was really cool and unexpected. Let's share some stories from your products out in the wild.

[00:13:32] **Chris McCabe:** I've got a fun one. Back in 2022, I think there was a lot of civil unrest in Iran and the Iranian government started blocking Signal, Telegram, WhatsApp, and any sort of messaging application that required a phone number to set up an account. And then Session just cut through all these sort of blockages and we started getting about half a million new downloads per day. And just kept growing and growing and growing for about two weeks until the government sort of, I think they worked out how to take Session off the app stores.

And I thought after that, I was like, wow, it's still like the user base still was growing, but we didn't know where it was coming from after that. And then I remember traveling after that and people said they'd heard of Session or their friends are using Session because they had no way to talk to their family in Iran. And it was this sort of became this portal for them. And that was a really, that was a cool feeling to be like, oh, great. It's done something that we designed it to do to help people to communicate when there's like overreaching government trying to control people.

So that was a nice feel. Yeah.

[00:14:43] **Christopher Allen:** I'd like to follow that up and sort of also connect the, all three of these projects. I mean, what we're really talking about is how do we interact, whether or not it's financially or via communications or coordinate in adversarial environments. What, and those that the adversaries may be very minor or they may be really big. And they're also constantly getting better. So for instance, Session, one of the things it really tried to do is make it such that you weren't using, like Signal has a Amazon infrastructure and you have to have a real phone number and all that type of stuff.

So they eliminated that and then the FreeSocks is basically trying to hide traffic inside these a variety of existing protocols, et cetera. And so this is all really important things, but we have to think where is this, where is this going? What are kind of worst cases, what Vitalik called recently, sanctuary technologies. So we've been working on a research of something called a dead drop protocol. It's called Hubert. And what it does is it doesn't even directly connect the two users. It basically uses things like BitTorrent DHT and IPFS or even Instagram photos to basically allow certain kinds of communications to happen between Multiple people in an adversarial environment where no way they can be correlated together.

And this is not necessarily required today, but if Taiwan, God forbid, gets invaded by China there, we want to have and be prepared with sanctuaries that allow for communications to continue to happen. And that's what we're all trying to do.

[00:16:39] **Zach:** Absolutely. Yeah, I think that's really interesting. I'd like to talk with you more about that. It sounds right up our alley.

[00:16:49] **Zach:** Yeah, I guess one story that we, that I have that I think is kind of interesting is that we've, we had one user that reached out to us and said they've been using FreeSocks for a few years. And they actually started using it because of the Myanmar government where they kind of took over the country through military force and they're actually a rebel fighting in the mountains, like fighting the authoritarian government. And so I thought that was pretty fun. And so I thought that was pretty fun.

They use, they said they use FreeSocks to connect to the open internet. So that was one that I had.

## Where privacy stands now

[00:17:23] **Nico:** Sweet. Awesome. Love the different use cases. We hear each livestream more and more interesting ones. So yeah, congrats on all the usage maybe taking a step back a little bit and looking at the whole privacy industry and technology. Where is it today and how have you seen changes maybe even related to AI and well, I think that in a way makes tools like the ones that you're using and technologies that like the ones that you're building more urgent. So I wanted to learn more about the, what's the current status on privacy.

[00:18:15] **Christopher Allen:** Well, I'll begin a little bit there. Cause I've been doing a lot of work at not only the technological level, but at the advocacy level. And I mean, I'm sad to say that we've really lost ground. I mean, we had lots of people in a variety of our communities talking about, oh, we're going to have decentralized identifiers and co-author of that standard. And there was all kinds of work and we had DID ETH and we had all of these other different types of projects. But somewhere along the way, we kind of lost the some of the things that were happening at the international level.

So the international standards organization started working on something called mDL, which is largely used by the European universal wallets and kind of secretly got it working in the U.S. through the new driver's license on your Apple or [unclear: "Walnut"] Android wallets. And they're very centralized systems. They kind of lock you into the models of what the issuer wants to be able to do and what governments want you to do. And at any point Google can just say, oh, we don't like you. You did something wrong over here.

So we're going to just shut down your whole system. And there actually is no recourse. So I've been very disappointed and fight fighting against that. And right now, the only one that I've really seen, that's kind of interesting, that's bucking this trend is the state of Utah, who's doing something called state endorsed identity. And they've actually embodied the self-sovereign principles that I wrote 10 years ago into law and are putting fiduciary responsibility type law things to stop Google and Apple from sort of going, oh no you signed a click wrap.

You consented to it, even though you really had no choice. And so that's part of what we have to solve that's going on right now. We have to provide alternatives.

[00:20:30] **Graven:** Zach or Chris, any current state that strikes you right now?

[00:20:36] **Zach:** Yeah. Yeah. Oh, sorry. Okay. Yeah. So I, yeah, I think it's concerning. There's a lot of different legislation that has been introduced across the world. Especially with my major concerns are the restriction or banning of VPNs or identity verification on social media or the internet in general is really concerning to me. I think those are the current risks that we face in countries where privacy has typically been somewhat normal and hasn't been criminalized in a way or made really difficult to achieve. So those are the major things that I'm concerned about and I've got kind of seen change over the years.

So there's like in the U.S. for example, there's many states that are have introduced like VPN restrictions or bands, or if you're accessing a certain kind of website or a social media platform, you have to verify your identity. You have to basically dox yourself to the platform. And you don't really have a choice if you want to use the platform. And so it's really it's really limiting cause a lot of people are on these platforms. That's where a lot of the discussion happens like X Facebook meta, whatever kind of platform.

So there's not really many great alternatives out there either. That have really caught on it for example. So that's kind of what I think about it.

[00:22:19] **Chris McCabe:** Yeah. Cool. When I think about data privacy and AI, I think first thing I think of is medical records as like an example. So AI models hacking different databases all around the world. And that's been, that's sort of come out in the last few weeks and months. Medical facilities, whole data for extended periods of time, maybe seven years like really long periods of time. And we have these humongous honey pots being created and the liability is sort of on the facilities to keep it safe, but you can't fight off this wave of like infinite energy AI hacking.

Like it's just gonna happen. So I, and that sort of thing concerns me, but how I think about what I want my data how I want my data to be handled in this modern world is don't hold onto my data. Either if something, if I need my medical records held long-term, I want to hold them physically and that's okay. And I don't want anyone taking copies and I want like my communications to be temporary. And I think people have to let go of the concept of handing over to a third party all your data for them to keep it safe because they can't keep it safe anymore.

There's just no world where a third party can keep your data safe anymore. It's that's in the past. So now it feels like to protect you more companies want to collect more data to make sure it's actually you, but that's going to be gone. Like they cannot hold that data. It's not within that capacity. So when I think about this model, it's like, how do we use the internet in the future or staying sovereign of the data that's actually important and companies have to stop holding data.

And it's sort of like, yeah, like when I think about WhatsApp, Telegram, all these sort of messaging apps, you get into them, your whole chat history is there. And if you got access to it, then in the future, who else can get access to that? And I think we need to be more in the moment and accept that we don't need to get access to our data forever for our own safety and protection online in this AI era.

[00:24:25] **Christopher Allen:** So that's actually, we have a number of things that are in the Gordian Stack, but at the core of it is something called Gordian Envelope. And that's very much the purpose of Gordian Envelope. So we have represented with session protocol and with the more modern VPNs, et cetera. They're all about data in transit. How do we secure it? So somebody doesn't snoop. What we're not good at is how do we store and secure and make data private locally on once it's right data at rest. And the most important way is simply not to have the data there.

So we have this concept called elision, which is sort of a shortcut for what is called, what also has been called the ability to revoke provably different kinds of information and not have it there. So one of the things that we've learned is that, yeah, we need to not have the data on disk, but then various parties want to go, but I want you later to be able to prove that you did do something. You did deliver the goods. You did participate in the in the commons or in the governance or in these other different types of things.

So how do we have the provable redaction? How do we basically not have the data there, but then later I can basically go, oh yeah, I, you had here's a proof that was around there all the time. And that's, I think the real power of Gordian Envelope even simple use cases, like being able to take, say my Ethereum wallet, all the private transactions, be able to give an elided set without the security information in it to my CPA so that he can do my books, which were due day before yesterday.

Those are the very simple use cases, but it can also be the medical records that. Somebody, a third party can attest these were properly done. We're not coerced from the medical meet the various standards of consent and the medical privacy and yet are valid records that can be provably valid such that when somebody does need that data. For a study or for whatever can have it. So that's the, one of the particular focuses that I'm interested in.

## Wallet risks below the chain

[00:27:03] **Graven:** Yeah. And so we're talking about hacking and Christopher, in your video, you talked about OpenSSL and the Heartbleed hack and kind of low level underappreciated things that like the whole internet can rely on. Do you see like any equivalent products or things kind of risks for wallets? Cause you've talked a lot about identity, but also do some work. Yeah.

[00:27:29] **Christopher Allen:** So we really have had a lot of success at what I would call layer zero. So if we talk about whether or not it's Bitcoin or Ethereum or Tezos or whatever, those are all protocols at the layer one or even layer two. What we need to do is secure even below that. And I might even argue layer minus one things like chips. We recently had an experience in the Bitcoin world where a hardware wallet that was really talking about privacy and security and how great their product was.

Basically had some bad randomness at the bottom of the of their stack. And despite all of their claims and everything of the greatness of their solution hundreds of thousands of dollars were stolen by people who use that because of. Really bad randomness. And we've known about bad randomness. I can remember when I was, I'm the co-author of TLS. I, we were doing security reviews of various companies, half of them failed because of bad randomness. Now we've gotten better, but it's still hard. There's still stupid things you can do.

Now I might be able to point to this one particular hardware company and go, well, you did a bad job. You didn't you didn't cooperate with others to make sure that it was worthwhile to. To have each other review each other's code, but there are other solutions. I mean, one of the things that's beginning to emerge is with multi-party computation tools. We can, rather than relying on one device, one piece of hardware, we can do heterogeneous hardware. So all that is required is that one of them works.

One of them works properly. And the property of the math is such that it secures it entirely. So as long as we are not cooperating with wallets and we're going to be vulnerable to these, some of these stupid mistakes that happen. And put funds and our communications and our human rights at risk. When we really need to be evolving to multi-party computation and other different types of things so that we don't have, be at risk because one wallet made a mistake.

## Building trust

[00:29:50] **Graven:** Yeah, absolutely. So trust obviously play, helping build trust and I'll maybe kick to Zach and Chris, like you guys products. How do you build trust with your users to and like accumulate that over time?

[00:30:11] **Chris McCabe:** So I would say for the first three or so years of session people just didn't trust us. And we just had to keep doing what we thought was right. And the amount of times people said like, you don't need to do what you're doing. What's the point of this? Like, what's this useful for? Like, why do you need this much privacy? Like on one side, people didn't understand it. And they're like, why are you doing this? And the other side people were just paranoid, didn't trust us.

And then after about three or so years, people could see the usefulness of it because of the events that then their imagination can go. Oh yeah. If I was in that situation, this would be super useful. And then on the privacy side, people just kept watching us build, just build and talk, build and talk, build and talk. And they realize, Hey, these guys are legitimate and they could see the innovation in terms of how sessions actually built. And then we got a few people who were like respected in the space and they started talking about us and that built more trust.

It was a very organic way. It wasn't forceful, sort of just the people who are respected and like what we did came to us and just shared what they liked. And I think with privacy, the more you force, like say we spent a million dollars a year on advertisements to say session number one privacy. It, I think it actually has like, it gets rejected, gets sort of spat out especially from real privacy people. Cause they realize you're trying to control me. You're trying to think control how I think.

At least that's how I believe. That's how I feel as well. So yeah, it was more of a, you just have to be there and ready for when people needed it and just build it in a way that is sort of. Full of integrity and sort of passion. That's kind of how I, that's kind of see. That's how I see us getting to this point and beyond.

[00:32:03] **Host (Graven or Nico):** Oh, you, Zach.

[00:32:07] **Zach:** Yeah, I think yeah, it's a very difficult problem to like be how it would be like, how can you trust us kind of thing. I think really our main thing has been all of our code is open source. You can see what it's doing. Really you can see what kind of data that we can potentially store. And we're also thinking about how can we make it like auditable? So how can you confirm that we're not, we don't have like tcpdump running on our servers and we're not capturing your packets at all times and that kind of thing.

So we're trying to think of ways to make it so it's verifiable. It's very tricky though. You have a few ideas, but they're not kind of the spec hasn't been written for them or anything like that yet. So and the other thing is really that I think people can, if they have questions, if they don't trust us, if they think that we're doing something malicious, often people will come into our community and start asking questions. And we're very open about what we're doing, how we're doing it.

A lot of people ask about like our infrastructure do we have our own IP addresses? Do we have our own network? That kind of thing. So yeah, just if you're curious, if you're skeptical I encourage you to come to us and we can talk through things and you can see that we're a trustworthy organization. And we've been around for a while.

[00:33:50] **Christopher Allen:** So one of the things I appreciate about looking at the Zach's projects is that it's more than just open source. I mean, I kind of consider open source to be the absolute minimum. What you really want is open development. And that means you have to be open with listening participating with your community, having things like roadmaps. Having the, not just the ability to inspect the code, but be able to maybe use it yourself so that so that there isn't a single point of failure. And but there's a lot more we can do here.

I mean there's things like verifiable builds various other different kinds of things that we want to build on top of these things so that we have more confidence as a community in all of these different kinds of approaches. And right now one of my concerns is that we're all kind of building these things alone. I mean, I just went out to a conference last week where I spent two days working with some engineers who've been doing security reviews of cryptography software to learn how they're doing it better.

And then also sharing with them the things that I'm doing trying to do better, especially given LLMs and some models won't let you do certain kinds of security reviews and others will. And all that type of stuff. And there's too little of that going on. I learned a lot. I have a lot more confidence in some of the code that we've written than I had the week before because I had other people looking at it as well. And it's not good enough just to have your team do it.

You need to have incentives and reasons why other people will want to look at it as well. And that's where the kind of, it's more than open source. It's really open development.

[00:35:52] **Graven:** And there's a sense of ownership, the community, right? Like if open development, they feel like they have a part in the trajectory. You talked about incentives and it might be just, I really love this thing and I want it to continue to exist. Maybe I'll contribute and like, I'll be that extra set of eyes. I didn't mean to cut you off there.

[00:36:14] **Chris McCabe:** Yeah. Yeah. I was just saying that's a really good point, Christopher. Yeah. Thanks for bringing that up. I feel like whenever a company has a section of their product that you have to trust them on, it's like, it's done. It's kind of done for like we don't trust certain companies because they're just a black box. And I think building trust is like not building black boxes, like building verifiable open boxes. And I think that's sort of what sets us apart here is like, we're building open, we're building open source.

We're not, I don't want to know in big tech companies, but we're not this big tech company who is just protected by the government building in a black box. Just, you have no idea what they're doing and they just spend all their money on advertisement to say, trust us safe. Like we're actually just doing the thing and showing it to. Yeah.

[00:37:05] **Christopher Allen:** I'm also I'm a little concerned about security theater. It's, it, as he said, marketing, that's one thing, but I've also seen a certain amount of security theater where they're constantly harping on how good they are and how bad everybody else is. And I feel like that's not the best way to do it. We have to work together. We have to solve problems together. And cause a lot of the code is the same. I mean, most of us are using the same keys that others are using in one of the three different kinds of formats.

A number of us are now being challenged to do post-quantum versions of these as well. If we're all doing it independently and not talking to each other about what is the best libraries and what is the best approach and how do we support those libraries and such, then we all can lose.

[00:38:09] **Graven:** Yeah. Privacy is a stack and you're only as strong as your weakest link in a lot of cases. And so I'll do one shameless plug since we're kind of midstream. I'll say like, we did select these grantees because of this idea that like stronger together. And with this quadratic funding round, we don't think supporting just one project is the best way to go. Maybe like do what you want, but there's a lot of great projects out there. And with quadratic funding, if you spread out your contributions across multiple projects, you're going to end up with higher matching multiplier more or less.

And we really want to get this all underneath out to these projects. And I props to the grantees for buying into this. We've been, I've been working with them for the last week and a half ish. We did a little bootcamp some marketing stuff, talking about QF, and it's been awesome seeing them connect. We saw Zach and Christopher talking a little bit. Oh, maybe we should follow up on that. And so beyond the funding, it's also awesome to see these projects working together and create some new connections.

And so hopefully as users check these out, support them, fund them, but then also give the apps a try or give their GitHub repo a follow, some stars, whatever you can to help elevate these projects all to all working together.

## Advice for voters

[00:39:44] **Nico:** Yeah. Maybe segue to ask another question here regarding people who are voting on the on Epoch 13. What are some practical things that they can do to improve their privacy? Like what advice would you give those people who are voting and feel free to plug your own thing there of course as well, because I believe a lot of the things that you guys are building are super important for improving individuals privacy, but feel free to give some more general advice as well.

[00:40:25] **Chris McCabe:** I'm going to say the most pressing current thing it was there's a recent firm that was using [unclear: "average ads"] to put in spyware into devices like web browser ads. So I'm just going to hop on about brave browser. I just love what they're doing in the industry in terms of by default blocking ads and sort of giving you consent over the ads that you want to see and sort of pay for it. So I'm not involved with them, but I just just so grateful that they exist.

Yeah, that's my most recent thing is like block ads, not only for your mental for your head, but for your computer as well.

[00:41:08] **Christopher Allen:** Yeah. So one of the things that I think is really important is we need to find more ways to leverage each other's resources. So I was really impressed. And I would love to see Zach's organization do this. There is basically now a version of the Tor browser specifically for Mullvad. Okay. And the advantage of that is the Tor browser has a bunch of anti correlation properties in it so that you can, you don't know that it is a the Tor browser that does it. But then if you're only going over Tor, there's a lot of protocols that are too slow to function over Tor effectively, especially things like this chat would not be very effective.

So they basically took the advantage of that and they have basically a special version of the Tor browser that uses the VPN and is optimized for that VPN. And then I thought it was really cool. I saw a demo last week of somebody who basically is doing, it's kind of like halfway between Tor and a VPN where it is a dual hop technology. And they were also working with the Firefox based Tor browser Mullvad Browser. So I'd like to see more of that. And then the other side of things is I'd like to see more support by different organizations for infrastructure.

So for instance, one of the things that we did was we helped create a funded exit node for Tor. So Tor, you have to have lots of different places where the data can basically exit. And so we funded donated a [unclear: "swamp sea"] helped somebody who had access to a very critical colo to be able to do that. To be able to do that. Once it was established, now it's 20, 30 dollars, 20 or 30 hours a year of maintaining that exit node that is useful for everybody.

And since we were already maintaining other systems, the cost was very, very low. If we all contributed to those types of things and maybe Zach can share with us how can you add more proxies and participate in a variety of things. If you're already got some team that is working on operations, you don't have to be a cryptocurrency developer or a privacy developer. But if you've got a team that's spending some time doing sysadmin work and such say, I want 5% of your time to be devoted to these kinds of infrastructure that we need so that we can all benefit.

[00:44:11] **Zach:** Yeah, definitely. I think I would love to see more of that kind of stuff. People different organizations and companies and people just running Tor bridges or Tor relays or different kinds of like anti-censorship or privacy infrastructure. I remember this is kind of a little off topic, but I remember a project of many years ago that would try to get libraries to run their own Tor exit relays. And I thought that was a really interesting concept. I kind of would like to do something with with that, with our organization, except have them run Tor bridges or something else. Cause Tor exits a little bit, a little bit hard to justify to some of the boards. In terms of like they get a lot of abuse complaints, for example, but yeah, in terms of like, yeah, sorry, go ahead.

[00:45:04] **Christopher Allen:** I was just going to say, I came real quick. I during the NFT craze, a major museum got involved with NFTs and they have as one of their missions to preserve art for the future. So we were actually talking with them about possibly hosting a sharding server so that they could basically help protect the keys of artists. And it actually went fairly up the stack before ultimately NFTs values crashed and they decided this wasn't something they wanted to do, but it doesn't there are a lot of little pieces here and there where an organization, whether or not it's a museum or.

Or a nonprofit that or human rights foundation, EFF you name it, can't do some small things to contribute and work together. And I think keys were all dealing with secrets of one kind or another. Maybe we're not all I don't how, I don't know how many private keys users have to have and to use some of the VPN technologies. But there are secrets you have to have when you get that little code from Zach that lets you use the VPN that they have that's a token, that's a secret.

And how do we keep, allow regular users to keep that safe when their machine crashes or they've got a variety of different kinds of obstacles. So we all got to get better at that and the best way to get together is to get better is to work together.

[00:46:41] **Speaker uncertain:** Definitely.

## Usability and recovery

[00:46:43] **Graven:** Yeah. So usability and UX has been one of the common themes that we've talked about and Chris opened the door a little bit there, but privacy and doing all this extra stuff often comes at a cost of UX and it's trying to find a balance. But without making critical trade-offs how have you guys approached and like, where do you see yourself in that journey of being able to reach the average user or someone that maybe isn't as intrinsically motivated or like in tune with the privacy that how important privacy is.

Yeah.

[00:47:27] **Chris McCabe:** Yeah. From a product perspective session was, it's always make a privacy, but in terms of how we approach things, but we, this signup was a little bit jarring for people. So, cause it's all public private key, set up an account. It used to be like bombarded with this save these, save this seed phrase. So this your backup and a hardcore tech person would be like, Oh, this is so important. Like if you're in crypto, you see a phrase, like I gotta stop right now. I gotta go home.

I like put this in like whatever you do. And then if you are sort of a grandma, you're like, what is this? Like, what is this? Like, why am I being shown this? So there's that two spectrums of people rather taking it too seriously or just getting confused. So with session, what we've done is when you download it, you just hit like create account. And then that's it. The seed phrase is sort of in the background. And then once you start making connections and friends and the app has value, then we say, Hey, you should back this up.

We don't force you to do it. There is a downside to that. If you lose your device or you never backed it up, then see you later friends. But we just had to make these decisions to go, how do we just make that onboarding experience so smooth? So you can actually make a friend because privacy tech is completely useless unless, and has no value to you at all. Unless you connect with something or someone, or you actually get something out of it. So we speed run to that moment.

And that was what my life demo was going to be about today, but I'm having some technical difficulties, but that's it. It takes like download it. You click go and then count them. That's it. And then you just add a friend. Right. So yeah, speed, ease ability, and just sort of put the technical stuff aside until it has value.

[00:49:03] **Christopher Allen:** So we all have this problem of the, especially when with new accounts and things of that nature. And we've, we actually have a bunch of existing standards more used in the Bitcoin community than in other places, but it's beginning to get out there and it applies to other places. So as an example when you create a passkey with some server out there, typically they're going to give you a bunch of one-time codes. And they don't really have a method. They just sort of say, well, here's a bunch of one-time codes, save this somewhere.

What we have is a spec where those can be basically bound together and whatever on your system is the recovery tool can basically register and receive those and save those and, or even shard them into multiple things. We support social key recovery, we support offline recovery, QR code recovery, and all of this. And it isn't just for seeds which Chris and I probably use the most, but also for all the other kinds of things that allow you to recover your account. That I think probably Zach, I mean, I don't know how many different stacks you're using right now, but I bet every single one has a slightly different recovery code format.

They don't have to be, it's just a secret why can't we have a common format for the secret and have some sort of really easy UX to be able to register. Oh put this all in my 1Password in some kind of organized way. Why can't 1Password have a recovery here's a stack of recovery codes that are associated with this. Instead, you have to copy it and then you go to 1Password and you have to look up the particular thing. And then you have to go to notes and you paste it in there.

Why do we have to have all of those steps when it would be just a simple matter? If 1Password supported the receipt and Zach's software and Chrises and mine knew about it, we could just store those things away. And now we don't even have to worry about the getting people to start using the strong tools. But we've got to work together.

[00:51:23] **Zach:** Yeah, I definitely agree. I think the standardization of those sorts of things is really important. And one thing that we do support is passkeys for people that want it, which are a little bit easier to use. And by default, we just generate an account number and you have to save it. And we try to confirm okay, you said you saved it somewhere safe. And beyond that, it's kind of in, it's kind of in your hands then. But yeah, people often forget where they place things. And recovery codes is definitely one of those things where sometimes I even forget where I replaced mine or if I downloaded mine or I forgot to enter it into my password manager or something.

So it can be really tricky. I feel like passkeys solve a little bit of that login flow, but it's not perfect. And there's definitely room to improve there. But for us, we try to make the signup flow as simple as possible. You kind of get guided through a flow of what you need to do. And then you, the end stages you download the VPN app and you can put your key in there. You can scan it with the QR code. The, one of the big problems we have currently is that a lot of these VPN clients that are focused on anti-censorship protocols.

There's so many of them, there's all kinds of forks of different ones and all of them behave in different ways. So one of the things we're going to be doing in the future is building our own native cross platform app that just works with the service and doesn't have a any kind of quirks that some of the clients have. So, yeah, it's a tricky thing to solve. But yeah, we definitely.

[00:53:12] **Christopher Allen:** Yeah, I'd love to show you, to show you Zach. I just released last week something called passkey demo. And basically it kind of applies some of the self-sovereignty type things, but to passkeys. And if you go to passkeydemo.gordianstack.com and just register a passkey there, you'll actually see that it will let you save your recovery codes in three different formats. A UR format that works with QRs a Bytewords one, which is optimized for reading over the phone. If you're giving it, doing social key with it. And it can also make [unclear: "C words"] out of it.

If you already have a [unclear: "C words"] app that you're using. And then of course, what I want to do is be able to simply have a button there and it will basically look where on my browser or on my system is it the passwords app on the iPhone or something of that. Or something of that nature and would just store all of those away in an appropriate fashion. So I would love to see, as you said some kind of best practices app from you guys that says these are the best ways to do it.

And let's make a standard around making it really easy and safe.

[00:54:35] **Zach:** Yeah, I'll check that out after. It's really cool.

## Demo day reminder

[00:54:39] **Graven:** The live collaboration continues. It's love to see it. We're coming up on the end of the hour. And so I just want to say a few things and we'll kind of get some closing thoughts from the grantees. Chris said earlier, he wanted to do a demo. We still have a demo day on Wednesday. I'll just remind everyone that's tuned in throughout the stream. Where all the projects in one hour is going to be action packed. It will not be boring. Three minutes to show their project, make their last pitch before voting closes, because lots of people are procrastinators and they don't vote until the very end of the deadline.

But you can vote now change your vote later. You can vote as many times and edit your ballot. So don't hold back, but there will be a lot of great information on Wednesday. At, I believe it's 1500 UTC. Hopefully I got that right. Converting to UTC, but so Wednesday we'll have some more demos. We'll be able to see these products in action. And yeah, looking forward to seeing like that focus on delivering value before you put up the big red stop sign and scare tactics around privacy.

So I think that's a really good way to approach it.

## What the funding is for

[00:55:58] **Graven:** So, but with these last couple of minutes, let's go around the horn. I'd love to just hear kind of for each of your projects, what funding would mean, what's next, where the trajectory of what you're doing is going. And really just try to get people excited about following along with your journey.

[00:56:18] *(crosstalk)* I'll jump. Oh, I'll start. Oh, yeah. Go for it. Go for it.

[00:56:23] **Christopher Allen:** Sure. So basically what we're going to be using the money for sort of in three different tranches, one of which is obviously the ongoing support. There are a lot better tools today for doing security reviews. So we want to review not only our code base, but other dependencies that we have. The security reviews is especially AI facilitated is getting a lot more powerful right now, but there's also a lot of work you have to do as far as double checking it. Oftentimes it finds bugs that aren't really bugs.

And then if it actually is a bug, the whole going through the CVE process and whatever is time consuming. So we want to spend about two fifths of the budget for continuing to do that for us and for others. We want to make it a lot easier for developers to do to use these different tools. All of our original tools were in C or in Rust, but we have a lot of people who want to use TypeScript. So we recently rewrote a lot of our tools in TypeScript and did security reviews on them, but we need to make it even easier.

We need to have more documentation better entry, better examples for developers. So that's going to be another two fifths. And then finally, we've got these more advanced things. These things like Zach was kind of interested in the dead drop Hubert protocol. And we have this thing called Garner, which is a quick on and off Tor client and server. So that you can, on any device, turn on Tor real quickly, do send a message, turn off Tor real quickly. These are all research projects and we want to spend a little bit of the money to do this research.

So that the next generation and then of, and the needs that we need next year, we'll have solutions for them.

[00:58:16] **Host (Graven or Nico):** Nice.

[00:58:18] **Chris McCabe:** With session so grateful for all of the votes and donations so far that have gone towards the project. Super grateful for that. In terms of what we're looking to spend the funds on session is this relatively complicated, like multi-platform application that we're actually trying to build right now into a unified stack Which would allow more third parties and community members to contribute without it conflicting with other platforms iOS, Android and desktop. So bringing that all together and then bringing in post-quantum encryption perfect forward secrecy as well.

And yeah, just allowing a much more streamlined application for everyone to contribute to and bring more community into. So yeah, that's what we're looking to utilize those funds for.

[00:59:04] **Zach:** And for us at FreeSocks, I think there's three things. So I think the major one that we've been putting a lot of time is development. We're trying to make the control plane better and more usable for people. And then the second thing is documentation. So it, at the moment it's genuinely difficult to spin up your own version of FreeSocks. I would say you have to really, really read into it and we want to make it a little bit easier. So people can just go from A to Z essentially, and just easily spin up their own FreeSocks.

Like platform if they'd like to do that play around with its features and maybe even contribute some. That'd be amazing. We would really like that as well. And the third thing is infrastructure. So we run all of our own hardware and we have our own network stack. So it takes a lot of manpower kind of to go in and build that infrastructure. It's genuinely exhausting to like go into the data center and rack all the hardware up and put it all together and get it all working.

So that's the, one of the major things too, is we'd like to have more locations that people can connect to. Particularly a lot of people are concerned about latency or they're playing games or something. They're doing video calls for example, and they want lower latency and so more locations better in that sense. And yeah, I think it also costs quite a bit to run the infrastructure too. So at the moment we're pushing about three petabytes of traffic per month across our services. So, and it's continuing to grow at a pretty rapid rate.

I think we'll hit four petabytes in a, in another month or two. And the more locations that we add, the more traffic that we can push. So.

## Close

[01:01:04] **Graven:** Yeah. Excellent. Yeah. These funds will not go to waste. One last plug I'll say about these grantees. Obviously very privacy focused, cutting edge, but they all have traction. They're all out there doing the thing, not just talking about it. People rely on their tools and use them. And that's one of the things that we really put into the application. Cause we wanted to see this, these funds drive impact. And no doubt with these three and the other 10 grantees that your funds will help advance privacy and keep it, keep the lights on.

Create the next big thing, make it easier to use all the great things that these guys have shared with us today. So one last plug, epoch.Octant.app. You can contribute from your wallet or wrapped ETH. And if you've been a long time Octant community member, and you have GLM locked vote, we've actually, the grantees have done a great job onboarding new voters. We, I know that we have a lot of Octant lockers that are still have yet to submit their ballots. So definitely get out there, submit your ballot, support these these innovators and builders.

And yeah, can't thank you guys enough for all the work that you do and spending time with us. We crossed the 450 people watching barrier. I think that's a pretty good audience. So thank you to everyone for tuning in as well.

[01:02:36] *(several speakers)* And yeah, thanks. Thanks. Thanks. Thanks. That's all I can say. Thank you.

[01:02:44] **Christopher Allen:** Well, thank you for the opportunity to connect to your platform and connect to the donors in your community. So thank you.

[01:02:52] *(several speakers)* Cheers. See everyone Wednesday. See y'all. Thank you for the great. Thank you so much. Bye. Thank you.

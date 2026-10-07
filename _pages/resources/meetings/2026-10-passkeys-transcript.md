---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/otherresources.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "Passkeys & Agentic Repos"
title: "October 2026 Gordian Developer Meeting Transcript"
hide_description: true
classes:
  - wide
permalink: /meetings/2026-10-passkeys/transcript/
sidebar:
  nav:
    - resources
    - meetings
---

## Welcome

**Christopher:** Welcome, everybody, and good morning to you in the US. And for those in Europe, good evening. Thanks for joining us. This is our monthly Gordian meeting on the topic of secure, open, interoperable, and compassionate digital infrastructure.

What is Blockchain Commons? We're a community interested in the self-sovereign control of digital assets. We bring together stakeholders to collaboratively develop interoperable infrastructure. We design decentralized solutions where everyone wins, and we are a neutral not-for-profit to enable people to control their own digital destiny.

Many thank-yous to our sponsors, who help us be able to do this work for you, and of course, if you're not a sponsor, we would love to have you join us.

At our last meeting in August, we had a discussion about Bitcoin interoperability, and then a dive into resilience using a lot of our existing technologies like SSKR and our whole collaborative recovery tooling: to not only back up things like Bitcoin descriptors and other kinds of objects that are difficult to back up, and shard them for social recovery, but also things like SSH keys and other kinds of secrets, which is relevant to our meeting today.

Today's topics. I wanted to announce a new project that we're going to be building a number of new tools on, called Cloudflare passkeys, and then we're going to have a larger discussion about agentic AI and security, and how we can be transparent about that in our own work.

## passkey-cloudflare: Why Build It

**Christopher:** So first, the Cloudflare passkeys. The repo is called passkey-cloudflare. It's around passkey-only identity. In particular, it is built on Cloudflare Workers and Durable Objects, so it's highly distributed. I wouldn't call it decentralized, but it is at least distributed, and it's designed to support a variety of collaborative web apps.

So why did we do this? Well, there are a couple of different reasons. One, we wanted to improve authentication diversity. There's a lot of tooling that gets locked into a platform such as Google or Apple. And yet we want to be able to bootstrap a variety of other kinds of security. So passkeys can work outside of the Apple and Google ecosystem. If you've ever used one of the browsers that allows you to manage passkeys independently, you can do it there.

Passkeys can support biometrics, and they're really useful for projects like collaborative seed recovery, where we're looking forward to being able to do backups of a lot of the secrets we talked about in our last meeting in a collaborative way. One of the ways would be on a passkey server.

One of the other things we really wanted to do was to piggyback on a secure infrastructure. We could have just written a Rust app that did passkeys. But then we have to manage the infrastructure for that, the demo and the code. And doing this on Cloudflare, which has a lot of security team people working on it, allowed us to kind of leverage their tooling and security reviews, et cetera.

And then really being able to piggyback on robust recovery. Apple has a very deep system for how to recover your keys. We might argue, well, that means that there's a centrality there. But if you're only recovering a shard that lets you recover your keys, now all of a sudden you've got a very reliable shard, which really makes for better independence. Same thing with Google. Both have independent infrastructures, lots of security around recovery. And if it's not a single point of failure, then it can be very powerful for self-sovereign scenarios.

## Core Functionality and Live Demo

**Christopher:** The core functionality is that the passkey verifies identity. It isn't the identity. I've seen a lot of passkey implementations where the passkey is the identity, and that's not the right way to do it. The application determines the permissions, and it is not precisely OCap, but it is OCap-inspired.

Another thing is that users can hold several passkeys. So on your Mac, you could have the same passkey for Apple and Chrome, or you could have two different passkeys: one for Apple, one for Chrome, one for Brave, et cetera, and one for your Google phone and one for your Apple phone.

It currently operates on Cloudflare Workers Paid. There's a free plan that allows you to do Cloudflare Workers, and I was almost able to get it to work. But right now the current implementation requires the $5-a-month plan, and it fits well into that. I am hoping to puzzle out some optimizations to put it just under the 10-millisecond limit, so that anybody can host a passkey server.

And we do have a live demo available, so let me see if I can share that. Here we go, and share again.

Okay, so right now it's just the passkey part of it, but the rest of your app would do the rest of whatever interface you have here. I can basically sign in. It does some smart things here to recognize that I don't have a passkey, so I can register. And I can say, this is the "Gordian meeting demo."

You won't be able to see my little dialog here, but I basically have a passkey dialog: do Touch ID to do it. And I'm given a number of recovery codes. Note that recovery is not with an email address, it's with one of these codes. Also note that all of these are Blockchain Commons URs. So in this case, they're ur:seed, which is a 32-byte secret. Same exact format that is used by wallet recovery, but these are one-time secrets for recovering your identity, say, on another phone.

In this particular case, I'm going to say I saved my recovery codes. And now I can see all my settings. I can see my passkeys. I can revoke passkeys. I can replace my recovery codes, which will erase all the old recovery codes. And then I can see a variety of other details that allow things to be visible. If you are an operator on this server, you also have an operator panel that allows you to revoke things, et cetera.

Here we go. And I probably have to stop share and reshare. Okay.

## Design Principles, Gordian Technology, and Next Steps

**Christopher:** So, some design principles. A new user handle is created for every ceremony. There's exactly one refusal for every failure. Plural: you can have multiple passkeys from the first registration. And internally, the recovery codes are stored only as hashes. There are a number of advantages to these if you know anything about passkeys. In particular, these are not common things, but they're very much part of my architecture.

And the usage principles. Identity is explicitly separate from permission. There are no fallbacks to weaker credentials. You can enable step-up for what can lock the owner out. So for instance, some passkeys can be backed up; some passkeys can't. We can identify that. You can set it up such that one of your passkeys is on a hardware key, which can't be backed up, but that one maybe has more privileges than, say, the Apple passkey, which is backed up and runs on every one of your Apple devices. So you have a lot of ability to do that.

And then for privacy purposes, the operators can see state and counts, but not the actual device details. So there are some privacy things I'm trying to enable there. I want to be able to do more from the server side around privacy.

This is leveraging a lot of our Gordian technology. Simple design. It's written with TypeScript libraries, and we're validating it against some of the TypeScript that Leonardo wrote for us in our TypeScript libraries for Gordian tech. We use Bytewords to label the passkeys. And then, as I showed you before, ur:seed encodes the recovery codes, and you get all the advantages of URs, which are that they can be stored in a variety of tools like Gordian Seed Tool. It has a CRC in it, so if you have an error, it can identify the error, and you can immediately shard a recovery code with some of the tools, both the CLI and Gordian Seed Tool, that are available now.

So anyhow, that's the passkey tool. Probably the first thing that I'm going to be working on post this is actually implementing a simple passkey social key recovery server, and starting to enable that. I'm hoping that other organizations will step up to have different authentication methods for multiple passkey servers.

I mean, it doesn't do a lot of good for you to get a passkey that you put on your Apple device from us, and then you shard it with somebody else, and you put those shards in an Apple server. Well, they can get both of those shards via an attack on your phone. So we've got to puzzle out how to make this easier for people to do. But we need to start with one.

And if you're interested in maybe supporting another authentication technique and keeping things off of a platform infrastructure, let us know. I figure we have to have a minimum of two, plus offline backup, which will allow people to do two-of-threes, and then hopefully we have a whole ecosystem of servers. I would love to see, say, the Electronic Frontier Foundation host one, or some other human rights organization hosting one.

Beyond that, we've demonstrated FROST ceremonies where you have a central coordinator that you don't have to completely trust to coordinate all of your FROST signatures and other kinds of multi-party computation. So this will be at the heart of some of those demos. And then of course, if anybody's interested in using these for other purposes, let us know.

## Passkey Q&A: Device-Bound Identity, Sharing, and Personhood

**Christopher:** Leonardo, you have some experience with passkeys. How were you using it in the past?

**Leonardo Custodio:** All right, very nice. Yeah, so we had a project inside our company this year. We used not really the passkey, but the hardware-backed secret, to be able to create a stable identity for the device. In our use case, we were hoping to avoid bots. Since you don't have a hardware-backed secret on emulators or simulators, we were trying to avoid user-created bots for our product. That was our use case.

We did have some issues. The main issue that we had was how to avoid people creating many accounts on the same device. We did find that Apple and Google provide some APIs to avoid that. But the main issue started on Android, when people use another kind of operating system, for example GrapheneOS, that doesn't have those APIs, right? So that's still a challenge for us: how to avoid someone using a real device to create many accounts.

**Christopher:** Yeah, that's definitely an issue. We're going to talk a little bit about that when we get to the next part of the discussion.

I have been experimenting with some of those APIs. The code right now supports multiple passkeys from the same provider, but you can actually set it up such that the provider can tell you, "Oh, you've already registered with Apple iCloud."

However, it looks like if you, for instance, use a Chrome browser and then turn off sharing with Apple, then the Chrome browser on the Mac will use the Google share. And I was working with somebody recently, and they had an oddball browser which just stored it in the browser. And then Shannon uses LastPass as the storage for the passkeys, which is a way you can link these different storage servers. So yeah, there are a lot of weird cases in there.

I am interested in how to do different kinds of key agreement things. You can't really do FROST or multi-party computation with these keys, because they are not really accessible at the user level, or they're in the trust zone. So there are some real limitations on what you can do there. But you can use them to establish trust in a series of keys, which I'll actually talk about in a minute.

Mike, Vinay, Leonardo, any questions on this project?

**Leonardo:** I think another interesting thing is that, at least the way we did it, we could have leveraged the keychain access from Apple. So when you created the passkey, or the secrets, or however you are using it, it would get synced with the MacBook if it's in the same account. But I think that's exactly the point that maybe we're trying to solve here. This feature is only available if you have the devices all synced in the same Apple account, right? So if you want to share that with another account, with another person, that's not really possible.

**Christopher:** Yeah, so that's another thing. With this tool… well, first off, my partner and I do share some passkeys, because I can do family sharing with my passkeys, and now she can log into some of the things that require a passkey. But it's the same passkey. Whereas this tool allows me to give her a separate passkey if she needs it, and I can also give one of these recovery codes to somebody, and then they can add a passkey rather than revoking. There's a whole revoking thing there.

But the user experience for this, how to educate people, how to use it, where to store it… there's no API like "please store my recovery code someplace." That's not there. I have some ideas about that.

I'm also very interested in proof of personhood, which is a very tough problem. I feel like one of the experimental servers we will be playing around with is leveraging some of the passkey tooling to be able to bootstrap into a proof-of-personhood ceremony that will then give you a social recovery that is unique to a person and can't be shared with other people. But we'll work on that.

**Vinay:** I just wanted to clarify: did you mean proof of personhood, proof of unique personhood in a context, or proof of personhood?

**Christopher:** Yeah, definitely. I'm using the common term, and I try to be more precise and say proof of personhood in a unique context, because of the problems of a global proof-of-personhood thing. But at least with a collaboration server, there are a variety of cases where you really need to have Sybil resistance. And how do you do that in a pragmatic, practical fashion for the needs of that particular server?

And like I said, there's a wide variety of work in this area. A lot of it has raised challenging problems, not only in just how to do it, but then, once you've got it, it can be used in problematic ways. So we want to address those in the longer term. So if you're interested in funding this kind of research, let us know.

Mike, Shannon? Mike, go ahead.

**Mike:** Yeah, hey. If I telescope out and see this as a tile in the larger mosaic of all the things you've built, how close are you to allowing sort of a chain of custody or attestation of an AI agent to a human? Would that be out of scope? Would that be stretching this too far, or…?

**Christopher:** No, and in fact, we're going to talk about it in just a second, because that's exactly part two.

**Mike:** Oh, okay. Thanks.

**Christopher:** Okay, any last questions on passkeys?

## Repos in the Age of Agentic AI: What Is Changing and Who Pays for Audits

**Christopher:** Okay, so we'll move forward to repositories in the age of agentic AI. I think the key thing is we all recognize that AI is really changing the nature of what it is to be coding today, and of maintaining the security of our code. And I really wanted to also talk about being transparent about some of the choices that we're making, both individually, as organizations, and as a community.

So what is changing? Well, I think the clear truth is we must use AI to protect against AI. I don't see an alternative to that. Because agents write code: more, faster, but with uneven quality. Agents review code: cheap, also uneven, and sometimes not disclosed. So you also get different kinds of gaming happening.

Agents attack code. I was just talking to somebody who referred me to a wonderful Linux core discussion where they've gone from 33 CVEs to hundreds of CVEs a week, 100-plus a day sometimes. And a lot of them are what you would say would be minor bugs, and sometimes very irritating little minor bugs. But the AIs are able to put them together in weird and interesting, novel ways to eventually break into a system. And that's what is new.

So that means we've gone from the stage of being able to address things with code fixes and updates in a reasonable amount of time. Now, I think the video that I saw earlier this month said we're at negative eight days: an attack is used in the wild eight days before it is disclosed that it's even a bug that needs to be fixed. So that's a serious problem.

And a real problem on the other side is that AI agents can just dump out loads and loads of vulnerability reports, many invalid, incorrect, et cetera. I think you may have heard in the news about OpenAI announcing, I think they said, 76 vulnerabilities in Linux. And he was basically saying, well, like half of them weren't even vulnerabilities, and of the remainder, only 10 were actually real bugs. So even with an Anthropic news release, where supposedly they went through a little bit more due diligence, there was a lot of stuff that wasn't accurate, and this is just overwhelming a lot of projects.

We haven't experienced it much in the Blockchain Commons community. We've received some kind of spammy reports. But we've seen more in the decentralized identifier community, and they can be quite annoying, and you have to respond to them.

So the second half of this is: how do we disclose and sign things so that we can have some integrity with our repositories? How do we credit AI work? How should we? How do we do code signing? How do we do versioning and tags?

In particular, our revenues are down, many other open source projects' are as well, and this makes human security audits unaffordable. I mean, the last time I had one done, it was $25,000. So that's just a real challenge, and that was inexpensive. Many can be up to a quarter million dollars to have a security review. So that means we're going to be doing more computer audits, even if they are flawed. And then we have to review the computer audits.

How do we do this? How do we talk about it? How do we talk about things that were human audited, versus human moderated, versus a human looked at every bit of the code? And then how do we disclose who or what did that, and what standards would we like to see emerge for the content and security of repos?

## Git's Gaps, the Inception Commit, and a Demo Repository

**Christopher:** So the first step is some improvements in how we do Git as individuals. Today, in a typical Git repo, the author and committer names are unverified text. Anybody can claim to be anybody. Signing is often optional, and unsigned work easily merges, except in some of the more restrictive projects: Bitcoin Core is very restrictive about merging, and obviously Linux is. But if you look at the typical open source project, you will see lots of things where you wonder: was that actually authored by that person?

And also, the history can be rewritten in a variety of ways, including the very first commit. So that's an interesting problem. Nothing ties a clone of a repository to who started the repository, especially if you're trying to be platform-independent and not solely use GitHub. And that's because trust lives in the hosting platform's accounts and settings.

So yeah, I can go to a page and see, "Oh, all of these commits were verified." But some of those were verified by email address against GitHub's records, and that email address may have been compromised five years ago, because they don't constantly check to see if that email address is still valid.

So how do we address some of these? What I have established is some working code around something I call the inception commit, which establishes a root of trust per repository. Basically, the repository's first commit is empty. No files at all. And this is for very deliberate reasons: it makes it more difficult for that security to be compromised. It is signed by the founder's SSH key, and its committer name is that key's fingerprint. And in the message, it states the rules: who may add signers, and how.

So let me show you an example of this. Share.

So if we go to Blockchain Commons, Open Integrity. This is our document on Open Integrity. This is our Open Integrity project, which has a variety of repos. And in that is the OI demo, which demonstrates that I have an inception commit.

So this is the very, very first commit. It says that I'm initializing the repository; this key also certifies future commits' integrity and origin. And then what the rules are, which is that you can add additional signers in future commits by adding to this allowed commit signers file. And it was signed off not by Christopher Allen, but by the key that was used to sign this commit. So there's kind of a loop there.

So even though Git and GitHub largely use SHA-1, which is not a secure hash algorithm… I mean, it's 80 bits, so it's reasonably secure, but not as secure as SSH keys typically are. Combining these two gives us much greater security, and a root of trust for that particular repository.

So once that key is allowed to do things with that repository… oh, and by the way, that particular key that I used was my Secure Enclave key on one of my machines called chryseikori. So you will see here the human chryseikori Secure Enclave key, which happens to be the same as the inception key. I am explicitly declaring it to be allowed for future things. So the way it works is you always use the key from before. So I kind of revoked my own key and then re-added it, but with more details, in my second commit.

Now, I have multiple machines with Secure Enclave keys. [Say] I take this on my trip to Atlanta next week, and it falls in the ocean: I need to be able to continue to use this repository. So I have multiple machines, each with a Secure Enclave key that requires Touch ID, for my other devices. So now I have some resilience.

## Agent Keys, the Chain of Trust, and Cheap Signatures

**Christopher:** This is where we start talking about AI. What I do is I create Secure Enclave keys for each of my AI tools. They're configured so they don't require a constant biometric. They have to be validated every time I reboot my machine, to say, yes, Claude can continue to use my key for this session. But it doesn't require my biometric for every commit and such. And it is only allowed to work on branches. It cannot commit to main. That's how the rule works for that.

So now I've allowed Claude to do some things, and like everything else, maybe I want to have more than one. And this is where you'll start seeing, over here in the branch, a variety of my original commits, but also things where Claude committed using the Secure Enclave key that it has. But it's not verified with GitHub, because it isn't a verified user, and thus can't commit to main.

And so, going back to the main branch, I have additional work. This is where I'm actually putting in various Git tooling to make it such that Git will refuse if you try to do the wrong thing. So that's basically a quick demo of what's possible there. It's definitely a work in progress. I need to find some other people that are interested in supporting this and puzzle out some of the edge cases.

Let's see. Stop share, and go back to the presentation. Okay. And so that's a simple root of trust.

Okay. So longer term, we want to have more definition for the files in the repository that list which keys may sign what. Obviously the inception key signs the first list. Every later change must be signed by a key that exists in a previous commit, and so that means each commit is judged by the list as of its parent, so no key can authorize itself. Keys can be added, rotated, limited by date, and given different roles.

So how do we use this technology with Open Integrity to support some of the AI future things we want to do? Well, the challenge is that signatures got really cheap. An agent can sign thousands of plausible commits a day. A valid signature says which key made a commit. It says nothing about whether or not anyone competent looked at it. What that means is that the scarce thing is no longer authorship; it is accountable review.

So, we were talking earlier about the passkey tooling, and there is some ability there to basically say, "Oh, this particular device can only register so many unique things in a context." So it's not precisely unique personhood in a context; it is a unique identity within a context. And that way, a party using trust zone keys can't register another trust zone key. They have to go to a completely new device. You can't have a single AI Claude go and create a thousand different keys; the hardware would limit it to just one. At least that's the plan. Obviously, there are a lot of things to work out there.

## Adoption Tiers

**Christopher:** Another thing is that Open Integrity has been out for several years now, mostly with inception commits. Shannon and I have done a number of repos that at least have the inception commit in them. But then we've allowed for web edits of, say, the web pages from the GitHub interface. And obviously all those things are signed by GitHub's key, not by a key that we've attested to.

So the simplest tier of adoption of Open Integrity is just the inception commit, and we have a variety of tools there. We're hoping to add more and make it easier. But even then, you have to remember to do it. I just discovered that my passkey repo, by accident, didn't have it. I somehow forgot to do it at the beginning of setting that up, so I may have to rebuild that repo and all of the commits in it so that it's properly in Open Integrity.

The next level is to allow for these other kinds of commits from GitHub, from wherever, but really focus on signed tags, so that you have a human with a Secure Enclave key attesting: "Version 0.3 was reviewed by Christopher Allen and has gone through all of these criteria." Hopefully being transparent about what those criteria are, and then we don't worry about every single individual commit.

Then the criteria statements, and what level of review, would be the next step up from that. You might do, between commits, that Leonardo, as somebody who does a lot of stuff with TypeScript, and Christopher Allen, who doesn't do as much with TypeScript, have at least looked at whether it's using idiomatic practices, X, Y, and Z, but didn't do a line-by-line review of the code. That's a useful security point. So I'd like to be able to put those in the repository and let people know.

And obviously the full chain, where it's every commit, with all the roles and the branch protection and things of that nature.

## Tier 4: Gordian Envelope, Elision, and Pseudonymous Developers

**Christopher:** Something I've also been puzzling out, which I haven't really talked about here, is that there can be some advantages to using Gordian Envelopes as you migrate. So there might be a Tier 4 where you switch over to Gordian Envelope.

So why would you want to go to a Gordian Envelope? Well, when I revealed my three machines, that is information that is not necessary for the whole world to know, right? Now an attacker watching this video, or going to my GitHub and looking at my repo, will know, "Oh, Christopher has three machines. Where are they?" But with elision, we can commit to keys that are not disclosed, and devices, and other kinds of information, and then be able to reveal, "Oh yeah, back in commit 5, where we became a Tier 4 Open Integrity repo, there were commitments to keys then that I'm only revealing now, now that one of those devices is gone."

I think that offers a lot of privacy, in particular privacy for the developer. One of the things I want to be able to support in Tier 4 is things like XIDs, which allow for pseudonymous development. As many people know, there have been a lot of challenges with developers being threatened with lawsuits under contract law and commercial law, or actually being threatened with penalties for people using their software improperly. So pseudonymous development, I think, is going to become an important part. And if we're going to have pseudonymous developers, then we need to know they're not pseudonymous AI. So this is going to be an interesting problem.

And we have a whole bunch of work in our XID toolbox and such around how to establish trust in a pseudonymous entity. In our use case, it's Amira, who is an Iranian immigrant who has been threatened by people back in Iran. How does she continue to be a developer? And so we could potentially merge Open Integrity, which uses nothing but Git, nothing but standard shell files, nothing but standard keys, and move and transition into a structure that supports privacy, integrity, and openness.

## Review Disclosure Statements and What Works Now

**Christopher:** So we have a bunch of things around this in our XID repos about what the nature of public disclosure for code is. Some of the things we've discussed there are: who the person was, what kind of entity they were. And then this whole thing of principal authority, where I am the principal authority for the Claude agent, which I call Chatelaine, that does things on my behalf. So that whole tree has to be there.

What was reviewed or excluded? How? What was done? Was it just reading it? Was it tests? Was it fuzzing, static analysis, AI review, human review? What was done with the results? A lot of findings should be ignored for a period of time, or just aren't relevant to the use case. I mean, that was one of the things that was evident from the Linux thing. It was like, "Yeah, that's a bug, but we said a long time ago that's not what Linux is responsible for; that's somewhere else." So the finding can be "we've passed it on to somebody else."

And I think it's perfectly fine to say that no human security audit has been done. That's still better than not saying anything about it at all. And then of course, where to report, and things of that nature.

Right now, my standard is that an AI reviewer can sign its own statement under an agent key, but I must sign the tag that adopts it. So that's the rule that I've set, at least for my own repositories. Different repositories can set up different rules.

What's working right now? We have a verifier for every commit and tag against the signers files. Right now we have some simple agent rules. We are using hardware-backed human keys that can change main and tags and the trust files. The code agents can't, but they do sign as themselves.

And then I have to approve each merge with Touch ID, which can kind of be annoying. I thought that thing was going to get finished, but half an hour ago it said, "Oh, I need a Touch ID," hiding over here, and I didn't see it, especially since I have multiple screens.

Obviously, a lot of this is Tier 3 and isn't needed by everybody. I don't know that Leonardo needs to adopt it for some of his personal libraries and things of that nature. So we need to write some better support for Tiers 1 and 2.

## Questions for the Group

**Christopher:** So now we get to the conversation side of things. What did I miss in our review statements? Do we need to identify the model? I can say it's Claude, but Claude is very different this year than it was a year ago, or even two months ago. So do I also say what model I use? I happen to use Opus 5.5 lately. Fable is in theory better, but I'm finding it's just as good and a lot cheaper. So that might be something. And then, is it relevant that Fable went from 5 to 5.5, and Opus went from 5 to 5.5? I don't know.

Do I need to say what agentic skill technology I use? I have two very different agentic support toolings, and I'm probably going to create a third one. And these are public repositories, both mine and others'. And is it relevant? I mean, I happen to love Matt Pocock's architecture review skill. It's much better than the review skill I wrote. Do I need to disclose that?

Obviously, we'd want to not be vulnerable to the hordes of AI-generated vulnerability reports. What are we going to require of our agents to label and do? And there's some interesting stuff. There is a very interesting way of rewriting English that was invented by the aircraft industry back in the '90s, which basically limits the number of words that you can use, and has very, very strict requirements around grammar and synonyms and things of that nature, and it has hugely reduced airplane maintenance problems. Because a lot of the people repairing planes in other countries aren't English speakers. And the improvements are phenomenal, just by switching to this very constrained language. Should we be doing a constrained language around vulnerability reports? Is that doable? As far as I know, nobody's done it.

There are a lot of weird conventions around version tags. Do we ever want to have a version tag that isn't signed by a human? I don't know.

And then obviously, what existing standards? I've seen five now: source code management projects out there. There's one in W3C, one in IETF, there's one in ISO, there's one by the Linux Foundation. And there's, I think, a fifth. Well, I guess the fifth could be mine. I like Open Integrity because you can bootstrap without any special tooling or code. It's all code that's on every Mac, every Windows machine in the world. There's nothing you have to install in order to use Open Integrity. But maybe there are some things in those other standards that we should adopt, or we should try to get them to use some of our ideas. I don't know.

And then, who is interested in moving to Tier 2 on a real multi-person project?

## Discussion: Parallel Contributors, Branch Roles, and Squash Merges

**Christopher:** So those are my questions for you guys. Leonardo, you want to start? Do you have any thoughts on this?

**Leonardo:** So, you said before that the commit needs the key from the one before. So does that mean we need a queue system for the commits? I mean, if we have multiple people working in parallel, do they always have to have the latest main before being able to merge?

**Christopher:** No. So the way Shannon and I have done it is that I have authorized him to be able to do various things with the repo that I initiated. And so that means he can do merges, he can do all different types of things there, once I added him. And I could have added multiple of his keys. The way that was set up, he has all the permissions that I have.

It is very plausible to basically say that collaborator A can commit to the… I forget what the Git Flow term is. It's not the develop branch, but there's a whole bunch of things that other systems use with different kinds of branch methodologies. So collaborator A has the ability to commit and merge to a testing branch or to a develop branch, but it requires a different set of keys to merge to the main branch.

Now, one of the things I like about the Blockchain Commons Gordian Envelope stuff is we can support multisig, and there's not really a multisig thing with Git. But it is possible to sign a commit twice. So you could set up something where, for this really important, low-level, potentially vulnerable TypeScript library, it requires Leonardo and Christopher to sign it. So you would do a commit, I basically countersign that commit, and then the GitHub Actions will go, "Oh, I can now merge this," and automatically merge it. Stuff like that.

**Leonardo:** Another question I have. I personally use GitHub a lot squashing commits. So for every PR, I squash the commits into one. So how would we… [inaudible: transcribed as "our GitHub took to Jesus question"; context suggests how squash merges would work with signed commits]

**Christopher:** Yeah, so there are a bunch of things here. What I really want to be able to do is allow the squashing to happen, but in the commit, which can be a fairly long message, would be the details of what was squashed. So you would squash, but it would have details about the other commits that made it up, and where they fit in, and such.

That's why, if you notice, there is an author and a committer field in Git. Usually they're the same person, but in merges and squashes and things of that nature, those fields can be very different. So you can set it up so that you have one, and then let the other one be done by the agent. But this all requires investigation. And of course, if somebody really wants to give this a try…

Right now, this is friction. It slows down development. So that's why I've always wanted to puzzle out what those tiers are. Because maybe certain kinds of projects, before they get to 1.0, don't require all of this, but when they switch to 1.0, they do. I don't know.

**Leonardo:** Okay, thank you.

## Discussion: Anchoring AI Reviewer Identity and Witness Statements

**Christopher:** Thank you. Vinay, any comments on code reviews? And am I doing the xkcd fifteenth standard again?

**Vinay:** I mean, one question comes to mind, which is: you asked how AI reviewers should be identified. How would that actually be sourced and anchored? Because you could disclose something completely different. You might be using Claude 5.5, but you could claim to be using Fable. So how would that anchoring actually happen?

**Christopher:** So this is… excuse me. Oh. I honestly don't believe… I mean, I know there's some discussion about being able to do different kinds of attestations by the frontier models and things of that nature. And of course, I'm increasingly moving to local models. I don't believe that it's fundamentally possible.

So then it becomes a matter of trust. And that's where a lot of the different things that we do with the XID technology, where we can create a kind of web of trust with a variety of public disclosures, claims, and witness statements, and things of that nature, really make that possible.

And then I've also been working on how we incentivize this type of stuff. So I've also been working on how you have sort of a time-bank token around witnessing. Right now, a lot of the different kinds of projects that are trying that, which would, say, reward tokens for committing and things of that nature, get gamed. I've personally looked at three or four, and my AI research agent has looked at like 50 Ethereum projects and DAOs and stuff that have tried to incentivize good behavior, only to have them be gamed. But I think I've got something that is potentially useful there.

So what I'm seeking is… Leonardo might be interested in having various witness statements about the stuff that I am doing for, say, the Ethereum Foundation. Right now, if, say, the Ethereum Foundation wanted me to do X, the Ethereum Foundation can say, "Yeah, Chris delivered it." And I can say, "The Ethereum Foundation paid for it." But that can be gamed in a variety of ways. I mean, it's useful, but it can be gamed. And especially if you're trying to build some large trust graphs, there's some history of what can be done there.

What we really want is that Vinay and Blockchain Commons had this thing, and then we want Leonardo to witness it and make some fair witness statements: "Yeah, it looks to me like the criteria that the Ethereum Foundation wrote were reasonable, and what Blockchain Commons did was reasonable." And again, these are fair witness statements. If you don't know what a fair witness is, it goes back to an old Heinlein novel where somebody asks, "What color is that house?" And the fair witness answers, "Well, it's white on this side." So it's a practice to learn: being very precise where you can be precise, and being very clear where you can't be precise.

And so we need to reward people to be able to do this. And I've kind of got a mechanism where Leonardo can then earn the ability to get witnesses from other people. And we have sort of a witness economy that is separate from the peer-to-peer accountability type of approach. Again, long term, I'd love to have some funding for this type of stuff. Otherwise it falls into my one day a week of research.

## Discussion: Text Provenance Watermarks

**Christopher:** Okay, well, it's a little after 11. Shannon, did you have any questions or thoughts?

**Shannon:** The only thing I was thinking about, on provenance, was that it's just been a few days since the EU text provenance rules started being adopted, by OpenAI at least. I don't know if we're thinking that might help any when we are identifying which AIs are doing what on repos, or if we think text provenance isn't going to help code at all.

**Christopher:** I don't think it's going to help code at all. I mean, we already have a bunch of tools out there that claim they're breaking this particular technique. Since it's not revealed, we don't know. But my gut is that it's going to be an ongoing battle. That doesn't mean that we can't leverage it as a data point, but I don't think it'll ever be deterministic.

**Shannon:** Yeah, I think they're restricting it still to researchers and such, because they don't have faith in it. So that sounds fair.

**Christopher:** What have you been hearing about this, Vinay?

**Vinay:** Limited at this point. That's why I was asking you.

**Christopher:** Oh, okay. Yeah, I'm very interested in a lot of this type of stuff, but I think ultimately a lot of it comes around to how you incentivize good behaviors without having the economic gamesmanship that happens in a variety of these scenarios. Which is part of why I've never participated in a DAO. As somebody who's been studying these kinds of incentive systems for 35, 36 years, and even though I helped write the Wyoming DAO law and have offered advice on a variety of them, I've never actually done one. Because when I actually look at the governance, I kind of go, "Hey, I think I know how to game this, so why do I want to play in this playground?" But I think it is possible to have some things that work.

## Closing

**Christopher:** Okay, well, thank you very much. These are the links to our Open Integrity repo and the little demo with some real Open Integrity commits. And then obviously, you can get more details at www.blockchaincommons.com, or you can reach me, Christopher Allen. I'm @ChristopherA on GitHub, on X, and Bluesky, et cetera. So I hope that you will join us at a future Gordian meeting. Thank you very much.

**Vinay:** Thank you.
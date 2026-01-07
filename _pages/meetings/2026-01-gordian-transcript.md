---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-arch-background.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "2025 in Review, IDE, Known Values"
title: "January 2026 Gordian Developer Meeting Transcript"
hide_description: true
classes:
  - wide
permalink: /meetings/2026-01-gordian/transcript/
sidebar:
  nav:
    - meetings
---

**Participants:** Christopher Allen (Blockchain Commons), Leonardo Custodio (Parity), Wolf McNally (Blockchain Commons), Shannon Appelcline (Blockchain Commons), Irfan Bilaloğlu, Dorian, Mike Warner, Christop Dorn, etc.

**Duration:** ~90 minutes

**AI Transcription:** Automated transcription (App: Mac Whisper 13.8,
  Model: Large v3 Turbo w/Speaker Recognition)

**AI Processing:** Medium cleanup (App: Claude Code, Model: Claude
  Sonnet 4.5)

*Note: This transcript has been moderately edited for readability -
 filler words removed, errors fixed, fragmented sentences cleaned up,
 paragraphs added, technical terms standardized. Original content and
 speaker voice preserved throughout.*


## Opening and 2025 Highlights

**Christopher Allen:** Welcome everybody to the Blockchain Commons Gordian meeting for January 2026. What is Blockchain Commons? We're a community interested in self-sovereign control of digital assets. We bring together stakeholders to collaboratively develop interoperable infrastructure. We design decentralized solutions where everyone wins. And we're a neutral not-for-profit to enable people to control their own digital destiny.

Thank you very much to our sponsors over the last couple of years. It is through their funding and support that we've been able to do this work. If you're interested in becoming a sponsor, please contact me.

So happy new year. I hope everybody had a great break. We're going to, in this session, briefly go over highlights of the year. We're going to have a demo of a stack of our blockchain commons technologies and a discussion about updating known values.

So 2025, while we're working on the full blog post to summarize the year, in brief, we've had quite a few new technologies, new directions, and a Frost and Hubert capstone at the end of the year.

One of the key technologies that we've introduced is Gordian Clubs. These are autonomous cryptographic objects. You pass them around as envelopes of data. Thus, you're able to take advantage of encrypting the data, signing the payloads, et cetera. But these are infrastructure-free in the sense that these objects don't require a server somewhere for you to be able to work on them. Thus, they preserve agency when infrastructure fails. That makes them a very strong foundation for a number of kinds of projects we want to be working on in the next couple of years.

In 2025, we moved forward more on provenance marks. What is a provenance mark? It's a forward commitment hash chain. Basically, it orders linked digital objects. This allows you to do a wide variety of things, track additions, state changes, histories. You can verify against fraud. And I particularly am pleased with how it really is very simple and elegant and doesn't try to overdo what some other kind of provenance systems work. And that allows us to have a lot more flexibility to use it in ways that other kinds of provenance and attribution do not. Kind of think of it as a mini personal blockchain.

Hubert was the capstone of last year where we demonstrated a dead drop hub. One of the critical problems in privacy is point-to-point connections can reveal a lot of information. The fact that I am having a conversation with someone in Taiwan is much easier to track than you might think. You may not be able to know what the conversation is, but the signal analysis to know that I am communicating to someone in Russia or whatever is a problem. So we wanted to be able to offer an alternative. The alternative is Hubert with its dead drops. It basically exchanges messages through distributed storage, and it enables extreme privacy because the messages look like noise. This foils surveillance and censorship and offers decentralization. So you definitely should be taking a look at our videos on that.

And of course, on the cryptographic side, Frost is something that we've been talking about for a couple of years. It's finally reached a level of maturity to be able to do true aggregated multi-sig and have all the privacy advantages for it. It enables some fundamental new capabilities, including more reliability and resilience in key shares, et cetera. We did demos of Frost with Bitcoin. We did a demo of Frost with Hubert. We have Zcash Foundation's signing verification tool. And we have a new short course called Learning Frost from the Command Line. So if you're interested in this important future technology, it's becoming less future and more today.

Thank you very much for support from our community. Many of our libraries have been converted to a variety of languages, over 10. In particular, late in 2025, there was a big expansion of TypeScript support. Our native support is in Rust, but TypeScript is a very important community. And in fact, there are now two playgrounds based on our specs that people have been working on. Which brings us to Leonardo. So, Leonardo, I'll let you share your screen. And if you would introduce yourself a little bit.

## TypeScript Playground Demo

**Leonardo Custodio:** Yes. Hello, guys. My name is Leonardo. I'm a software engineer at Parity, the company that develops Polkadot. And over there, I work in the products department. And I was introduced to blockchain commons by our product director, where he gave me the direction that you guys had some specifications that could help us out building our products. So that's how I got here.

And as a product builder, I know in the blockchain world, Rust is probably one of the best languages. But the products that we are building, they are not in the blockchain layer. They are in the web layer. And I know you can use Rust for web as well. You can compile for WebAssembly, all those kind of stuff. But we have a few requirements, specific requirements. And it would be easier for us if we had a comprehensive TypeScript implementation of all those specs.

So that's when I started to migrate all the amazing work Wolf made with the Rust implementation. And I started to migrate all that to TypeScript. Been doing that for the past few months. And together with that, I started a new playground where I could myself debug and see what's happening, to help them out with my own development and see if everything is working fine or not.

So, this playground was designed with my necessities right now. As far as I know, I'm one of the very few users that I'm using. But I appreciate any feedback and collaborations. Everything is open source. Anyone can contribute, can open issues, can give their experience and feedback. and I would love to have them and to collaborate together. So today I'm going to go over the playground. I know there is another one. I think Irfan's here, he made the other one. I got some ideas from your playground as well. So thank you a lot for those. And well, let's go for it.

So I started, again, just Playground out of my own necessity. And it changed, the UI changed a bit over the course of the development. And I'm now in a position where I want to make something more like an IDE-like Playground. So I might even drop the word playground, and it's going to be like an IDE, integrated development environment. And the reason for that is that I'm putting on the playground not just the data analysis, but also a few other features that will be able to help us to build this data.

Because right now, at least for me, when I had the data playground, I had to make the TypeScript code and code the CBOR or Gordian Envelope or whatever, paste it here and see the analysis. But I felt the necessity to, okay, but I don't want to create a project. I don't want to make the TypeScript code, encode all that. I just want to do that from the browser.

And that's when I came up with the idea of the Envelope Builder. And that's when I changed the whole thing to maybe, okay, maybe we should expand the idea and not be just a playground, but rather a set of tools, integrated tools that can work with the whole blockchain commons stack and can help out a lot of different things. So I'm going to present to you guys what I am right now. And hopefully, in the next meeting, if I have more improvements, I can talk about a bit more again.

So like the other playground that we have, we can import, and we have some examples here, any CBOR objects and it also auto identifies what kind of data you are putting on there if it's a hex or CBOR if it's JSON by words or whatever you don't need to specify and it again took the idea from the other one. You can also convert that to other formats. So if I want to convert that to JSON, I can see how it looks on JSON. I can see how it looks with byte words. I can see how it looks with the diagnostic notation and the CBOR object itself.

One thing that I made a bit different is, when we are analyzing the data, usually we focus on the output. So whatever I chose over here to see the data and I thought that having a split screen between inputs and outputs was kind of a waste of screen because I'm not touching the input, I'm not looking at the input and just looking the output. So I made this difference where we have just a single window and you paste there the input and that's as well your output. You just change over here and that becomes in the same window you can see everything. Also you can change over here, I mean, it's not like a static, right? So you can change the information over here.

And the other thing that I felt, at least for me, necessity was to when we are comparing some data, we want to compare different objects. So I made this kind of multiple tabs approach where we can have one data here, another data over here, and we can see how they look side by side. And just multiple tabs approach. There are some issues, it seems, I might need to look at. But let's see. And so there is just split screen. You can add as many tabs as you want. And I mean, as many tabs as you want in a screen, in a window, or you can add as many windows as you want. So each window can have multiple tabs.

The other thing that I felt the necessity, and I think that's not different from the other program. Also, again, took the idea from there is to scan the QR codes, right? So you can over here, you can either scan a QR code using the camera over here, or you can upload a QR code. So let's open, for example, this QR code over here. It's going to open another tab with the output of the QR code. Let's use another one over here. And again, it's going to open another tab. And I chose to always open other tabs because sometimes you don't want to overwrite what you have because you're going to lose and then you have to find the information yet.

Another cool feature that I thought was, let's say I have over here, right? So I have this object and this idea I got from the VS Code IDE. When you are looking at Markdown files, you have this approach where you can make the split screen, but you have the same data and showing in a different representation. And that's what it means over here. So I can see that data as a QR code over here. And if I change the data, it's going to update the QR code as well, or it should update the QR code as well.

So you can either just like VS Code and see the data over here, or I want to see the QR codes, or I want to see both. Another thing that you can do is there is the dark theme, those kind of stuff. Some people prefer to have another kind of theme, right? But another thing is the playground also, or this website, it also saves data on your browser in your local storage. So if I, let's close this over here. Let's open again. It's everything going to be there again. It's saved. So you don't need to put the data over and over again whenever you go to the website. So it's always going to be saved regardless how many tabs you have.

And again, this playground, I'm not sure about the other, I might say something wrong here right now. But I'm starting to support as well the Gordian Envelopes. And the playground identifies that's an envelope automatically. So if I open a CBOR object here, you're going to see it says that's a CBOR. It has 20 bytes. But if I open an envelope, it's going to say it's an envelope. It doesn't even need to have this. It still knows it's an envelope. Or if I get where you do it. So let me just remove that. And it still knows it's an envelope. And while if I go here and that's not an envelope, it's going to say it's not an envelope because it knows by the tag that it's not an envelope.

So I start to add this envelope support. And that's really helpful because for my requirements, what I'm looking for is specifically the Gordian Envelopes and that's why I'm working this to build the envelopes.

Another thing about the playground and that I see the reason why I'm maybe changing the name but before that let's go to the register again. Another idea from here, nothing new. We have the CBOR tags. We have the known values over here. I think Christopher is going to talk about how we can add known values later. Right now I have a register in my own repository but I think it would be nice if we could find a way to have a repository where we can have those. Because if I add something in my repository, it's not going to be in the Rust one. If you add something on the Rust one, it's not going to be online. So maybe we need some way to have a centralized register.

**Wolf McNally:** But I intend to address this in my part of our talk. So I think you'll be pleased with the progress we've made there.

**Leonardo:** Okay, awesome. Thank you very much. And so we have the registers over here. And I have also added the IANA register. And this one actually is not in the repository. It fetches directory from the IANA website. It parses the XML file and shows over here. So if they change over there, it's going to show over here again. It's going to automatically update over here.

So the more, again, most of that is nothing new. I got the ideas from you. Thank you very much again for all that. And what I think was new to me and what I hope to make some progress in the next few days is just envelope builder parts. So again, I found this necessity when testing, where I always had to use either the TypeScript code to generate the envelopes, or even I know Wolf has a CLI to do that, and that's very useful. But I want to just go in my browser and see, oh, how can I do it? So I'm just going to do a sneak peek over here. This is not fully working. There is a lot of things working, but it's not all features working yet. It is still work in progress, but I hope I have something more stable by next month.

So the envelope builder basically has, either you can start building your own envelope with assertions step-by-step. You can use a template or you can use a wizard over here that basically guides you to what you want. So I'm going to start with the first one over here. So just create from scratch.

And so Leonardo identity. So I created Leonardo identity over there. It adds the envelope subjects. It shows me the three formats, or I can change to the hex to the CBOR format, or you can see both again. And from here, I can start adding other fields, other envelope features. So for example, if I go over, it also shows the route to digest the short form over here. But again, I can go there and start to add more stuff.

So let's say I want to add now, I don't know my name. So my name is Leonardo. So it has added over there my name. Now I want to add my age, age 33. It has added over there my age. And it's not just adding information, but you can also use all the other features. And that's what makes Gordian Envelope awesome, right? So the privacy stuff with encryptions and all those kind of stuff. And I can also do that from here.

So let's say, for example, I want to hide the age here. I can go over there and select selective elision. And, okay, I don't want to show my age anymore. So the field is now elided. Or I can go there again and show it again. It's going to go back. You can also add signature, the proof, type attachment, whatever you want. You can wrap the whole thing. You can add the salt.

So let's say I want to encrypt the information. It's going to encrypt. And you can see it's gone from here because you cannot see anymore. And what I'm still working is the key management part. So what if I want to test, okay, someone with a key would be able to decrypt this part or someone with other keys will be able to decrypt other parts. So I'm working this part on the key management where you would be able to see how the envelope works for different people based on their keys.

You can also do that's just for displaying. You can just order the fields over here. I know that doesn't change the output because it's sorted, right? But anyway, just for looking in the interface, it's useful.

And another thing is, OK, I have built my envelope, and now what? Well, you can export that as hex, as uniform resource, as a tree, basic, or you can even save as a QR code. You can do the opposite. You can get a data, you can get over here. And we can come here and import that. And it's going to show in the same way. You also have redo, undo, so I did something wrong. You can go back.

And again, about the templates, so that's refresh over here. Just to go new. So with the wizard over here, you can choose, OK, that's someone who has no idea what can be built with an envelope. You can use the wizard and the wizard is going to help you out with a few suggestions and obviously that's the suggestions that I have right now but that can be expandable and improved. And the same for the templates. Yeah so let's go over here so let's say I want a template, a certificate, it shows over there one template of an envelope and that's easy. You can put different templates in the repository very easily, many of them or whatever.

And that's why I have renaming the thing going to an IDE because I want to make those kind of tools where you can do, you can use the whole blockchain commons stack in here without installing anything, without dependencies, and you can even do it in your phone if you need to.

Yeah, so I think that basically covers where I am right now. Like I said, there is a lot of things that are already working, and not all of them. Those are based on my needs. If you guys have any needs that you might have any suggestion, please send an email or open issue in the repository, I would be glad to improve it.

And about the repository, about the TypeScript implementation, hopefully I can go over, not right now, maybe in the next meeting. I prefer, I talked with Shannon, I prefer to not go over the TypeScript implementation right now because as I said, I'm importing everything Wolf has made and to make it easier, to speed up this part, I'm importing line by line as is. So I'm not using any TypeScript idiomatics, not that much, object-oriented approach.

So I'm using how things are done in Rust, because otherwise it would be more difficult to translate other packages if I have changed the way it was done. So I'm doing that first. After I'm finished translating all packages, then my plan is to start refactoring and using more TypeScript idiomatics. And that way it makes it easier to do the refactor because we have all packages. And at least with the TypeScript, it's organized as a monorepo. So if I change something on the CBOR and try to compile is gonna fail all the other packages and that helps us the refactor.

Anyway, that's the reason why I do things are working. I would say if possible, wait a bit more before using because the interface, the APIs might change after I do this refactor. But again, if any of you guys don't mind some breaking changes, it's ready to use and it's working. Yes, thank you very much. That's my progress in the playgrounds. Again, hope we can collaborate together and improve that as well.

**Christopher:** Thank you, that's fabulous. I'm excited and also a little, it's like I'm working on this ZID tutorial that walks you through step-by-step creating a digital identity to the best practices of a pseudo anonymous identity and all that. And I'm kind of like, but it'd be so much easier if I could do your builder instead rather than assembling it, I mean, this is the, where is the, like, here's my code for the, creating your first ZID, here's the shell command for creating it. And then this is what the output looks like and all that kind of stuff. And it's I think it's a solid demo, but it'd be so much better if there was some way to do it within the playground.

**Leonardo:** Thank you. Thank you very much. And another idea that I want to build, but it will be maybe the next step. There's a lot to do yet. I want to make it possible to be like JS Fiddle where you have the snippets and you can make a link and share to others. So if you build a really good envelope or you want to document some APIs or envelopes, how it works, instead of you saving that data in somewhere or just saying to the person, you can send this link and it's going to open the playground with the snippets, with the information and all that. So that's one next step that I have in my roadmap right now.

**Wolf:** I think it's great progress you've made, Leonardo, and I really want to congratulate you on your progress so far. And I think I speak for Christopher and myself that the community starting to pick up what we've done, translate it to other languages, TypeScript is a major language that we've been looking at and so on. It's a dream come true for us because it means that there is traction there and this will continue to snowball. So thank you for the leading role you're taking in helping our technologies migrate to other platforms.

So I take it that right now, at this point, you haven't deployed these packages to NPM or anything like that, but that will happen later on after you've got more kind of a TypeScript interface on them. Is that my understanding?

**Leonardo:** No, no, it's actually deployed to NPM, but it's on alpha version. I'm making sure that people know it's an alpha state so you could have breaking changes. But yes, if you go to NPM and search PCTS over there, you can already import the packages in your own projects and you can use it. And another thing that I made sure is to put on what was the reference implementation on each of the packages because I expect you to be updating and making them better. So it's important for me and maybe for other people to know, okay, so this package only has stuff up to this version of the Rust implementation. But again, all packages are there already and available for use.

**Wolf:** Yeah, I definitely agree with your strategy of doing a straight across translation from the Rust and then adding on a more TypeScript interface as a second phase. So obviously I spent a lot of effort trying to make all our Rust APIs very rusty. So people who are comfortable with Rust feel very comfortable with our APIs, same thing with our Swift implementations. And so obviously the finishing touches on the TypeScript things will be to have APIs that TypeScript experienced users feel comfortable with. But I definitely appreciate the progress you've made towards that.

**Christopher:** I also want to encourage you that if you spot kind of some edges, I mean, you're basically going through the tests that are in Wolf's code and then implementing the tests in TypeScript. But if you see edge conditions or things where there's an additional test that maybe should be added, you know, there's an additional test that maybe should be added, you know, won't necessarily change the Rust code. I mean, obviously, if you find bugs, report bugs. But when you find an edge condition that might help another port, that also could be really useful.

Because I know that we have some people that want to move it to Python, especially given a lot of the AI work these days is done in Python. And that seems to be one of our other big targets. And Python is yet another difficult to be deterministic on variables language. So if you spot something that would make the test code, test harnesses better, let us know.

**Leonardo:** For sure. And I would like to thank you all because I'm just making the playground where you guys made all the specifications and you guys made all that possible. So without you guys, nothing would exist.

**Christopher:** So, Irfan, you also have been working on a playground. Did you have any questions or thoughts?

**Irfan Bilaloğlu:** Well, Leonardo, it's great work. Actually the side-by-side view. I think we are coming from the same place. I had a lot of trouble when trying to change some variables and put them into documentation and creating the URs and CBORs. So that's why, actually, I needed a playground to play with the data and see it immediately. Otherwise, we were just writing in the code and seeing the results. This is much easier like this.

And I really like the side-by-side view because generally, that's what I do also. I change a little bit and see the differences, maybe do a compression or use types and see the result. If actually on the CBOR or in the byte size, it's smaller. So I love your work.

And it's a great idea to create the envelope creator. I also had an idea to create the UR creator where you can craft the URs for you. But for us, we already have been using. And so my mission was to make it compatible with the previous versions. So it would be more compatible with what we were using. Yeah, no questions. It's great. It's great. We can actually work on the definitions and known values, maybe.

**Wolf:** Let's talk about that in a moment, because I think that's a larger conversation.

**Christopher:** I did want to say a little bit about what's coming up this year that this will help with, just so you both kind of have an understanding. So we have a number of mostly Bitcoin wallets that have been using a variety of our standards at the lower levels. So pretty much any animated QR today on a Bitcoin wallet is using our standard. So that means they're using CBOR, they're using some simple envelope stuff, and then the UR for the multi-part and then the UR QR of things.

But what has not been widely adopted is, and most of that is for multi-party descriptors that allow for multi-sig to happen. So somebody can basically have a dedicated device that it doesn't have a net connection or whatever, and can do signing and other different types of stuff with an air gap. That seems to be the primary use case.

But last year, funded by the Zcash grants, the Zcash community basically said, hey, we have interoperability problems, too. We're getting ready to deprecate Zcash-D, which was based on Bitcoin-D and has been getting woefully out of date for years. So we've got this interoperability problem. So they began work on something called ZWIF, which is a wallet interchange format that also uses more of our stack. And there's been some success with that, but it's also been kind of hard driving it forward, helping people understand what it is that's going on. And these tools will help.

But as we now move to the upcoming year, all of us have key reliability and resilience issues. How do you store private keys? Where do you store them? How do you do so safely? How do you do them in a distributed fashion, either through secret sharing or a variety of other different techniques? How do you do air gaps? And I think there's a real opportunity this year and some critical mass since these two communities.

And now I think I don't remember, both of you guys are involved with blockchain communities of really standardizing and making what we started calling CSR, which is collaborative seed recovery, working a lot more broadly across multiple blockchains. Bitcoin deals with the specifics. Zcash wants to know the number of blocks since Zcash started. Bitcoin wants to know the number of blocks since Bitcoin started. And so you have all these things that are almost the same, but not quite, that we should tussle on so that we can basically not have our keys in the browser.

I'm looking forward to the point when you need to sign something in your browser and you just flash up the QR code and the QR code between you and your browser will present, please sign this, signs it, returns it back. Or does a key share or all those types of things. So we don't have keys in our browser.

And so I'm hoping that these efforts that you guys are doing with TypeScript will help us get there, especially more support with the QR UR. We are doing a lot with ZIDs, which is our kind of DID-like. I kind of call it a DID 3.0. I don't know as much if your teams are as interested in that. Some interesting things about ZIDs is we do support lots of different key formats, and we pretty much support everything that SSH supports. So we can basically leverage all the different technologies that are out there that use SSH keys, such as YubiKeys and other different types of things over this next year. So if there's some particular interest in supporting some of those types of things in your tooling, let us know. But that's definitely a big target for 2026.

So in summary, these are going to be great for learning how this stack works. But I'm really looking forward to having more examples of things like the envelope that we use for seeds, SSKR for social key recovery, various kinds of elision. I mean, one of the things that I personally run into as we're moving into tax season is I want to be able to give my accountant all of the things so I can do my taxes properly. But I don't want to give them all. So I really want this elided capability to be solid in there where I can just basically give them an elided version of my transactions and they can deal with the taxes stuff. And anybody who's tried to do the right thing with crypto accounting knows how painful this is.

So that's another area that we'd like to do, provide an alternative to ledger key recovery, which locks you into Ledger's choices as far as key recovery places. And then moving toward the ZID identity and clubs and Hubert if those are important to you in the coming year. Am I missing anything, Wolf or Shannon, as far as for the coming year in regards to these playgrounds?

**Shannon Appelcline:** I don't think so. I think we're good.

## Known Values Discussion

**Christopher:** So as we move to known values discussion, let me see. Quickly, let me look at this, if I should pull up the slides again. Yeah, I will quickly share.

So for those who aren't as familiar with what are known values, so basically it is a way to have integers represent concepts. It's a little bit different than the way things are registered for CBOR tags in the sense that those are more about the actual format of the data. Instead, this is things of what is the concept of this is associated with this in this particular way.

So like CBOR tags, they are compact objects. So the lower values are smaller, the higher values are larger. And we have a basic set of them in our research. And so both Irfan and Leonardo have requested about adding some more values there and leveraging them in the stack. I mean, part of our thing here is you can obviously just put a text string in the, instead of a known value, but then it's larger and there are other different kinds of issues there. We'll always support that, but what is our process to add things?

And I think the tension that even Wolf and I and Shannon in our discussions haven't quite puzzled out is how broadly and how fast do we want to move on this? Part of me kind of just wants to do it one by one with a spec behind it or a justification of some kind behind it. But then there's also potentially ideas and Wolf will talk a little bit about it in his thing about bringing in some broad swaths for this.

We're not a standards organization. So right now, since we believe in decentralization, there's also some itchiness of being a centralized repository for this. And how do we move it to be a distributed solution in here? But perfection is the enemy of the good. So we need to do more soon and then we can puzzle out some of the bigger problems later.

But before Wolf talks, Irfan, I think you were the first to basically start adding some known values. Can you basically characterize what were your instincts as far as adding some additional known values?

**Irfan:** Well, we have a standard way to communicate data, but we didn't have a standard way for understanding that data. I mean, we can all just put a string in there, but in order for other wallets to communicate with us, I mean, if I'm a hardware company and I can show the QR code, I need a way to sync it with the watch only wallet and then be able to sign it, right? So that brings adaptation.

And when I first started this, I started to search for, are there any standards for the synchronization between hardware wallets and watch only wallets or are there any standardization between signing these things because it's a broad economy and the wallets are generally supporting more than one coin.

So the problem started with that and so we started writing a sync protocol for us. First we were only using a JSON data, but then it's just JSON data and it doesn't really use the features of the CBOR, which you can represent the binary data as binary, right? Then we started to think about it and try to come up with a sync protocol that every wallet can use.

So that was the start of it and come up with a way to identify the unique coins, their tokens if there are NFTs in it. So unique identification. So those types I believe are going to help us broaden the community. I mean even Metamask as you said, even the web browser wallets can support those types or just import in TypeScript library which will be able to give you a class or a type and then you know what you are getting, you know what you're sending.

That was the first idea for standardization and using the types that we all have. Actually, we have a paper about the sync. Maybe I can write here if you want to check it out.

**Christopher:** So you added some known values to your own list, which we should formalize. How many did you add to your list? How did you make decisions about those?

**Irfan:** Well when writing the library our focus was what we used in the sync protocol or sync type, that was it because we had a time constraint problem. So only be able to migrate those types, not everything that you already wrote as specifications. But that's just a library problem that in a broader sense, the libraries should be able to support all of the core blockchain commons values, then the companies or persons can select or import any other types they might support.

**Christopher:** Okay.

**Wolf:** So Christopher, I actually do have a fair amount to say that may kind of help steer this conversation.

**Christopher:** Yeah, I want to get there. I'm more trying to get the motivation. So I think the clear thing here was one of the key motivations was issues having to do with the wallet and syncing and having a common architecture for describing how those synced objects are represented and the concepts for them.

I'm actually going to jump. Dorian, you had some experience with the ZWIF project where we basically did not do known values, add new known values in the end. We used all the ASCII representation for things there. Was there any particular aspect of known values or other stuff when you were working on some of the ZWIF interchange format that were known values or having further discussions about this would have been useful?

**Dorian:** If I remember correctly, yeah, I did discuss this with Wolf at some point, but like we ended up using the one that was already existing. But yeah, definitely moving forward, we'll definitely need to know, or it'd be easier to know how to contribute to that list, for sure.

**Christopher:** So in that particular case, it's really around how do you fully port, I mean, the goal of the Zcash ZWIF project is I have wallet by manufacturing, by software company A, I want to be able to move all of my data to another wallet by a different company and be able to have full fidelity of at least what they have in common. And so that's everything. That's transaction data, that's private notes, that's everything.

And I think we kind of punted on some of these known values in there. And then I want to close with what's kind of motivating, Leonardo, your thoughts on, not so much proposals because that's what Wolf's going to be talking about, but what's motivating your work here? What's our target?

**Leonardo:** Yeah so it's very it's a bit similar to Irfan. So we are at Parity and we have our own chains and that's Polkadot. And at least for the hardware wallets maybe we don't have that much issue to worry that much about if the object is a bit bigger or not in the computer or the browser or in the iPhone. But for hardware wallets like a Ledger, there is a very strict limits of the information that I can give to Ledger and to decode and show that.

And at least on Polkadot, we had an issue, not anymore, but with earlier versions of the Polkadot app on the ledger, we were unable to load the whole metadata of the network to show the users what they were signing. Because the metadata was larger than what ledger could handle. So there was a few information. There was what we call it extrinsics, but it's mainly transactions. So there were a few transactions that we were unable to show the user what each thing means because there was not enough space to load the whole metadata on the ledger device.

So for me the known values is important especially for this kind of device that has very limited memory and my goal would be mainly to add these values that are related to Polkadot itself.

**Christopher:** Right. So one of the other things that, we've had 10 years or more now with kind of tussling with the context and schemas at W3C. And one of my fears is that we end up repeating maybe some of the mistakes that they made. I think fundamentally, I think one of the fundamental mistakes of the W3C standards, JSON-LD in particular, is there's such a tight binding between the context and the data objects because you sort your data objects by the context. That means you have to understand the context and the schemas in order to do the sorting. And that is just a fundamental violation as far as I'm concerned of layers.

We don't have that problem with envelope. I don't think so. I don't think we're going to have that kind of problem that they have. And this has caused a lot of problems for W3C and IETF where the IETF people have kind of rejected schemas entirely. And we, I think we're avoiding some of that also.

But I think there is a real tension here of how much is it that we really want to just allow various parties to claim a space and here's a Polkadot specific value or here is the Zcash date, which is the date since Zcash, the number of blocks since Zcash was first started or the number of blocks of Bitcoin versus things that have a little bit more cross-chain compatibility and defining them well versus importing much larger sets of things. So I think that's the tension we're dealing with this conversation.

So Wolf, you had some thoughts and a proposal to consider?

**Wolf:** Yeah, actually, I have more than a proposal. I have actually some advancements to kind of talk about here that I've made recently. And so I'm going to introduce myself, if you want to break this out as a separate video, because I kind of think I want to do a bit of a known value State of the Union. That's okay. I'm sorry, Irfan, wait one second.

**Christopher:** Irfan, you raised your hand. Did you have a question?

**Irfan:** Before we move on, I wanted to clarify one thing about the motivation. We had the output descriptors and HD keys for syncing and we used them. But there was one fundamental problem with the watch-only wallets, at least for our case, is that they could only share Bitcoin and Bitcoin-based wallets generally. But they are just sharing one coin in that sense. There is no separation of the coins.

But in real world right now, the hardware wallets or watch-only wallets are consisting of portfolio of coins. So we need a way to share a portfolio along with the tokens, if they have tokens. And the problem is you're sharing Solana, Ethereum, Ethereum-based coins, which has different network IDs. And all of them, that was our main motivation to be able to share your whole portfolio with the watch only wallet in one go. So they used the known types like HD key or descriptors inside our types. That was the motivation. I just wanted to clear that up.

**Christopher:** Okay. Go ahead.

**Wolf:** Great. Okay. So I'm going to share my screen and then introduce myself and just kind of talk about known values in general for a moment and then talk about the advancements I've been making in that, which I think will address a lot of the points that have been raised so far in the conversation. So let me show my screen here. You should be seeing my browser window here with the Blockchain Commons Research Repo.

**Christopher:** Yes.

**Wolf:** Hi, I'm Wolf McNally. I'm lead researcher for Blockchain Commons, and I'm going to give a little bit of a known values state of the union talk. This will be centered around our research repo at GitHub Blockchain Commons Research. And the research repo contains a set of documents in various stages of development. You can see the various kinds of stages of development we have here at the top. And then the BCRs themselves, Blockchain Commons Research Papers.

The one we're going to focus on is 2023-02, Known Values, a Compact Deterministic Representation for Ontological Concepts. So let's look at that paper. This has been recently updated in several ways. So if you've read it before, you might want to go through it again.

But so what are known values? So in the realm of the semantic web, which is the idea that you can connect everything in the web using graphs of objects, and this was pioneered by a lot of search engines like Yahoo and Google and so on, the idea of semantic triples, subject predicate object, to represent knowledge data, like Alice Knows Bob, became very widespread.

And to support this, people started creating what are called ontologies. Ontology, and you see the word ontological used here, ontology is the branch of philosophy that deals with what exists. And that's all you really need to know about it. It's a fancy word, but it basically means what exists. So an ontology is a list of things that exist. And what do we mean by things? Well, things in general, we mean concepts.

And each of these concepts, like say a box is a concept, a unit of cryptocurrency is a concept. So almost anything, well, pretty much anything you can think about or point at is a concept. And we can distinguish these concepts by giving them names like box or cryptocurrency type. Some of them are classes, some of them are properties on things like the color of the box. The color is a property. That is also a concept.

And so people have come up with a number of different ontologies and ways of representing ontologies, the chief among which is the Resource Description Framework, or RDF.

Gordian Envelope was also based on the subject predicate object triple idea of representing knowledge. And as we proceeded in this, we wanted to come up with ways that we could do this compactly. Gordian Envelope is a binary structure. And although you can use anything as a subject, a predicate or an object, including just a string. So if you have Alice, knows, Bob, you can use the string knows literally as a UTF-8 string. We realized that it would be valuable to have a more compact way of representing these concepts like knows.

Now, the way that ontologies currently do this is by using a URI, a full URI that points to both the ontology and the concept within the ontology. And we'll talk a little bit more about that later. But that is a longer string, much longer than just simply knows, because it contains essentially a pointer to where the definition of that concept lies. And that's a valuable idea. But when you're trying to create compact binary structures, then that tends to bloat your structure.

And so what we did is we introduced the idea of known values, which is a centralized registry of integer values in a 64-bit namespace that represent where each one identifies a particular ontological concept. And so known values is the unique 64-bit integer. A code point is a specific integer in that space. The canonical name is a unique name that can be a symbol for that concept. And the registry is a centralized table or database containing the mapping between code points, canonical names and synonymous URIs, if any, because all the current ontologies have URIs, but known values don't require a separate URI, although we recommend that you have some kind of documentation somewhere for what you're using known values for.

Known values themselves, as I said, are integers and our stack is based on CBOR, the concise binary object representation. And in particular, Gordian Envelope is a CBOR structure. Within a Gordian Envelope, you can have basically tagged CBOR objects. In this case, anything tagged with the CBOR tag 40,000 is in fact a known value. That's in the IANA registry.

But within Gordian Envelopes, known values are represented not by a tagged value, but just by the bare integer. In fact, the only place we ever use bare integers in a Gordian Envelope is for known values. And so their use within Gordian Envelopes is very compact. So if you want to have a CBOR object, which is a known value standing alone, then that would be the integer one. This is for the is a concept. But by itself, that would be encoded as CBOR as these bytes. But within a Gordian Envelope, it just stands alone as the integer one. So they're very cheap to use inside that.

Now, as I mentioned, this is a centralized list. So one is always going to be for everybody, everywhere, always. But it's a 64-bit namespace. So for human purposes, it's effectively infinite. So each registry point includes the code point, its canonical name, its type, for example, as a class, a relationship, and then a description, and then a URI.

There's a little bit of outdated information here that I will be updating soon, because it says the assignment project is a work in progress, but I've made some progress on that that I'm going to report to you now. So this is where we basically registered the CBOR tag 40,000 to be used with known values with IANA. And that's already in their registry.

So now our registry. So the way CBOR represents integers, basic 64-bit integers, is by using variable-length integers. So in the range of 0 to 23, only one byte is needed. And so, for example, ISA, which is number one, is represented as just the byte 01 in Gordian Envelopes. In the range 24 to 255, it takes two bytes and so on, up to nine bytes for the full 64-bit namespace.

So at this point, until now, we've had a range in the 0 to 999 range, which are the code points we've been assigning for our own various use. And before I scroll down, I'm going to click through to this because I've separated out now our assignments into a separate document.

This is our own kind of low-level ontology. It has 88 entries currently in the range of 0 to 705, but we reserve 0 to 999. And as you can see, one is a property. It has an equivalent in the existing ontologies, which is the RDF type object. The subject is an instance of the class identified by the object. And I'm not going to get into the details of that, but we can parse it out. You can parse it out pretty easily.

Anyway, these are the ones that we've identified as general. So we've basically occupied the very low numbers because these are things that we use a lot in our stack. We have a set of three that we use for attachments in Gordian Envelopes. We have a set that we use for ZID documents. We have a set we use for ZID privileges. We have a set we use for Gordian Envelope expression function calls. And as you can see, when you print out a Gordian Envelope, often with our tools, you'll see the actual canonical name. Internally, it's these integers.

We have cryptocurrency, cryptocurrency assets. This is where we would add additional cryptocurrency identifiers, cryptocurrency networks, Bitcoin specific, and then basically for graphs of various kinds. So we have, if you want to make a, if you want to mark a graph as a digraph or a directed graph, then it's 603. So that's our current registry.

So back to this. Now, there's been two big holes in how we've been handling known values. And I've made some progress in plugging those holes. One is the integration of existing ontologies into our structure, which would basically require that we go through all the existing ontologies, some of which like schema.org have several thousand different concepts and assign them unique known values. That would be a lot of work to do manually.

So what we've done is we've written a script that actually has gone through and assigned those, basically scrapes the machine readable versions of these ontologies, takes each one in order, in a deterministic order, and assigns them a known value.

So for example, RDF, which is kind of like the RDFS, which is the RDF schema language is basically kind of like the assembly language of the web. And so if I go to that, in our known value registration, we see here's the source of our source of truth. It has 36 entries. It has these code points assigned to it, which will now never change. It shows basically just the summary, not the full details, but you can actually always go to the source and check the details here. Their one-line description of these things.

But again, this is not the authoritative reference for these ontologies. If you want to use these known values, you should understand the ontologies themselves. That's not our job, but we are pointing to how we've assigned them here.

In some cases, like type is a one-for-one correspondence. So instead of using 1018 for type, we basically use one, which is ISA. And you'll see that in our registry, number one ISA refers back to this URI. So we have it duplicated. When we've already assigned low-numbered values that are what we expect to be widely used that are in one of these ontologies, we have basically assigned that value to that element of the ontology.

And that's rare. And we don't expect to do that any further because at this point, now we've assigned these numbers, we're not going to reassign them. So even if we come up, so we're either going to reuse numbers from existing ontologies now or define entirely new numbers in our own ontologies. And you're encouraged to do the same.

So now, obviously, if we're creating these assignments, we want a machine readable version. So each of these has a JSON version as well, which has the same information you saw in the markdown file just now, but in machine readable form in JSON, including the code point, the canonical name, what type it is, its authoritative URI, and the one-line description.

So this is basically a summary of the kinds of ontologies we have here. We have the blockchain commons core concepts, which breaks down the categories I mentioned to you. And then we have the set of standard support ontologies, which includes RDF and RDFS, which I've already mentioned. The web ontology languages are brief summaries of why they are important and why you might want to browse them for known values to use yourself. The Dublin Core, the Simple Knowledge Organization System, Friend of a Friend, FOAF, Solid, the W3C Verifiable Credentials Ontology, and Schema.org, which is the big one. And that basically has about 2,600 different wide-ranging concepts in it.

Now, the last entry is community assigned. And while we want to accept additional, for example, if you want to add new cryptocurrency identifiers that properly kind of belong in the 300 range that I mentioned here, I think it's 300, but let's see, cryptocurrency, 300 to 499. If you feel like there's known values that belong there, then just contact us and we would be happy to add them as known values in that range.

But we wanted to open up a range that anybody could do experimentation with and know that they had unique known values. And so we basically open up the range of 100,000 and up. Now, just want to point out here that this is within this range. So that's five bytes. This is also the range we're using for several other things like 4000 to 4099 is in this range. That's three bytes. So the kind of standardized ranges have a smaller byte footprint, although the max you can ever have is nine.

So we're not expecting this is going to be large. This is smaller than many strings, not all strings, but definitely smaller than all the URIs that you might consider using as a predicate. And of course, known values can be predicates or objects or subjects even. So known values are not restricted to predicates. It's important to understand that.

And so how are we basically doing the community assignment? So as you can see at the bottom here, this was the other big hole. So I mentioned the two big holes. One is the integration of existing ontologies, and the other is community known values, which is basically first come, first serve assignment.

So I'll just read this. The known value namespace reserves code points 100,000 and above for community submitted registrations. This allows organizations and individuals to register their own ontological concepts while maintaining the centralized coordination benefits of the known value system.

The community registration process is automated through GitHub Actions workflows. Submitters create a JSON request file specifying their desired code points, greater than or equal to 100,000, and submit it via pull request. The system validates the request for schema conformance, code point availability, and uniqueness constraints. Upon successful validation and merge, which happens automatically, the code points are automatically registered in the community registry.

And you saw these pages right here. Right now, if I go to Community Assigned, you'll see it has exactly one entry in it, which is my test assignment when I tested the workflow.

Then for detailed information on submitting community known value requests, including JSON schema, validation rules, and submission process, see community-known values. This is all taking place within the research repo. We may move this to another repo, its own separate repo. And we also may eventually, ideally, delegate this to IANA if this becomes a full standard.

So as Christopher mentioned, we don't consider ourselves to be a standards organization. But if this gets the ball rolling and an organization like IANA feels like it would be valuable to take that over, then we would work with IANA to actually have them take over this full registry.

So going into the community known values, there's how to submit a known value assignment here. Here's the full JSON schema of that. Here's an example request. Here's a template that you can use as a request. Like I say, all you do is you submit this as a PR, and then the GitHub action workflows automatically validate it. And assuming it's available, valid, and not taken, then it's automatically added. So you'll actually see it added to this. In fact, if you do some today, you'll see them added today.

Then if it doesn't work, let me know. But this way we have basically a very open way of allowing the community to just register their own code points for known values. And yet we have still a way of working with them for values that are going to be more of a highly standardized nature and deserve lower numbers.

So that's my update in a nutshell. I'd be happy to take any questions.

**Christopher:** I guess my, so the community registry aspect is very important and what we want to try to do, or what we are doing, I guess. And it kind of maps in a similar fashion to the way we do attachments, which is that everybody can do an attachment, declare that they're the vendor, and you can have arbitrary different things inside an attachment. So we have those kind of escape hatches.

But the other problem is that the history of ontologies and things of that nature is that there's just been lots of arguments and, well obviously we're not—

**Wolf:** Going to settle those arguments here. We're just the—

**Christopher:** But they keep things from moving forward. Now we've kind of addressed that by the fact that you can have, unlike every other format that I know of, you can have multiple predicates. So if you want to say this is an ISA, but it is also a specific kind interpretation of ISA that is only the way OWL thinks of ISA, because I think they're the same, but I've certainly heard arguments about various ontologies say, no, no, that is not the same. We think about it differently. Well, you can put both in if you don't care.

**Wolf:** As I mentioned, ontology is a branch of philosophy, and philosophers love to argue. The point here is that if something suits your needs and you're specifying a particular kind of Gordian Envelope format, these things are now all available to you. You don't have to think about, well, if I need something from RDF or need something from OWL2 or whatever, I can just go and use it. And it's on you to make sure that you're using it in a way that the philosophers will agree on, which they're never going to.

Or you can define your own. And it doesn't matter if there's overlap and you can either define your format to acknowledge that overlap or to be your own thing and document it publicly or keep it private and just keep those numbers to yourself and only use them for your private needs. The point being that there's no chance of collision as long as we have a central registry.

**Christopher:** Yeah. So I think that this addresses a lot of that. I do think that we may need to think a bit more about a slightly orthogonal side of this, which is how do we make decisions about numbers in the lower range? Or maybe we need to create another block in there for things that are genuine community decisions or at least two parties.

I mean, we're not a standards organization, but I really do appreciate the idea of, hey, something has a lot more value when two unassociated organizations are working together and both agree on something. So I think it'd be great, for instance, that if there's some Bitcoin specifics in PSBT that we kind of have locked in with some of our known values, but that would need to be slightly different for different blockchains. It could be that we have something that is proposed by two different blockchains or two different companies that gets labeled as, hey, there's evidence this isn't just an arbitrary addition to the registry. There actually is some discussion and reasoning behind this assignment and what is the rationale and the reasoning and how it works for two different blockchains or two different companies or whatever.

**Wolf:** Well, yeah. And that's, so what I would intend, and I've left the, I mean, you raised an excellent point. We definitely need to address that. The way the CBOR community addresses that with tags is they have three levels here. This is the IANA CBOR tag registry, where 0 to 23, which is only a couple of those left, require like an RFC level standards action. 24 through 367, of which we've registered two for Gordian Envelope and deterministic CBOR to CBOR, we've registered tags 200, 201. One, our specification required, which means that you can submit a proposal for allocation to IANA. They refer it to a community expert, a CBOR community expert who reviews it and makes sure that it has some merit and is consistent and all that. Their job is not to judge the standard or say this is on RFC tracker or not, but their job is to make sure that it at least has some consistency and some specification behind it.

And then first come first serve, you can submit the proposal to IANA and it just approves it. A human will review it. In our case, our current system that I just outlined using pull requests is entirely automated. So you wouldn't have to wait for me to get around to reviewing it. If it becomes abused, we'll have to rethink that. But for now, it's entirely automated.

I would say use it and use it well, but use it gently because I don't want to have to put in various kinds of human reviews or other controls, but that may be required eventually. Anyway, if IANA wants to take it over, they'll do that anyway.

So, but yeah, so having several different levels of kind of review process is definitely, I think, in the future of known values. For now, we basically have the level we control, which we have three levels. We have the zero through 999, which we're basically assigning as we see fit. And that can include other submissions from outside parties, but we'd have that conversation with them.

We have the range from 1,000 up to 100,000 minus one, which is basically reserved for existing ontologies that people have published as ontologies. And if somebody publishes a new ontology that they want to include in that, we would definitely, like a standard format, like RDF or whatever as the ontologies, that we could add that in a new range, in that range.

And then we have 100,000 and above, which are first come first serve and available for anybody, anytime, just by doing a pull request. So that kind of mirrors the idea of how IANA has done it here.

**Christopher:** Yeah. I would certainly maybe consider before we roll all of this in, because this is still new that we might want to reserve some of the smaller points for community specification. So maybe not start at a thousand, maybe start the RDF at a higher number or something that makes sense.

**Wolf:** Well, I mean, our RDF is pretty fundamental in terms of what it's, we're already using certain things that are synonymous with RDF. So also a pretty small set. So I understand that.

**Christopher:** I'm more concerned about some of the ones above it that have 2000 some odd. So—

**Wolf:** Well, obviously, yeah, yeah, we can discuss that offline. I think that the idea of putting things on thousand unit breaks was, so basically if people are seeing these numbers, they can say, oh, that's in the FOAF range or, oh, that's in the RDF range or that's in the OWL range. And so, I didn't see a need to be that conservative about how we allocate code points, because essentially we do have an infinite space and the most we can ever take up is nine bytes. And we do have a huge amount of space in the five byte range that is probably never going to be used up anyway. So I'm not really concerned.

**Christopher:** So like, instead of saying that it's a thousand to, can you bring, go down a little bit further? Yeah. So maybe schema.org doesn't have 10,000 to 19,000 or even, anyhow, you get my drift.

**Wolf:** Maybe this might, yeah, we can discuss that. I mean, perhaps we can do 1,000 to 1,100 and 2,000 to 2,200 and things like that. So I agree this is being fairly profligate with code point spaces, but there's nothing that prevents us from inserting additional, using those dark code points at some point in the future for any reason we would choose. And so I don't see a real problem with that. This doesn't really waste code points. It just puts the current ones on even breaks. And if we really need more three byte code points, there's plenty to be had.

**Christopher:** So, if people have thoughts on this, we need to get it in soon so that we can break, if there's one we're missing. Mike, I know it's 1130 and you may have to head out, but I saw that you had your hand up.

**Mike Warner:** Yeah, I can wait till after. Several suggestions, thoughts, questions, but I don't want to hold people.

**Christopher:** No, no, it's fine. Why don't you go ahead and I'd like to get your input.

**Mike:** Sure. So in terms of the horizon here, adjacent to what you're doing here, there's a whole bunch of unique known identifiers like MIC codes for markets, BIC codes for banks, country codes, currency codes, OIDs for standards, right? The list goes on. They're alphanumeric, right?

And a lot of them might, and then I'll also add interesting, which would be a lot of fun, kind of the lattice coordinates, like correlating to symmetric systems effectively that could be leveraged for creating kind of clustering or a lot of sort of advanced analytics type use cases. How well would this play with like alphanumeric? And then does it make sense to look at things, existing codes? For example, the UN has, I think it's, they have like geo codes. The world is 001, for example. Why not leverage those in effect and build them in?

**Wolf:** Sure. Well, some of those aren't predicates though, right?

**Mike:** Okay, hang on. May I answer the question?

**Wolf:** Sure.

**Mike:** Because I've given a lot of thought to things like this. So known values don't need to be used for everything. Known values are, because in an envelope, anything can be a subject, predicate, or object, including, say, for example, a tagged value, which, of course, known values are tagged outside of envelopes and untagged integers inside envelopes.

But if you take something like a country code or a language code, an ISO language code and so on, those are already pretty compact. You know, US hyphen EN or EN hyphen US for English is spoken US is only like five bytes itself as a UTF-8 string. And in cases like that, I wouldn't bother assigning known values to them because there's not really a big advantage. You just basically define your schema as this predicate or this object is an ISO country code and be done with it.

The big win when it comes to known values is having a very specific format because strings can be subject to misspellings, capitalization errors, and things like that. And one of the goals of Gordian Envelope is to be as deterministic as possible. Whereas a lot of these things like the codes you mentioned and so on are already very deterministic in terms of how they are. They're either integers or small strings with very specific formats that they have syntactic rigidity, so they're either valid or they're not.

And in cases like that where they're small, then I don't see a need to define one correspondence. In the case of all these ontologies I've mentioned from RDF to OWL to schema.org, their identifier strings are rather long. And if you use a lot of them in an envelope, they become quite bloated. Plus, small variants in typography or case spelling, which may be ignored by some, if you're trying to create a deterministic algorithm.

And again, part of a big emphasis of our entire stack is that if you want to create a deterministic consensus algorithm, something using a Gordian Envelope, we're providing a substrate to do that. We want to make it, we don't give you determinism automatically, but it's not hard. We want to make it so it's easy to get.

And so by assigning all these ontological URIs to single integers that are unambiguous, then you eliminate all those other potential foot guns when it comes to expressing these ontological concepts, as well as you get a size benefit because the integers always can be way smaller than those URI strings. Does that address your question?

**Mike:** It does, but I've got a million more, which is great. So yeah, I appreciate that. Really, really cool to see this. This is, I'm going to want to follow up with you guys on it. I've got a few things that'd be fun to talk about.

**Wolf:** Yeah, absolutely. And you feel free to set up a zoom conference with me or Christopher, just sidebar and just have one-to-one conversation as well.

## Community Registry and Future Directions

**Christopher:** Thanks. I do want to basically say there are some principles that were behind, one of which is that we want things like the community assignments, attachments, and other different types of things that are friction-free, that allow anybody to do what they need to do to solve their problem.

We don't want to say you have to do things, have to have, well we've kind of run into this with IETF. Supposedly a bunch of those lower numbers that are more compact, so they say specification required. But in fact there are two sets in there and there's a lot of little rigmarole that you go through to even get one of those. And we eventually gave up on trying to get even some of the ones that supposedly the only requirement was specification required.

So we want people to be able to get what they need on demand, make things work, et cetera. But I also feel like that there's some real value, especially as new people come in to basically go, this wasn't a one-time thing. This was something that multiple organizations, at least for blockchain commons, at least two, basically came to some kind of agreement on. And that those are signals of value, of consideration and care that we want to communicate as new members come to the community, that those at least have had a bit more review and discussion and whatever.

**Wolf:** And it's quite possible that an organization can create a public specification using known values, 100,000 or higher, that they register themselves that become even an RFC or other forms of standard. And that's fine because they just take five bytes each. And that's much smaller than any URI and all that.

But it's also quite possible that they may speculatively use lower numbers that are unallocated with the intention that to become part of the registry, they'll have a conversation with us at some point. And we'll figure out how that process goes as we go. What we would discourage is people choosing lower numbered things because they're not taken and then never having that conversation. That's known as code point squatting. And we, just like IANA, would discourage that. But obviously there's nothing we can do to enforce that directly.

**Christopher:** Yeah. So we are needing to wrap up on time. I wanted to ask Christophe, you had said that you were interested in maybe looking for a home. Any thoughts on the discussion that we've had today, given that?

**Christop Dorn:** Hi, can you hear me? I'm just on my phone.

**Christopher:** Yes.

**Christop:** Yeah. I've been using JSON-LD and trying to fit, I'm building a modeling system and I need to tag a lot of data so I can transform it and so on. And JSON-LD, I've been struggling to work with and I've had to kind of add various things to it to make it work for me. So I am very excited about this to fit it all into a very compact representation because that's exactly what I found that as you collect more and more data that you want to use in your model, it's just so much data. And how do you start organizing that?

So to combine that with the Gordian Envelope and all the features to disclose data and not, and obviously all the other things that come with it. Yeah, that's, I think, a great foundation to build on. So I'm very excited about that.

So my interest is in building on the TS playground that Leonardo is working on and exploring how I can visualize some of these lower level data structures. Because I think that that's a barrier to me. Like Leonardo, when you're working on the command line and encode, you get lost with all these different structures and relationships and so on. And that's my interest to visualize that stuff on a web page with the eye towards creating private spaces that you expand and you share and so on. So, yeah, anyway, so I'm very excited about that. So, yeah, thank you.

**Christopher:** And then one thing I would, especially given that we have people from Bitcoin, Zcash, Polkadot, and I'm sorry, Irfan, you do multiple chains, as I recall. Oh, you're on mute.

**Irfan:** Sorry, can you repeat it again?

**Christopher:** Yeah, you do multiple chains at this point or what?

**Irfan:** Yeah, we are like 20 chains.

**Christopher:** Yeah. So I would like to see sort of a side effort to kind of review some of our low numbers to find things that are ways to, I mean, at the root we have, I think we're all using BIP32 for keys on all 25 or so of these. But then they begin to diverge in how they do certain types of things.

And I know, for instance, with descriptors, rather than actually trying to define a descriptor as a type, we ended up just using the Bitcoin descriptor spec with the caveat that any public key can be replaced by an inline type of thing. But we need better solutions for this stuff.

So that would be another request is that you work with us to maybe have some better, not just the known values, but definitions that are more thoughtful to work, not just with Bitcoin, which we have more familiarity with, but work for Zcash, Polkadot, Cosmos, Tezos, whatever.

**Irfan:** Actually, can I give an example? I know we are out of time. I'm a little behind on the envelope thing. Maybe we should have started this with the envelope. We already drafted a sync layer or sync protocol, let's say. One of the problems we had was actually identifying the coins. So normally, we are just using SLIP44, right? Yeah. I've seen you already put the IDs for the coins as well. Maybe they can be directly taken from the SLIP44. They have a direct representation from the coin type to the coin name.

But we had another proposal for uniquely identifying every possibility of coins out there. It's a structured data. Maybe it's another talks topic, but can I just show the idea of how?

**Christopher:** Yeah, why don't you just very quickly, I mean, while you're posting that up, I think one of my concerns about SLIP44 is that it also made some very specific choices about revealing the paths to the derived keys that don't work with some of the privacy-oriented things that we're moving toward today. So I'm not quite sure, do we try to get SLIP44? I've talked with them before. They're not very open to community discussions there. You can add it as long as it fits their scheme. So there is some challenges here that are worthy of community discussion.

**Wolf:** I think it's important to point out that when it comes to defining known values that are one-to-one corresponding with any other namespace of values, that's not the intent of known values. If you discover that there is coincidentally a space available that corresponds one-to-one and you want to discuss us moving it there, then we could do that. But there's always a possibility that things will be added to the other space later that will conflict with an existing assigned known value.

So again, this might be something like ISO country codes. We don't need to define a known value for every ISO country code or language code, because those are perfectly good values themselves. And in your own data structures, you may just want to use the SLIP44 codes for the various kinds of cryptocurrencies and things like that. But, it's something to talk about. But again, I want to emphasize that known values have a specific purpose that is complementary to a lot of the existing kind of code bases out there, but more applicable to certain ones than others when it comes to saving space or creating unique identifiers for concepts.

**Christopher:** Again, it's around concepts. So sometimes coin is not a concept. It maybe needs to be a different thing.

**Wolf:** Well, it may be a concept, but it may already be successfully cataloged elsewhere, in which case there's no need to assign a range of values to it.

**Christopher:** So you were going to, you shared in a...

**Irfan:** Or you can just share your screen with the link. It's the concept or way to represent it that we came up. There are various problems with the SLIP44. One of them is for example Tezos. It doesn't just matter if you know the Tezos or not. It's okay, this is Tezos coin you are going to sync or sign the Tezos coin, but the problem is with which EC curve because Tezos supports three curves.

**Christopher:** Yeah. That's exactly the kind of problem. And then in Frost and with coordinators, there's this whole discussion about you don't necessarily want to reveal what your keys are. And there are ways to basically have that be determined later that doesn't represent well with the SLIP. And well, you get my drift. There's some privacy things as we move forward that we'd like to be able to support.

But anyhow, just mainly wanted to point out that we have people from multiple blockchains here. And I think there is some worthy discussion either offline, in the Gordian channel, in GitHub conversations, or in a future meeting to try to coordinate to do this right.

**Irfan:** Maybe you can put this also a topic for the next one, or can I just squeeze a little bit?

**Christopher:** Sure, go ahead.

**Irfan:** And for this one, the idea we came up with is actually for hardware wallets. Maybe it's better if I show it here, is actually started with the EC curve. So whatever hardware wallet you have, it will first check the EC curve if it supports that curve or not. Then comes the BIP44, SLIP44 ID of the coin. So I know this is Ethereum by looking at it. And the third one is just its ID. So I know which coin it is and which ID it is in.

And this one is actually CDDL for it. We have taken the data from IANA again for all the EC curves that are supported. And the first one is the elliptic curve, then we have the SLIP44 type and we have the subtype which helps us understand if this is Bitcoin or the hardware wallet can look at it and do I support secp256k1, do I support this EC curve. If it's just going to blind sign or like you're using Solana, you know it's on the Ed curve so you look at it in that perspective.

Another thing is for example Polygon. Is it Polygon on Ethereum or Polygon its own chain? So with this, you will be able to identify if it's Polygon on Ethereum chain or Polygon by itself at all.

**Christopher:** So I definitely like this as a starting point. I do think there's some richness of some things emerging that we also want to puzzle out. I mean, we've kind of run into this with ZIDs where we want to be able to represent a variety of things, including the quantum resistant curves. But also we have this encapsulation that we do with, we can do with Gordian Envelope where you can sign with SSH keys, but you can't encrypt with SSH keys. You can only sign with them, which is kind of a gotcha that a lot of these different approaches don't quite—

**Shannon Appelcline:** Although there is a workaround for that by deriving keys from a signature, but yeah.

**Christopher:** Yeah. But you get my drift that there are some interesting problems in this space. And for whatever reason, when I've tried to bring these up through both W3C and IETF, they're not very pragmatic about them because they basically go, oh, well, that's cryptography. So thus you have to go through the cryptography research group, which then is going to take a couple of years for them. And there's a lot of what I would call academic bias and not invented here type of problem.

**Irfan:** Just trying to get something working. Community, exactly. Community moves faster than that.

**Wolf:** I mean, one of the things, we want to work with these organizations whenever we can, but a big part of Blockchain Commons' mission is to kind of rethink privacy and digital sovereignty from first principles rather than designed by committee, based on kind of working with Christopher's vision for privacy and sovereignty and then rapidly producing things that just work in those areas better than anything else we know of.

Where there are things that we know of, like CBOR, for instance, we adopted CBOR, but it wasn't deterministic. So we created DCBOR, which is a subset of CBOR, which makes it much easier to create deterministic protocols. So we're basically working with what's out there rather than creating everything from scratch, but we're not letting ourselves be held back by things that are out there that are very designed by committee and so on. When we think we can do better, like Gordian Envelope over something like JSON-LD or whatever, then we're creating Gordian Envelope.

## Closing

**Christopher:** Okay, well, we're over time. Thank you, everybody, for hanging out for our whole session and thank you for the demo, Leonardo, and we're excited to see more with that and we're excited to work with you in the coming year.

**Leonardo:** Thank you, guys. Thank you very much. I hope to see you soon.

**Unknown Speaker:** Yeah, thanks all for great contributions. Have a good day.

**Wolf:** Bye.
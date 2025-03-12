---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-frost-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "FROST Round Table I: November 2023"
tagline: "Introducing the FROST Implementers"
hide_description: true
classes:
  - wide
permalink: /frost/meeting1/
sidebar:
  nav: frost
---

The first FROST Round Table occurred on **November 8, 2023**. Almost a dozen experts in the field, including the designers of the protocol, members of the [ZF Frost team](https://github.com/ZcashFoundation/frost), and members of the [secp-zkp FROST team](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/138) came together to talk about their experience with FROST and answer questions about its usage.

For an intro to FROST, see the [FROST page](/frost/) and ["A Layperson's Intro to Schnorr"](https://www.blockchaincommons.com/musings/Schnorr-Intro/).

## Media

<table width="100%">
  <tr>
    <td width="640px">
      <b>Video:</b>

{% include video id="U9MvNuyCpE4" provider="youtube" %}

    </td>
  </tr>
</table>

For more, also see the [rough summary](/frost/meeting1/summary/) or the [raw transcript](/frost/meeting1/transcript) of the event.

## Key Quotes

Quotes are drawn from [raw transcripts](/frost/meeting1/transcript/) and may not be entirely precise as a result, but convey many of the major themes of the meeting. See the video for more.

### Initial Thoughts

_Simplicity:_ "It's fairly straightforward to implement, but the hard part is getting deployed."
{: .notice--info}

_Community:_ "I'm excited to see the community coalescing and finding FROST to be useful."
{: .notice--info}

_UI:_ "my biggest concern ... is the user experience of this and what the user workflows are going to be."
{: .notice--info}

### Distributed Key Generation (DKG)

_Difficulty:_ "I think the most difficult part is the DKG."
{: .notice--info}

_Possibilities:_ "Several people have talked about ... rekeying and other different types of things that might be possible with distributed key generation."
{: .notice--info}

### Trusted Dealer Generation

_Usefulness (I):_ "I think my take on Trusted Dealer Generation is that it's not really worth the effort. I mean, if you're going to add so much complexity because you add a threshold scheme, then I think DKG really, really makes sense."
{: .notice--info}

_Usefulness (II)_: "I think Trusted Dealers can be useful for some custodians."
{: .notice--info}

_Usefulness (III)_: "The ability to do VSS and be able to have the shareholders sign different types of things without necessarily restoring is just really useful for wallet resilience of legacy keys, personal data, etc."
{: .notice--info}

### Resilience & Trust

_Backups:_ "In the case of Frost, it's a bit tricky ... You need to share in the group key and the identifier and maybe other people's identifiers. So it's more information that you need to back up or save."
{: .notice--info}

_Restoration:_ "If you have a threshold of people come together, they can restore another person's share."
{: .notice--info}

### Curves

_Variety:_ "The IETF draft covers a range of curves."
{: .notice--info}

_Efficiency:_ "[25519] is a bit less efficient than Ristretto because FROST needs a prime order group and 25519 is not a prime order."
{: .notice--info}

_Conversion:_ "If you compare Ristretto and SecP, I don't think they are any compatible because they are different groups, right? ... So I don't see how you could do a single DKG for both. But if you talk about Ristretto and 25519, they're kind of related, so maybe there's some way to make it work, but I would need to also think about it."
{: .notice--info}

### Trusted Channels

_Privacy:_ "If your system  wants to have secure channels for privacy reasons ... then I guess you would use them. You would run DKG over the secure channel. You would also run signing over the secure channel."
{: .notice--info}

### Performance

_Signing:_ "You can use a low power device to generate the shares. And then anyone on any computer anywhere can combine them into the final signature."
{: .notice--info}

### Wild Card Uses of FROST

_Nesting:_ "There's an interesting cryptographic piece of this that I don't think has been fully explored, which is the notion of nesting FROST. So for example, if you have a MuSig construction, like in Lightning with a two of two MuSig for lightning channel ... to what extent can you nest FROST inside of one or two of the MuSig keys?
{: .notice--info}

### ROAST

_About ROAST:_ "It's supposed to provide robustness so you can guarantee to get a signature and of course you if you implement it wrongly, then it won't work and you won't get a signature in the end."
{: .notice--info}

### FROST Quorums

_Creating Accountability:_ "The easiest thing to do would be you publish your FROST signature and then you have some ledger or something where you persist information. ... You publish that and then you could either persist the T individual signature shares or we have this nice multi signature scheme called MuSig."
{: .notice--info}

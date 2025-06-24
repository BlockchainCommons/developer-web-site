---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Seeds: Gordian Envelope Support"
hide_description: true
classes:
  - wide
permalink: /ckm/
sidebar:
  nav: ckm
redirect_from:
  - /ckm/overview/
---

## Overview

Cryptographic seeds are the heart of crypto asset control. [#SmartCustody](https://www.smartcustody.com/), one of Blockchain Commons' earliest initiatives, was all about keeping them safe. That's continued forward, with resilience being a core [Gordian principles](https://developer.blockchaincommons.com/principles/). We believe that loss of a seed or private key is one of the most likely ways for the average user to lose a digital asset; Blockchain Commons is working to help developers and users to avoid that.

One of the major ways to keep a seed safe is encode it in a Gordian Envelope. Not only is it a well-known, well-specified format that should be readable into the far future, but it also allows for encryption, sharding, multiple permits, and in the future storage with [GSTP](/envelope/gstp/) and CSR [/csr/].

The following examples demonstrate how many of these techniques work using the [Rust envelope-cli](https://github.com/BlockchainCommons/bc-envelope-cli-rust). The [bytewords-cli](https://github.com/BlockchainCommons/bytewords-cli) and [cbor2diag](https://github.com/cabo/cbor-diag) are also used for a few minor examples, but they're not necessary to fully understand this tutorial (so if you don't have them, no problem). 

⚠️ **Warning:** ⚠️ Do not work with real assets using envelope-cli. Because it's a command line, it's probably not secure enough for most digital assets; but, as a reference app, envelope-cli shows what envelopes can do for seeds and how they work. It can also be used to generate sample envelopes for testing elsewhere.

## Generating Seeds

Seeds and their associated private keys and public keys can all be generated using `seedtool-cli`, but this capability should be used solely for testing purposes. 

(You'll ideally want a hardened offline wallet for generating your real seeds.)

```
SEED=$(envelope generate seed)
echo $SEED
ur:seed/oyadgdcmynztcnronejkqzadamkbaesroytsgtgmbsdrvs

PRVKEYS=$(envelope generate prvkeys --seed $SEED)
echo $PRVKEYS
ur:crypto-prvkey-base/gdtburldbkjpjeclprcnwpfnsrcakkgdwmnbjzdeae

PUBKEYS=$(envelope generate pubkeys $PRVKEYS)
echo $PUBKEYS
ur:crypto-pubkeys/lftanshfhdcxldrlemtomeiarlnsfsdloybgzoeeecbyzctpjlmslenyuochehgufetkwtemrhfttansgrhdcxjzytdndyuodaamhfaeaywmhdlbrdnnpsmwsteozmttuytbgyrpfzgltptboedpdtdtzomhbn
```

## Examining Seeds

Seeds are generated as `ur:seed`s, in accordance with the [crypto-seed CDDL](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md#cryptographic-seed-seed). If you want to examine a seed more closely, you can do so by stripping off the `ur:seed` prefix. What's left is the CBOR of the seed, prepared per the CDDL.
```
SEED_CBOR=$(bytewords -i minimal -o hex `echo $SEED | awk -F"/" '{print $2}'`)
echo $SEED_CBOR
a10150d6df890a726b21b223ec3cc31d7950eb
```
In [CBOR](https://cbor.me/), that's:
* a map [`a1`]
* Whose first entry [`01`]
* Is 16 bytes [`50`]
* Which is the seed `d6df890a726b21b223ec3cc31d7950eb`

The cbor2diag utility will do that breakdown for you, which is why it's a convenient tool:
```
cbor2diag -x $SEED_CBOR
{1: h'd6df890a726b21b223ec3cc31d7950eb'}
```
Per the CDDL, there could have been an optional creation date, name, or note, as map entries 2, 3, or 4, respectively, but there aren't in this simple example.

## Putting a Seed in an Envelope

The envelope-cli allows a seed to easily be placed in an envelope: you just define the seed you've generated as the subject of an envelope, using type `ur`:
```
SEED_E=$(envelope subject type ur $SEED)
echo $SEED_E
ur:envelope/tpsotantjzoyadgdtburldbkjpjeclprcnwpfnsrcakkgdwmprkgvlzc
```
### Examing a Seed Envelope

The envelope-cli will directly output the CBOR of an envelope for you:

```
% envelope format --type cbor $SEED_E 
d8c8d8c9d99d6ca10150d6df890a726b21b223ec3cc31d7950eb
```

That can also be viewed with cbor2diag or with [cbor.me](https://cbor.me/):
```
SEED_E_CBOR=$(envelope format --type cbor $SEED_E)
cbor2diag -x $SEED_E_CBOR
200(201(40300({1: h'd6df890a726b21b223ec3cc31d7950eb'})))
```

That's now:
* a envelope (CBOR tag 200)
* With a single leaf (CBOR tag 201)
* That contains a seed (CBOR tag 40300)
* Which is still a map with one entry for key `1` (`{1:`)
* Which is the seed `d6df890a726b21b223ec3cc31d7950eb`

One important difference between the UR Seed and the Envelope Seed is that the UR seed was preceded by `ur:seed`, so it did not need a CBOR tag for the seed. However, once it's an envelope, it now needs the CBOR tag of `40300` to define what that part of the envelope is. A particular element will self-identify with a CBOR tag (like `200` or `40300`) or a UR tag (like `ur:envelope` or `ur:seed`) but not both.

### Modifying a Seed

There are many advantages to having a seed in an envelope, rather than it being a bare seed. One advantage is that you can add metadata as assertions in the envelope (with the actual seed remaining the subject):
```
SEED_E=$(envelope assertion add pred-obj known note string "Zcash seed" $SEED_E)
```

The seed now looks like this:
```
envelope format $SEED_E
Seed [
    'note': "Zcash seed"
]
```

### Encrypting a Seed

You can also encrypt an envelope, or rather you can encrypt the _subject_ of an evelope.

This demonstrates the generation of a 

SKEY=$(envelope generate key)
SEED_E_ENCS=$(envelope encrypt --key $SKEY $SEED_E) 

ENCRYPTED [
    'note': "Zcash seed"
]

 SEED_E_ENCPUB=$(envelope encrypt --recipient $PUBKEYS $SEED_E)

envelope format $SEED_E_ENCPUB
ENCRYPTED [
    'hasRecipient': SealedMessage
    'note': "Zcash seed"
]



envelope encrypt --recipient $PUBKEYS --recipient $PUBKEYS2 --key $SKEY $SEED_E | envelope format
ENCRYPTED [
    'hasRecipient': SealedMessage
    'hasRecipient': SealedMessage
    'note': "Zcash seed"
]

envelope sskr split $SEED_E -g 2-of-3 -r $PUBKEYS

SEED_E_W=$(envelope subject type wrapped $SEED_E)

{
    Seed [
        'note': "Zcash seed"
    ]
}

SEED_E_W_ENCPUB=$(envelope encrypt --recipient $PUBKEYS $SEED_E_W)

ENCRYPTED [
    'hasRecipient': SealedMessage
]


envelope sskr split $SEED_E -g 2-of-3
ur:envelope/lftansfwlrhddwgofzbsfxrltpdelpuoptsakelsemprfmrtongtrfmyaooxbshphfpaoxldeehltiihcyjsfrjoprrymhlswmjsbegsytwmchfmplltmuwtahtdtkdwgdioplonynrfgywsfhrfkpnlwtbdvssapshddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyamtpsotantkphddainkoaeadaeurskzefrbdfwtkvobeemgacabdwyjzkkjopmgmdivszsjljtwlotknhlnyyncsvapmhseykk 

ur:envelope/lftansfwlrhddwgofzbsfxrltpdelpuoptsakelsemprfmrtongtrfmyaooxbshphfpaoxldeehltiihcyjsfrjoprrymhlswmjsbegsytwmchfmplltmuwtahtdtkdwgdioplonynrfgywsfhrfkpnlwtbdvssapshddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyamtpsotantkphddainkoaeadadaobarkvdbnlpetwfgtdlkppyiewfcssfnnlrwynyamiyhsgwgomknllpltvwoydtckdtlprp 

ur:envelope/lftansfwlrhddwgofzbsfxrltpdelpuoptsakelsemprfmrtongtrfmyaooxbshphfpaoxldeehltiihcyjsfrjoprrymhlswmjsbegsytwmchfmplltmuwtahtdtkdwgdioplonynrfgywsfhrfkpnlwtbdvssapshddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyamtpsotantkphddainkoaeadaokbfdjymkahtsftrtpkatehimtltylrayrlzmehfgdltajkdwletlosynnbtijsiartzemkhh

ENCRYPTED [
    'sskrShare': SSKRShare
]

envelope sskr split $SEED_E -g 2-of-3 -r $PUBKEYS

ur:envelope/lstansfwlrhddwecheengronhhisesvshlrpzednprmkoehtrddrfgrsfgfgehdamefyjymehdrhhggrdwgtkkcabwfdzojeyahglugstllsykfncsjtwytpdtsbbdmhgdkimncafxpllgbkgojtmnvdtpdttdwtzehddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyahtpsotansgulftansfwlshddasfvymnrlwncyueidmekiqdvwgtclledtnsmhcehhvwfyaologlfwhsvwlfvllevtpmiegduocngsuesspeolwefeamrlytaeptlogdeyisrdtkpsksjzbgcsgesoksmwhldygrtansgrhdcxstfzqzfhyadmlbimnnfpcxwtflmkrokkhedyfhhgsnlbtyaxlawsvezslnhhykgwoyamtpsotantkphddalrjzaeadaelaamcpktaogstpjnjkmyuooyhhsgospmonfyldrdjywpjzlgnbdiasrffgmoahosaxaxgwpf 

ur:envelope/lstansfwlrhddwecheengronhhisesvshlrpzednprmkoehtrddrfgrsfgfgehdamefyjymehdrhhggrdwgtkkcabwfdzojeyahglugstllsykfncsjtwytpdtsbbdmhgdkimncafxpllgbkgojtmnvdtpdttdwtzehddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyahtpsotansgulftansfwlshddabsttwzgsgltbntwzgolnskidpakortrfltcldmdakedkntjpiypklggshseecafrutmwflwzhtgsbepfpenbrlwmhkqdluatgdlkgdadbwayfysedmtsdrweqdoxuttlgwtemktansgrhdcxtbnnssmenlsadihfurpdrogrdszemuprlbtkvanykibkswcwdibnrknegrtpuobwoyamtpsotantkphddalrjzaeadadcxtavtchadnbttehlnftkbntgrjnclnsynlovltlgrolcevddsgevezcsbmufyecadlpqzdl 

ur:envelope/lstansfwlrhddwecheengronhhisesvshlrpzednprmkoehtrddrfgrsfgfgehdamefyjymehdrhhggrdwgtkkcabwfdzojeyahglugstllsykfncsjtwytpdtsbbdmhgdkimncafxpllgbkgojtmnvdtpdttdwtzehddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyamtpsotantkphddalrjzaeadaouyotryrlaamysgtllfzelstajpnepftkaxsthliebkkslkhkrlzcspfmflmhltmkoyahtpsotansgulftansfwlshddafgdyenwzcajpfyvwsnditdwsoxmelebatiftfpghssrnqdhsuylfzcbbldkgvaswetttmwjkcsgsbzaozcstvtcfkpsrkimottgugdiabbfmihpkfgeelbvyykueuebeftutbbtansgrhdcxhffrjndwfyfywmihtprlhhpectsbzmsanblfsflusefplegllowpnbsoosgsdscxknwfmnbw

ENCRYPTED [
    'hasRecipient': SealedMessage
    'sskrShare': SSKRShare
]

envelope sskr join
ur:envelope/lstansfwlrhddwecheengronhhisesvshlrpzednprmkoehtrddrfgrsfgfgehdamefyjymehdrhhggrdwgtkkcabwfdzojeyahglugstllsykfncsjtwytpdtsbbdmhgdkimncafxpllgbkgojtmnvdtpdttdwtzehddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyahtpsotansgulftansfwlshddasfvymnrlwncyueidmekiqdvwgtclledtnsmhcehhvwfyaologlfwhsvwlfvllevtpmiegduocngsuesspeolwefeamrlytaeptlogdeyisrdtkpsksjzbgcsgesoksmwhldygrtansgrhdcxstfzqzfhyadmlbimnnfpcxwtflmkrokkhedyfhhgsnlbtyaxlawsvezslnhhykgwoyamtpsotantkphddalrjzaeadaelaamcpktaogstpjnjkmyuooyhhsgospmonfyldrdjywpjzlgnbdiasrffgmoahosaxaxgwpf 
ur:envelope/lstansfwlrhddwecheengronhhisesvshlrpzednprmkoehtrddrfgrsfgfgehdamefyjymehdrhhggrdwgtkkcabwfdzojeyahglugstllsykfncsjtwytpdtsbbdmhgdkimncafxpllgbkgojtmnvdtpdttdwtzehddatansfphdcxkpltendpolzcrtdickwmtpplmybbsnwefhinmntabzotlodirddkjlzsmdftztgaoyahtpsotansgulftansfwlshddabsttwzgsgltbntwzgolnskidpakortrfltcldmdakedkntjpiypklggshseecafrutmwflwzhtgsbepfpenbrlwmhkqdluatgdlkgdadbwayfysedmtsdrweqdoxuttlgwtemktansgrhdcxtbnnssmenlsadihfurpdrogrdszemuprlbtkvanykibkswcwdibnrknegrtpuobwoyamtpsotantkphddalrjzaeadadcxtavtchadnbttehlnftkbntgrjnclnsynlovltlgrolcevddsgevezcsbmufyecadlpqzdl 
ur:envelope/lftpsotantjzoyadgdtburldbkjpjeclprcnwpfnsrcakkgdwmoyaatpsoimhtiahsjkiscxjkihihieguaturnb


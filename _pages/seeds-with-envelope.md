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

Cryptographic seeds are the heart of crypto asset control. [#SmartCustody](https://www.smartcustody.com/), one of Blockchain Commons' earliest initiatives, is all about keeping them safe. That's continued forward with resilience being a core [Gordian principles](https://developer.blockchaincommons.com/principles/). We're aware that loss of a seed or private key can be one of the most likely ways to lose a digital asset; Blockchain Commons is working to help developers and users to avoid that.

One of the major ways to keep a seed safe is encode it in a Gordian Envelope. Not only is it a well-known, well-specified format that should be readable into the far future, but it also allows for encryption, sharding, multiple permits, and in the future storage with [GSTP](/envelope/gstp/) and CSR [/csr/].

The following examples demonstrate how many of these techniques work using the [Rust envelope-cli](https://github.com/BlockchainCommons/bc-envelope-cli-rust). The [bytewords-cli](https://github.com/BlockchainCommons/bytewords-cli) and [cbor2diag](https://github.com/cabo/cbor-diag) are also used for a few minor examples, but not necessary to fully understand this tutorial. You don't necessarily want to engage in this digital-asset work with [envelope-cli], as a command line is not secure enough for most digital assets; but, as a reference app, [envelope-cli] shows what Envelopes can do for seeds, and how they work, and can also be used to generate sample envelopes for testing elsewhere.

## Generating Seeds

Seeds and their associated private keys and public keys can all be generated using `seedtool-cli`, but this capability should be used solely for testing purposes. You'll ideally want a hardened offline wallet for generating your real seeds.

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

Seeds are generated as `ur:seed`s, in accordance with the [crypto-seed CDDL](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md#cryptographic-seed-seed). 

[TBD: this is process]

bytewords -i minimal -o hex `echo $SEED | awk -F"/" '{print $2}'`
a10150d6df890a726b21b223ec3cc31d7950eb

SEED_CBOR=$(bytewords -i minimal -o hex `echo $SEED | awk -F"/" '{print $2}'`)

envelope subject type ur $SEED
ur:envelope/tpsotantjzoyadgdtburldbkjpjeclprcnwpfnsrcakkgdwmprkgvlzc
SEED_E=$(envelope subject type ur $SEED)

cbor2diag -x $SEED_CBOR
{1: h'd6df890a726b21b223ec3cc31d7950eb'}

% envelope format --type cbor $SEED_E 
d8c8d8c9d99d6ca10150d6df890a726b21b223ec3cc31d7950eb

SEED_E_CBOR=$(envelope format --type cbor $SEED_E)

cbor2diag -x $SEED_E_CBOR
200(201(40300({1: h'd6df890a726b21b223ec3cc31d7950eb'})))

SEED_E=$(envelope assertion add pred-obj known note string "Zcash seed" $SEED_E)

envelope format $SEED_E
Seed [
    'note': "Zcash seed"
]

 SEED_E_ENCPUB=$(envelope encrypt --recipient $PUBKEYS $SEED_E)

envelope format $SEED_E_ENCPUB
ENCRYPTED [
    'hasRecipient': SealedMessage
    'note': "Zcash seed"
]

SKEY=$(envelope generate key)
SEED_E_ENCS=$(envelope encrypt --key $SKEY $SEED_E) 

ENCRYPTED [
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

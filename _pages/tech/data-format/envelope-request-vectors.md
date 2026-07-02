---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "Envelope Request & Response Test Vectors"
hide_description: true
classes:
  - wide
permalink: /envelope/request/vectors/
sidebar:
  nav: envelopereq
---

## Base Seed

{% include seed-128.md %}

## Seed Request

_Request for a specific seed._

**QR:**

![](/assets/images/envelope/request-seed.png)

**UR:**
```
ur:envelope/lstpcstansfytansgshdcxkgeorojthnglfnrewsntcmkptlnsjorpwdkictidhlamdmgtbbssehbsdmhsjztaoyaatpcsksmtfpjzinhsjkcxjskphsjncxkpjzjzhsjncxjskpincxjpihjojpihisihjtieihjpinjycxhsiecxjskpinidkpjkiehsjncxinjtcxisiniacxjliaiahsihiahsjyincxhskpjycxkpjycxkojljzkpjojyhsjkcxieiniajyhscxihjzinioihjtieincxjtjlidinjkdmcxgtjljzihjkjyinhsihcxjtihjskpihcxkojljzkpjojyhsjyinidkpjkcxihjycxiejljzjljpcxjskpincxjskphsjkfhoycsielftpcstansfgcsieoytpcstansflcssptpcstansfphdcxzmoycylumhmdgwspnyvadaktnsoycwmyaodihgftdllugltphlmtutytadosdwwdieferftd
```
**Envelope:**
```
request(ARID(7b33b86e)) [
    'body': «getSeed» [
        ❰seedDigest❱: Digest(ffa11a8b)
    ]
    'note': "Alias quam ullam qui reprehenderit ad quibusdam in hic occaecati aut ut voluptas dicta eligendi nobis. Molestiae neque voluptatibus et dolor qui quas?"
]
```
**CBOR:**
```
[24(40004(40012(h'7b33b86e604e3cb5ef9d1675d59c70b6ea7d1f625d062e4d14c4310f2e616cd9'))),
 {4: 24("Alias quam ullam qui reprehenderit ad quibusdam in hic occaecati aut ut voluptas dicta eligendi nobis. Molestiae neque voluptatibus et dolor qui quas?")},
 {100: [24(40006(100)),
        {24(40007(200)): 24(40001(h'ffa11a8b90954fc89ae625779ca11b8f0227573a2f8b4ed85d96ddf901a72cea'))}]}]
```

### Seed Response

**QR:**

![](/assets/images/envelope/response-seed.png)

**UR:**
```
ur:envelope/lftpcstansfetansgshdcxkgeorojthnglfnrewsntcmkptlnsjorpwdkictidhlamdmgtbbssehbsdmhsjztaoycsihlrtpcsgdhkwzdtfthptokigtvwnnjsqzcxknsktdoyadcsspoycfadzttpcstantjyoeadisktjojeisdefzdydtaolytantjloxaxhdclaxvlcprfttldjobkredtlnhsidwybaeyjtswyandlgjnehtkdsidbkqzsrkphyfhsaaahdcxhnfgnepefxgdytryckticelyotsstoknfntavevaskiddmolsarntykbrybtjpksamtantjooeadlncsghykaeykaeykaocyhngrmuwzaycyzssajpsnoybdtpcskpfyhsjpjecxgdkpjpjojzihcxgdihiajecxhfinhsjzfzrdrsko
```

**Envelope:**
```
response(ARID(7b33b86e)) [
    'result': Bytes(16) [
        'isA': 'Seed'
        'hasName': "Dark Purple Peck Vial"
        'outputDescriptor': output-descriptor(Map)
    ]
]
```

**CBOR:**
```
[24(40005(40012(h'7b33b86e604e3cb5ef9d1675d59c70b6ea7d1f625d062e4d14c4310f2e616cd9'))),
 {101: [24(h'59f2293a5bce7d4de59e71b4207ac5d2'),
        {1: 200},
        {508: 24(40308({1: "wpkh(@0)",
                        2: [40303({3: h'03e322bcd189700ab529866162ee0e326ec6f89b8d6d31cf26620ab4c3755e3fc2',                            
                                   4: h'60469faf4350f9bd1ed01c81a3c4ce7a3cd9e4e6c5622ea6c2bed47ebd0d7278',
                                   6: 40304({1: [84, true, 0, true, 0, true],
                                             2: 1615565810}),
                                   8: 4207047373})]}))},
        {11: 24("Dark Purple Peck Vial")}]}]
```

## Key Request

_Request for a specific Cosigner Private Key._

**QR:**

![](/assets/images/envelope/request-key.png)

**UR:**
```
ur:envelope/lstpcstansfytansgshdcxtsmhasueckldzmlnnncagasbtyaxaxteolcekkrkderolufstabsbwskayayktpfoycsielftpcstansfgcsihoytpcstansflcssotpcstantjootadlocsdyykaeykaeykaoykaocyhngrmuwzaxaaoyaatpcsksjkgykpjliecxhsjkjoihjpjthsjykpjpcxjpihjokpieinhsjtiehsihcxhsjtinjnincxihjkjycxjtjljkjyjpkpjnfhcxgykpincxinjzjzjlcxjtjljtcxidjzhsjtieinjyininjkcxkojljzkpjojyhsjkcxjpihjpkpjncxhsjzinjskpiniecxjskpinhscxihjycxiekpiainjnkpjkcxjnjlieindmjouozskn
```

**Envelope:**
```
request(ARID(d79009de)) [
    'body': «getKey» [
        ❰derivationPath❱: keypath(Map)
    ]
    'note': "Quod aspernatur repudiandae animi est nostrum? Qui illo non blanditiis voluptas rerum aliquid quia et ducimus modi."
]
```

**CBOR:**
```
[24(40004(40012(h'd79009de1e89ff869e1d49cbd40303d3a61c79bb28b88b3dd90f13c5080877b0'))),
 {100: [24(40006(101)),
        {24(40007(201)): 24(40304({1: [48, true, 0, true, 0, true, 2, true],
                                   2: 1615565810,
                                   3: 4}))}]},
 {4: 24("Quod aspernatur repudiandae animi est nostrum? Qui illo non blanditiis voluptas rerum aliquid quia et ducimus modi.")}]

```

### Key Response

**QR:**

![](/assets/images/envelope/response-key.png)

**UR:**
```
ur:envelope/lftpcstansfetansgshdcxtsmhasueckldzmlnnncagasbtyaxaxteolcekkrkderolufstabsbwskayayktpfoycsihlotpcshdclaevytktyhkfthkglbyzehflpenbsfxbkvlvyghtsondrzeskswvoclemaswtzodlhnoycfaddwlfcfaddpoycfadmhcfadmeoycfadyktpcshdcxlejtimcnrlbtdemdoereyaqzprkpndbdgwfzflqdbzkohgzobycxcnvabaosbglfoycfadyttpcscynewncnlboyadcfadwkoycfadyltpcstantjootadlocsdyykaeykaeykaoykaocyhngrmuwzaxaaoybdtpcskscxfdfygrihkkcxiyjpjljncxfyhsjpjecxgdkpjpjojzihcxgdihiajecxhfinhsjzoyadcssorhwsbbse
```

**Envelope:**
```
response(ARID(d79009de)) [
    'result': Bytes(33) [
        'isA': 'BIP32Key'
        'isA': 'PrivateKey'
        'asset': 'BTC' [
            'network': 'MainNet'
        ]
        'chainCode': Bytes(32)
        'hasName': "HDKey from Dark Purple Peck Vial"
        'parent': keypath(Map)
        'parentFingerprint': 2683380607
    ]
]
```
**CBOR:**
```
[24(40005(40012(h'd79009de1e89ff869e1d49cbd40303d3a61c79bb28b88b3dd90f13c5080877b0'))),
 {101: [24(h'00e1cfd4593a594e11fe5685360f430ae3e154d7a52afec5c6e2213709f0fb2f60'),
        {300: [301,
               {400: 401}]},
        {501: 24(h'8a6e6a23b70d2895a2b5f8b4b2759b0b4f4047b3157657fb112023e60ea71282')},
        {505: 24(2683380607)},
        {1: 500},
        {503: 24(40304({1: [48, true, 0, true, 0, true, 2, true],
                        2: 1615565810,
                        3: 4}))},
        {11: 24("HDKey from Dark Purple Peck Vial")},
        {1: 201}]}]
```

## Key Derivation Request

_Request for any Cosigner Public Key._

**QR:**

![](/assets/images/envelope/request-path.png)

**UR:**
```
ur:envelope/lstpcstansfytansgshdcxmeleiycsuefgvlctmoprkiaomwwzptflvtotrfrszsayosgrktykzmbayavlmurloycsielstpcstansfgcsihoytpcstansflcssgtpcswkoytpcstansflcssotpcstantjooeadlocsdyykaeykaeykaoykaxaaoyaatpcsksmogthsinjljpihjkcxjkjljzkpjyhscxkoihjzinjycxiykpioinhsjycxihjycxiniecxjkihjskpincxinjojkkpjncxjskpjliecxjtjljkjyjpkpjncxjliaiahsihiahsjyincxihkoihjtinihjydmcxfgkpiohscxiejljzjljpkpjncxiajljtjkihjskphsjykpjpcxhsieinjoinjkiaincxkpjzjzhsjncxjkihjskpincxjnjljzihjkjyinhsjkcxjskpinhscxiykpioinhsjyfhuefrwnqd
```

**Envelope:**

```
request(ARID(918a6618)) [
    'body': «getKey» [
        ❰derivationPath❱: keypath(Map)
        ❰isPrivate❱: false
    ]
    'note': "Maiores soluta velit fugiat et id sequi ipsum quod nostrum occaecati eveniet. Fuga dolorum consequatur adipisci ullam sequi molestias quia fugiat?"
]
```

**CBOR:**
```
[24(40004(40012(h'918a6618de46e31f92b27d0294f2a947e0a3bcbffa08a74b77f5ff0ef8e393b7'))),
 {100: [24(40006(101)),
        {24(40007(202)): 24(false)},
        {24(40007(201)): 24(40304({1: [48, true, 0, true, 0, true, 2, true],
                                   3: 4}))}]},
 {4: 24("Maiores soluta velit fugiat et id sequi ipsum quod nostrum occaecati eveniet. Fuga dolorum consequatur adipisci ullam sequi molestias quia fugiat?")}]

```

### Key Derivation Response

_Response with Default seed selected._

**Animated QR:**

![](/assets/images/envelope/response-path.png)

**UR:**
```
ur:envelope/lftpcstansfetansgshdcxmeleiycsuefgvlctmoprkiaomwwzptflvtotrfrszsayosgrktykzmbayavlmurloycsihlotpcshdclaohldlmdrtlacxhnfpptplfyltwelafsnezslyndhllnvdimmwlpylkbwzjltbdmenoycfaddwlfcfaddpoycfadmhcfadmeoycfadyktpcshdcxlejtimcnrlbtdemdoereyaqzprkpndbdgwfzflqdbzkohgzobycxcnvabaosbglfoycfadyttpcscynewncnlboyadcssgoyadcfadwkoycfadyltpcstantjootadlocsdyykaeykaeykaoykaocyhngrmuwzaxaaoybdtpcskscxfdfygrihkkcxiyjpjljncxfyhsjpjecxgdkpjpjojzihcxgdihiajecxhfinhsjzwzescyhp
```

**Envelope:**

```
response(ARID(918a6618)) [
    'result': Bytes(33) [
        'isA': 'BIP32Key'
        'isA': 'PublicKey'
        'asset': 'BTC' [
            'network': 'MainNet'
        ]
        'chainCode': Bytes(32)
        'hasName': "HDKey from Dark Purple Peck Vial"
        'parent': keypath(Map)
        'parentFingerprint': 2683380607
    ]
]
```
**CBOR:**
```
[24(40005(40012(h'918a6618de46e31f92b27d0294f2a947e0a3bcbffa08a74b77f5ff0ef8e393b7'))),
 {101: [24(h'025d2f95c080206041a9ae4487ed803d9ffa819b5d86e76a9485f77ef26fd62e36'),
 {300: [301,
        {400: 401}]},
 {501: 24(h'8a6e6a23b70d2895a2b5f8b4b2759b0b4f4047b3157657fb112023e60ea71282')},
 {505: 24(2683380607)},
 {1: 202},
 {1: 500},
 {503: 24(40304({1: [48, true, 0, true, 0, true, 2, true],
                 2: 1615565810,
                 3: 4}))},
 {11: 24("HDKey from Dark Purple Peck Vial")}]}]
```

## PSBT Signing Request

_Request for the signature of a PSBT._

**Animated QR:**

![](/assets/images/envelope/request-psbt.png)

**UR:**
```
ur:envelope/lstpcstansfytansgshdcxswjevokirdtssnashhoskoflfzjnatmsjnrtwfhebtgtihgrpfwljntddioywlztoycsielftpcstansfgcsiyoytpcstansflcssnlftpcshkaodnjojkidjyzmadaekpaoaeaeaeaddslyjsemckurwzlpwlempmwyoxqdkgksaebnahiysbqdpmieiechbwsgfwchcwynaeaeaeaeaezezmzmzmaoteurykahaeaeaeaecfkoptbbtisknlaxskrdsalnlthnwlbstlcloxiyhtosihcxlopsaevyykahaeaeaeaechptbbecfevavlfrlsdwflahbsdktewyrhfnnsaxmwlustltqddmbwaeaeadaezconadadaeaeaeaeadaoldotstckpygtcxvtemcwrkoxsfinmyoemdsofgftzsdmeslblpeosfrpdlmdiovwadaeaeaechcmaebbrncsttgmptpfbgaxntpefsosuegwgueennwprhlpzmzmzmzmlnyapkfxoscazmbbfdldftgubkjpemwsjefgayrkprutdpadjsvaftwpimfdmhqzadaeaeaechcmaebbzefmnnwnosfewljytaaossechkfxpysbeeryguguzmzmzmzmaoaesawmbdaeaeaeaecfkoptbblptkwnaslbtavtayrkeepejonsidcfkgetmslefdlopsjpzeyagldwaeaeaechptbbeomsdardclwstbdrstguptrftiiotbstolotntahltaofldyfyaocxdibgrncpvtdibsesgwhflsbyuokeptolldjoroaoheutfrdkaodtwtlbleheftdkaocxadluettsuotebbvdeesodijetbzofzynjkeyhpssrdoyfyspaetdwzwtdpprkohhadclaxtdvyhfjymwcwpmgenliajpsbltvylpjnengmhnjnmkhfdlvlnshynnkbfpfhclahaofddyfeaoclaettdnlpdplpuotahstdykwkpyiyamghurjtwesfkkgsbneotohhsraszmreztvwlgaocxioeolemnbachdasemszocylopehkykckfyvedahpcxcmkelnlraxceahttwzhkdradclaocnrldnwywtmthlbernatkswswptbctsgswylnygloyineseolajkfyieyagwdrqdaeaeaeaeaeaeaeoyadcfadzsoyaatpcsieghihjkjyjemhihhf
```
**Envelope:**
```
request(ARID(c66be27d)) [
    'body': «signPSBT» [
        ❰psbt❱: Bytes(555) [
            'isA': 'PSBT'
        ]
    ]
    'note': "Test"
]
```
**CBOR:**
```
[24(40004(40012(h'c66be27dbad7cd095ca77647406d07976dc0f35f0d4d654bb0e96dd227a1e9fc'))),
 {100: [24(40006(102)),
        {24(40007(205)):[24(h'70736274ff0100750200000001268171371edff285e937adeea4b37b78000c0566cbb3ad64641713ca42171bf60000000000feffffff02d3dff505000000001976a914d0c59903c5bac2868760e90fd521a4665aa7652088ac00e1f5050000000017a9143545e6e33b832c47050f24d3eeb93c9c03948bc787b32e1300000100fda5010100000000010289a3c71eab4d20e0371bbba4cc698fa295c9463afa2e397f8533ccb62f9567e50100000017160014be18d152a9b012039daf3da7de4f53349eecb985ffffffff86f8aa43a71dff1448893a530a7237ef6b4608bbb2dd2d0171e63aec6a4890b40100000017160014fe3e9ef1a745e974d902c4355943abcb34bd5353ffffffff0200c2eb0b000000001976a91485cff1097fd9e008bb34af709c62197b38978a4888ac72fef84e2c00000017a914339725ba21efd62ac753a9bcd067d6c7a6a39d05870247304402202712be22e0270f394f568311dc7ca9a68970b8025fdd3b240229f07f8a5f3a240220018b38d7dcd314e734c9276bd6fb40f673325bc4baa144c800d2f2f02db2765c012103d2e15674941bad4a996372cb87e1856d3652606d98562fe39c5e9e7e413f210502483045022100d12b852d85dcd961d2f5f4ab660654df6eedcc794c0c33ce5cc309ffb5fce58d022067338a8e0e1725c197fb1a88af59f51e44e4255b20167c8684031c05d1f2592a01210223b72beef0965d10be0778efecd61fcac6f79a4ea169393380734464f84f2ab300000000000000'),
        {1: 506}]}]},
 {4: 24("Test")}]
```

## Output Descriptor

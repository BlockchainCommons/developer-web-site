---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: A Guide to Using URs for PSBTs
hide_description: true
classes:
  - wide
permalink: /ur/psbts/
sidebar:
  nav:
    - ur
---

Uniform Resources (URs) allow for the encoding of a variety of
information, including a variety of crypto-data. This document
describes how they are used to encode PSBTs, or Partially Signed
Bitcoin Transactions.

As usual, this will typically be done for you automatically using
Blockchain Commons'
[libraries](/ur/#libraries)
but what follows is an explanation of how everything works.

## Why Use URs for PSBTs?

PSBTs are perhaps the ultimate interoperable data type in Bitcoin
Core. They were explicitly built to allow different devices to create
and/or sign Bitcoin transmissions. As such, they are crucial to
[#SmartCustody](https://www.smartcustody.com/) best practices, which advocate for the storage of seeds
and other vulnerable data on non-networked devices. Using PSBTs,
transactions can be created on networked devices, shipped to
non-networked devices for signing, then shipped back to the networked
devices for transmission.

So why do you need URs rather than PSBT's standard data format? Quite
simply, because PSBTs are too big. If you're just sending around a
PSBT's mass of data as base64, no problem. But when transmitting to
non-networked devices, for maximum resiliency of your seeds and keys,
you probably need to use QRs, and even if you can jam a PSBT into a
QR (unlikely!), it may not be readable by a computer camera.

This is especially true when PSBTs are used for multisigs, and
multisigs are a prime use case for PSBTs: that's because PSBTs allow a
transaction to be easily passed around until it's signed by a
threshold of people required to authenticate a UTXO. However, as
signatures accrue, a PSBT gets ever bigger, to the point where it will likely
be too large for a QR code entirely.

URs resolve this by using fountain codes to enable [Animated
QRs](/animated-qrs/). A single UR is turned into a sequence or URs,
each preceded by its type and by its place in a sequence. Each
individual frame is not just small enough to fit into a QR, but small
enough to be read by a lower-resolution camera. (Best practice is to
allow a user to dynamically downsize an animated QR if they're having
troubles transmitting it.) The reading device then reads the animation
until it's put together enough frames to reconstruct the original
data.

The proof is in the pudding. `ur:psbt` has been Blockchain
Commons' most widely used UR specification with [about a dozen
wallets](/ur/implementations/) already incorporating it.

### Should I Use UR PSBT or Envelope?

Though we are generally suggesting the use of [Gordian Envelope](/envelope/) rather than the bare UR format, UR PSBT might be a use case where the continued use of bare URs makes sense, as it's focused on the live transmission of data, where longer term metadata is not required, and becaause that data exchange often happens with constrained devices. 

However, please also consider whether metadata might be a useful additional to your PSBT packages:

* Would you like to include notes about *when* to sign the PSBT, which might not be immediately?
* Would you like to include notes about *who* should sign, which might not be all the possible signatories?
* Would you like to include notes about who to return the fully signed transaction to?
* Do you want to package the PSBT for use at some far future time?

If the answer to any of these questions is "yes", then you have a use case for exchanging PSBTs in Envelopes, likely using the [GSTP protocol](https://developer.blockchaincommons.com/envelope/gstp/).  

What follows are some details on deconstructing bare `ur:psbt`s, in case your answer to all the above questions was "no".

## PRBTs: `[ur:psbt]`

`ur:psbt` is the simplest conversion of UR data type.

Take the following PSBT:
```
cHNidP8BAHECAAAAAUKAmWVFG/zLmhHQZ8Q8bQKCoNxxiurIcNZVhafCoLCyAAAAAAD9////AjksAAAAAAAAFgAUvDLbTPtQXj/JTiGj7HTRrvz8ASwiwQAAAAAAABYAFOq0zUDTkmdWzFLl+RydPI47QA4ReCIMAE8BBIiyHgPG2cycgAAAADKRxb1j69WGKYT3nQrjs2zcdlE3UiHFNYyGd3vysjMcAiVPCY671cdIpViKxJr/88kFWlty0jqxfwY8PfU3Tbu3EMh3/URUAACAAAAAgAAAAIAAAQEfN/cAAAAAAAAWABSUBvAKoN3uZMfqYvmU5pVYAoCj5QEDBAEAAAAiBgNa5FjqiQq3akaVNyAsPdho05JgUpccQlhll1wyKD/3FhjId/1EVAAAgAAAAIAAAACAAAAAAAEAAAAAACICAkjtpvsDgJ+za5zGXZjCZhj5fNVw8Uqc+AgQItHhEYiTGMh3/URUAACAAAAAgAAAAIABAAAAAAAAAAA=
```

Which decodes as shown, per `bitcoin-cli decodepsbt`:
```
{
  "tx": {
    "txid": "076f3283f86ddc7777b0dd2553a6631119e22e7eda5e130c60e0906ae9209072",
    "hash": "076f3283f86ddc7777b0dd2553a6631119e22e7eda5e130c60e0906ae9209072",
    "version": 2,
    "size": 113,
    "vsize": 113,
    "weight": 452,
    "locktime": 795256,
    "vin": [
      {
        "txid": "b2b0a0c2a78555d670c8ea8a71dca082026d3cc467d0119acbfc1b4565998042",
        "vout": 0,
        "scriptSig": {
          "asm": "",
          "hex": ""
        },
        "sequence": 4294967293
      }
    ],
    "vout": [
      {
        "value": 0.00011321,
        "n": 0,
        "scriptPubKey": {
          "asm": "0 bc32db4cfb505e3fc94e21a3ec74d1aefcfc012c",
          "desc": "addr(bc1qhsedkn8m2p0rlj2wyx37cax34m70cqfvx5jeg8)#09kmq8a0",
          "hex": "0014bc32db4cfb505e3fc94e21a3ec74d1aefcfc012c",
          "address": "bc1qhsedkn8m2p0rlj2wyx37cax34m70cqfvx5jeg8",
          "type": "witness_v0_keyhash"
        }
      },
      {
        "value": 0.00049442,
        "n": 1,
        "scriptPubKey": {
          "asm": "0 eab4cd40d3926756cc52e5f91c9d3c8e3b400e11",
          "desc": "addr(bc1qa26v6sxnjfn4dnzjuhu3e8fu3ca5qrs3p7a8hh)#edm3lcy2",
          "hex": "0014eab4cd40d3926756cc52e5f91c9d3c8e3b400e11",
          "address": "bc1qa26v6sxnjfn4dnzjuhu3e8fu3ca5qrs3p7a8hh",
          "type": "witness_v0_keyhash"
        }
      }
    ]
  },
  "global_xpubs": [
    {
      "xpub": "xpub6D7YLKByaDi6h8vP6rQRN8sSn87DzYfP82dYcSaY52KcLDWBVoexTfZa2kbR2RxFHyJvf9aykd9fD6aX3NcfQE55Bf6WcjjLxs2X4EMqS11",
      "master_fingerprint": "c877fd44",
      "path": "m/84'/0'/0'"
    }
  ],
  "psbt_version": 0,
  "proprietary": [
  ],
  "unknown": {
  },
  "inputs": [
    {
      "witness_utxo": {
        "amount": 0.00063287,
        "scriptPubKey": {
          "asm": "0 9406f00aa0ddee64c7ea62f994e695580280a3e5",
          "desc": "addr(bc1qjsr0qz4qmhhxf3l2vtuefe54tqpgpgl988w202)#j2mz0t4w",
          "hex": "00149406f00aa0ddee64c7ea62f994e695580280a3e5",
          "address": "bc1qjsr0qz4qmhhxf3l2vtuefe54tqpgpgl988w202",
          "type": "witness_v0_keyhash"
        }
      },
      "sighash": "ALL",
      "bip32_derivs": [
        {
          "pubkey": "035ae458ea890ab76a469537202c3dd868d3926052971c425865975c32283ff716",
          "master_fingerprint": "c877fd44",
          "path": "m/84'/0'/0'/0/1"
        }
      ]
    }
  ],
  "outputs": [
    {
    },
    {
      "bip32_derivs": [
        {
          "pubkey": "0248eda6fb03809fb36b9cc65d98c26618f97cd570f14a9cf8081022d1e1118893",
          "master_fingerprint": "c877fd44",
          "path": "m/84'/0'/0'/1/0"
        }
      ]
    }
  ],
  "fee": 0.00002524
}
```
This is a simple PSBT with a UTXO requiring a single signature.

To turn the PSBT into a UR only requires wrapping the PSBT. No effort is made to rewrite the PSBT, which has its own format.

The first step is to convert the base64 of the PSBT into hex code:
```
70736274ff010071020000000142809965451bfccb9a11d067c43c6d0282a0dc718aeac870d65585a7c2a0b0b20000000000fdffffff02392c000000000000160014bc32db4cfb505e3fc94e21a3ec74d1aefcfc012c22c1000000000000160014eab4cd40d3926756cc52e5f91c9d3c8e3b400e1178220c004f010488b21e03c6d9cc9c800000003291c5bd63ebd5862984f79d0ae3b36cdc7651375221c5358c86777bf2b2331c02254f098ebbd5c748a5588ac49afff3c9055a5b72d23ab17f063c3df5374dbbb710c877fd445400008000000080000000800001011f37f70000000000001600149406f00aa0ddee64c7ea62f994e695580280a3e5010304010000002206035ae458ea890ab76a469537202c3dd868d3926052971c425865975c32283ff71618c877fd445400008000000080000000800000000001000000000022020248eda6fb03809fb36b9cc65d98c26618f97cd570f14a9cf8081022d1e111889318c877fd44540000800000008000000080010000000000000000
```
The hex then needs to be recorded as [a byte string in CBOR](https://datatracker.ietf.org/doc/html/rfc7049#section-2.1). This is done with a short prefix.
```
59017f70736274ff010071020000000142809965451bfccb9a11d067c43c6d0282a0dc718aeac870d65585a7c2a0b0b20000000000fdffffff02392c000000000000160014bc32db4cfb505e3fc94e21a3ec74d1aefcfc012c22c1000000000000160014eab4cd40d3926756cc52e5f91c9d3c8e3b400e1178220c004f010488b21e03c6d9cc9c800000003291c5bd63ebd5862984f79d0ae3b36cdc7651375221c5358c86777bf2b2331c02254f098ebbd5c748a5588ac49afff3c9055a5b72d23ab17f063c3df5374dbbb710c877fd445400008000000080000000800001011f37f70000000000001600149406f00aa0ddee64c7ea62f994e695580280a3e5010304010000002206035ae458ea890ab76a469537202c3dd868d3926052971c425865975c32283ff71618c877fd445400008000000080000000800000000001000000000022020248eda6fb03809fb36b9cc65d98c26618f97cd570f14a9cf8081022d1e111889318c877fd44540000800000008000000080010000000000000000
```
Note that the "59017f" is all that's added here. As described in the CBOR spec, the "59" is a bit string: "0b01011001". CBOR breaks that down into three bits to describe the CBOR major type and five bits to describe the length of the content: "0b010_11001". The "010" is type 2 (byte string) and the "11001" is size "25" (in decimal). According to the CBOR spec that means the actual length is stored in a `uint16_t`, which is the next two bytes. The two bytes that follow, "017F", then describe the actual length of the hex: it converts to 383 (bytes).

Looking at this in [cbor.me](https://cbor.me/), you can see it breaks down to:
```
59 017F                                 # bytes(383)
   70736274FF010071020000000142809965451BFCCB9A11D067C43C6D0282A0DC718AEAC870D65585A7C2A0B0B20000000000FDFFFFFF02392C000000000000160014BC32DB4CFB505E3FC94E21A3EC74D1AEFCFC012C22C1000000000000160014EAB4CD40D3926756CC52E5F91C9D3C8E3B400E1178220C004F010488B21E03C6D9CC9C800000003291C5BD63EBD5862984F79D0AE3B36CDC7651375221C5358C86777BF2B2331C02254F098EBBD5C748A5588AC49AFFF3C9055A5B72D23AB17F063C3DF5374DBBB710C877FD445400008000000080000000800001011F37F70000000000001600149406F00AA0DDEE64C7EA62F994E695580280A3E5010304010000002206035AE458EA890AB76A469537202C3DD868D3926052971C425865975C32283FF71618C877FD445400008000000080000000800000000001000000000022020248EDA6FB03809FB36B9CC65D98C26618F97CD570F14A9CF8081022D1E111889318C877FD44540000800000008000000080010000000000000000 # 
```
That hex, with its prepended type & size, then needs to be converted into minimal [Bytewords](/bytewords/) and prepended again, this time with the `ur:psbt` tag so that it's self-describing:
```
ur:psbt/hkadlbjojkidjyzmadaejsaoaeaeaeadfwlanlihfecwztsbnybytiiossfnjnaolfnbuojslewdspjotbgolpossanbpfpraeaeaeaeaezczmzmzmaoesdwaeaeaeaeaeaecmaebbrfeyuygszogdhyfhsoglclotwpjyttplztztaddwcpseaeaeaeaeaeaecmaebbwdqzsnfztemoiohfsfgmvwytcentfnmnfrfzbabykscpbnaegwadaaloprckaxswtasfnslaaeaeaeeymeskryiawmtllndtlrylntbkvlqdjzuokogyemgmclskeclklnktkgwzpreoceaodagwasmnrktlstfdonhdlessnyzmwfsoahhthpjptdftpalbamfnfsykemgtrkrlbespktzcfyghaeaelaaeaeaelaaeaeaelaaeadadctemylaeaeaeaeaeaecmaebbmwamwtbknbutwyiestwdidytmwvamdhdaolaotvwadaxaaadaeaeaecpamaxhtvehdwdldbkrlimfgmdemcxdwfstpistemohngmmscefwhdihmshheydefhylcmcsspktzcfyghaeaelaaeaeaelaaeaeaelaaeaeaeaeadaeaeaeaeaecpaoaofdweolzoaxlaneqdjensswhlmksaiycsytketljowngensyaaybecpttvybylomucsspktzcfyghaeaelaaeaeaelaaeaeaelaadaeaeaeaeaeaeaeaecfktctrf
```

Voila! You have a PSBT in UR form. Sequencing that, for usage in an
animated QR, would take some additional work, which is best done with
the [UR
libraries](/ur/#libraries) and/or the [MUR guide](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2024-001-multipart-ur.md).

## Integrating PSBT URs Into Your Code

You can incorporate URs into your own code using the UR library of your choice:

{% include lib-ur.md %}

## Conclusion

PSBTs are perhaps the easiest data type to convert into URs. They also
offer one of the strongest use cases for bare URs.

1. The animated QRs that are easy to create using UR sequences are
very important for PSBTs because of their potential for large sizes,
particularly with multisig PSBTs.
2. The fact that PSBTs are very likely to be passed from device to
device makes their interoperability that much more important, and URs
(especially when integrated with QRs) offer a way to do so that's
accessible and easy to use for an average user, while its
self-describing nature also ensures that devices know what to do with
the data!

However, if you have any use case for incorporating additional data into your PSBT, such as descriptions or instructions, you should use [Gordian Envelope](/envelope/) instead. Since Gordian Envelopes encode as URs, they still have also these advantages, but also the additional benefits of being able to package the PSBT with more content and being able to use Envelope Extensions such as [GSTP](/envelope/gstp/), which allows for secure transmission of your PSBT by means other than QRs.

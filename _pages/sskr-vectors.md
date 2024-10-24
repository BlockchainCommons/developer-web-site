---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "SSKR Test Vectors"
hide_description: true
classes:
  - wide
permalink: /sskr/vectors/
sidebar:
  nav:
    - sskr
    - seedlayer
---

The following examples demonstrate SSKR test vectors for Blockchain
Commons' standard seeds.

> :warning: **WARNING.** SSKR is non-deterministic. There is a random
factor introduced when the shares are created, which means that
every time you generate shares, they will be different. This is an
expected and correct result.

> For the purposes of these test vectors, this means that if generate
SSKR shares from these seeds, they will be different. However, you can
still test your implementation by reading these shares back in to make
sure they generated the expected seeds.

## 128-bit Seed

{% include seed-128.md %}

#### Two-of-Three SSKR

A traditional use of Shamir's Secret Sharing shards a secret into
three shares, and then allows the secret to be reconstructed from two
of those shares.

The following set of 2-of-3 shares was generated by Gordian SeedTool and exported as ByteWords:
```
tuna acid epic gyro jury jump able acid able flap rust king 
roof item vast real hang plus free menu good plus keno vast 
gray maze oval whiz tomb

tuna acid epic gyro jury jump able acid acid kiln main tuna 
keno able view plus gush body nail silk girl ruin visa curl 
yell loud rock jury fuel

tuna acid epic gyro jury jump able acid also exam high dark 
echo ruin wand lazy gray swan visa fish jazz logo gift kick
atom wall yank atom many
```
The following shows that same 2-of-3 share exported as a [Uniform Resource (UR)](/ur/):
```
ur:crypto-sskr/gojyjpaeadaefprtkgrfimvtrlhgpsfemugdpskovtgybagtiomh
ur:crypto-sskr/gojyjpaeadadknmntakoaevwpsghbynlskglrnvaclylcmgdvyad
ur:crypto-sskr/gojyjpaeadaoemhhdkeornwdlygysnvafhjzlogtkkamkockmuso
```

A UR uses the minimal [ByteWords](/bytewords/) form, meaning that it
shows only the first and last letter of each word. This means that
"go" is "gyro", "jy" is "jury", "jp" is "jump", "ae" is "able",
etc. You can see that the "UR" form of the three shares thus precisely
matches the ByteWords except that the first three words ("tuna acid
epic"), identifying it as an SSKR, has been omitted, and this changes
the checksum, which is the last four words.

Finally, here are the shares in scannable QRs:

<figure class="third">
  <img src="/assets/images/sskr/128-sskr-1.jpg">
  <img src="/assets/images/sskr/128-sskr-2.jpg">
  <img src="/assets/images/sskr/128-sskr-3.jpg">
</figure>

#### The Two-of-Three Two-of-Three SSKR

SSKR can support arbitrary groups, where you can then reconstruct a secret by using some number of shares from some number of groups.

One example is a two-of-three two-of-three (grouped four-of-nine) SSKR. Three groups are created, each of which has three shares. Reconstructing the secret requires a threshold of two shares from a threshold of two groups. In other words, the secret can be reconstructed from four of the nine shares, provided that they are two each from two of the groups.

For a more complex SSKR such as this, you might want to use Gordian SeedTool's print function, rather than cutting and pasting SSKRs or URs:

![](/assets/images/sskr/128-sskrgroup-1.png)
![](/assets/images/sskr/128-sskrgroup-2.png)
![](/assets/images/sskr/128-sskrgroup-3.png)

## 256-bit Seed

{% include seed-256.md %}

#### Two-of-Three SSKR

A traditional use of Shamir's Secret Sharing shards a secret into three shares, and then allows the secret to be reconstructed from two of those shares. 

The following set of 2-of-3 shares was generated by Gordian SeedTool and exported as ByteWords:
```
tuna acid epic hard data love wolf able acid able
duty surf belt task judo legs ruby cost belt pose
ruby logo iron vows luck bald user lazy tuna belt
guru buzz limp exam obey kept task cash saga pool
love brag roof owls news junk

tuna acid epic hard data love wolf able acid acid
barn peck luau keys each duty waxy quad open bias
what cusp zaps math kick dark join nail legs oboe
also twin yank road very blue gray saga oboe city
gear beta quad draw knob main

tuna acid epic hard data love wolf able acid also
fund able city road whiz zone claw high frog work
deli slot gush cats kiwi gyro numb puma join fund
when math inch even curl rich vows oval also unit
brew door atom love gyro figs
```

The following shows that same 2-of-3 share exported as a [Uniform Resource (UR)](/ur/):
```
ur:crypto-sskr/hddalewfaeadaedysfbttkjolsryctbtperyloinvslkbdurlytabtgubzlpemoykttkchsapllebgvdgleedp
ur:crypto-sskr/hddalewfaeadadbnpkluksehdywyqdonbswtcpzsmhkkdkjnnllsoeaotnykrdvybegysaoecygrbavssktbti
ur:crypto-sskr/hddalewfaeadaofdaecyrdwzzecwhhfgwkdistghcskigonbpajnfdwnmhihenclrhvsolaoutbwdrhliazcia
```
A UR uses the minimal [ByteWords](/bytewords/) form, meaning that it
shows only the first and last letter of each word. This means that
"hd" is "hard", "da" is "data", "le" is "love", "wf" is "wolf",
etc. You can see that the "UR" form of the three shares thus precisely
matches the ByteWords except that the first three words ("tuna acid
epic"), identifying it as an SSKR, has been omitted, and this changes
the checksum, which is the last four words.

Finally, here are the shares in scannable QRs:

<figure class="third">
  <img src="/assets/images/sskr/256-sskr-1.jpg">
  <img src="/assets/images/sskr/256-sskr-2.jpg">
  <img src="/assets/images/sskr/256-sskr-3.jpg">
</figure>

#### The Two-of-Three Two-of-Three SSKR

SSKR can support arbitrary groups, where you can then reconstruct a
secret by using some number of shares from some number of groups.

One example is a two-of-three two-of-three (grouped four-of-nine)
SSKR. Three groups are created, each of which has three
shares. Reconstructing the secret requires a threshold of two shares
from a threshold of two groups. In other words, the secret can be
reconstructed from four of the nine shares, provided that they are two
each from two of the groups.

For a more complex SSKR such as this, you might want to use Gordian
SeedTool's print function, rather than cutting and pasting SSKRs or
URs:

![](/assets/images/sskr/256-sskrgroup-1.jpeg)
![](/assets/images/sskr/256-sskrgroup-2.jpeg)
![](/assets/images/sskr/256-sskrgroup-3.jpeg)

## Final Notes

SSKR is a powerful way to back up digital secrets. This example
demonstrates a specific seed and how it can be used to generate SSKR
shares using two different groupings.

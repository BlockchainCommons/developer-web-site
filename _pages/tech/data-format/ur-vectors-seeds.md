---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-data-background.jpg
  og_image: /assets/images/bc-card.jpg
title: UR crypto-seed Test Vectors
hide_description: true
classes:
  - wide
permalink: /ur/vectors/seeds/
sidebar:
  nav:
    - ur
---

* [128 Bit Examples (Yinmn Blue)](#128-bit-examples-yinmn-blue)

## 128 Bit Examples (Yinmn Blue)

{% include seed-128.md %}                                              

The following examples demonstrate versions of this seed encoded as bare URs with a variety of data.

### Seed with No Additional Data
![](/assets/images/ur/vector-seed-yinmn-new.png)

```
{
  1: h'59f2293a5bce7d4de59e71b4207ac5d2'
}
```

### Seed with Date

![](/assets/images/ur/vector-seed-yinmn-date-new.png)

```
{
  1: h'59f2293a5bce7d4de59e71b4207ac5d2',
  2: 1(1645539742)
}
```

### Seed with Date & Name

![](/assets/images/ur/vector-seed-yinmn-name-new.png)

```
{
  1: h'59f2293a5bce7d4de59e71b4207ac5d2',
  2: 1(1645539742),
  3: "Yinmn Blue Acid Exam"
}
```

### Seed with Date, Name & Note

![](/assets/images/ur/vector-seed-yinmn-note-new.png)

```
{
  1: h'59f2293a5bce7d4de59e71b4207ac5d2',
  2: 1(1645539742),
  3: "Yinmn Blue Acid Exam",
  4: "This is our standard 128-bit test seed."
}
```

### Seed with Date, Name & Long Note

**Static Version:**

![](/assets/images/ur/vector-seed-yinmn-lnote-new.png)



```
{
  1: h'59f2293a5bce7d4de59e71b4207ac5d2',
  2: 1(1645539742),
  3: "Yinmn Blue Acid Exam",
  4: "This is our standard 128-bit test seed. However, this version of it has a very long note. Why? The object is to force the creation of an animated GIF, which demonstrates the power of URs, not just to allow for the self-identification of information, but also to do so in a way the integrates well with QR codes, creating an animated QR when the data would otherwise be too long for a static QR. To force that to happen, this note was made to be over 500 characters long, which is a lot of data for a QR, which maxes out at about 4,000 alphanumeric characters, but which gets very hard to read (especially from a phone) before that."
}
```

## Appendix: Standard Process for Creating Test Vectors:

The following process, which can be used for all test vector creation,
uses a detailed `ur:seed` as an example.

1. Write the JSON vector using [BCR-2020-006](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2020-006-urtypes.md#cryptographic-seed-crypto-seed) as reference. Place the hex seed in map element #1, and then optionally place a creation date in element #2, a name in element #3, and a note in element #4.
```
{
  1: h'59f2293a5bce7d4de59e71b4207ac5d2',
  2: 1(1645539742),
  3: "Yinmn Blue Acid Exam",
  4: "This is our standard 128-bit test seed."
}
```
2. Encode the JSON with [CBOR Playground](https://cbor.me/). First test it with full description, then check "plain text" to just produce the hex.
```
A4015059F2293A5BCE7D4DE59E71B4207AC5D202C11A6214F19E037459696E6D6E20426C75652041636964204578616D04782754686973206973206F7572207374616E64617264203132382D626974207465737420736565642E
```
3. Use `bytewords` with `hex` input and `minimal` output to produce the UR body.
```
bytewords -i hex -o minimal A4015059F2293A5BCE7D4DE59E71B4207AC5D202C11A6214F19E037459696E6D6E20426C75652041636964204578616D04782754686973206973206F7572207374616E64617264203132382D626974207465737420736565642E
oxadgdhkwzdtfthptokigtvwnnjsqzcxknsktdaosecyidbbwnnnaxjyhkinjtjnjtcxfwjzkpihcxfpiainiecxfekshsjnaaksdighisinjkcxinjkcxjlkpjpcxjkjyhsjtiehsjpiecxeheyetdpidinjycxjyihjkjycxjkihihiedmksjpaate
```
4. Add `ur:seed/` as a prefix.
```
ur:seed/oxadgdhkwzdtfthptokigtvwnnjsqzcxknsktdaosecyidbbwnnnaxjyhkinjtjnjtcxfwjzkpihcxfpiainiecxfekshsjnaaksdighisinjkcxinjkcxjlkpjpcxjkjyhsjtiehsjpiecxeheyetdpidinjycxjyihjkjycxjkihihiedmksjpaate
```
5. Encode the `ur:seed` as a QR.
```
echo "ur:seed/oxadgdhkwzdtfthptokigtvwnnjsqzcxknsktdaosecyidbbwnnnaxjyhkinjtjnjtcxfwjzkpihcxfpiainiecxfekshsjnaaksdighisinjkcxinjkcxjlkpjpcxjkjyhsjtiehsjpiecxeheyetdpidinjycxjyihjkjycxjkihihiedmksjpaate" | qrencode -o ~/vector-seed-yinmn-note.png
```


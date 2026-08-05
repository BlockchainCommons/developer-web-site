---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/otherresources.jpg
  og_image: /assets/images/bc-card.jpg
tagline: "Sharding Secrets to Protect Your Bitcoin"
title: "August 2026 Gordian Developer Meeting CLI"
hide_description: true
classes:
  - wide
permalink: /meetings/2026-08-resilience/cli/
sidebar:
  nav:
    - resources
    - meetings
---

_The following commands can be used to follow along with the Bitcoin Core demo from the August 5, 2026 Gordian Developer meeting._

## Installations

This CLI tutorial requires the installation of five tools: `bitcoin-cli`, `envelope-cli`, `keytool-cli`, `seedtool-cli`, and `jq`. We suggest spinning up a cloud Debian server for clean setup. The following instructions assume that setup.

For a more exhaustive and better described look at these processes, see [Learning Bitcoin from the Command Line Chapter 10](https://learningbitcoin.blockchaincommons.com/10_0_Working_with_Secrets/).

### Installing Bitcoin

Our Learning Bitcoin Course suggests a number of methods for setting up Bitcoin. We suggest using our [Bitcoin Standup script](https://github.com/BlockchainCommons/Bitcoin-Standup-Scripts/blob/master/Scripts/LinodeStandUp.sh) on a Debian system as described in [Learning Bitcoin §2.1](https://learningbitcoin.blockchaincommons.com/02_1_Setting_Up_a_Bitcoin-Core_VPS_with_StackScript/). 

A listing of alternative options is available in [Learning Bitcoin §2.2](https://learningbitcoin.blockchaincommons.com/02_2_Setting_Up_Bitcoin_Core_Other/).

### Installing Seedtool & Envelope

`envelope-cli` and `seedtool-cli` are both available as Rust crates.

If you do not have Rust installed, you'll need to do so. The following installation instructions work on a Debian system:
```
sudo apt-get install build-essential
curl https://sh.rustup.rs -sSf | sh
. "$HOME/.cargo/env"
```
Afterward, you can easily install the crates:
```
cargo install seedtool-cli
cargo install bc-envelope-cli
```

### Installing Keytool

Keytool is the most challenging installation because it requires setting up a Python environment, compiling, and installing.

The following will install Python on a Debian system:
```
sudo apt-get install llvm clang lsb-release wget git apt-transport-https pkg-config autoconf libtool libc++-dev libc++abi-dev python3 python3-setuptools
sudo bash -c "$(wget -O - https://apt.llvm.org/llvm.sh)"
sudo ln -s /usr/bin/python3 /usr/bin/python
```

This will them compile and install:

```
cd ~
git clone https://github.com/BlockchainCommons/keytool-cli.git
cd keytool-cli/
autoreconf -i
export CC="clang" && export CXX="clang++" && ./build.sh
sudo make install
```

### Installing JQ

JQ is a parsing tool available from [jqlang.org](https://jqlang.org/). On A Debian system, you can install it with:

```
sudo apt-get install jq
```

## Exporting a Secret from Bitcoin

### 1. Understand the Descriptor

```
bitcoin-cli listdescriptors true | jq -r '.descriptors[].desc'
```
A descriptor is the standard interoperable methodology for Bitcoin Core.

### 2. Extract Descriptors

```
DESCS=$(bitcoin-cli listdescriptors true | jq -r '.descriptors[].desc')
DESC_ARRAY=($DESCS)
for ((i = 0; i < 8; i++)); do
   echo "$i: ${DESC_ARRAY[$i]}"; 
done
```

### 3. Extract Private Key

```
MP_KEY=$(echo ${DESC_ARRAY[0]} | awk -F"[()]" '{print $2}' | awk -F"/" '{print $1}')
echo $MP_KEY
```
We're exporting both descriptors and the private key to show two _different_ ways to backup Bitcoin Core.

### 4a. Store Master Key

```
KEY_ENVELOPE=$(envelope subject type string "$MP_KEY")
KEY_ENVELOPE=$(envelope assertion add pred-obj known 'isA' known 'MasterKey' "$KEY_ENVELOPE")
KEY_ENVELOPE=$(envelope assertion add pred-obj string "createdBy" string "`bitcoin-cli --version | head -1`" "$KEY_ENVELOPE")
KEY_ENVELOPE=$(envelope assertion add pred-obj string "usedBy" string "`bitcoin-cli --version | head -1`" "$KEY_ENVELOPE")
KEY_ENVELOPE=$(envelope assertion add pred-obj known 'DerivationPath' string "m/44h/0h/0h" "$KEY_ENVELOPE")
KEY_ENVELOPE=$(envelope assertion add pred-obj known 'DerivationPath' string "m/84h/0h/0h" "$KEY_ENVELOPE")
KEY_ENVELOPE=$(envelope assertion add pred-obj known 'DerivationPath' string "m/49h/0h/0h" "$KEY_ENVELOPE")
KEY_ENVELOPE=$(envelope assertion add pred-obj known 'DerivationPath' string "m/86h/0h/0h" "$KEY_ENVELOPE")

envelope format $KEY_ENVELOPE
```

This is a fairly minimal set of metadata. Best practice would also typically add a 'date' and a 'note'. The object is to have sufficient information to make use of the seed when it's recovered in the future.

### 4b. Store Descriptors

```
DESC_ENVELOPE_1=$(envelope subject type string "descriptors-for-bitcoin")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'isA' string "collectionOfDescriptors" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj string "createdBy" string "`bitcoin-cli --version | head -1`" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj string "usedBy" string "`bitcoin-cli --version | head -1`" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[0]}" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[1]}" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[2]}" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[3]}" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[4]}" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[5]}" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[6]}" "$DESC_ENVELOPE_1")
DESC_ENVELOPE_1=$(envelope assertion add pred-obj known 'OutputDescriptor' string "${DESC_ARRAY[7]}" "$DESC_ENVELOPE_1")

envelope format $DESC_ENVELOPE_1
```

These descriptors are stored as raw text. Blockchain Commons also has a [BCR](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-010-output-descriptor.md) laying out how to store a descrpitor in a more standardized UR format.

### 5. Shard Envelope

```
KEY_SHARES=$(envelope sskr split --group "2-of-3" $KEY_ENVELOPE)
KEY_ARRAY=($KEY_SHARES)
```
An SSKR Envelope creates a new secret, encrypts the envelope with it, and just shards just that secret.

### 6a. Check Your Work

```
echo ${KEY_ARRAY[0]}
echo ${KEY_ARRAY[1]}
echo ${KEY_ARRAY[2]}
```

### 6b. Check Your Work

```
RESTORED_KEY=$(envelope sskr join "${KEY_ARRAY[0]}" "${KEY_ARRAY[1]}")
envelope format $RESTORED_KEY
```
Best practice would be to verify each pair of shares reconstructs the envelope.

## Importing a Secret into Bitcoin Core

### 1. Create a Seed

```
SEED=$(seedtool)
echo $SEED
```

### 2. Create a Fingerprint

```
FINGERPRINT=$(keytool --seed $SEED master-key-fingerprint)
echo $FINGERPRINT
```
The fingerprint isn't technically necessary to create a descriptor, but it's metadata that reminds you where a descriptor came from.

### 3. Create Account Key

```
AKEY=$(keytool --seed $SEED --account-derivation-path "m/84h/0h/0h" account-key-base58)
echo $AKEY
```
We choose to create an account key rather than a master key to full the [least & necessary design philosophy](https://developer.blockchaincommons.com/architecture/design/#least--necessary).

### 4. Create Descriptor

```
DESC="wpkh([$FINGERPRINT/84h/0h/0h]$AKEY/0/*)"
DESC_CS=$(bitcoin-cli getdescriptorinfo $DESC | jq -r '.checksum')
DESC_WITH_CS=$DESC#$DESC_CS
echo $DESC_WITH_CS
```
Obviously, you need to know what a descriptor should look like. `listdescriptors` is a great guide but [BIP-380](https://github.com/bitcoin/bips/blob/master/bip-0380.mediawiki) is the ultimate source.

A checksum is required by Bitcoin Core to work with descriptors.

### 5. Import Descriptor

```
bitcoin-cli -named createwallet wallet_name="seedtool" blank=true
bitcoin-cli -rpcwallet=seedtool importdescriptors '''[{ "desc": "'$DESC_WITH_CS'", "timestamp":1780329126, "active": true, "range": [0,100] }]'''
```

### 6. Check Your Work

```
bitcoin-cli listdescriptors
```

### 7a. Backup with Seedtool 

```
seedtool -i hex $SEED -o bip39
seedtool -i hex $SEED -o sskr --groups 2-of-3 --sskr-format ur
```
Seedtool can do Shamir's Secret Sharing on its own, as shown here, but it's better to put the seed into an envelope and add metadata before sharding.

### 7b. Test Your Backup

```
echo "ur:sskr/gosgtyaeadaegylnahsomsfxstamonssdwotasynuofxoybdnshd ur:sskr/gosgtyaeadaocavdihwplnpkfwiodnpfkgzodtmkidkssbndhgwl" | seedtool -i sskr
```

## 8a. Backup with Envelope

```
SEED_ENVELOPE=$(envelope subject type string "$SEED")
SEED_ENVELOPE=$(envelope assertion add pred-obj known 'isA' known 'Seed' "$SEED_ENVELOPE")
envelope format $SEED_ENVELOPE
```

## 8b. Add Metadata to Seed

```
SEED_ENVELOPE=$(envelope assertion add pred-obj string "createdBy" string "`seedtool -V`" "$SEED_ENVELOPE")
SEED_ENVELOPE=$(envelope assertion add pred-obj string "usedBy" string "`bitcoin-cli --version | head -1`" "$SEED_ENVELOPE")
SEED_ENVELOPE=$(envelope assertion add pred-obj known 'DerivationPath' string "m/84h/0h/0h" "$SEED_ENVELOPE")
envelope format $SEED_ENVELOPE
```

This is a fairly minimal set of metadata. Best practice would also typically add a 'date' and a 'note'. The object is to have sufficient information to make use of the seed when it's recovered in the future.

## 8c. Shard Seed

```
SEED_SHARES=$(envelope sskr split --group "2-of-3" $SEED_ENVELOPE)
SEED_ARRAY=($SEED_SHARES)
echo ${SEED_ARRAY[0]}
```

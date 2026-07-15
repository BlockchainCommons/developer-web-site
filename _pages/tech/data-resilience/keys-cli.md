---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/images/tech-dataresilience.jpg
  og_image: /assets/images/bc-card.jpg
title: "Learning Key Hierarchies from the Command Line (with XIDs)"
tagline: "Separate Keys by Purpose"
hide_description: true
classes:
  - wide
permalink: /keys/cli/
sidebar:
  nav:
    - keymanagement
    - dataresilience
    - technology
---

_The Key Management [best practices](/keys/best-practices/) discuss a
number of ways to make your keys more resilient, avoiding SPOFs and
SPOCs. One of those best practices is "Separate Keys by Purpose". The
following demonstrates how to do so using [XIDs](/xid/) to create a
key hierarchy as an example._

_This was one of the explicit design goals for XIDs: to allow robust,
heterogeneous key management._

## Creating a Trust-Based Key Hierarchy

Key hierarchies allow for the creation of different keys with
different permissions, which in turn allows you to use the key with
the least necessary permissions at any time, reducing the odds of
losing a crucial "primary" key to SPOF or SPOC.

XIDs were built to demonstrate the models of key hierarchy, as the
below examples demonstrate.

### Create Primary Identity Key

XIDs are created from keys. This "primary identity key" or "inception
key" controls the XID. It should be stored securely and rarely used in
real-life (just like you shouldn't use a "root" account or "Admin"
account for everyday usage of a computer).

```sh
PRIMARY_KEY_PRIVATE=$(envelope generate prvkeys)
PRIMARY_KEY_PUBLIC=$(envelope generate pubkeys $PRIMARY_KEY_PRIVATE)
XID=$(envelope xid new --nickname "BRadvoc8" --generator include $PRIMARY_KEY_PRIVATE)
```

### Create Function-Specific Keys

Additional keys can be created with a single purpose such as signing
or encryption. These are the keys that should be used for everyday
operations, each with the least necessary permission needed at any time.

```sh
SIGNING_KEY_PRIVATE=$(envelope generate prvkeys)
XID=$(envelope xid key add --nickname "Attestation Signing Key" --allow sign "$SIGNING_KEY_PRIVATE" "$XID")
```

### Create Project-Specific Keys

Keys can also be created for specific projects. Besides being linked
to a XID (identifying the key as belonging to a specific identity),
the key can also be linked to that project (such as by uploading the
related public key to GitHub), demonstrating its intended usage.


```sh
PROJECT_KEY_PRIVATE=$(envelope generate prvkeys)
XID=$(envelope xid key add --nickname "API Security Project" --allow sign --allow encrypt "$PROJECT_KEY_PRIVATE" "$XID")
```

### Create Device-Specific Keys

Keys can be create for specific devices such as a laptop, a tablet, or
a phone. Permissions can then be set for the specific needs of that
device (e.g., a phone may only need `auth` access to services, while a
laptop may need more complete operational permissions).


```sh
TABLET_KEY_PRIVATE=$(envelope generate prvkeys)
XID=$(envelope xid key add --nickname "Tablet Key" --allow sign "$TABLET_KEY_PRIVATE" "$XID")
```

Device keys also encourage occasional rotation if you're not good at
that, because when you get a new device you should create a new key
(and hopefully retire the old one).

### Create Recovery Keys

Though an inception key can allow for the recovery of a XID, special
keys with `transfer`/`elect` permissions can do so as well. These can
be created, then stored securely offline or with trusted individuals.


```sh
RECOVERY_KEY_PRIVATE=$(envelope generate prvkeys)
RECOVERY_KEY_PUBLIC=$(envelope generate pubkeys $RECOVERY_KEY_PRIVATE)
XID=$(envelope xid key add --nickname "Recovery Key" --allow transfer --allow elect "$RECOVERY_KEY_PRIVATE" "$XID")
```

### Review Your Results

This hierarchical approach combines security with usability by using
the right key for each context.

```sh
envelope format $XID

| XID(20870a4f) [
|     'key': PublicKeys(3fef60b0, SigningPublicKey(4daf7979, SchnorrPublicKey(62f03dd5)), EncapsulationPublicKey(83731632, X25519PublicKey(83731632))) [
|         {
|             'privateKey': PrivateKeys(885b4d80, SigningPrivateKey(93f5ce1b, SchnorrPrivateKey(d8058882)), EncapsulationPrivateKey(ae4db7da, X25519PrivateKey(ae4db7da)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Encrypt'
|         'allow': 'Sign'
|         'nickname': "API Security Project"
|     ]
|     'key': PublicKeys(5f0c91bf, SigningPublicKey(20870a4f, SchnorrPublicKey(78586164)), EncapsulationPublicKey(15f17dd7, X25519PublicKey(15f17dd7))) [
|         {
|             'privateKey': PrivateKeys(5bcbcdee, SigningPrivateKey(ea7be2dc, SchnorrPrivateKey(86ab479e)), EncapsulationPrivateKey(d55b3ff6, X25519PrivateKey(d55b3ff6)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "BRadvoc8"
|     ]
|     'key': PublicKeys(655ce15f, SigningPublicKey(1b58b484, SchnorrPublicKey(476f7c0e)), EncapsulationPublicKey(7628b77b, X25519PublicKey(7628b77b))) [
|         {
|             'privateKey': PrivateKeys(1b5b0981, SigningPrivateKey(5ab4321e, SchnorrPrivateKey(5cdb70c1)), EncapsulationPrivateKey(8f06a338, X25519PrivateKey(8f06a338)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Sign'
|         'nickname': "Attestation Signing Key"
|     ]
|     'key': PublicKeys(8d830bd9, SigningPublicKey(3ff0f24a, SchnorrPublicKey(08ef557e)), EncapsulationPublicKey(4a4994ee, X25519PublicKey(4a4994ee))) [
|         {
|             'privateKey': PrivateKeys(6eb4d50a, SigningPrivateKey(52904cb8, SchnorrPrivateKey(ea5e676e)), EncapsulationPrivateKey(2a62cac4, X25519PrivateKey(2a62cac4)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Sign'
|         'nickname': "Tablet Key"
|     ]
|     'key': PublicKeys(d98523e2, SigningPublicKey(df7eed15, SchnorrPublicKey(badf9467)), EncapsulationPublicKey(b2be4c8e, X25519PublicKey(b2be4c8e))) [
|         {
|             'privateKey': PrivateKeys(57554a65, SigningPrivateKey(19721999, SchnorrPrivateKey(5f628686)), EncapsulationPrivateKey(483c48b8, X25519PrivateKey(483c48b8)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Elect'
|         'allow': 'Transfer'
|         'nickname': "Recovery Key"
|     ]
|     'provenance': ProvenanceMark(81035eb21e558e58cba1a0cd705a7853ec03318d763c32c4222ea9e41a9a4ba1) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
v|                 "next-seq": 1
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
```

## Building Progressive Permission Models

One of the architectural designs of XIDs was to purposeful divide keys
and key permissions. This is what allows for the building of a
progressive permissions model, where permissions for a key are
increased over time.

Obviously, you might do this for your own key, say if you created a
laptop key and later realized that it needed more
permissions. However, you can also do for a key created for a
collaborative partnership, reflecting an increase in [progressive
trust](/progressive-trust/).

### Create Limited Access Collaborative Key

This example starts with a new XID, intended for a collaboration, with
a collaborative key that will be shared given minimal permissions.

```sh
PRIMARY_COLLAB_KEY_PRIVATE=$(envelope generate prvkeys)
COLLAB_XID=$(envelope xid new --nickname "Collaborative XID Primary Key" --generator include $PRIMARY_COLLAB_KEY_PRIVATE)

SHARED_COLLAB_KEY_PRIVATE=$(envelope generate prvkeys)
SHARED_COLLAB_KEY_PUBLIC=$(envelope generate pubkeys $SHARED_COLLAB_KEY_PRIVATE)
COLLAB_XID=$(envelope xid key add --nickname "Shared Collaboration (Initial)" --allow encrypt "$SHARED_COLLAB_KEY_PRIVATE" "$COLLAB_XID")
```

### Document Trust Development

Keeping with the best practice of transparency, when you decide to
expand trust, you create a signed statement about that trust increase.

```sh
DELIVERABLE=$(envelope subject type string "Collaborative Deliverable")
DELIVERABLE=$(envelope assertion add pred-obj string "outcome" string "Successfully completed initial security analysis" "$DELIVERABLE")
   DELIVERABLE=$(envelope assertion add pred-obj string "evaluationResult" string "Exceeds expectations" "$DELIVERABLE")
WRAPPED_DELIVERABLE=$(envelope subject type wrapped $DELIVERABLE)
SIGNED_DELIVERABLE=$(envelope sign -s $PRIMARY_COLLAB_KEY_PRIVATE $WRAPPED_DELIVERABLE)
```

### Evolve Permission

Then, you update the XID with the new permissions for the shared key.

```sh
COLLAB_XID=$(envelope xid key update --nickname "Shared Collaboration (Stage 1)" --allow encrypt --allow sign "$SHARED_COLLAB_KEY_PUBLIC" "$COLLAB_XID")
```

The result shows the new key permissions and not the old ones, exactly
what you want for progressive permissions.

```sh
envelope format $COLLAB_XID

| XID(f8224c70) [
|     'key': PublicKeys(d5a17d44, SigningPublicKey(f8224c70, SchnorrPublicKey(217d329e)), EncapsulationPublicKey(6d6cc3f4, X25519PublicKey(6d6cc3f4))) [
|         {
|             'privateKey': PrivateKeys(14ad20f2, SigningPrivateKey(0edd78e3, SchnorrPrivateKey(b5ae08b6)), EncapsulationPrivateKey(bf98e2e9, X25519PrivateKey(bf98e2e9)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "Collaborative XID Primary Key"
|     ]
|     'key': PublicKeys(fc578721, SigningPublicKey(a8ab292b, SchnorrPublicKey(bab0433b)), EncapsulationPublicKey(88ee7252, X25519PublicKey(88ee7252))) [
|         {
|             'privateKey': PrivateKeys(1eb6368a, SigningPrivateKey(1f5c9f99, SchnorrPrivateKey(d9a201b5)), EncapsulationPrivateKey(b9a259d5, X25519PrivateKey(b9a259d5)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Encrypt'
|         'allow': 'Sign'
|         'nickname': "Shared Collaboration (Stage 1)"
|     ]
|     'provenance': ProvenanceMark(b395164b834b4db8573c4f5d2798188d95e69b264fb14d2be69763a1373b87e9) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
|                 "next-seq": 1
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
```

### Advance Provenance Mark

But how do you know which XUD is the newer one? The one with more or
less permissions? The nickname gives a hint (since one is labeled
"Initial" and the other "Stage 1"), but you really want something
cryptograpically secure to depend on. That's why you advance the
provenance mark.

```sh
NEW_COLLAB_XID=$(envelope xid provenance next $COLLAB_XID)
```

If you have the [provenance mark
CLI](https://github.com/BlockchainCommons/provenance-mark-cli-rust)
installed, you can now check the sequence of each, which will tell you
which is the newest.

```sh
OLDPROV_MARK=$(envelope xid provenance get "$COLLAB_XID")
provenance validate --format json-compact "$OLDPROV_MARK" 2>&1 | grep -o '"end_seq":[0-9]*'

| "end_seq":0

NEWPROV_MARK=$(envelope xid provenance get "$NEW_COLLAB_XID")
provenance validate --format json-compact "$NEWPROV_MARK" 2>&1 | grep -o '"end_seq":[0-9]*'

| "end_seq":1
```

### Document Key Evolution

Again, you should record the upgrade.

```sh
UPGRADE_RECORD=$(envelope subject type string "Permission Upgrade Record")
UPGRADE_RECORD=$(envelope assertion add pred-obj string "addedPermissions" string "sign" "$UPGRADE_RECORD")
UPGRADE_RECORD=$(envelope assertion add pred-obj string "justification" envelope "$SIGNED_DELIVERABLE" "$UPGRADE_RECORD")
WRAPPED_UPGRADE=$(envelope subject type wrapped $UPGRADE_RECORD)
SIGNED_UPGRADE=$(envelope sign -s $PRIMARY_COLLAB_KEY_PRIVATE $WRAPPED_UPGRADE)
```
## Rotating Keys

The following steps demonstrate how to replace (rotate) a key.

### Document the Reason

Record the why rotation is happening.

```sh
TABLET_KEY=$(envelope xid key find name "Tablet Key" "$XID" | envelope extract ur)   
ROTATION_RECORD=$(envelope subject type ur $TABLET_KEY)
ROTATION_RECORD=$(envelope assertion add pred-obj string "rotationReason" string "Suspected device tampering at public cafe" "$ROTATION_RECORD")
```

### Generate New Key

Create fresh key material.

```sh
NEW_TABLET_KEY_PRIVATE=$(envelope generate prvkeys)
NEW_TABLET_KEY_PUBLIC=$(envelope generate pubkeys "$NEW_TABLET_KEY_PRIVATE")
```

### Remove Old Key

Take out the key being rotated

```sh
ROTATED_XID=$(envelope xid key remove "$TABLET_KEY" "$XID")
```

### Add New Key

Add the replacement with appropriate permissions

```sh
ROTATED_XID=$(envelope xid key add --nickname "Tablet Key (Rotated)" --allow sign "$NEW_TABLET_KEY_PUBLIC" "$ROTATED_XID")
```

### Advance Provenance Mark

Update your provenance mark to show ordering of the key update.

```sh
ROTATED_XID=$(envelope xid provenance next $ROTATED_XID)
```

### Notify Collaborators

Inform others who need to verify your signatures.

```sh
NOTIFICATION=$(envelope subject type string "Key Rotation Notification")
NOTIFICATION=$(envelope assertion add pred-obj string "oldKey" envelope $ROTATION_RECORD $NOTIFICATION)
NOTIFICATION=$(envelope assertion add pred-obj string "newKey" ur $NEW_TABLET_KEY_PUBLIC $NOTIFICATION)
WRAPPED_NOTIFICATION=$(envelope subject type wrapped $NOTIFICATION)
SIGNED_NOTIFICATION=$(envelope sign -s "$PRIMARY_KEY_PRIVATE" "$WRAPPED_NOTIFICATION")
```

Here's what the notification looks like:

```
envelope format $SIGNED_NOTIFICATION

| {
|     "Key Rotation Notification" [
|         "newKey": PublicKeys(e2e1a081, SigningPublicKey(6f810d79, SchnorrPublicKey(5b9899e9)), EncapsulationPublicKey(d9362375, X25519PublicKey(d9362375)))
|         "oldKey": PublicKeys(8d830bd9, SigningPublicKey(3ff0f24a, SchnorrPublicKey(08ef557e)), EncapsulationPublicKey(4a4994ee, X25519PublicKey(4a4994ee))) [
|             "rotationReason": "Suspected device tampering at public cafe"
|         ]
|     ]
| } [
|     'signed': Signature
| ]
```

Here's what the new XID looks like:

```sh

Result:
```
envelope format $ROTATED_XID

| XID(20870a4f) [
|     'key': PublicKeys(3fef60b0, SigningPublicKey(4daf7979, SchnorrPublicKey(62f03dd5)), EncapsulationPublicKey(83731632, X25519PublicKey(83731632))) [
|         {
|             'privateKey': PrivateKeys(885b4d80, SigningPrivateKey(93f5ce1b, SchnorrPrivateKey(d8058882)), EncapsulationPrivateKey(ae4db7da, X25519PrivateKey(ae4db7da)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Encrypt'
|         'allow': 'Sign'
|         'nickname': "API Security Project"
|     ]
|     'key': PublicKeys(5f0c91bf, SigningPublicKey(20870a4f, SchnorrPublicKey(78586164)), EncapsulationPublicKey(15f17dd7, X25519PublicKey(15f17dd7))) [
|         {
|             'privateKey': PrivateKeys(5bcbcdee, SigningPrivateKey(ea7be2dc, SchnorrPrivateKey(86ab479e)), EncapsulationPrivateKey(d55b3ff6, X25519PrivateKey(d55b3ff6)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'All'
|         'nickname': "BRadvoc8"
|     ]
|     'key': PublicKeys(655ce15f, SigningPublicKey(1b58b484, SchnorrPublicKey(476f7c0e)), EncapsulationPublicKey(7628b77b, X25519PublicKey(7628b77b))) [
|         {
|             'privateKey': PrivateKeys(1b5b0981, SigningPrivateKey(5ab4321e, SchnorrPrivateKey(5cdb70c1)), EncapsulationPrivateKey(8f06a338, X25519PrivateKey(8f06a338)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Sign'
|         'nickname': "Attestation Signing Key"
|     ]
|     'key': PublicKeys(d98523e2, SigningPublicKey(df7eed15, SchnorrPublicKey(badf9467)), EncapsulationPublicKey(b2be4c8e, X25519PublicKey(b2be4c8e))) [
|         {
|             'privateKey': PrivateKeys(57554a65, SigningPrivateKey(19721999, SchnorrPrivateKey(5f628686)), EncapsulationPrivateKey(483c48b8, X25519PrivateKey(483c48b8)))
|         } [
|             'salt': Salt
|         ]
|         'allow': 'Elect'
|         'allow': 'Transfer'
|         'nickname': "Recovery Key"
|     ]
|     'key': PublicKeys(e2e1a081, SigningPublicKey(6f810d79, SchnorrPublicKey(5b9899e9)), EncapsulationPublicKey(d9362375, X25519PublicKey(d9362375))) [
|         'allow': 'Sign'
|         'nickname': "Tablet Key (Rotated)"
|     ]
|     'provenance': ProvenanceMark(986ec75c208954b285ae30fae37b4768cce7d2938c1c2e6ae7d85fa5e1370fe3) [
|         {
|             'provenanceGenerator': Bytes(32) [
|                 'isA': "provenance-generator"
|                 "next-seq": 2
|                 "res": 3
|                 "rng-state": Bytes(32)
|                 "seed": Bytes(32)
|             ]
|         } [
|             'salt': Salt
|         ]
|     ]
| ]
```

## Recovering from Loss

Recovering from the loss of a primary key works just like rotating a
key as long as you created a recovery key.

### Swap Out Key

```sh
RECOVERED_XID=$(envelope xid key remove "$PRIMARY_KEY_PUBLIC" "$XID")
NEW_PRIMARY_KEY_PRIVATE=$(envelope generate prvkeys)
RECOVERED_XID=$(envelope xid key add --nickname "Primary Identity (Recovered)" --allow all "$NEW_PRIMARY_KEY_PRIVATE" "$RECOVERED_XID")
```

### Update Provenance Mark

As usual, you should update your provenance mark to show which XID is newer.

```sh
RECOVERED_XID=$(envelope xid provenance next $RECOVERED_XID)
```

###Document Recovery

And you should transparently document what you did.

```sh
RECOVERY_ATTESTATION=$(envelope subject type string "Recovery Authorization")
RECOVERY_ATTESTATION=$(envelope assertion add pred-obj string "regarding" ur "$XID" "$RECOVERY_ATTESTATION")
RECOVERY_ATTESTATION=$(envelope assertion add pred-obj string "recoveryKey" ur "$RECOVERY_KEY_PUBLIC" "$RECOVERY_ATTESTATION")
RECOVERY_ATTESTATION=$(envelope assertion add pred-obj string "action" string "Recovery of primary identity key" "$RECOVERY_ATTESTATION")
RECOVERY_ATTESTATION=$(envelope assertion add pred-obj string "methodology" string "Recovery key used" "$RECOVERY_ATTESTATION")
WRAPPED_RECOVERY_ATTESTATION=$(envelope subject type wrapped $RECOVERY_ATTESTATION)
SIGNED_RECOVERY_ATTESTATION=$(envelope sign -s "$RECOVERY_KEY_PRIVATE" "$WRAPPED_RECOVERY_ATTESTATION")

envelope format $SIGNED_RECOVERY_ATTESTATION

| {
|     "Recovery Authorization" [
|         "action": "Recovery of primary identity key"
|         "methodology": "Recovery key used"
|         "recoveryKey": PublicKeys(d98523e2, SigningPublicKey(df7eed15, SchnorrPublicKey(badf9467)), EncapsulationPublicKey(b2be4c8e, X25519PublicKey(b2be4c8e)))
|         "regarding": XID(20870a4f) [
|             'key': PublicKeys(3fef60b0, SigningPublicKey(4daf7979, SchnorrPublicKey(62f03dd5)), EncapsulationPublicKey(83731632, X25519PublicKey(83731632))) [
|                 {
|                     'privateKey': PrivateKeys(885b4d80, SigningPrivateKey(93f5ce1b, SchnorrPrivateKey(d8058882)), EncapsulationPrivateKey(ae4db7da, X25519PrivateKey(ae4db7da)))
|                 } [
|                     'salt': Salt
|                 ]
|                 'allow': 'Encrypt'
|                 'allow': 'Sign'
|                 'nickname': "API Security Project"
|             ]
|             'key': PublicKeys(5f0c91bf, SigningPublicKey(20870a4f, SchnorrPublicKey(78586164)), EncapsulationPublicKey(15f17dd7, X25519PublicKey(15f17dd7))) [
|                 {
|                     'privateKey': PrivateKeys(5bcbcdee, SigningPrivateKey(ea7be2dc, SchnorrPrivateKey(86ab479e)), EncapsulationPrivateKey(d55b3ff6, X25519PrivateKey(d55b3ff6)))
|                 } [
|                     'salt': Salt
|                 ]
|                 'allow': 'All'
|                 'nickname': "BRadvoc8"
|             ]
|             'key': PublicKeys(655ce15f, SigningPublicKey(1b58b484, SchnorrPublicKey(476f7c0e)), EncapsulationPublicKey(7628b77b, X25519PublicKey(7628b77b))) [
|                 {
|                     'privateKey': PrivateKeys(1b5b0981, SigningPrivateKey(5ab4321e, SchnorrPrivateKey(5cdb70c1)), EncapsulationPrivateKey(8f06a338, X25519PrivateKey(8f06a338)))
|                 } [
|                     'salt': Salt
|                 ]
|                 'allow': 'Sign'
|                 'nickname': "Attestation Signing Key"
|             ]
|             'key': PublicKeys(8d830bd9, SigningPublicKey(3ff0f24a, SchnorrPublicKey(08ef557e)), EncapsulationPublicKey(4a4994ee, X25519PublicKey(4a4994ee))) [
|                 {
|                     'privateKey': PrivateKeys(6eb4d50a, SigningPrivateKey(52904cb8, SchnorrPrivateKey(ea5e676e)), EncapsulationPrivateKey(2a62cac4, X25519PrivateKey(2a62cac4)))
|                 } [
|                     'salt': Salt
|                 ]
|                 'allow': 'Sign'
|                 'nickname': "Tablet Key"
|             ]
|             'key': PublicKeys(d98523e2, SigningPublicKey(df7eed15, SchnorrPublicKey(badf9467)), EncapsulationPublicKey(b2be4c8e, X25519PublicKey(b2be4c8e))) [
|                 {
|                     'privateKey': PrivateKeys(57554a65, SigningPrivateKey(19721999, SchnorrPrivateKey(5f628686)), EncapsulationPrivateKey(483c48b8, X25519PrivateKey(483c48b8)))
|                 } [
|                     'salt': Salt
|                 ]
|                 'allow': 'Elect'
|                 'allow': 'Transfer'
|                 'nickname': "Recovery Key"
|             ]
|             'provenance': ProvenanceMark(81035eb21e558e58cba1a0cd705a7853ec03318d763c32c4222ea9e41a9a4ba1) [
|                 {
|                     'provenanceGenerator': Bytes(32) [
|                         'isA': "provenance-generator"
|                         "next-seq": 1
|                         "res": 3
|                         "rng-state": Bytes(32)
|                         "seed": Bytes(32)
|                     ]
|                 } [
|                     'salt': Salt
|                 ]
|             ]
|         ]
|     ]
| } [
|     'signed': Signature
| ]
```

## Check Your Understanding

1. Why is a key hierarchy more secure than a single key for everything?
2. What are the different types of permissions in the XID system?
3. How do progressive permission models enhance security?
4. What is the process for safely rotating a key?
5. How do XIDs maintain stable identity despite key changes?
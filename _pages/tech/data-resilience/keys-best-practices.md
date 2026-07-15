---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/images/tech-dataresilience.jpg
  og_image: /assets/images/bc-card.jpg
title: "Key Management Best Practices"
hide_description: true
classes:
  - wide
permalink: /keys/best-practices/
sidebar:
  nav:
    - keymanagement
    - dataresilience
    - technology
---

Cryptographic keys are the foundation of digital-asset security and
functionality. Proper key management is critical because:

- Keys control who can use your assets & identity.
- Keys enable verification, signing, and encryption capabilities.
- Key loss without recovery options means losing access to your assets or identity.
- Key compromise could lead to asset theft or identity impersonation.

## Key Management Best Practices

1. **Separate Keys by Purpose**: Authorize different keys for different functions.
2. **Design Progressive Permissions**: Align permissions with trust development.
3. **Document Key Operations**: Keep clear records of key changes.
4. **Rotate Regularly**: Change keys on schedule (and immediately if compromise is suspected).
5. **Backup Before Change**: Always ensure recovery options before key operations.
6. **Communicate Transparently**: Notify collaborators of key changes.
7. **Create Multiple Recovery Paths**: Don't rely on a single recovery mechanism.

### Separating Keys by Purpose

Rather than using a single key for everything, a trust-based key
hierarchy uses different keys for different purposes and contexts:

[XIDs](/xid/) were built to offer an example of a key hierarchy by
supporting a variety of permission types.

The most powerful types of keys allow management of the XID:

- **all**: Allow all operations
- **delegate**: Give function access to third parties
- **verify**: Update this XID
- **update**: Update service endpoints
- **transfer**: Remove inception key
- **elect**: Add or remove other keys
- **burn**: Transition to a new provenance mark chain
- **revoke**: Revoke this XID

Lower level permissions allow operational use of the XID:

- **auth**: Authenticate as this identity
- **sign**: Sign documents and messages as this XID
- **encrypt**: Encrypt/decrypt messages for this XID
- **elide**: Create elided (redacted) versions of documents
- **issue**: Issue or revoke credentials for this XID
- **access**: Access resources allocated to this XID

For XIDs, or anywhere else where a hierarchy of keys has been created,
permissions should be based on the architectural principle of least
privilege: grant only what's needed for each specific key's purpose.

The [Learning Key Management from the CLI
article](/key-management/cli/) uses XIDs as an example to demonstrate
key management through permissions.

### Designing Progressive Permissions

Key capabilities don't have to be locked in. Progressive permission
models align key capabilities with trust development. As a
relationship grows and expands, existing keys can be given new access.

This approach implements least privilege while allowing trust
relationships to develop naturally. It is a form of [progressive
trust](/progressive-trust/).

## Key Rotation & Recovery Best Practices

Key can be rotated and if lost they can be recovered.

### Key Rotation

Key rotation is the process of replacing existing keys with new
ones. It's an essential practice for:

- Limiting exposure from lost/stolen devices
- Mitigating other potential compromise
- Adapting to organizational changes
- Implementing regular security hygiene

As noted above, key rotation should occur regularly, should be well
documented, and should be carefully administered, with backups.

### Key Recovery

Key loss shouldn't mean asset loss. A variety of different best
practices allow for recovery after some loss. These include:

* Use an operational key for day-to-day usage and keep a high-level management key in backup. Our [Learning XIDs course](https://learningxids.blockchaincommons.com/05_5_Responding_to_Key_Compromise/) demonstrates how this can allow recovery.
* Maintain a backup of your keys sharded with [SSKR](/sskr/) and potentially [CSR](/csr/). Ensure a threshold of at least two keys to reconstruct your key and that at least one key can be lost. This means you should minimally use a 2-of-3 threshold, but see our Smart Custody article on ["Designing SSKR Share Scenarios"](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/SSKR-Sharing.md) for more options.
* Use a methodology such as [CKM](/ckm/), [FROST](/frost/), or multisigs to allow access to an asset with a threshold of signatures.

Though key recovery can be self-sovereign, which is something you do
yourself, you can also use social recovery, where other peoples' keys
have been identified as the recovery keys. This allows friends,
families, or fiduciaries to aid in the recovery process.

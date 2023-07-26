---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.25"
  overlay_image: /assets/images/dev-seed-background.jpg
  og_image: /assets/images/bc-card.jpg
title: "CSR Progressive Use Cases"
hide_description: true
permalink: /csr/use-cases/
sidebar:
  nav: csr
toc: true
toc_title: Use Cases
---

These use cases are detailed progressively within a number of categories, such that they grow increasingly more complex (and less naive) over time.

The use cases are primarily focused on Collaborative Seed Recovery (CSR), a methodology for recovering seeds using `crypto-envelopes` and traditional secret-sharing. 

The next generation of usage will be [Collaborative Key Management (CKM)](/ckm/), which will allow for the collaborative generation of keys, such that they don't exist anywhere until they're used. Some of the most complex progressive use cases shade over from CSR into CKM, as noted.

## CSR-Focused Use Cases

This set of use cases is specifically focused on the intended development of CSR. It overlaps with the next, "Secret Sharing Use Cases".

**CSR Use Case #1: Simple is as Simple Does (Sharding, Metadata).** Barney has valuable digital holdings, but Barney doesn't have strong computer knowledge, so he needs something that can protect his holdings in an automated way. He chooses the SimpleSave app. When Barney enters his seed into SimpleSave it's automatically sharded, with one share sent to Barney's local Cloud Backup and the other two sent to randomly determined share servers. The only decision Barney ever has to make is to initiate a recovery, which he would do if he lost his seed. When that happens, SimpleSave retrieves the share from the Cloud, learns from unencrypted metadata associated with that share where the other two shares are, recovers the first of those (and the second in case of problems), then reconstructs the seed. Simple. _[CSR Level 0: 2 of 3 sharding]_

**CSR Use Case #2: A Security Upgrade (Storage Choice, Authentication).** Seven months later, Barney feels a little more comfortable with how the storage of his shares works, so he decides to increase its security. He does so by making a single choice: where to store one of his shares. He chooses Wells Fargo, because they're also his bank. All three of Barney's old shares are deleted, then a set of new shares is generated from his seed. One is kept in his local Cloud, one goes to a still-unspecified share server, and the last goes to Wells Fargo. When Barney stores his share with Wells Fargo, he also verifies his phone number with them. Now, if SimpleSave tries to recover a share from Wells Fargo, it'll first send a code to Barney's phone, which must be entered before the share is released. Not only has Barney improved his control of his seed splitting by making a choice, but he's also improved the safety of his recovery. _[CSR Level 1: 2 of 3 sharding with extra authentication]_

**CSR Use Case #3: Finishing the Job (Multiple Authentication).** Drunk on his new power, Barney also chooses his other share server. He picks the EFF rather than another financial institution to create diversity. EFF could use phone authentication, but different forms of authentication is a core rule for SimpleSave. Instead, Barney links to the EFF through Google Authenticator. Once more, Barney's extant shares are deleted and a new set is sent out. When Barney wants to recover his seed, he now needs to either return a phone code to Wells Fargo or send Google Authenticator's code to the EFF. This means that his digital assets are now protected with full two-factor authentication: one factor is required to log into his cloud account, and a second factor required by either the EFF or Wells Fargo. _[CSR Level 1: 2 of 3 sharding with 2FA]_

**CSR Use Case #4: Moving Up in the (Secret-Sharing) World (Progressive Sharding).** A year on, Barney even understands how the sharding process works, and he comes to the conclusion that a 3-of-5 sharding would offer him better resilience than a 2-of-3 sharding, because he could have two failures and still reconstruct his seed. So he tells SimpleSave to replace his 2-of-3 with a 3-of-5. Barney doesn't have opinions on any other share servers, so SimpleSave selects two randomly — which is fine with Barney because he's already made choices on two, which together with his cloud storage, is the threshold for reconstruction. There's already a 3-of-5 shard sitting with Wells Fargo, with the EFF, and on Barney's cloud server, in preparation for this eventuality, so all that's required is the deletion of the old 2-of-3 shards, and the storage of new 3-of-5 shards with the two new share servers. Each of them registers a new authentication method with Barney: one of them requires a video call, the other thumbprint verification by his mobile device. _[CSR Level 2: 3 of 5 sharding with 2FA and SSKR deck rotation]_

## Secret Sharing Use Cases

This set of use cases more generally demonstrates the ability of crypto-envelope to share secrets of different sorts in progressively more complex ways.

**Secrets Use Case #1: Blowing the Whistle.** Bill has discovered that his employer, Acme Petroleum Company (APC), is dumping wastes into the Gulf of Mexico. He collects proof of this, but is afraid that APC might find that proof, destroy it, and perhaps even make him disappear. So he encrypts the information in a crypto-envelope using his easily acessible public key. It now can't be decrypted except with his private key, which is only available at his house. As long as he can remove the crypto-envelope from APC's offices, he'll be able to securely decrypt it at a later time and send the information to the proper authorities. Meanwhile, APC has no way of either decrypting the information or forcing Bill to do so while he's under their power at his work site. _[public-key cryptography.]_

**Secrets Use Case #2: Do with Your Will.** Alfred has written a will. But he wants to make sure the information on his financial success doesn't get out prior to his death lest he have a constant stream of supposedly impoverished relatives constantly darkening his doorstep. So he decides to store his will in a crypto-envelope. However, Alfred hopes that the day of his demise is far in the future, so he's aware that a crypto-envelope could get lost in the meantime. To protect against that, he encrypts the envelope with a secret-sharing scheme: he actually creates three envelopes, each of which contains the encrypted will as well as one share of the key required to unlock the envelope. Combining the shares from any two envelopes will decrypt the content. He then leaves one envelope in his safe, one with his lawyer, and one with his trusted nephew, Newman. _[2-of-3 sharding.]_

**Secrets Use Case #3: Alfred Returns.** Five years on from his encoding of his will in a 2-of-3 crypto-envelope, Alfred finds that his attorney has passed away and his nephew, Newman, has become a drunken partier. He can no longer trust them to recover his will, and in fact discovers that he can't do so anymore either! So, Alfred writes a new will and again shards it in three parts, keeping one share and giving others to a new attorney and a new nephew. However, he's not going to fall into the same trap again! This time, he makes sure the shares are verifiable. At any point, he can ask his attorney and Newerman to demonstrate their shares and prove that they can unlock the secret — which they can do without the danger of _actually_ recombining the shares! _[verifiable 2-of-3 sharding.]_

**Secrets Use Case #4: Business as Usual.** Nalda works as a professional investor, buying, selling, and holding cryptocurrencies and other digital assets for her clients. To protect her clients, and to meet her insurance requirements, she has to meet certain criteria, including both dual-control of assets (wherein two different people must work together to adminster them) and public verifiability of control (where she must be able to post public proofs that she retains control of those assets). She manages dual-control with crypto-envelopes containing cryptographic seeds that support 2-of-3 sharding. The system also allows her to not only verify that all shares still exist, but also to post a public proof of that — all without actually recombining the shares or publicly revealing any information about the shares or their holders. _[publicly verifiable 2-of-3 sharding.]_

**Secrets Use Case #5: The Water Belongs to the Tribe.** Natasha has come up with a new world-changing technology for purifying water, but she doesn't have the manufacturing capacity to produce her tech yet, so she's just keeping the plans in her head for the moment. However, she's terrified that the technology would be lost forever if she passes away, so she's written out a backup copy of her blueprints, which she keeps encrypted in a crypto-envelope. Unfortunately, she doesn't trust either her business partners or her family not to steal her secrets, and in fact is concerned that her somewhat unscrupulous business partners might be able to bribe a single family member, so she's created a multi-level scheme for sharding her secret. She divides it into nine shares, three of which she gives to family members, three to business partners, and three to escrow agents. Each share is contained in a crypto-envelope that also contains the encrypted blueprints. Four out of nine shares are required to open an envelope, but not any four shares: they must be two each from two different groups. Thus two family members and two business partners, or two escrow agents and two business partners, or two escrow agents and two family members could open the envelope. The possibility of corruption is greatly reduced, and the need for just four of the nine shares should ensure that the blueprints are never lost, even if they need to be recovered years later. _[multi-level 4-of-9 sharding.]_

**Secrets Use Case #6: Bitcoin Gets Backed.** Freddy protects his NFTs and his Bitcoin and Ethereum investments by keeping the seeds to generate their private keys in a crypto-envelope that is openable by two of three shares of a sharded key. Since Freddy holds all three envelopes (and thus all three shares), that gives him self-sovereign control of his digital assets. However, Freddy keeps all of his shares within a few miles of his house, so he's worried that a disaster could wipe them out, especially since he lives in California: earthquake country. To further protect his assets, Freddy also creates a key for social recovery and then splits it out into a multi-level 4-of-9 sharding scheme. He gives out keys to three groups: his friends, his relatives, and his wife's relatives. He's confident that the groups are distant enough that they can't collude, and so the sharded key will only be used if he requests it. Freddy now has 12 copies of his envelope: all of them with his encrypted seeds, 3 with self-sovereign shares and 9 with social-recovery shares. Each envelope also contains multiple permits that can be used for entry: it can be opened by Freddy combining any two of his three self-sovereign crypto-envelopes or by four of the nine social-recovery crypto-envelopes, two each from two different groups. _[multipermit with 2-of sharding and multi-level 4-of-9 sharding.]_

**Secrets Use Case #7: Alfred Forever.** Every six months, Alfred, his young attorney, and his nephew Newerman prove that they have the shares to reconstruct his will, so Alfred feels confident about its longevity. But, he's annoyed by the fact that he has to call up someone whenever he wants to make a change to write out the newest relative that's annoyed him. So, he reconstructs his crypto-envelope with a more sophisticated multi-permit crypto-envelope. This new envelope contains three different permits. The first is locked with his verifiable sharded key that can be unlocked by combining any two of three shares. The second is locked with his public key and can be unlocked with his private key. The third is an adapter signature that allows the envelope to be opened with no key if an oracle determines that Alfred has died, based on finding at least two obituaries published in newspapers of record. Though Alfred's original setup gave him some resilience, and though his second one improved that, this final one serves him best because it allows his envelope to be unlocked by different people for different reasons: including himself to consult his will (using his private key); the general public if he has died (using the adapter signature and oracle); or his attorney and nephew if something more unusual has happened, such as Alfred disappearing (using their key shares, which can be verified whenever desired). _[multipermit with public-key cryptography, verifiable 2-of-3 sharding, and adapter signature with oracle.]_

## Root of Trust Use Cases

This set of use cases depict a very different use of crypto-envelopes, to create secure and provable hardware.

**RoT Use Case #1: Taking Control.** Ali buys a hardware wallet from Bedrock Machinery. After verifying the supply-chain attestation, she replaces the default code-signing signatures needed to update the firmware with ones from the seed in her own crypto-envelope. She's now responsible for an updates to the firmware; updates coming in from Bedrock Machinery will be rejected. She can also now prove that the hardware is hers. _[public-key cryptography.]_

**RoT Use Case #2: Working Together.** Ali has founded Crypto-Currents Inc (CCI) and is now working with a half-dozen other crypto-entrepreneurs. They create a set of crypto-envelopes to lay the foundation for the root of trust of their organization, replacing the default signatures on their wallet devices. Due to the crypto-envelope's multi-permit system they can do multiple things with their envelopes. First, any one of them can prove CCI's ownership of any of their devices. Second, any two of them can update the firmware on any device. _[multipermit with public-key cryptography and 2-of-n sharding.]_

**RoT Use Case #3: Take It to the Limit.** World Holdings ISP (WHISP) has a few thousand machines spread across Europe and Asia. Each individual machine holds a crypto-envelope with a sharded secret that's a bare fraction of the overall key. Without even having to reform the key, each machine can verify that they still hold that small share, and thus that they're a trusted member of the network. This creates a root-of-trust that encompasses the entirety of WHISP's network. _[verifiable m-of-n sharding at a massive scale.]_

## Data Store Use Cases

This set of use cases looks at the creation of data stores.

**Data Store Use Case #1: Replacing Facebook.** James has come to the conclusion that Facebook is evil because of its amplification of political lies. However, thanks to the GDPR and CCPA, social-media companies have begun to forced means to download user data. James does so, storing it in an encrypted data store. The data store is encrypted with a symmetric key that is then sharded, with the whole packet of data stored in three places, each containing the encrypted data and one share of the key. _[2-of-3 sharding]_

**Data Store Use Case #2: Sharing Facebook.** Ansel ran a Facebook group for photographers before he decided that he needed to make a break from the corporation. He downloaded the Group and stored it in a crypto-envelope locked with his public key. However, he also wanted to share the data with his co-admin Annie. He creates a second permit on the data envelope that is locked with Annie's public key. Now either of them can access the data. _[multipermit with multiple public-key cryptography.]_

**Data Store Use Case #3: Recreating the Group.** After Bruce downloaded his Batman Fans Facebook group, he decided that he wanted to recreate it as an independent platform. Though the data was originally locked with a 2-of-3 sharding of a symmetric key, he creates a second permit that he locks with a private key whose public key is widely distributed. Only Bruce can update the group by signing it with the symmetric key, but anyone can read it using the public key. _[multipermit with separate write and read permissions based on public-key cryptography and a symmetric key.]_

## Collaborative Use Cases

This set of use cases demonstrate the use of crypto-envelope in collaboration, and thus touches upon the different permissions possible in the system. It's again presented with progressive complexity.

**Collaborative Use Case #1: Consecutive Chronicling.** Jan and Guy are working on the final novel in their 27-book series, and they're worried about its secrets getting out. So Jan, who does the initial drafts, encrypts each chapter in a crypto-envelope with Guy's public key before sending it to Guy, who can than decrypt it with his private key. _[public-key cryptography]_

**Collaborative Use Case #2: Almost Famous.** Stan and John have produced a script for Super-Patriot Man, which they think will totally reinvent super hero movies. To protect it from theft until they're able to sell to a studio, Stan and John have encrypted the script in a crypto-envelope which is openable with both shares of a sharded key. Jack has a crypto-envelope containing the encrypted script and one shard and Stan the other. The crypto-envelope also contains a timestamp which can be used to prove the date that they encrypted the script, in case any proof of provenance is  needed — and it often is in the ever-litigious entertainment industry. _[2-of-2 sharding with timestamp.]_

**Collaborative Use Case #3: The Gallery Group.** Artist Georgia is working with the Nifty NFT Gallery (NNG) to display her works and make them available for sale. She places the seeds and metadata for her NFTs in a crypto-envelope that is locked with two different permits: one based on her public key, the other based on NNG's public key. Either of them can retrieve the NFTs: Georgia can choose to remove them for sale, or NNG can transfer them to a new owner after a purchase. _[multipermit with multiple public-key cryptography.]_

**Collaborative Use Case #4: The Comic Crew.** Writer-artist Jack is working on _The FORTH World_, a comic book about programming. As he completes his pages, he stores them in a crypto-envelope that he has full read-write permissions to. He's thus able to update it as he creates new pages. Though he'll be publishing a book down the road, for now he's sharing his work on Patreon: all of his patrons there have read-only access to the crypto-envelope. They can look at the pages, but they can't update the content. These permissions are managed through two different keys: Jack's own private key and a separate key that he posts to his Patreon for his patrons' read access. _[multipermit with separate write and read permissions based on public-key cryptography and a symmetric key.]_

**Collaborative Use Case #5: The Movie Mob.** A large crew creates _For Whom the Dell Tolls_, a farming action-adventure movie, but is still looking for a studio to distribute it. The rights for the movie are locked up on a rights-management blockchain using collectively generated keys and then the seeds and metadata for those keys are put into a crypto-envelope. The envelope is locked with two different permits. One is a 3 of 5 secret-share that allows any three of the five executive producers to transfer the rights. The other is a 80 of 100 secret-share that allows any 80 of the crew members (including the executive producers) to transfer the rights. Each crypto-envelope contains the encrypted keys; crew member envelopes also contain one of the crew shares while executive envelopes contain both a crew share and an executive share. A majority of producers will usually make a decision about selling the movie, but a super-majority of the crew can also do so if they don't agree with what the producers are doing — all without involving courts! _[CKM. collaborative key generation plus multipermit with 3-of-5 sharding and 80-of-100 sharding.]_

## Amira Use Cases

This final set of use cases reimagines important turning points in the [Amira Engagement Model](https://github.com/WebOfTrustInfo/rwot5-boston/blob/master/final-documents/amira.pdf) as use cases for crypto-envelope. This is again presented progressively, but in service to the Amira story.

**Amira Use Case #1: Taking a RISK.** Amira decides to joing RISK, a pseudonymous programming reputation network, to work on projects for the public good. When she joins the network, she creates a DID identifier using a new keypair and then uses that RISK private key to sign the identifier and RISK's Terms of Engagement. She stores the seed for the RISK private key and the Terms of Agreement she signed in a crypto-envelope, which she locks with a separate symmetric key. She can later access it, and thus the RISK network, by using that symmetric key. _[symmetric key with metadata storage.]_

**Amira Use Case #2: Reducing the RISK Risks.** After working a few projects, Amira has received some positive reviews that are linked to her RISK DID. She decides that she needs to protect it better, so she creates a new crypto-envelope, which contains the DID identifier, her RISK seed, and now the reviews as well. This time, she locks it with a key that is sharded using 2-of-3 secret sharing. She keeps one envelope on her phone and another on her computer, to allow for regular usage of the info. She stores the third one at her work, as a replacement in case either of her actively used keys is lost. She can now access her RISK information by using any two envelopes, each of which contains the encrypted seed and metadata as well as one of the shares of the symmetric key locking it. She doesn't even have to worry about keeping that extra envelope at work, which would not approve of her moonlighting, because the information is entirely hidden. _[2-of-3 sharding with metadata storage.]_

**Amira Use Case #3: Amira on the Skids.** Unfortunately, that doesn't protect Amira from being compromised by a man-in-the-middle attack, which results in her RISK seed and thus RISK DID being lost. She was targeted because of the public posting of her old DID, and so she decides not to make that mistake again. Instead, she generates a SCID, a self-certifying identifier based on her crypto-envelope, and thus not registered with any public blockchain. When she reconnects with the RISK system using her new SCID, she then stores all of her new info (including her new seed, new terms, and new reviews) in a new crypto-envelope, which also maintains her SCID and is again locked with 2-of-3 secret sharing. _[2-of-3 sharding with metadata storage and SCID.]_

**Amira Use Case #4: Beyond RISK!** Amira is able to reestablish her presence on RISK, and soon discovers a wider world via the CommonX programming and reputation network. She creates a new seed to access CommonX, and signs her CommonX public key with her RISK SCID. She is able to simply add her new seed and the cryptographic signature to her crypto-envelope, and it too becomes protected by her 2-of-3 secret-sharing. _[2-of-3 sharding with metadata storage update and SCID.]_

**Amira Use Case #5: Back to the Grind.** Amira decides to concentrate more on her public career, so she retires her accounts on RISK and CommonX. However, she needs to transfer control of the SisterSpace app that she wrote for one of her clients. This currently depends on the seed linked to her SCID, which she doesn't want to give away as it also controls an identity. Amira works with new programmer Firefly to collaboratively generate a keypair that neither of them has initial access to. It's placed in a crypto-envelope with two permits: one allows Firefly to open it with their private key and another connects to an oracle that will allow Amira to open it if the current version of the app has not been updated for 180 days or more. Amira then uses her old SCID keypair to sign permissions over to the new public key generated by the CKM. She can be confident that her program is in good hands — and that she can take control if it isn't. Meanwhile, Firefly can be confident that they won't have any contention in updating the app, unless they abandon it. _[CKM. collaborative key generation plus multipermit with public-key cryptography and adapter signature with oracle.]_

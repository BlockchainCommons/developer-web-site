_Much of Blockchain Commons' work is focused on providing users with secure & resilient protection of their cryptographi secrets._

* **Aymmetric Keys.** Private and public keys are generated from seeds using specific derivation paths. They enable encryption and signing.
 
* **Seeds.** Seeds are the foundation of digital-asset security, as they're used to generate signing keys and auth keys, often in a hierarchical manner. URs allow the encoding of seeds as `ur:seed`, but Envelopes are now preferred because they can store metadata and also allow for encryption or elision. Blockchain Commons' [seedtool-cli-rust](https://github.com/BlockchainCommons/seedtool-cli-rust) and [Seedtool for iOS](https://github.com/BlockchainCommons/GordianSeedTool-iOS) offer best practices for the management of seeds.
    * See our [128-bit test seed](/seed-128/)
    * See our [256-bit test seed](/seed-256/)
    * See [Seeds in envelopes](/envelope/seed/)

* **Entropy.** The randomness used to create seeds is Entropy.

_Cryptographic secrets can be stored in one of two ways: as simple text-based URs, or as more descriptive Envelopes._

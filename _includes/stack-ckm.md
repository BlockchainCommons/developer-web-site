_The CKM layer is an area for future development. It will shift from the CSR model of creating a key and then splitting it to the CKM model of keeping a key split at all times._

* **Collaborative Key Management (CKM).** A next-generation key-management solution, CKM uses Secure Multi Party Computation (SMPC) to keep a key separated, rather than having it all in one place.  Currently, ZF FROST is the only production-ready implemention of CKM, so it's likely CKM and FROST will go hand in hand.
   * See our [CKM page](/ckm/) 

* **Distributed Key Generation (DKG).** A cryptographic process where portions of a key are kept separate in separate locations, never combining them until necessary.
 
* **FROST.** FROST is a Schnorr-based signature system that offers more robust multisigs. It also has the advantage of optionally integrating Distributed Key Generation (DKG), where portions of a key are held by different devices.
   * See our [FROST page](/frost/)
 
* **MuSIG2.** MuSIG 2 is another Schnorr-based signature method for creating multisigs. Although it doesn't support threshold signing like FROST does, it instead offers strong accountability.
   * See our [MuSIG2 page](/musig/)


_Digital-asset storage uses Gordian Envelope, Blockchain Commons' smart document system, which allows for hashed data elision, encryption, and much more._

* **Envelope.** Gordian Envelopes are built on URs, Blockchain Commons' foundational plain-text encoding system, and can be output as `ur:envelope`. However, Envelopes have much more functionality than simple UR text strings due to their recursive storage capabilities. They also support the new capability of hashed elision, which allows for provable redaction of data in support of [data minimization](https://www.blockchaincommons.com/musings/musings-data-minimization/). Extensions support even more functionality.
   * See our [Envelope page](/envelope/)

* **Seeds & SSKR.** Seeds and SSKR shares can be stored in Envelopes. This goes beyond simpler storage techniques such as `ur:seed` and `ur:sskr` because it can include metadata to help identify a seed and also allows the use of Envelope functionality such as elision and encryption.

* **Other Envelope Extensions.** Other Envelope extensions include existence proofs, encryption, signatures, compression, salting, known values, attachments, diffing & merging, and expressions. Envelope's features can be used or not as a developer prefers: _you only pay for what you use_. 

### <font color="#ffac1c">Envelope's</font> Expressions & Communication

_Envelope's Expressions are an extension that enables secure communication._

**Expressions.** Expressions are Envelope functions. They are a specified way to request that a specific action be undertaken when an Evelope is accessed.  
   * See [Expressions Research Paper](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-012-envelope-expression.md)
   
**Request/Response.** Request/Response packages up Expressions so that they can be sent to a remote recipient as a Request, who can then send a Response tied to the original request. This allows for interoperable communication using Envelopes when further security is not required.
   * See our [Envelope Request & Response page](https://developer.blockchaincommons.com/envelope/request/).

**Gordian Sealed Transaction Protocol (GSTP).** GSTP builds on top of Request/Response for those times when security <i>is</i> required. It not only supports encryption and signatures but also uniquely supports Encryption State Continuations (ESC) to securely encapsulate state information. It's used by the Gordian Depository as part of CSR.
   * See our [GSTP Research Paper](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-014-gstp.md).
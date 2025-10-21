
_Digital-asset storage uses Gordian Envelope, Blockchain Commons' smart document system, which allows for hashed data elision, encryption, and much more._

* **Envelope.** Gordian Envelope is a binary storage format built in CBOR that can be encoded with URs, Blockchain Commons' foundational plain-text encoding system, as `ur:envelope`. They're built around recursive storage and support for hashed elision, which allows for provable redaction of data to allow [data minimization](https://www.blockchaincommons.com/musings/musings-data-minimization/). Extensions support even more functionality.
   * See our [Envelope page](/envelope/)

* **Permits.** Envelope content can be encrypted. Permits can allow that content to be decrypt in a variety of ways, including symmetric keys, private keys, streched passwords, and SSKR shares.
   * See our [Permits page](/envelope/permits/)
 
* **Seeds & SSKR.** Seeds and SSKR shares can be stored in envelopes. This goes beyond simpler storage techniques such as `ur:seed` and `ur:sskr` because it can include metadata to help identify a seed and also allows the use of Gordian Envelope functionality such as elision and encryption.
   * See our [Seeds in envelopes page](/envelope/seed/)
    
* **Other Envelope Extensions.** Other Gordian Envelope extensions include existence proofs, encryption, signatures, compression, salting, known values, attachments, diffing & merging, and expressions. Envelope's features can be used or not as a developer prefers: _you only pay for what you use_. 

### <font color="#ffac1c">Envelope's</font> Expressions & Communication

_Envelope's Expressions are an extension that enables secure communication._

**Expressions.** Expressions are Envelope functions. They are a specified way to request that a specific action be undertaken when an Evelope is accessed.  
   * See [Expressions Research Paper](https://github.com/BlockchainCommons/Research/blob/master/papers/bcr-2023-012-envelope-expression.md)
   
**Request/Response.** Request/Response packages up Expressions so that they can be sent to a remote recipient as a Request, who can then send a Response tied to the original request. This allows for interoperable communication using envelopes when further security is not required.
   * See our [Envelope Request & Response page](https://developer.blockchaincommons.com/envelope/request/).

**Gordian Sealed Transaction Protocol (GSTP).** GSTP builds on top of Request/Response for those times when security <i>is</i> required. It not only supports encryption and signatures but also uniquely supports Encrypted State Continuations (ESC) to securely encapsulate state information. It's used by the Gordian Depository as part of CSR.
   * See our [GSTP page](/envelope/gstp/).

**Encrypted State Continuation (ESC).** ESC is a feature of GSTP that allows for the storage of local state in a secure way as part of the communication messages. It supports constrained environments, load-balanced servers, time-differentiated environments, and more.
  * See our [ESC page](/envelope/esc/).

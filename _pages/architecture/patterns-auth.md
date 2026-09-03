---
cover: false
header:
  overlay_color: "#000"
  overlay_filter: "0.35"
  overlay_image: /assets/headers/architecture.jpg
  og_image: /assets/images/bc-card.jpg
title: Authentication Design Patterns
tagline: Based on the Heterogeneuity Principle
hide_description: true
classes:
  - wide
permalink: /architecture/patterns/auth/
sidebar:
  nav:
    - archdesign
    - architecture
---

_This document is intended to reveal a bit about how Blockchain
Commons thinks about architectural design and thus how it constructs
its scenarios and other studies. However, it's just a tiny corner of
the overall security design space. To be specific, it's focused on the
intersection of Authentication and Authorization with the
[Heterogeneity design principles](/architecture/design/).  Many other
types of [design patterns](/architecture/patterns/) are relevant in the security space and could
complement this work._

# An Overview of Hetergenity

_Heterogeneity_ is a architectural principle that says: 

> _**Security is improved when things are different from each other**_. 

This document looks at the Heterogeneity principle through a single
lens: Authentication. It not only examines specific Authentication
patterns that support heterogeneity (especially [Multifactor
Authentication](#multifactor-authentication), [Multichannel
Authentication](#multichannel-authentication), and [Threshold
Authentication](#threshold-authentication)) but it expands that into a
complete look at the design patterns underlying Authentication and
Authorization, to create a larger blueprint for how to make these
systems secure.

## An Overview of Authentication & Authorization

Authentication and Authorization are both included in this document in
part because they're often confused with each other. It's not just
that they're very similar words, but also that their concepts are
tightly intertwined.

Our definitions of the terms are essentially as follows:

* **Authentication.** Prove access to a specific identity, group, or service.
* **Authorization.** Be granted specific capabilities or permissions.

Typically, an Authentication grants a specific level of Authorization,
which is a pattern called the [Authorization-Authentication
Link](#authentication-link). However, there's a lot more complexity
than is apparent either in these definitions or their
interconnections.

_What does Authentication really prove?_ Only that a user holds the
proof of Authentication. That might be a pretty tight connection to a
person when it's biometrics (maybe!), but less so when Authentication
depends on a secret or token.

_Who has the right to Authorization?_ Is it the person originally granted it? Is it the holder of a specific secret or token? Is it anyone who can Authenticate?

These are some of the questions that must be considered, even when picking and using design patterns from this document.

## Authentication Design Patterns ToC

Patterns are organized into the following groups:

* [**Core.**](#core-authentication-patterns) The fundamental patterns of Authentication.
* [**Heterogeneity.**](#authentication-heterogeneity-patterns) Improving Authentication through variation.
* [**Method.**](#authentication-method-patterns) How Authentication is done.
* [**Protection.**](#authentication-protection-patterns) Improving the security of Authentication.
* [**Storage.**](#authentication-storage-patterns) When/how Authentication validation is stored.
* [**Subject.**](#authentication-subject-patterns) Who/what is being Authenticated.
* [**Authorization.**](#authorization-patterns) The linked system of Authorization.


Specific patterns are:

| Category | Pattern | Summary |
|-----------|-----------|---------|
| Core | [Authentication](#authentication) | prove who you are
| Core | [Authentication Levels](#authentication-levels) | progressively authenticate | 
| Core | [Authentication Proof](#authentication-proof) | prove authentication |
| Heterogeneity | [Multifactor Authentication](#multifactor-authentication) | multiply authenticate |
| Heterogeneity | [Multichannel Authentication](#multichannel-authentication) | separately authenticate |
| Heterogeneity | [Threshold Authentication](#threshold-authentication) | m of n authenticate
| Method | [Biometric Authentication](#biometric-authentication) | auth physically |
| Method | [Control Authentication](#control-authentication) | auth second-hand
| Method | [Secret Authentication](#secret-authentication) | auth with password
| Method | [Token Authentication](#token-authentication) | auth with device
| Protection | [Authentication Delay](#authentication-delay) | slowly authenticate |
| Protection | [In-Channel Warning](#in-channel-warning) | warn in app |
| Protection | [Out-of-Channel Warning](#out-of-channel-warning) | warn out of app |
| Protection | [Usage Analysis](#usage-analysis) | verify auth |
| Storage | [Hashed Authentication Storage](#hashed-authentication-storage) | store hashes |
| Storage | [Local Authentication Storage](#local-authentication-storage) | store locally |
| Subject | [Anonymous Validation](#anonymous-validation) | no identity |
| Subject | [Identity Validation](#identity-validation) | real identity |
| Subject | [Pseudonymous Validation](#pseudonymous-validation) | pseudo identity |
| Authorization | [Authorization-Authentication Link](#authorization-authentication-link) | who can do what |
| Authorization | [Authorization](#authorization) | what you can do |
| Authorization | [Authorization Levels](#authorization-levels) | progressively authorize

## Core Authentication Patterns

_The Core Authentication patterns define the fundamental design of an Authentication system._

### Authentication

> **Problems:**
>
> * Digital users can be anonymous.
> * Authorization can be dangerous.
>
> **Solution:**
>
> * Require users to prove who they are; and/or
> * Require users to prove they have access to some secret, service, or token.
>
> **Related Patterns:**
>
> * Authentication Levels, Authorization-Authentication Link,  Authorization
> * 
>
> **Exemplary Uses:**
>
> * Faceprint Unlock
> * FIDO Security Key
> * Password

Authentication is required when a system contains information that is sensitive or that could be dangerous if it was released, or when system functions could be used to ill effect if they were widely available. More generally, Authentication might be required simply because someone wants privacy in a specific location. By verifying Authentication, a system can then grant the **Authorization** that is required to access that data, those services, or that private information.

Implementing Authentication requires creating an access control. A user must by some Method prove who they are (or at least prove that they've been granted access through a secret or token) and that Authentication must then be linked to some Subject, after which an Authorization system determines what they can do on the system.

In Gordian Seed Tool, Authentication is required whenever a user accesses private data such as seeds or private keys. To do so, the user must provide a password or biometric info (depending on their hardware's capabilities). The same is not true for less sensitive information, such as public keys. 

> **Advantages:**
> * **Data or Access Protected.** Only Authenticated users have access.
> * **Accountability Possible.** With logging, you can see who did what.
>
> **Disadvantages:**
> * **Loss of Access Possible.** If Authentication fails, access fails, suggesting the needs for Threshold Authentication.
> * **Privacy Threats.** There may be permanent records associated with identifiers.

### Authentication Levels

> **Problems:**
>
> * Authentication methods may be either weak or strong.
> * A system may have multiple Authorization Levels.
>
> **Solution:**
>
> * Support increasing levels of Authentication.
> 
> **Related Patterns:**
>
> * Authentication, Authorization Levels, Biometric Authentication, Control Authentication, Secret Authentication, Token Authentication
>
> **Exemplary Uses:**
>
> * 2FA (Two-Factor Authentication) only for first access from a new machine.
> * 2FA only for password changes.

Access to system functions and data is not monolithic. Some data may be public, some data may be private, and some data may be higher security. This model is clearly displayed in multi-user OSes such as UNIX, which typically can vary access between public, group-only, user-only, and private (root-only). Systems need to allow for increasing Authentication Levels to support these increasingly levels of privacy, access, or authorization.

A poor man's Authentication Level system requires more sensitive passwords to be used to gain additional access on a system. The root password on a UNIX system offers an example of this. A better Authentication Level system provides increasing levels of actual **Authentication**, each of which provides better proof that the person is who they say they are. For example, a system might require a password for system access but then send an email to verify a sensitive action and further require verification from a friend to allow a super sensitive action.

Gordian Seed Tool provides two main Authentication Levels. The basic level of access depends on the Authentication built into the device, which minimally includes access to the device (but might also include **Biometric Authentication** or **Secret Authentication**). A second Authentication Level then occurs whenever a user wants to access private information: they must provide their password or biometric proof depending on the capabilities of their device. What's basically a third Authentication Level is required if a user wants to restore Gordian Seed Tool information to a new device: they must Authenticate themselves to Apple by providing the password of their Apple account and/or proof from a previous device.

> **Advantages:**
> * **Improved Security.** More Authentication makes services and data more secure.
> * **Stratification of Permissions.** With multiple levels of Authentication, multiple levels of Authorization are also possible.
>
> **Disadvantages:**
> * **Single Points of Failures Multiply.** Multiplying access requirements multiplies the ways that access could be lost.

### Authentication Proof

> **Problems:**
> 
> * Local system is incapable of Authentication.
> * Authentication happened on a remote system.
> * Authentication validation information is considered very sensitive.
>
> **Solution:**
>
> * Authenticating system sends proof of Authentication to other systems.
>
> **Related Patterns:**
>
> * Authentication, Control Authentication, Local Authentication Storage
>
> **Exemplary Uses:**
>
> * Some federated logins

**Authentication** systems aren't necessarily monoliths. There is the possibility for one system to validate Authentication for another system that needs the Authentication. There may be any number of reasons for this, including limited trust of low capabilities for the system requiring Authentication, high sensitivity of the Authenticating information, or high cost of Authentication.

The solution is to have one system Authenticate and then pass on a proof of that Authentication to another system. Theoretically, this system can work if the systems just trust each other, but a trustless solution is better: the Authenticating system might pass a Zero-Knowledge Proof to the other system or sign something with a previously validated private key or prove knowledge of something, possibly through a hashed and elided piece of data.

The Gordian Architure is built to allow for this sort of pattern. It suggests each service within the architecture has very constrained capabilities. Having one member in the ecosystem do Authentication then provide proof to others across Tor Gaps fits right in with the design.

> **Advantages:**
> * **Partitioning.** This is effectively another partitioning pattern, as it separates Authentication and service, and thus creates many of the benefits of other partitioning patterns. 
> * **Constrainted System Support.** This pattern can support very constrained systems that are simply unable to provide sufficient Authentication for some reason.
> 
> **Disadvantages:**
> * **Requires Some Trust.** Though the goal of an Authentication Proof system is trustlessness, it's likely that there's ultimately going to be some kernel of Trust involved, even if it's just a trust to protect against correlation.

## Authentication Heterogeneity Patterns

_The Authentication Heterogeneity patterns are where partitioning comes into an Authentication system, whether it be variety or variety **plus** separation._

### Multifactor Authentication

> **Problems:**
> 
> * Authentication methods are hackable.
> * Authentication methods are weak.
> * Multiple people want to Authenticate together.
> * Multiple devices are required to Authenticate together.
>
> **Solution:**
>
> * Require two or more successful methods for Authentication.
>
> **Related Patterns:**
>
> * Authentication, Multichannel Authentication, Threshold Authentication
>
> **Exemplary Uses:**
> 
> * Two-Factor Authentication (2FA)
> * Collaborative Seed Recovery (CSR)

**Authentication** sometimes isn't enough. To avoid Single Points of Compromise (SPOCs), you instead want at least two methods of Authentication before you give someone access to a system or data.

Two-factor Authentication (or 2FA) is the prime way to provide Multifactor Authentication of this sort: a user is asked to authenticate their identity through two separate means, most often a password and ownership of a device (such as a phone that receives a message or a device that runs Google Authenticator or another 2FA program), but sometimes through a password and a biometric measure (such as a fingerprint or facial recognition).

In the Gordian architecture, Multifactor Authentication is suggested as a best practice for [Collaborative Seed Recovery](https://developer.blockchaincommons.com/csr/) and the reference app [Gordian Depo](https://github.com/BlockchainCommons/bc-depo-rust). A user will typically have to query two or more CSR servers to reconstruct their seed: it's suggested than each have a separate **Authentication** method.

> **Advantages:**
> * **No Single Point of Compromise.** If at least two separate Authentication methods are required, then security is increased.
> * **Potential Loss Improvements.** Allowing multiple Authentication methods offers the possibility of also incorporating Threshold Authentication to reduce the chance of loss.
> * **No Honey Pots.** Provided that the different factors of Authentication are separated, there is no honey pot that combines together a large amount of information about the user.
>
> **Disadvantages:**
> * **Loss without Thresholds.** Without a Threshold Authentication pattern, each new type of Authentication increases the opportunity for loss.
> * **Correlation Threats.** If all the varied Authentication methods are combined on one server, that's obviously a major overidentification threat.

### Multichannel Authentication

> **Problems:**
> 
> * Multifactor Authentication methods are the same.
> * Multifactor Authentication methods can be hacked in the same way.
> * Multifactor Authentication methods use the same channels.
>
> **Solution:**
>
> * Require Multifactor Authentication where Authentication methods are entirely discrete. 
>
> **Related Patterns:**
>
> * Authentication, Biometric Authentication, Control Authentication, Multifactor Authentication, Secret Authentication, Threshold Authentication, Token Authentication
>
> **Exemplary Uses:**
>
> * 2FA: require a password, then show control of device with Authenticator
> * 2FA: require a password with FIDO token control

**Multifactor Authentication** is only as powerful as the real separation of its **Authentication** methods. If Authentication occurs through multiple services, but the same method of Authentication is used by all of them, that's little better than singular Authentication (and could be worse, as suddenly multiple services have Authentication information). 

However, the problem can be more insidious. If one Authentication method can be used to override or change another Authentication method, then the security improvements are again limited. Notably, email addresses can often be used to change passwords, which means that Authenticating an email address and a password doesn't usually provide true Multifactor Authentication, it just causes the Authentication method to be the stronger of the two (which is likely demonstrating control of the email account). Similarly, Authenticating via two different systems that are each controlled by an email offers limited variety, even when the email isn't explicitly involved as an Authentication system.

Multichannel Authentication is implemented by ensuring that all methods of Multifactor Authentication are truly separated from each other. Drawing from separate Authentication Method Patterns is a good way to prioritize separation. For example, requiring **Token Authentication** [via an SMS to a phone], **Biometric Authentication** [via a faceprint], and **Control Authentication** [via proof of ownership of a web site] could ensure Multichannel Authentication. Conside the fact that channels can also affect the security level: some transit channels such as email or SMS have weaker security than demonstrating control of a hardware device, whether that be a FIDO token or a mobile device with Google Authenticator installed. 

This is the best practice for use of Gordian Depos to shard secrets: each depository should have not just a distinct authentication method, but an unconnected authentication method. 

> **Advantages:**
> * **No False Security.** 2FAs can offer false security if they all devolve to the same factor.
>
> **Disadvantages:**
> * **Single Points of Failures Multiply.** As with any sort of Multifactor Authentication, more points of Authentication means more points of failure.
> * **Correlation Threats.** Different types of authentication might be correlatable, even if they're separated on different servers through Multifactor Authentication.

### Threshold Authentication

> **Problems:**
> 
> * Singular Authentication methods can be lost.
> * Use of Multifactor Authentication can increase Single Points of Failure.
>
> **Solution:**
>
> * Define `n` Authentication methods and require `m` of them for successful authentication, where `m<n`.
>
> **Related Patterns:**
>
> * Authentication, Authentication Levels, Multichannel Authentication, Multifactor Authentication
>
> **Exemplary Uses:**
>
> * Multisigs
> * Shamir's Secret Sharing Reconstruction

When **Multifactor Authentication** has been adopted, each individual Authentication can become a Single Point of Failure. If any Authentication is lost, access is lost.

The solution is to use Threshold Authentication, where `n` different **Authentication** methods are defined, but only `m` of them are necessary to complete Authentication, where `m<n`. 

This is the fundamental methodology that Blockchain Commons suggests for [Smart Custody](https://www.smartcustody.com/).  For now we suggest Shamir's Secret Sharing, which allows reconstruction of lost secrets with Threshold Authentication defined by holding the shares. In the future, when ease-of-use is improved, we suggest multisigs with Threshold Authentication defined by holding private keys. Longer term, we hope to see [Collaborative Key Management](https://developer.blockchaincommons.com/ckm/) as a next-generation Threshold Authentication system.

**Advantages:**

* **Reduced Single Points of Failure.** SPOFs are reduced. As long as `m<n`, an Authentication method can be lost without losing access to the resource.
* **Potentially Reduced Single Points of Compromise.** Most Threshold Authentication systems also reduce SPOCs, by requiring more than one Authentication method to provide Authentication.

**Disadvantages:**

* **Complexity.** Threshold Authentication systems can have increased complexity. Our current [Multisig Self-Custody Scenario](https://github.com/BlockchainCommons/SmartCustody/blob/master/Docs/Scenario-Multisig.md) is complex enough that it puts off potential users.
* **Potential for Third-Party Corruption.** If you hold all your own Authentication, it'll only be used in a way you want. But if you have a Threshold Authentication scenario where other people hold some of your Authentication, they could be corrupted. (The solution isto ensure that third-parties never hold enough Authentication to access your system.)

## Authentication Method Patterns

_Authentication Methods are ways that Authentication can be validated._

### Biometric Authentication

> **Problems:**
> 
> * Authentication can't be loseable.
> * Authentication needs to link to personal identity.
>
> **Solution:**
>
> * Require Authentication via biometric means such as thumbprint, faceprint, voiceprint, etc.
>
> **Related Patterns:**
>
> * Authentication, Control Authentication, Hashed Authentication Storage, Identity Validation, Secret Authentication, Token Authentication
>
> **Exemplary Uses:**
>
> * Thumbprint login for Mac computer
> * Faceprint login for mobile device
> * Faceprint login for modern airport/passport-control systems

Biometric Authentication might be needed for either the most personal types of **Authentication** or the ones where it's the most important to not lose the ability to authenticate.

It's usually implemented by taking a scan of a faceprint or thumbprint. There are severe privacy concerns here, so biometric records should always support **Local Authentication Storage** and never be networked. See also the ["Six Principles for Self-Sovereign Biometrics"](https://github.com/WebOfTrustInfo/rwot6-santabarbara/blob/master/draft-documents/Biometrics.md).

Gordian Seed Tool uses the Biometric Authentication built into Apple iOS to require a faceprint or thumbprint to access sensitive information.

> **Advantages:**
> * **No Single Point of Failure.** Except in pretty extreme situations, you'll never lose your biometric information.
>
> **Disadvantages:**
> * **Privacy Threatening.** You can never (practically) change your biometric information, so if it's compromised, you're out of luck. This makes privacy protections extremely important, and often biometric info has not been well protected. 
> * **Reliability Issues.** Biometric reliability has generally risen to somewhere in the 98-99.8% range, but the chance of unreliability still seems quite high for an Authentication mechanism, and there appear to still be worse problems for [some demographics](https://sitn.hms.harvard.edu/flash/2020/racial-discrimination-in-face-recognition-technology/). The masking requirements of COVID demonstrated another issue with biometric reliability.
> * **Inheritance Issues.** Biometric information is not currently available to heirs, which can cause inheritance problems.
 
### Control Authentication

> **Problems:**
>
> * Authentication happened on a remote system.
> * Authentication needs to link to some third-party identifier.
> * Proof is required of control of a third-party service.
>
> **Solution:**
>
> * Require Authentication via demonstration of control of a third-party identifier or service.
>
> **Related Patterns:**
>
> * Authentication, Authentication Proof, Biometric Authentication, Pseudonymous Validation, Secret Authentication, Token Authentication
>
> **Exemplary Uses:**
>
> * Let's Encrypt proof of website ownership
> * Creation of SPF, DMARC, DKIM records in DNS
> * Login via a federated account
> * Emailed access code

Control Authentication is used when you either want to kick the issue of **Authentication** to someone else, or when you want Authentication to be based on proof of ownership (access) to some other account or service, or when you just need a second factor for **Multifactor Authentication**. Control Authentication is not the same as **Token Authentication**, which typically requires proof of ownership of some closely held device.

Control Authentication is most simply managed by responding to a private message sent to a service such as an email. Alternatively a federated login may grant **Authentication Proof** for that service. Services without message receipt or public Authentication methods can support Control Authentication through the creation of some publicly viewable data from within an account, such as a TXT record in DNS or a specific `/.well-known/` record on a website. 

Signing a public key with a private key is another way to demonstrate control of something and is suggested by Blockchain Commons as a best practice for code signing.

> **Advantages:**
> * **Directly Linked Services & Auth.** One of the purposes of Control Authentication is to prove access to a service in order to help manage that service, such as when you're requesting a TLS cert for a site that you control. Doing so is more direct than proving that you're linked to a identity and then proving that your identity has control over a service.
> * **Less Likely Single Point of Failure.** Access to services or sites is usually backed up in some way, and thus there's less likelihood for it to be a SPOF than with something like a cryptographic key or even a hardware device.
> * **Distributed Authentication Requirements.** Passing off Authentication services to another site might allow for the use of a much more powerful Authentication method than you could afford yourself.
>
> **Disadvantages:**
> * **Complex.** Many Control Authentication methods are complex, as demonstrated by the complicated grammar of DKIM, DMARC, and SPF records.
> * **Privacy Threats.** Control Authentication is often public, as the authenticator creates a record that anyone can see.
> * **Third-Party Trust.** Trust is required for whatever third party is doing the Authentication. This is less of an issue when it's self-sovereign, such as showing ownership of a domain or a website, and more of an issue when it's not, such as a federated login.

### Secret Authentication

> **Problems:**
> 
> * Authentication needs to be simple.
> * Authentication needs to be portable.
>
> **Solution:**
>
> * Allow authentication with revelation of a specific secret.
>
> **Related Patterns:**
>
> * Authentication, Biometric Authentication, Control Authentication, Hashed Authentication Storage, Token Authentication
>
> **Exemplary Uses:**
>
> * Account Password
> * Private Key

Secret Authentication is perhaps the most traditional form of **Authentication** on computer systems, where nothing more than a password (or other secret) is required. It's important mainly for its ease-of-use. You don't need to go through the more convoluted requirements of **Control Authentication** or **Token Authentication**, you just remember a password and off you go.

The implementation of Secret Authentication is usually equally simple: you type in a password (or you have a password manager do so). Alternatively, you might need to enter a private key, and this is more complex because the key is too long to remember and so has to be securely stored for replay. This is increasingly the case for passwords too, as they grow longer and more complex.

The Gordian architecture's main use of Secret Authentication is through the storage of seeds and keys, which are both large secrets. Recognizing the vulnerability of secrets, Blockchain Commons has built numerous systems around protecting them, such as [SSKR](https://developer.blockchaincommons.com/sskr/) and [CSR](https://developer.blockchaincommons.com/csr/), which allow ro4 the sharding and storage of secrets.

> **Advantages:**
> * **Easy.** There's not much easier than typing in a password.
> * **Available.** If you can remember it, you always have your password, no need to recover a phone or token.
>
> **Disadvantages:**
> * **Not Secure.** Passwords are increasingly insecure in the modern age. Any password of 7 characters or less can be cracked within seconds, which means that a password long enough to actually be secure is often too long to remember. Reusage of passwords across multiple sites increases the insecurity more.
> * **Forgettable.** Any password is forgettable, but the problem gets even bigger for large secrets such as private keys that are largely impossible to remember.
> * **Inheritance Issues.** Unless specifically preserved, secrets aren't passed down to heirs.

### Token Authentication

> **Problems:**
> 
> * Authentication needs to be very secure.
> * Authentication needs to be very personal.
> * Privacy concerns preclude use of biometrics.
> 
> **Solution:**
>
> * Allow authentication with proof of access to some device.
>
> **Related Patterns:**
>
> * Authentication, Biometric Authentication, Control Authentication, Secret Authentication
>
> **Exemplary Uses:**
>
> * FIDO Device
> * Google Authenticator
> * SMSed Access Code

Today, **Authentication** for systems of any importance usually needs to be more secure than a simple password. **Biometric Authentication** and **Control Authentication** offer this, but they each have limitations: Biometric Authentication can be privacy-busting and Control Authentication can be overly complex. Token Authentication offers an alternative that is simpler yet still more secure than simple passwords.

FIDO devices were the first widely used example of Token Authentication. They actually depend on the control of a secret (a private key), but by embedding it in a hardware device they changed the usage from **Secret Authentication** to Token Authenication: instead of having to know something, you have to hold something. SMSed Access Codes are also a means of Token Authentication, but because the transit system is
less secure, the overall methodology is less secure. Usage of a system like Google Authenticator offers a better Token Authentication method for proving access to the phone/token since it's unaffected by transit insecurities (though this may be a distinction without a difference, unless you're being actively targeted, which is not the standard attack vector).

Gordian Seed Tool (GST) itself is effectively a Token Authentication method since it holds your secret (seed) just like FIDO devices do. When you want to send a transaction whose UTXO is connected to a related private key, you prove your access to the Token by having GST sign a PSBT for the transaction.

> **Advantages:**
> * **Easy.** A well-built Token Authentication mechanism is largely automated, just requiring the user to link the token to the system requiring Authentication.
>
> **Disadvantages:**
> * **Can Be Lost.** A token can easily be lost, creating a Single Point of Failure without Threshold Authentication.
> * **Can Be Stolen.** A token could also be stolen, which would prevent a user from accessing the token, but which usually would not allow the thief to do so, as token usually themselves have some Authentication system.
> * **Susceptible to Bitrot.** A hardware token could decay over time or could become incompatible with newer systems.
> * **Opaque.** Compared to most other types of Authentication, Token Authentication is pretty opaque: the user doesn't really know what the token is doing.

## Authentication Protection Patterns

_Protections allow Authentication to continue, but with various warnings or other safeguards._

### Authentication Delay

> **Problems:**
>
> * Authentication pattern is irregular.
> * No additional Authentication channels or methods are available.
> * Potential actions are dangerous.
>
> **Solution:**
>
> * Delay before finalizing Authentication.
>
> **Related Patterns:**
>
> * Authentication, In-Channel Warning, Out-of-Channel Warning, Usage Analysis
>
> **Exemplary Uses:**
>
> * Lockout periods after incorrect passwords/PINs.
> * Delay before money transfer.

Methods like **Multichannel Authentication** and **Multifactor Authentication** are great, but sometimes they're just not reasonable. They may cost too much (perhaps just in user annoyance), or an **Authentication** system may not have access to other channels or factors. In these situations, alternatives are needed to maintain  security.

One solution is simply to delay: to not allow Authentication, or not allow a dangerous action, until some period of time has passed. For this to be truly useful, the original user must receive some notification such as an **In-Channel Warning** or **Out-of-Channel Warning**.

The Apple devices that Gordian Seed Tool runs on have an Authentication Delay built into their device's main login. If you fail to Authenticate in a few tries, you can get locked out for 10 minutes, 60 minutes, or more.

**Advantages:**

* **Foils Attacker Expectations.** Most attackers expect all-or-nothing access. A delay can foil that and cause an attacker to move on to the next, less secure, system.
* **Surety.** A delay can increase the surety of a dangerous action.

**Disadvantages:**

* **Lock Out.** A delay is essentially a lock-out, and that means a user can't access some of their functionality for a time. This can vary from annoying to life-threatening. Thus Authentication Delay systems sometimes have to build in exceptions: if you get locked out of an Apple device, you can still make an emergency call.
 
### In-Channel Warning

> **Problems:**
>
> * Authentication pattern is irregular.
> * No additional Authentication channels or methods are available, but there may be additional use of main channel.
> * Potential actions are dangerous.
>
> **Solution:**
>
> * Provide an in-channel warning about access.
>
> **Related Patterns:**
>
> * Authentication, Authentication Delay, Out-of-Channel Warning, Usage Analysis
>
> **Exemplary Uses:**
>
> * Apple's login warnings across devices

In situations where **Multichannel Authentication** or **Multifactor Authentication** aren't possible or are too costly, there may be ever-improving alternatives. One of those is having a singular channel (such as an existing login) that might be used in mutiple instances, and thus can transmit warnings.

Apple takes full use of this when a new login occurs for its online developer accounts: the user receives warnings across all accounts that are logged in to the same Apple login. Similar warnings occur when data is restored from iCloud. Though it's all the same Authentication channel, the fact that some devices were previously logged into the same channel provides additional security. Note that this pattern presumes a warning without required action (though in the Apple example, some warnings actually require a 2FA code to be reentered on the same system, moving it toward the Multichannel Authentication pattern).

There are currently no factored warnings within the Gordian architecture or its references. An example of this pattern might be a user logged into Gordian Seed Tool on two different devices, and receiving a warning on one when something dangerous is done on another.

> **Advantages:**
>
> * **Innocuous.** A warning is relatively innocuous. It doesn't cause problem for the user if there's nothing to worry about.
> * **Warning!** With that said, it's helpful because it's a warning. If there really is a problem, then the user can spot it, and probably pretty quickly if they have access to the device where they're logged in.
>
> **Disadvantages:**
>
> * **No New Channels.** In some way, this is a poor man's warning, since it presumes that the Authentication channel wasn't previously compromised, that it's a brand-new problem.
> * **Ignorable.** Obviously, a user can ignore a warning message, but the problem is deeper than that. The more common this design pattern becomes, the less useful it becomes, in a "Boy Who Cried Wolf" manner. If a user is constantly beset by warning messages from all of their online services, they'll lose the ability to pick out important ones. 

### Out-of-Channel Warning

> **Problems:**
>
> * Authentication pattern is irregular.
> * No additional Authentication  methods are available, but there is an additional Authentication channel available
> * Potential actions are dangerous.
>
> **Solution:**
>
> * Provide an out-of-channel warning about access.
>
> **Related Patterns:**
>
> * Authentication, Authentication Delay, In-Channel Warning, Multichannel Authentication, Usage Analysis
>
> **Exemplary Uses:**
>
> * Email warning of access from new IP
> * Credit card SMS warning about usage

Some systems that can't support **Multifactor Authentication** or **Multichannel Authentication** nonetheless have access to an extra channel such as email (or even SMS).

This allows a system to provide a warning that is out-of-band with the normal **Authentication** methodology. When Authentication is attempted, An email or SMS is sent to the owner of the account. However, _no_ protective action is taken unless the user says there's a problem.

There are currently no factored warnings within the Gordian architecture or its references. However, it might make sense to send an out-of-band warning when a seed is being reconstructed from Gordian Depos, as an example of the pattern.

> **Advantages:**
>
> * **Innocuous.** A warning is relatively innocuous. It doesn't cause problem for the user if there's nothing to worry about.
> * **Warning!** With that said, it's helpful because it's a warning. If there really is a problem, then the user can spot it, and probably pretty quickly, especially if it's an SMS rather than email.
>
> **Disadvantages:**
>
> * **Correlatable.** Emails and messages can create correlation and even an attack vector. If an attacker gains access to the place that receives the warning messages, then they now know an additional place where the user has an account.
> * **Ignorable.** Obviously, a user can ignore a warning message, but the problem is deeper than that. The more common this design pattern becomes, the less useful it becomes, in a "Boy Who Cried Wolf" manner. If a user is constantly beset by warning messages from all of their online services, they'll lose the ability to pick out important ones. 

### Usage Analysis

> **Problems:**
>
> * Authentication is fallible.
> * Cost of Authentication failure is high!
> * Usage patterns are irregular.
>
> **Solution:**
>
> * Test behavior and usage of system to verify that Authentication hasn't failed.
>
> **Related Patterns:**
>
> * Authentication, Authentication Delay, In-Channel Warning, Out-of-Channel Warning
>
> **Exemplary Uses:**
>
> * Fraud Monitoring

No matter how good an **Authentication** system is, it will never be perfect. Authentication _will_ be broken. Fraudulent attackers will impersonate real users and make inappropriate use of their **Authorization**. Sensitive systems must respond to this worst-case scenario.

The solution is to continue monitoring usage after the fact, to establish if a system is being used in unusual ways, and then to offer **In-Channel Warning**, **Out-of-Channel Warning**, or even cut off Authentication if so.

There is currently no system like this in the Gordian Architecture, in large part because existing systems such as fraud monitoring depend on centralized authorities doing the work. If trustless systems were created to ensure usage were normative, it could be an improvement for self-sovereign Authorization systems.

> **Advantages:**
>
> * **Extends Auth into Behavior.** Essentially, Usage Analysis introduce a new factor to Authorization: behavior. This obviously increases its security.
> * **Minimizes Damage.** If unusual activity is detected and Authorization is cut off (or the user is warned), then any damage can be minimized.
>
> **Disadvantages:**
> 
> * **Damage May Already Be Done.** The damage may already have been done! A Usage Analysis system must be a complement to other Authentication, not a replacement.
> * **False Positives.** Traditionally, Usage Analysis has had a serious problem of false positives, as anyone who's credit card was suddenly cut off when they were visiting a foreign country or trying to make a large purchase can tell you. The damage of false positives needs to be minimized, especially in traditional situations where a small cost to a centralized entity is being balanced against a large cost to a user.

# Authentication Storage Patterns

_Authentication Storage declares how the material that verifies authentication is stored._

### Hashed Authentication Storage

> **Problems:**
>
> * If Authentication validation information is stolen, it may allow replay.
> * Certain types of authentication validation information, such as biometrics, cannot be changed.
>
> **Solution:**
>
> * Only store a hash of authentication validation information.
>
> **Related Patterns:**
>
> * Authentication, Biometric Authentication, Local Authentication Storage, Secret Authentication
>
> **Exemplary Uses:**
>
> * Password Tables with Hashed Entries

Sometimes the information that validates **Authentication** can be reverse-engineered to allow an attacker to fake the Authentication. This is particularly true for **Secret Authentication** and to a lesser extent for **Biometric Authentication**. When this is a threat, it needs to be better protected, particularly if it's an Authentication method that can never be changed, such as biometrics. One way to do so is to never store the authentication validating information itself, but only the hash of that information, which disallows its reverse engineering.

Password storage is the largest example of Hashed Authentication Storage. No modern system actually stores the passwords for a system, but instead hashes the pasword and stores that. When a user Authenticates, they type in their password, it's hashed, and that's then checked against the hashed password in storage. In modern days, this is not even sufficient, so passwords tend to be hashed with a salt, so that hashed password list can't be compared against a list of hashed dictionary words.

Protection of **Biometric Authentication** will become even more important going forward. The ["Six Principles for Self-Sovereign Biometrics"](https://github.com/WebOfTrustInfo/rwot6-santabarbara/blob/master/draft-documents/Biometrics.md) doesn't require that biometric data  be hashed, instead focusing on other topics such as **Local Authentication Storage**. That's because the hashing of biometric data is more challenging due to its noisy nature. Nonetheless, the field of biometric hashing is quickly maturing, and it should be considered a best practice as it does.

> **Advantages:**
> * **Privacy.** Hashed data can't be meaningfully stolen.
> * **Security.** Though security is not absolute (as shown by the issues with hashed passwords), it's much improved.
>
> **Disadvantages:**
> * **Compromised Authentication Still Possible.** Although hashed data can't be replayed, it can still be compromised if a known Authentication can be found that matches the hash.

### Local Authentication Storage

> **Problems:**
>
> * Authentication validation data is too sensitive to be kept on the 'net.
> * Centralized sources can't be trusted with very private Authentication data, such as biometrics.
>
> **Solution:**
>
> * Maintain all Authentication validation data locally.
>
> **Related Patterns:**
>
> * Authentication, Authentication Proof, Biometric Authentication, Hashed Authentication Storage, Secret Authentication
>
> **Exemplary Uses:**
>
> * Apple's Maintenance of Biometrics & Private Keys in Secure Vault

One privacy concern with most **Authentication** systems is that they require handing off the Authentication Validation to a third party. You have to trust not only them, but also their security measures — and frankly no big corporation has security that's strong enough to withstand attacks. If your Authentication Validation information allows replay of Authentication (as **Secret Authentication** does) or if it's deeply personal (as **Biometric Authentication** is), then you need to find a way to get that validation information out of third-party hands.

The solution is to store the Authentication validation information locally. Your mobile or desktop device then does all the **Authentication** validation and signals whatever service that the validation occurs. This likely requires some sort of **Validation Proof** although there are alternative patterns, such as locally unlocking a private key (another **Secret Authentication**) that's then used to validate the service itself.

Blockchain Commons feels that the use of a [Secure Enclave](https://support.apple.com/guide/security/secure-enclave-sec59b0b31ff/web) is a best practice: not only does it keep your secrets locally, but it also isolates them from the main processor.

> **Advantages:**
> * **Privacy.** Your Authentication validation information never leaves your local device.
>
> **Disadvantages:**
> * **Honeypot.** This potentially turns your local device into a honeypot of information, which is fine if it's secure, such as a mobile device with a Secure Enclave, and less fine if it's not, such as a desktop computer.
> * **Network Trust Issues.** Local Authentication Storage also has the effect of kicking the can down the road. Unless you're working on a totally local system, you now need to find a way to establish trust with a networked system and to offer proof of Authentication.


## Authentication Subject Patterns

_Authentication Subjects declare who/what is being authenticated._

### Anonymous Validation

> **Problems:**
> 
> * Authentication is needed.
> * Privacy concerns preclude identification.
> * Security requirements exist, but are low.
>
> **Solution:**
>
> * Authenticate without linking to any identity.
>
> **Related Patterns:**
>
> * Authentication, Pseudonymous Validation
>
> **Exemplary Uses:**
>
> * Access Stamp at a Concert or Bar
> * Anonymous Ticket to an Event

**Authentication** and Identification should not be considered equivalent. To protect privacy, it is often desirable to Authenticate someone, but to either not require any personally identifiable information in doing so, or to discard it afterward.

Anonymous Validation is rarely used in computer systems. Computer Authentication is almost always linked to either an identity or a pseudonymous identifier, allowing a computer system to correlate activity to an identifier. However, this is often not desirable for reasons of privacy.

Bitcoin is often seen as an anonymous currency, but it's not, as addresses (and linked public and private keys) act as identifiers that skilled analysts can use to correlate funds and identities. We should consider ways that this system can be improved on, to allow both Authentication and true anonymity. Systems like PayJoin and CoinJoin are a step in this direction.

> **Advantages:**
> * **Privacy.** Anonymity allows true privacy.
>
> **Disadvantages:**
> * **No Correlation.** Though privacy systems often try to defeat correlation, it's not all bad. We often _want_ to create correlation in certain systems, such as when we correlate our assets and ourselves to get a bank loan. With anonymity, that's not possible.
> * **No Responsibility.** With no identities, you can't track responsibility for actions within a system.

### Identity Validation

> **Problems:**
>
> * Real-world identification is needed.
> * Responsibility is required.
> * Security requirements exist, and are high.
>
> **Solution:**
>
> * Link Authentication to a long-lasting or verified identifier.
>
> **Related Patterns:**
>
> * Anonymous Validation, Authentication, Biometric Authentication, Pseudonymous Validation
>
> **Exemplary Uses:**
>
> * Driver's License
> * Facebook Account
> * KYC Systems

As much as privacy is usually a boon, there are situations where a real-life identity is needed, many of them related to financial or legal issues.

This typically requires an **Authentication** system that verifies someone's identity against a real-world identity, perhaps through use of a Driver's License, Social Security Card, or Passport. However, at a lower level of security, it could also require Authentication against a long-lived digital identifier that may or may not be linked to a real-world identity.

None of the Gordian architecture depends upon Identity Validation, as it's largely unnecessary in a digital-only ecosystem. However, Identity Validation would be a perfectly reasonable methodology for a [CSR](https://developer.blockchaincommons.com/csr/) system, as a means of Authentication for recovering a share of a seed.

> **Advantages:**
> * **Correlation.** A long-lived identity and an Authentication method are linked together, providing better proof of who someone is.
> * **Responsibility.** Authenticated actions can be linked to a responsible party.
>
> **Disadvantages:**
> * **No Privacy.** Requiring a link to a long-lived identity, and especially to a real-world identity, is 100% privacy-busting. It should only be undertaken when truly necessary.

### Pseudonymous Validation

> **Problems:**
>
> * Identification is needed, but not real-world identification.
> * Responsibility is required.
> * Security requirements exist, and are medium.
>
> **Solution:**
>
> * Link authentication to a short-lived or non-verified identifier.
>
> **Related Patterns:**
>
> * Anonymous Validation, Authentication, Control Authentication, Identity Validation
>
> **Exemplary Uses:**
>
> * Bitcoin Address
> * Forum Account

**Authentication** can often be built on a compromise, where there's a minimal level of identification. This is often sufficient for a low-security system.

In this situation, a user has an identifier that they authenticate to, such as an account name or a public key, but that that account doesn't necessarily have any correlation to a real-world identity.

As noted, Bitcoin addresses are an example of Pseudonymous Validation, one that leans toward the more anonymous side. A long-term account on GitHub that contributes to Blockchain Commons repos would lean toward the less anonymous side.

> **Advantages:**
> * **Privacy.** Pseudonymity allows for much stronger privacy that Identity Validation.
>
> **Disadvantages:**
> * **Correlation.** Any ongoing identifier can ultimately create opportunities for correlation.
> * **High Privacy Expectations.** Bitcoin in particular has shown that people have very high levels of privacy expectations for pseudonymous accounts, often confusing pseudonymity with true anonymity.
 
## Authorization Patterns

_Authorization is the flipside of Authentication. Authentication verifies your ability to access some system or asset while Authorization defines your permissions related to that system or asset. It's included here because the two are often confused or conflated._

### Authorization

> **Problems:**
>
> * Data needs to be private.
> * Service access needs to be limited.
> * Privacy is desired for some data or services.
>
> **Solution:**
>
> * Define what a user is allowed to do via Authorization.
>
> **Related Patterns:**
>
> * Authentication, Authorization Levels, Authorization-Authentication Link
>
> **Exemplary Uses:**
>
> * UNIX Permissions
> * Read/Write Permissions
> * Access Control Lists

**Authentication** and Authorization are _not_ the same thing. Authentication allows someone to validate who they are, usually giving them access to an account or a system. Authorization then defines what their account is allowed to do or what they can do within the system via an **Authorization-Authentication Link**.

Authorization is usually built with a permission list, an access control list, or a capabilities list. These describe what a specific account or a specific access type can do. UNIX permissions, for example, give a user access to certain files and certain programs controlled by their account-id or within a group that they are part of or that are generally available. UNIX Authorization is then more finely divided into read, write, and execute permissions.

Authorization and Authentication can collapse together if a single Authentication grants all Authorization for a system. This is effectively the case for Gordian Seed Tool. Looking at the wider digital asset world, Bitcoin can have either write permissions (if you have the private key) or read permissions (if you have the address or public key), which are two different levels of Authorization.

**Advantages:**

* **Security.** Fundamentally, the Authorization pattern creates security within a system.

**Disadvantages:**

* **Complexity.** Obviously, any permissions or capabilities system creates complexity. 
* **Logistics.** People might find themselves unable to do things that they should be able to do because Authorization isn't set up correctly.

### Authorization-Authentication Link

> **Problems:**
> 
> * A methodology is required to figure out who gets what access.
> * Access should be based on some sort of identification.
>
> **Solution:**
>
> * Link Authorization Level to Authentication
>
> **Related Patterns:**
>
> * Authentication, Authorization, Authorization Levels
>
> **Exemplary Uses:**
>
> * Login to any permissioned system
> * Providing an ID to get into a bar
> * Commit-only signing key

This fundamental problem seems a simple one: you can't just give people **Authorization** willy-nilly. Generally, someone needs to prove who they are to be granted their intended **Authorization Level**. It's a design pattern that's so commonly used as to be obvious — but a design pattern nonetheless.

The simplest example is perhaps offering an ID to enter an age-restricted area or purchase an age-restricted item (usually alcohol). All that's technically needed is **Authentication** of your age, and then the commercial vendor gives you the Authorization needed. Technically, this is what almost any Authorization system does. Once you Authenticate your biometrics, your control of a system, your knowledge of a secret, or your access to a token, you're given a level of Authorization.

This is of course how Gordian Seed Tool works too: your Authenticate yourself, and then you're given Authorization to access your seeds. But the pattern becomes more meaningful in some of Blockchain Commons' best practices. Our GitHub best practices, for example, suggest that different keys should be used for authentication and signing. This means that an auth key should be Linked to GitHub access permissions and a signing key should be Linked to identity validation permissions, and that they shouldn't be treated as the same thing.

**Advantages:**

* **Authorize the Right People.** This pattern is so obvious because it matches how we want things to work: we want to give specific Authorization Levels to specific people.

**Disadvantages:**

* **Ignores Other Patterns.** Because the Authorization-Authentication Link pattern is so obvious, it's caused other methodologies to be ignored. What if we wanted to Authorization people based on certain skill levels? Based on certain needs? What if we wanted to Authorize people without involving correlatable identities? These other possibilities have largely fallen by the way side.
* **Impersonation Possible.** Depending on the exact Authentication method used, impersonation might be possible. Some methods are more susceptible to impersonation (passwords), some are less (tokens), others fall in the middle. Unfortunately, impersonation can be very problematic, again because of the commonness of this pattern. Because authentication _is_ authorization for many systems, impersonation _provides_ unfettered access to a system, and can be very hard to unwind.

### Authorization Levels

> **Problems:**
> 
> * Some systems may require higher levels of Authorization.
> * Some digital assets may require higher levels of Authorization.
> * Some dangerous actions may require higher Levels of Authorization.
>
> **Solution:**
>
> * Define what a user is allowed to do via increasing levels of Authorization.
>
> **Related Patterns:**
>
> * Authentication Levels, Authorization, Authorization-Authentication Link
>
> **Exemplary Uses:**
>
> * Progressive 2FA
> * Single Sigs vs. Multisigs

**Authorization** is only sometimes an all-or-nothing affair. A system often has non-sensitive information, sensitive information, super-sensitive information, etc. Similarly, actions can run the gamut from non-sensitive (reading public data) to very sensitive (changing private data).

This issue can be resolved by progressive Authorization, where progressive levels of Authentication (via **Authentication Levels**) create an **Authorization-Authentication Link** to progressive levels of Authorization. Some online services now take this approach, where regular access is allowed with a password, but taking a sensitive action such as changing a password or sending money requires **Control Authentication** through an email address, which receives a validation code.

Gordian Seed Tool effectively has two levels of Authorization built upon two levels of **Authentication**. The lower Authorization level requires **Token Authentication** through control of the mobile device and either **Biometric Authentication** or **Secret Authentication** to unlock it; that provides access to listings of seeds and public data about them. The higher Authorization level requires Biometric Authentication or Secret Authentication within the app and provides access to seed details and private keys.

**Advantages:**

* **Protection for Sensitive Data or Actions.** Every system is likely to have some data or some actions that are more sensitive. Authorization Levels provide proper protection for that.

**Disadvantages:**

* **Modeling Difficulties.** Modeling a multi-layer authorization system can be much harder than a single-level system, and this is likely to introduce logistical difficulties and errors.

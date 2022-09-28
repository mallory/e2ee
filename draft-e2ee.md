---
title: Definition of End-to-end Encryption
abbrev: e2ee
docname: draft-knodel-e2ee-definition-07
category: info

ipr: trust200902
area: sec
workgroup: sec
keyword: Internet-Draft
stand_alone: yes
pi:
  RFCedstyle: yes
  toc: yes
  tocindent: yes
  sortrefs: yes
  symrefs: yes
  strict: yes
  comments: yes
  inline: yes
  text-list-symbols: -o*+

author:

-
       ins: M. Knodel
       name: Mallory Knodel
       organization: CDT
       email: mknodel@cdt.org

-
       ins: S. Celi
       name: Sofía Celi
       organization: Brave
       email: cherenkov@riseup.net

-
       ins: F. Baker
       name: Fred Baker
       organization:
       email: fredbaker.IETF@gmail.com

-
       ins: O. Kolkman
       name: Olaf Kolkman
       organization: ISOC
       email: kolkman@isoc.org

-
       ins: G. Grover
       name: Gurshabad Grover
       organization: Centre for Internet and Society
       email: gurshabad@cis-india.org

normative:

informative: 

   RFC3724:
   RFC3552:
   RFC7258:
   RFC7624:
   RFC8890:
   RFC3935:

   saltzer:
     title: "End-to-end arguments in system design"
     date: 1984
     author:
        - ins: J. H. Saltzer, et al
     target: https://web.mit.edu/Saltzer/www/publications/endtoend/endtoend.pdf

   dkg:
     title: "Human Rights Protocol Considerations Glossary"
     date: 2015
     author:
        - ins: D. Gillmor
     target: https://tools.ietf.org/html/draft-dkg-hrpc-glossary-00

   mls:
     title: "Messaging Layer Security"
     date: 2018
     author:
        - ins: IETF
     target: https://datatracker.ietf.org/doc/charter-ietf-mls

   openpgp:
     title: "Open Specification for Pretty Good Privacy"
     date: 2020
     author:
        - ins: IETF
     target: https://datatracker.ietf.org/doc/charter-ietf-openpgp

   GEC-EU:
     title: "Breaking encryption myths: What the European Commission’s leaked report got wrong about online security"
     date: 2020
     author:
        - ins: Global Encryption Coaliation
     target: https://www.globalencryption.org/2020/11/breaking-encryption-myths/
     
   komlo:
     title: "Defining end-to-end security"
     date: 2021
     author:
        - ins: Chelsea Komlo
     target: https://github.com/chelseakomlo/e2ee/blob/master/e2ee_definition.pdf

--- abstract

End-to-end encryption is an application of cryptography mechanisms and properties in communication systems between endpoints. End-to-end encrypted systems are exceptional in providing both security and privacy properties through confidentiality, integrity and authenticity features for users. Improvements to end-to-end encryption strive to maximize the user's security and privacy while balancing usability and availability. Users of end-to-end encrypted communications expect trustworthy providers of secure implementations to respect and protect them.

--- middle
  
Introduction
============

This document uses three different dimensions that define end-to-end encryption, which can be applied in a variety of contexts.

The first is a formal definition that draws on the basic understanding of end points and cryptography, where it is comprised of necessary constituent parts but considered as a system. The second looks at end-to-end encrypted implementations from a design perspective, both its fundamental features and proposed improvements on those features. Lastly this document considers the expectations of the user of end-to-end encryption.

These dimensions taken as a whole comprise a generally comprehensible picture of consensus at the IETF as to what is end-to-end encryption, irrespective of application, from messaging to video conferencing, and between any number of end points. It it worth noting that while the word "encryption" often refers to confidentiality (security) properties, this document shows that end-to-end encryption MUST provide both security and privacy properties. And it provides a definition of which specific security and privacy properties end-to-end encryption should provide.

Formal definition of end-to-end encryption
==========================================

End-to-end encryption, irrespective of the content or the specific methods employed, relies on two important and rigorous technical concepts: the end-to-end principle as defined in the IETF; and encryption, an application of cryptographic mechanisms and the primary means employed by the IETF to secure internet protocols and maintain the privacy of content delivered via these internet protocols. Where end-to-end encryption is comprised of these necessary constituent parts, a systems approach also defines their interplay. In the field of cryptography it is also possible to achieve a concise definition of end-to-end encrypted security.

End point
---------

Intuitively, an "end" either sends messages or receives them, usually both; other systems on the path are just that - other systems. Other systems MAY be used to facilitate the sending of messages between both "ends", but are not themselves "ends”."

It is, however, not trivial to establish the definition of an end point in isolation, because its existence inherently depends on at least one other entity in a communications system. Instead the end-to-end principle, which is well established {{RFC3724}} in internet standards and introduces nuance, is described in the following sub-section.

However despite the nuance for engineers, it is now widely accepted that the communication system itself begins and ends with the user {{RFC8890}}. Imagine people (through an application's user interface, or user agent) as components in a subsystem's design. An important distinction to this in end-to-end encrypted systems might be configuration channels, such as the use of public key infrastructure where a third party is often used in the authentication phase to enhance the larger system's trust model. Responsible use of public key infrastructure is required in such cases, such that the end-to-end encrypted system does not admit third parties under the user's identity.

User agent and user cannot be equated, nor can they be fully separated. As user-agent computing becomes more complex and often more proprietary, the user agent becomes less of an "advocate" for the best interests of the user. This is why this document introduces a later section on the end-to-end encrypted system being able to fulfill user expectations and serving only the user's interests.

End-to-end principle
--------------------

In order to describe what the "end-to-end" principle means, we need first to answer "What constitutes an end?", which is an important question in any review of the End-to-End Principle {{RFC3724}}. However the notion of an end point is more fully defined within the principle of end-to-end communications.

In 1984 the "end-to-end argument" was introduced {{saltzer}} as a design principle that helps guide placement of functions among the modules of a distributed computer system. It suggests that functions placed at low levels of a system may be redundant or of little value when compared with the cost of providing them at that low level. It is used to design around questions about which parts of the system should make which decisions, and as such the identity of the actual "speaker" or "end" may be less obvious than it appears. The communication described by Saltzer is between communicating processes, which may or may not be on the same physical machine, and may be implemented in various ways. For example, a Border Gateway Protocol (BGP) speaker is often implemented as a process that manages the Routing Information Base (RIB) and communicates with other BGP speakers using an operating system service that implements TCP. The RIB manager might find itself searching the RIB for prefixes that should be advertised to a peer, and performing "writes" to TCP for each one. TCP in this context often implements a variant of the algorithm described in RFC 868 (the "Nagle algorithm"), which accumulates writes in a buffer until there is no data in flight between the communicants, and then sends it - which might happen several times during a single search by the RIB manager. In that sense, the RIB manager might be thought of as the "end", because it decides what should be communicated, or TCP might be the "end", because it actually sends the TCP Segment, detects errors if they occur, retransmits it if necessary, and ultimately decides that the segment has been successfully transferred.

Another important question is "what statement exactly summarizes the end-to-end principle?". Saltzer answered this in two ways, the first of which is that the service implementing the transaction is most correct if it implements the intent of the application that sent it, which would be to move the message toward the destination address in the relevant IP header. Salzer's more thorough treatment, however, deals with end cases that come up in implementation: "Examples discussed in the paper", according to the abstract, "include bit error recovery, security using encryption, duplicate message suppression, recovery from system crashes, and delivery acknowledgement." It also notes that there is occasionally a rationale for ignoring the end-to-end arguments for the purposes of optimization. There may be other user expectations or design features, some explained below, which need to be balanced with the end-to-end argument.

More concisely, suppose that an end user is the end identity. An end-to-end encrypted system may run between potential end points at different network layers within the end identity's possession. These end points may then be considered acceptable sub-identities provided that no path between the end identity and sub-identity is accessible by any outside third party. There are quite a number of examples of common situations where tunnels are used and this does not apply. For instance, the examples below all provide encryption by which data is turned into clear text in locations that are not under control of the end user:

*  The common VPN business model whereby a TLS or an IPsec tunnel terminates at the service provider's server and is subsequently forwarded to its destination elsewhere in unencrypted form;
* Email transport whereby an unencrypted message traverses from sending mail user agent, between various mail transfer agents, and finally to a receiving mail user agent, all over TLS protected connections;
* The encrypted connection of last mile connections such as those in 4G LTE;

This definition of end points accounts for potentially several devices owned by a user, and various application-specific forwarding or delivery options among them. It also accounts for end-to-end encrypted systems running at different network layers. Regardless of the sub-identities allowed, the definition is contingent on that all end sub-identities are under the end identity's control and no third party (or their sub-identities, e.g. system components under third-party control) can access the end sub-identities nor links between the sub-identity and end identity.
This creates a tree hierarchy with the end user as the root at the top, and all potential end points being under their direct control, without third party access.
As an example, decryption at organizational network router before message forwarding (encrypted or unencrypted) to the end identity does not constitute end-to-end encryption. However, end-to-end encryption to a user's personal device and subsequent end-to-end encrypted message forwarding to another one of the user's personal devices (without access available to any third party at any link or on device) maintains end-to-end encrypted data possession for the user.

Encryption
----------

Encryption is fundamental to the end-to-end principle. "End-to-End: The principal of extending characteristics of a protocol or system as far as possible within the system. For example, end-to-end instant message encryption would conceal communication content from one user's instant messaging application through any intermediate devices and servers all the way to the recipient's instant messaging application. If the message was decrypted at any intermediate point--for example at a service provider--then the property of end-to-end encryption would not be present."{{dkg}} Note that this only talks about the encrypted contents of the communication and not the metadata (often in plaintext) generated from it.

The way to achieve a truly end-to-end encrypted communication system (with required security and privacy properties) is indeed to encrypt, authenticate and provide integrity of the content of the data exchanged between the endpoints, e.g. sender(s) and receiver(s). The more common end-to-end technique achieve this through the use of the double-ratchet algorithm with an authenticated encryption scheme and of an Authenticated Key Exchange (AKE). The usage of these algorithms (or variants of these) is present in many modern messenger applications such as those considered in the IETF Messaging Layer Security working group, whose charter is to create a document that satisfies the need for several internet applications for group key establishment and message protection protocols {{mls}}. OpenPGP, mostly used for email, uses a different technique to achieve security and privacy. It is also chartered in the IETF to create a specification that covers object encryption, object signing, and identity certification {{openpgp}}. Both protocols rely on the use of asymmetric and symmetric encryption, and exchange long-term identity public keys amongst end points. The IETF has a clear mandate and demonstrated expertise in defining the specifics of secure and private communications of the internet.

While encryption is fundamental to the end-to-end principle, it does not stand alone. As in the history of all security, authentication and data integrity properties are also linked, and contributed to the end-to-end nature of end-to-end encryption. Permission of data manipulation or creation of pseudo-identities for third parties to allow access under the user's identity are against the intention of end-to-end encryption. In other words, the application functions only for the end user and does not perform functions for any other entity coverly, nor overtly, say even if that entity claims to have obtained the consent of the end user. Thus, end point authenticity MUST be established as (sub-)identities of the end user, and end-to-end integrity MUST also be maintained by the system. There is considerable system design flexibility available in entity authentication mechanisms and data authentication that still meet this requirement.

Concise definition of end-to-end security
------------------------------------------

A concise definition for end-to-end security can describe the security of the system by the probability of an adversary's success in breaking the system. Example snippet:

The adversary successfully subverts an end-to-end encrypted system if it can succeed in either of the following: 1) the adversary can produce the participant's local state (meaning the adversary has learned the decrypted contents of participant's messages), or 2) the states of conversation participants do not match (meaning that the adversary has influenced their communication in some way). To prevent the adversary from trivially winning, the adversary is not allowed to compromise the participants' local state.

We can say that a system is end-to-end secure if the adversary has negligible probability of success in either of these two scenarios {{komlo}}.

End-to-end encryption implementations
=====================================

When looking at implementations of end-to-end encryption from a design perspective, the first consideration is the list of fundamental features that distinguish an end-to-end encrypted system from one that does not employ end-to-end encryption. Secondly, one must consider the development goals for improving the features of end-to-end encryption, in other words, the challenges defined by the designers, developers and implementers of end-to-end encryption.

The features and challenges listed below are framed comprehensively rather than from the perspective of their design, development, implementation or use.

Features
--------

Defining a system can also be done by inspecting what it does, or is meant to do, in the form of features. The features of end-to-end encryption from an implementation perspective can be inspected across several important categories: 1) the necessary features of end-to-end encrypted for the properties of authenticity, confidentiality, and integrity, whereas features of 2) availability, deniability, forward secrecy, and post-compromise security are enhancements to end-to-end encryption.

### Necessary features

Authenticity
: A system provides message authenticity if the recipient and sender attest to each other's identities in relation to the contents of their communications.

Confidentiality
: A system provides message confidentiality if only the sender and intended recipient(s) can read the message plaintext, i.e. message sent between participants can only be read by the agreed upon participants in the group and all participants share the identical group member list.

Integrity
: A system provides message integrity when it guarantees that messages that have been modified in transit. If they have been modified, it must be detected in a reliable way such that a recipient is assured that a message cannot be undetectably modified in any way.

### Optional/desirable features

Availability
: A system provides high availability if the user is able to access the contents of the message (decrypt them) when they so desire and potentially from more than one device, i.e. a message arrives to a recipient even if they have been offline for a long time. Note that applications that use this feature often implement a threshold for this property: number or aggregate size of messages; or messages from a month ago can be read by a user that has been offline, but not messages from a year ago.

Loss Resilience
: If a message is permanently lost by the network, sender(s) and/or recipient(s) should still be able to communicate.

Deniability
: Deniability ensures that anyone able to decrypt a record of the transcript, including message recipients, cannot cryptographically prove to others that a particular participant of a communication authored a specific message. As demonstrated by widely implemented protocols, this optional property must exist in conjunction with the necessary property of message authenticity, i.e. participants in a communication must be assured that they are communicating with the intended parties but this assurance cannot be transmitted to any other parties.

Forward secrecy
: Forward secrecy is a security property that prevents attackers from decrypting encrypted data they have previously captured over a communication channel before the time of compromise, even if they have compromised one of the endpoints. Forward secrecy is usually achieved by deriving new encryption/decryption keys, and old keys no longer required to encrypt or decrypt any new messages are immediately destroyed.

Post-compromise security
: Post-compromise security is a security property that seeks to guarantee future confidentiality and integrity even in the face of an end-point compromise (and consequently that communication sent post-compromise is protected with the same security properties that existed before the compromise). It is usually achieved by adding new ephemeral key exchanges (new randomness) to the derivation of encryption/decryption keys every 'x' amount of time or after 'n' messages sent. Note that post-compromise security is not met in the face of active attackers.

Metadata obfuscation
: Digital communication inevitably generates data other than the content of the communication itself, such as IP addresses, group memberships, date and time of messages. To enhance the privacy and security of end-to-end encryption, steps should be taken to minimize metadata leakage such as users hiding IP addresses, reducing non-routing metadata, and avoiding extraneous message headers.

Disappearing messages
: For truly confidential conversations, deleting one-by-one sensitive messages typically depends on a level of client-side security that is unsustainable. Features like "delete for me" or "delete for everyone" helps with individual messages. What is better is the automatic deletion of whole conversations after an agreed upon timeframe by all parties, eg disappearing messages. In any case, whenever a user has deleted content for all, the provider must ensure complete removal of the content and even then a certain level of trust among users of the system is needed.

Challenges
----------

Earlier in this document end-to-end encryption was defined using formal definitions assumed by internet protocol implementations. Also because the IETF is the place for "producing high quality, relevant technical documents that influence the way people design, use, and manage the Internet" {{RFC3935}} the reader can be confident that current deployments of end-to-end encrypted technologies in the IETF indicate the cutting edge of their developments, which is yet another way to define what is, or ideally should be, a particular kind of technology.

Below is best effort list of the challenges currently faced by protocol designers of end-to-end encrypted systems. Problems that fall outside of this list are likely 1) unnecessary feature requests that negligibly, or do nothing to, achieve the aims of end-to-end encrypted systems, or are 2) in some way antithetical to the goals of end-to-end encrypted systems.

* Making messaging applications interoperable is an important goal for a healthy and user centric internet ecosystem, however it requires careful design of protocols and systems, such as content type negotiation; provisions of global services, such as discovery; and a great deal of cooperation amongst implementers.

* Public key verification is very difficult for users to manage. Authentication of the two ends is required for secure and private conversations. Therefore, solving the problem of verification of public keys is a major concern for any end-to-end encrypted system design. Some applications bind together the account identity and the key, and leave users to establish a trust relationship between them, assisted by public key fingerprint information.

 * Users want to smoothly switch application use between devices, but this comes at a cost to the security and privacy of the communication. Thus, there is a problem of availability in end-to-end encrypted systems because the account identity's private key is generated by and stored on the end-user's original device and to move the private key to another device can compromise the security of one of the end-points of the system (by opening the door to key-impersonation attacks, for example).

 * Existing protocols are vulnerable to metadata analysis, even though metadata is often much more sensitive than content. Metadata is plaintext information that travels across the wire and includes delivery-relevant details that central servers need such as the account identity of end-points, timestamps, message size or more. Metadata is difficult to eliminate or obfuscate entirely.

 * Users need to communicate in groups, but this presents problems of scale for end-to-end encryption systems.

 * The whole communication should remain secure if only one message is compromised. However, for encrypted communication, in some schemes, you must currently choose between forward secrecy or the ability to fully communicate asynchronously. This presents a problem for application design that uses end-to-end encryption for asynchronous messaging over email, RCS, etc.

 * Users of end-to-end encrypted systems should be able to communicate with any medium of their choice, from text to large files. However, there is often a resource problem because there are no open protocols to allow users to securely share the same resource in an end-to-end encrypted system.

 * Usability, accessibility and internationalisation features often need careful design and implementation with respect to security and privacy, such as message read status, typing indicators, URL/link previews, third-party input/output applications.

 * End user security tools like anti-virus plugins, spam filters, fraud protections are in conflict with the security and privacy considerations of the end-to-end application. 

 * Deployment is notoriously challenging for any software application where maintenance and updates can be particularly disastrous for obsolete cryptographic libraries.

End-user expectations
=====================

While the formal definition and properties of an end-to-end encrypted system relate to communication security and privacy, they do not draw from a comprehensive threat model or speak to what users expect from end-to-end encrypted communication. It is in this context that some designs and architectures of end-to-end encryption may ultimately run contrary to user expectations of end-to-end encrypted systems {{GEC-EU}}. Although some system designs do not directly violate "the math" of encryption algorithms, they do so by implicating and weakening other important aspects of an end-to-end encrypted _system_.

A conversation is confidential
------------------------------

Users talking to one another in an end-to-end encrypted system should be the only ones that know what they are talking about {{RFC7624}}. People have the right to data privacy as defined in international human rights law, and within the right to free expression and to hold opinions is inferred the right to whisper, whether or not they are using digital communications or walking through a field.

Providers are trustworthy
-------------------------

While "trustworthy" can be rigorously defined from an engineering perspective, for the purposes of this document a definition inspired by an internal workshop for Internet Society staff is sufficient:

Trustworthy
: A system is completely trustworthy if and only if it is completely resilient, reliable, accountable, and secure in a way that consistently meets users’ expectations. The opposite of trustworthy is untrustworthy.

This definition is complete in its positive and negative aspects: what it is, e.g. "Worthy of confidence" and what it is not, e.g. in RFC 7258: "behavior that subverts the intent of communicating parties without the agreement of those parties" {{RFC7258}}.

Therefore, a trustworthy end-to-end encrypted communication system is the provider of the set of functions needed by two or more parties to communicate among each other in a confidential, authenticated and integrity-preserving fashion without any third party having access to the content of that communication where the functions that offer the confidentiality, authenticity and integrity-preservation are providing these services to only the participants whom all know who are in the conversation.

Access by a third-party is impossible
-------------------------------------

No matter the specifics, any methods used to access to the content of the messages by a third party would violate a user's expectations of end-to-end encrypted messaging. "[T]hese access methods scan message contents on the user’s [device]", which are then "scanned for matches against a database of prohibited content before, and sometimes after, the message is sent to the recipient" {{GEC-EU}}. Third party access also covers cases without scanning -- namely, it should not be possible for any third-party end point, even those under the user's identity as per Section 2.1, to access the data regardless of reason.

If a method makes secure and private communication, intended to be sent over an encrypted channel between end points, available to parties other than the sender and intended recipient(s), without formally interfering with channel confidentiality, that method violates the understood expectation of that security property.

The software of the end-to-end encrypted system can be trusted
--------------------------------------------------------------

A way by which users can check that a system does not have a "backdoor" or is performing in accordance to cryptographic protocols' specifications is by making them opensource. Opensource allows users to openly analyse the system and be assured of it. However, while some users might be able to do so, many users lack the technological knowledge needed to analyse source code. It is vital that systems provide public security analyses of their source code enabling reproducible audits and investigations that can be published and peer reviewed.


Pattern inference is minimised
------------------------------

Analyses such as traffic fingerprinting or other (encrypted or unencrypted) data analysis techniques, outside of or as part of end-to-end encrypted system design, allow third parties to draw inferences from communication that was intended to be confidential. "By allowing private user data to be scanned via direct access by servers and their providers," the use of these methods should be considered an affront to "the privacy expectations of users of end-to-end encrypted communication systems" {{GEC-EU}}.

Not only should an end-to-end encrypted system value user data privacy by not explicitly enabling pattern inference, it should actively be attempting to solve issues of metadata and traceability (enhanced metadata) through further innovation that stays ahead of advances in these techniques.

The end-to-end encryption is not compromised
----------------------------------

RFC 3552 talks about the Internet Threat model such as the assumption that the user can expect any communications systems, but perhaps especially end-to-end encrypted systems, to not be intentionally compromised {{RFC3552}}. Intentional compromises of end-to-end encryption are usually referred to as "backdoors" but are often presented as additional design features under terms like "key escrow". Users of end-to-end encryption would not expect a front, back or side door entrance into their confidential conversations and would expect a provider to actively resist -- technically and legally -- compromise through these means.

Conclusions
===========

From messaging to video conferencing, there are many competing features in an end-to-end encrypted implementation that is secure, private and usable. The most well designed system cannot meet the expectations of every user, nor does an ideal system exist from any dimension. End-to-end encryption is a technology that is constantly improving to achieve the ideal as defined in this document.

Features and functionalities of end-to-end encryption should be developed and improved in service of end user expectations for privacy preserving communications.

Acknowledgements
================

Fred Baker, Stephen Farrell, Richard Barnes, Olaf Kolkman all contributed to the early strategic thinking of this document and whether it would be useful to the IETF community.

The folks at Riseup and the LEAP Encryption Access Project have articulated brilliantly the hardest parts of end-to-end encryption systems that serve the end users' right to whisper.

Ryan Polk at the Internet Society has energy to spare when it comes to organising meaningful contributions, like this one, for the technical advisors of the Global Encryption Coalition.

Adrian Farrel, Eric Rescorla and Paul Wouters are acknowleded for their review, comments, or questions that lead to improvement of this document.

Chelsea Komlo and Britta Hale have contributed their deep expertise and consice and rigourous writing to this draft.

Security Considerations
=======================

This document does not specify new protocols and therefore does not bring up technical security considerations.

Because some policy decicions may affect the security of the internet, a clear and shared definition of end to end encrypted communication is important in policy related discussions.  This document aims to provide that clarity.


IANA Considerations
===================

This document has no actions for IANA.

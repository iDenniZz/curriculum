Security 201
************

Centralised accounts
====================

LDAP and Kerberos
-----------------

Active Directory
----------------

NIS, NIS+, YP, Hesiod
---------------------


Firewalls and packet filters
============================

host vs network
---------------

"Crunchy outer shell, soft center is bad"

In general terms, implementing defense-in-depth strategies is always a sensible 
practice.  The concept in which multiple layers of security controls (defense) 
are placed throughout an information technology (IT) system.  Its intent is to 
provide redundancy in the event a security control fails or a vulnerability is 
exploited.

Implementing a firewall on the network **and** host-based packet filters 
provides defense-in-depth layers to your infrastructure.  In today's landscape, 
the firewall aspect could be a service like ec2 security groups with host-based 
packet filters such as iptables, Windows Firewall (or WFAS).  Although this can 
add additional complexity to deployment, that is not a reason to not implement 
it where appropriate.

The defense-in-depth concept is mostly regarded in terms of attack and 
compromise, however in ops it also safeguards **us** as everyone makes mistakes.  
Sometimes, we ourselves or our colleagues are the point of failure.

Strange as it may seem, people often make the mistake of disabling "rules" when 
something is not working and they cannot figure out why.  The *just checking* 
test.  This is always the first mistake.  In real world operations these things 
do happen, whether it is a *just checking* mistake, an incorrect configuration 
or action, sometimes we make serious mistakes, to err is human.

In all these situations using a firewall/other and host-based packet filters 
comes to the fore:

- It protects us, the people working on the systems, from ourselves.
- It protects the organisation's assets in the event of a failure or compromise 
  at either layer, whether it be user error or a systematic failure.
- It keeps us proficient and *in-hand* on both the network specific 
  security implementation, the host-based security practices and the 
  applications related to their management.
- It is good practice.

An old skool, real world example:
---------------------------------

- You have a MSSQL server running on Windows Server 2000 (no "firewall" back 
  then & ip filters are not enabled) - it has both private and public network 
  interfaces.
- The MSSQL server has a public NIC because it has run replication with your 
  customer's MSSQL server sometimes for dev purposes and catalog updates.
- You have rules on the diversely routed, mirrored NetScreen NS1000 firewalls 
  that allows port 1433 between the MSSQL servers only on the public interface.
- Your colleague has an issue that cannot be resolved and quickly just sets the
  firewall/s to "Allows from all", the *just checking* test.
- **Immediate** result - network unreachable.
- SQL Slammer had just arrived and proceeded to gobble up 2Gbps of public T1 
  bandwidth.
- All points into the network and the network itself are saturated until you 
  debug the issue via the serial port on the firewall and figure out what 
  happened.

The synopsis is that a practice of disabling rules was implemented, as it had 
always been the last line in debugging.  It was a practice that had been done 
many times by the engineers in the organisation at the time with no "apparent" 
consequences in the past.  This time differed in that the MSSQL server had a 
public interface added to allow for replication with the customer **and** 
SQL Slammer was in the wild.

If the MSSQL server had ip filtering enabled as well the situation would have 
been mitigated.  Needless to say, it was the last time "Allow from all" debug 
strategy was ever implemented.  However it is useful to note, that because this
was done all the time, the engineer in question did not even tie the action of 
"Allow from all" and the network becoming unreachable together at the time, 
because the action previously had never resulted in the outcome that was 
experienced in this instance.

Stateful vs stateless
---------------------

IPTables: Adding and deleting rules
-----------------------------------

pf: Adding and deleting rules
-----------------------------


Public Key Cryptography
=======================
   
A public key infrastructure (PKI) supports the distribution and identification
of public encryption keys, enabling users and computers to both securely exchange
data over networks suck as the internet and verify the identity of the other party.
Without PKI, sensitive information can still be encrypted and exchanged, but there
would be no assurance of the identity of the other party. Any form of sensitive
data exchanged over the internet is reliant on PKI for security.

A typical PKI consists of hardware, software, policies and standards to manage the
creation, administration, distribution and revocation of keys and digital 
certificates. Digital certificates are most important parts of PKI as they affirm
the identity of the certificate subject and bind that identity to the public 
key contained in the certificate.

PKI includes the following key elements: 

- A trusted party, called a certificate authority (CA), acts as the root of trust
  and provides services that authenticate the identity of individuals, computers 
  and other entities  
- A registration authority, often called a subordinate CA, certified by a root CA
  to issue certificates for specific uses permitted by the root  
- A certificate database, which stores certificate requests and issues and revokes
  certificates 
- A certificate store, which resides on a local computer as a place to store 
  issued certificates and private keys  

Using public and private keys for SSH authentication
----------------------------------------------------

PKI can be used when you want to send a secure email to another person. I will 
use an example in which Bob wants to send an email to Alice. This can be 
accomplished in the following manner:

1. Both Bob and Alice have their own key pairs. They have kept their private 
  keys securely to themselves and have sent their public keys directly to 
  each other.
2. Bob uses Alice's public key to encrypt the message and sends it to her.
3. Alice uses her private key to decrypt the message.

This simplified example highlights at least one obvious concern Bob must have 
about the public key he used to encrypt the message. That is, he cannot know 
with certainty that the key he used for encryption actually belonged to Alice.
It is possbile that another party monitoring the communication channel between
Bob and Alice substituted a different key.

The public key infrastructure concept has evolved to help address this problem 
and others. When a CA is used, the preceding example can be modified in the 
following manner:


1. Assume that the CA has issued a signed digital certificate that contains its 
   public key. The CA self-signs this certificate by using the private key 
   that corresponds to the public key in the certificate.
2. Alice and Bob agree to use the CA to verify their identities.
3. Alice requests a public key certificate from the CA.
4. The CA verifies her identity, computes a hash of the content that will make 
   up her certificate, signs the hash by using the private key that corresponds 
   to the public key in the published CA certificate, creates a new certificate
   by concatenating the certificate content and the signed hash, and makes the 
   new certificate publicly available.
5. Bob retrieves the certificate, decrypts the signed hash by using the public 
   key of the CA, computes a new hash of the certificate content, and compares
   the two hashes. If the hashes match, the signature is verified and Bob can 
   assume that the public key in the certificate does indeed belong to Alice.
6. Bob uses Alice's verified public key to encrypt a message to her.
7. Alice uses her private key to decrypt the message from Bob.


Two factor authentication
=========================


Building systems to be auditable
================================

Data retention
--------------

Log aggregation
---------------

Log and event reviews
---------------------

Role accounts vs individual accounts
------------------------------------


Network Intrusion Detection
============================


Host Intrusion Detection
=========================


Defense practices
=================


Risk and risk management
========================


Compliance: The bare minimum
============================

What is compliance and why do you need it?

What kinds of data can't you store without it?

Legal obligations


Dealing with security incidents
===============================


ACLs and extended attributes (xattrs)
=====================================


SELinux
=======


Data placement
==============
Eg, local vs cloud, the implications, etc


Additional reading
==================
Ken Thompson, Reflections on Trusting Trust:
http://dl.acm.org/citation.cfm?id=358210

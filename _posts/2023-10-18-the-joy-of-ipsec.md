---
#**************************************
lang: en-AU
layout: post
social-share: true
comments: true

typora-copy-images-to: ../assets/img/${filename}
typora-root-url: ../
#**************************************

#*************************************
#fill this if you have renamed the page
redirect_from:
#*************************************

title: Why IPSEC Sucks
subtitle: otherwise known as I hate this stack to the moon and back
author:  Stephen
updated: 
cover-img: 
thumbnail-img: 
full-width: false

share-title: Why IPSEC sucks
share-description: One of my most hated parts of network engineering is IPSEC. Unfortunatly, for something that is just meant to work, when it doesn't it is a complete pain in the arse to get working. It usually involves choosing random hashes and algorithms until one of them sticks.
share-img: 

tags:
  - Cisco ASA
  - Fortigate
  - IPSEC
  - Network Engineer
---

One of my most hated parts of network engineering is IPSEC. Unfortunatly, for something that is just meant to work, when it doesn't it is a complete pain in the arse to get working. It usually involves choosing random hashes and algorithms until one of them sticks. :cry:

For those that have never had to deal with IPSEC or **I**nternet **P**rotocol **Sec**urity is a suite of communication protocols for that allows for traffic to be authenticated and protected as it crosses an IP network. Whilst most often used to secure VPN (**V**irtual **P**rivate **N**etworks) running across the public internet, it can also be utilised to authenticate and secure the traffic across LANs (**L**ocal **A**rea **N**etworks) and WANS (**W**ide **A**rea **N**etworks) where the traffic needs to be secured from being captured as it crosses the wire (goes from one computer to another).

IPSEC is an open is an open standard that uses multiple protocols to provide various functions:

* Authentication Header (AH) provides connectionless data integrity and data origin authentication for IP datagramsand provides protection against replay attacks.
* Encapsulating Security Payload provides confidentiality, connectionless data integrity, data origin authentication, an anti-replay service, and limited traffic-flow confidentiality. This is the recommended protocol for all new tunnels
* Internet Security Association and Key Management Protocol (ISAKMP) provides a framework for authentication and key exchange,  the most often protocold used to perform this are Internet Key Exchange IKE and the newer IKEv2.  The purpose is to generate the security associations (SA) with the bundle of algorithms and parameters necessary for AH and/or ESP to perform their functions.

The SA allows the communicating parties to establish and share security attributes including algorithms and keys. Before being able to establish the initial SA the communicating parties need to be able to authenticate themselves to the other party. This is either done via a certificate, or more often by using a pre-shared key.  This key as a key with large entropothy that the parties have shared with each other when initially setting up the tunnel thus the key had been pre-shared.

If using IKE there are two `modes` of setting up the SA. They are called `main mode` and `aggressive mode`.  Aggressive mode will send a plain text hash of the preshared key, and for this reason it is not recommended to use this option.

###  Today's Drama.

Today, I had the joy of trying to bring up an IPSEC tunnel between a Fortinet Fortigate in our secure network and a Cisco ASA runing on a customer's site. To make things even more fun, the Cisco was running a firmware version that was release when our ancestors were runing around clubbing baby seals to make slippers.

Our equipment is modern, and it was well known that our equipment could run the values that we were suggesting to the customer, nothing that extreme Phase one was IKEv2 with AES256-SHA256. Phase two was again AES256-SHA256 and DH 5 (this was the maximum that the customer could support).

We could get Phase one to start and establish, but the far end would not accept our proposal. After about going back about twenty years and finding the ASA manual pages for using a VTI to establish. It turns out that even though the configuration for the VTI was accepted as being a suitable settings we had do downgrade our encryption to the time of clubbing those poor baby seals.

As soon as we set the ISKAMP to IKE and the Phase one to AES256 3DES and Phase two to AES256 SHA with DH Group 5, the tunnel burst to life and we could exchange traffic.

Oh the joys of Network Engineering; there is nothing you can do except :rofl:

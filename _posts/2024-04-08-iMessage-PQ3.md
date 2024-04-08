---
layout: post
title:  "iMessage PQ3" 
date:   2024-04-08 09:30:00 +0200
---

Earlier this year, [we announced iMessage PQ3](https://security.apple.com/blog/imessage-pq3/), Apple's formally-verified protocol for end-to-end encryption that amongst other updates provides post-quantum protections against “*Harvest Now, Decrypt Later*” attackers.

![iMessage PQ3](/blog/img/pq3/RWC-iMessagePQ3-Conclusion.jpeg)

In a [previous blog entry, I talked about the Double Ratchet & the Quantum Computers](https://www.fredericjacobs.com/blog/2016/04/07/qc-axolotl/), concluding that Double Ratchet was '*Quantum Annoying*' and required an attacker to record almost every message to be able to continue decrypt a conversation due to the key continuity property of the public-key ratchet. 
Post-quantum ratcheting; however, still required some tricks to be practical in order to not trade post-compromise security for quantum-resistance.

In the [Real World Crypto 2024](https://rwc.iacr.org/2024/program.php) Invited Talk: *Designing iMessage PQ3: Quantum-Secure Messaging at Scale*, we cover the design of the iMessage PQ3 protocol, including the delicate balancing act between crytographic overhead from larger post-quantum parameters and tightness of security properties.

<iframe src="https://invidious.fdn.fr/embed/RVbHElGe518" width="700"
      height="480"
      frameborder="0"
      webkitallowfullscreen
      mozallowfullscreen
      allowfullscreen></iframe>

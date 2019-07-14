---
layout: post
title:  "Double Ratchet & the Quantum Computers"
date:   2016-04-07 15:00:00 +0200
---

*The double ratchet [was previously known](https://whispersystems.org/blog/signal-inside-and-out/) as the Axolotl ratchet.*

With recent developments in quantum information processing and computation, there has been a lot of chatter about moving to post-quantum cryptography. The [NSA](/blog/2016/01/27/NSA-QC/) and NIST have already called for the standardization of post-quantum crypto and its swift adoption.

Replacing some primitives is easier than others. Doubling symmetric key sizes suffices to counter the key space search speedup provided by Grover search which requires work proportional to the square root of the size of the key space.

Replacing public-key systems appears to be a more challenging, requiring to move to new primitives who derive their hardness from entirely different problems. Most of those problems have been around for some time and the hardness from the general problem is reasonably well understood. But when it comes to [instantiating specific instances](http://eprint.iacr.org/2016/351) and working on side-channel resistant implementations, we're still in infancy.

Other factors such as significantly longer [key sizes](https://en.wikipedia.org/wiki/Post-quantum_cryptography#Key_size_table) are hindering adoption. Whether a trade-off is acceptable or not depends on the context. Hash-based signatures are a very elegant construction that could already be used when signature sizes are negligible compared to the size of the signed content such as software updates. [Adam Langley](https://twitter.com/agl__) wrote a [brilliant post explaining how they work](https://www.imperialviolet.org/2013/07/18/hashsig.html).

<br>

| Algorithm             | Public key size (bits) &nbsp;&nbsp;&nbsp;&nbsp; | Private key size (bits) |
|:----------------------: |:----------------------:|:-----------------------:|
| Ring-LWE              | 6595                   | 14000                   |
| NTRU                  | 6130                   | 6743                    |
| Hash signature        | 36,000                 | 36,000                  |
| Goppa-based McEliece  | 8,373,911              | 92,027                  |
{: align="center"}

<br>

Some mobile messaging apps are experimenting with post-quantum primitives such as [PQChat](https://post-quantum.com/pqchat) that is using McEliece for message encryption. While McEliece is a nice primitive, the PQChat message encryption protocol has not been independently studied and documented. End-to-end encryption protocols such as OTR or the double ratchet use ephemeral keys extensively to provide strong perfect forward secrecy. 8Mb public keys are making it hard to engineer any kind of protocol with similar forward secrecy features that would work well on mobile, knowing that the double ratchet sends a new ephemeral DH key with every message.

This led me to think about what protection the double ratchet actually provides against an adversary who can use a quantum computer to break the ratchet's DH primitive.

If you're not familiar with the double ratchet, the Open Whisper Systems blog has [a very good post](https://whispersystems.org/blog/advanced-ratcheting/) about it. As its name suggests, it combines two kinds of ratchets, namely a Diffie-Hellman ratchet and a hash-iterated one.

The ratchet is initialized with a `RootKey` that is derived from a key exchange, in the case of Signal or WhatsApp, that is the [3DH key exchange](https://whispersystems.org/blog/simplifying-otr-deniability/).

```
MK      CK      RK      CK      MK
--      --      --      --      --
                |
                |
      PRF(RK, ECDH(A1,B0))
               /|
              / |
             /  - PRF(RK, ECDH(A1,B1))
     CK-A1-B0   |\
         |      | \
MK-0 ----+      |  \
         |      |   CK-A1-B1
MK-1 ----+      |       |
         |      |       +---- MK-0
MK-2 ----+      |       |
                |       +---- MK-1
```

An important fact is that each new `RootKey` is derived through a PRF from a previous `RootKey` and the output of DH key agreement. The fact that it is derived, not only from a DH agreement, but also from a previous value is a valuable mitigation when we think about the threat posed by quantum computers. If the PRF used for the derivation is secure, then even if the adversary can break the ephemeral DH part, he won't be able to recover the new `RootKey` without the previous one.

A consequence of that design, is that an adversary has to **record every single message since the session initialization or he loses his capability to decrypt any subsequent messages** for future messages, even if he can continue to break the ephemeral DH. If the adversary didn't intercept the 3DH key agreement, he would not be able to decrypt any subsequent messages.

That's a really cool property of the protocol. ZRTP has similar property if [key continuity](http://zfoneproject.com/faq.html#keycontinuity) is implemented (not the case in Signal) but most other protocols (MTProto, SSH, Threema ...) do not have that feature.

While I do think it's important to move to new primitives soon, cryptographic protocols can be used to mitigate the risk of anyone being able to break your public-key primitives by relying on well-proven symmetric key systems. Most people compare products solely on the primitives or key sizes, making abstractions of protocols and implementations, which are the global picture that truly matters.

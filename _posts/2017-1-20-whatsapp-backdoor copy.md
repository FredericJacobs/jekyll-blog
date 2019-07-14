---
layout: post
title:  On the "WhatsApp backdoor", Trade-Offs and Opportunistic Authentication
date:   2017-01-20 14:00:00 +0100
---

A week ago, The Guardian broke an "exclusive" story titled ["WhatsApp backdoor allows snooping on encrypted messages"](http://web.archive.org/web/20170113123010/https://www.theguardian.com/technology/2017/jan/13/whatsapp-backdoor-allows-snooping-on-encrypted-messages).

### The Reporting

My first reaction when I skimmed through the story is that this was nothing new . The Guardian's source had already blogged about it on the [16th of April](https://tobi.rocks/2016/04/whats-app-retransmission-vulnerability/) and given [a lightning talk at CCC](https://youtu.be/VmgfXkVQwpc?t=2903) explaining the issue. Other people in the security community had already looked into this behavior too. Running the story as "exclusive" and sending a push notification to all subscribers as if it were breaking time-sensitive news, while this was known in the security community for months, seems disingenuous.

The other issue I had with the reporting of this story was the framing that this was a backdoor. This claim has been debunked by now, not matching the [technical definition of a backdoor](https://www.zdziarski.com/blog/?p=6077). The Guardian ended up [rephrasing their headline](http://web.archive.org/web/20170120044416/https://www.theguardian.com/technology/2017/jan/13/whatsapp-backdoor-allows-snooping-on-encrypted-messages) to reflect that this was not a backdoor. While they did change that important categorization, they left the reaction and analysis from the people who commented on the story, largely unchanged.

Original:
> Professor Kirstie Ball, co-director and founder of the Centre for Research into Information, Surveillance and Privacy, called the existence of a **backdoor** within WhatsApp’s encryption “a gold mine for security agencies” and “a huge betrayal of user trust”

After adjusting headline:
> Professor Kirstie Ball, co-director and founder of the Centre for Research into Information, Surveillance and Privacy, called the existence of a **vulnerability** within WhatsApp’s encryption “a gold mine for security agencies” and “a huge betrayal of user trust”.

While I do agree that a backdoor would have been a huge betrayal of user trust, I do not agree that this particular issue was a betrayal of user trust. Unlike with a backdoor, subvert purposes are improbable and the edited story should have reflected that.

### The Trade-Off

Message encryption is maturing as a field and has some pretty elegant solutions deployed in "the real world", "usable authentication" is still an open problem.

WhatsApp is known for being [very reliable](http://www.erlang-factory.com/upload/presentations/558/efsf2012-whatsapp-scaling.pdf) and their users love them for that. They decided that unread messages, should be retransmitted upon a change of phone number, matching the behavior they had before adopting end-to-end encryption or any "cloud-based" messenger. Instead of losing the messages that were sent and never read, WhatsApp will automatically retransmit them to the recipient's new phone, without letting the sender verify the new "security code" first. Since [phone networks, that are used for authentication by mobile messaging apps, are insecure](https://www.fredericjacobs.com/blog/2016/04/29/more-on-sms-logins/) or WhatsApp servers could get hacked, this leads to some practical attacks that would lead to the compromise of a streak of unread messages upon retransmission.

You can agree that this was a reasonable trade-off to make in the name of reliability. But such security trade-offs are nothing new, and they are important. When Signal came around, many praised it for adopting a TOFU (Trust On First Use) model. Most "secure messaging" clients up to then, the biggest one being OTR, had been bugging users about fingerprint verification, significantly hurting the adoption of end-to-end encryption. With Signal’s TOFU model, keys are implicitly trusted on the first use. Peers may then confirm that trust by verify their keys through an out-of-band channel. That's a potential security risk. Does that make Signal backdoored or inherently insecure? No. It was a trade-off that made people actually use end-to-end encryption.

You can definitely discuss the merits of such trade-offs but dismissing them as backdoors will just drive people away to unencrypted or opportunistically encrypted solutions.

WhatsApp transitioned from plaintext to opportunistic encryption to all the messages, from over a billion people, being encrypted by default. Then, once that was complete, enabled the phase we are currently in of "**opportunistic authentication**" until we find a scalable solution to the authentication problem.

Organizations like [SimplySecure](https://simplysecure.org), or mailing lists such as Trevor Perrin's [Secure Messaging mailing](https://moderncrypto.org/mailman/listinfo/messaging) lists, promote great ideas for tackling these issues. Let's keep things moving forward to a place where we can all confidently say that we have "encrypted and authenticated communications" by default.

### Further readings

- [Key rotation, user experience, and crypto reporting](https://tonyarcieri.com/key-rotation-user-experience-crypto-reporting)
- [Google Launches Key Transparency While a Trade-Off in WhatsApp Is Called a Backdoor](https://www.eff.org/deeplinks/2017/01/google-launches-key-transparency-while-tradeoff-whatsapp-called-backdoor)

---
layout: post
title:  "On SMS Logins II : an example from Telegram in Russia"
date:   2016-04-30 08:30:00 +0200
---

A few months ago, I wrote [a post on SMS logins](/blog/2016/01/14/sms-login/) following a wave of attacks attempting to take over Iranian Telegram accounts. Recently, there have been new confirmed cases of attacks in both [Iran](https://twitter.com/Ammir/status/725754152677195777) and [Russia](https://www.facebook.com/kozlovsky/posts/10208948934790884).

[Oleg Kozlovsky](https://twitter.com/kozlovsky_en) is an opposition activist and the director of Vision of Tomorrow Center in Moscow.

In a [Facebook post written in Russian](https://www.facebook.com/kozlovsky/posts/10208948934790884), he explains how his Telegram account got hacked.

<iframe src="https://www.facebook.com/plugins/post.php?href=https%3A%2F%2Fwww.facebook.com%2Fkozlovsky%2Fposts%2F10208948934790884" width="100%" height="762" style="border:none;overflow:hidden" scrolling="no" frameborder="0" allowTransparency="true"></iframe>

At 2:25 AM MSK on Friday April 29th, [MTS](https://en.wikipedia.org/wiki/MTS_(network_provider)) (Russia's largest mobile operator), disables SMS service on his phone number. After 15 minutes of disconnection, someone tries to log into Oleg's Telegram account. The process for authorizing a new device to a Telegram accounts simply consists of entering a verification code received by SMS when 2-step authentication is not enabled. At 3:08AM the attacker enters the authorization code and gets access to Oleg's account. The Telegram login notification reveals that the attacker is using a [Telegram command-line interface](https://github.com/vysheng/tg) with an IP of a [Tor exit node](https://atlas.torproject.org/#details/6C143720FFF8469EF6A5C5B4066366340CF6C0D1).

![The Tor Exit Node used to hack into Oleg's Telegram account.](/blog/img/tghack/telegram-hack-exit-node.png)

Four minutes later, [the same Tor exit node IP address](https://twitter.com/alburov/status/725939782191206402?lang=en), logs into [George Alburov's](https://twitter.com/alburov?lang=en) Telegram account. It is currently unknown if other accounts were breached that night. Messages logs (excluding Telegram Secret Chats) and contacts information were likely the information seeked by the attacker.  

Because Oleg was sleeping during the attack, he only noticed the notification in the morning. He called MTS Customer Support, after consulting with the "expert department", they told him that the security service was involved and that SMS was indeed disabled from 2:25 AM to 4:55 AM. Oleg's request for talking to the security service was denied.

### SORM, Russia's CALEA

Given the political activities of both targets, the most probable theory is that the attacker was a Russian governmental agency working in collaboration with MTS under the SORM law.

[SORM](https://en.wikipedia.org/wiki/SORM), standing for Система Оперативно-Розыскных Мероприятий which translates to *System for Operative Investigative Activities*, is Russia's [CALEA](https://en.wikipedia.org/wiki/Communications_Assistance_for_Law_Enforcement_Act). SORM has been around since 1996 to enable wiretaps of telephone communications. It evolved over the years to allow broader access to electronic communications.

If SMS interception capabilities exist, why would they go through the hassle of disconnecting the SMS service? One possible theory is that they hoped the intrusion would be unnoticed if the targets didn't receive the text message. This would allow them to proceed to data exfiltration from the Telegram account without the risk of being shut down halfway through. The issue with this theory is that Telegram notifies you on your primary device when a new device is logged in. So despite not having seen the new activation SMS at the time, Oleg could have detected it instantly because of the Telegram notification. Additionally, attackers didn't log themselves out, remaining visible after Oleg woke up in Telegram's [active sessions](https://telegram.org/blog/sessions-and-2-step-verification). So it remains unclear to me what the advantage was over a regular interception. If they hoped to be undetected, this was really sloppy and unprepared.


### Phone numbers for authentication

Most messaging apps and some 2-step authentication providers rely on phone numbers as an authentication factor. Why are so may services using identifiers that are assigned by mostly state-owned companies and that are subject to CALEA-like laws and awfully insecure (IMSI catchers/SS7) against nation state adversaries?

Using phone number identifiers is a significant usability and growth advantage. You can leverage the social graph that people already have on their phone. After signup, they can instantly see who uses the service.

It also enables service providers to not have to store a buddy list unencrypted on servers since the social graphs are in the phone's address book.
(Note: Some applications like Telegram store a copy of your address book unencrypted on their servers, not using that property.)

But it also turns out that it's really convenient to rely on someone else key management infrastructure since usable authentication is a hard problem to solve. Most telcos have (insecure) processes to re-assign you your number when you lose your phone or if someone steals your SIM card keys. Those processes rely on providing some proof-of-identity. But that can be spoofed too. Last year, two investigative journalists at [Novaya Gazeta](https://en.wikipedia.org/wiki/Novaya_Gazeta) were told that their SIM cards were reissued by unknown people.

Those processes clearly fall short. As we cannot solely rely on phone numbers to authenticate, Pavel Durov recommended *users from troubled countries* to enable 2-step authentication.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Users from troubled countries: make sure you have 2-step verification enabled – in Telegram and other services <a href="https://t.co/w81h4PjFv4">https://t.co/w81h4PjFv4</a></p>&mdash; Pavel Durov (@durov) <a href="https://twitter.com/durov/status/726104566899609600">April 29, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I believe that mitigation is a good first step, but insufficient, especially for people in troubled countries. An attacker might also suspect a user to have enabled 2-step authentication and therefore target some of his contacts to access the chat logs.

Such an attack is significantly mitigated, and deterred, by the adoption of end-to-end encryption. The detection of new devices can be cryptographically enforced and previous message history not accessible to an attacker capable of intercepting an SMS.

I think the lesson from this might be something like:

> Always use end-to-end encryption. As the underlying authentication layer can be spoofed, verify fingerprints for important communications.

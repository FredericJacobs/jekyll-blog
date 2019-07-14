---
layout: post
title:  check.torproject.org & IPv6
date:   2016-08-17 22:30:00 -0700
---

*TLDR*: Exit nodes with IPv6 addresses connect to check.torproject.org over IPv6, causing the IP not to be recognised as being one of an exit node. This triggers a scary “Sorry. You are not using Tor.” warning.

- - -

When firing up [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) today and checking if the browser was properly using Tor, I got the following scary message.

![Scary IPv6 Tor Check](/blog/img/tor/ipv6TorCheck.png)

### What I assume happened

Confused, I quickly checked if that was my IPv6 address. Thankfully it wasn’t. Tor browser was correctly proxying traffic over the Tor network but the exit node resolved [check.torproject.org](check.torproject.org) over IPv6. A DNS lookup for [check.torproject.org](check.torproject.org) gives:

```
~ ❯❯❯ host check.torproject.org
check.torproject.org is an alias for chiwui.torproject.org.
chiwui.torproject.org has address 138.201.14.212
chiwui.torproject.org has IPv6 address 2a01:4f8:172:1b46::abba:20:1
chiwui.torproject.org mail is handled by 10 eugeni.torproject.org.
```

What likely happened is that the exit node connected to [check.torproject.org](check.torproject.org) over IPv6. Since Tor doesn’t have full support for IPv6 yet, the exit node appeared as unknown, hence triggering this warning.

### Quick fix

I think the warnings are scary and we shouldn’t be telling users to just ignore them. A quick easy fix would be to simply not have any IPv6 DNS records in [check.torproject.org](check.torproject.org) as long as IPv6 isn’t fully supported.

Any thoughts or suggestions? Discuss on [Tor-Dev](https://lists.torproject.org/pipermail/tor-dev/2016-August/011304.html).


*Update (24th of August)*: The AAAA record [was removed](https://trac.torproject.org/projects/tor/ticket/19940), waiting for a more broader IPv6 support in Tor.

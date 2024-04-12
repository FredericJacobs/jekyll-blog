---
layout: post
title:  "DNS Wildcards & GitHub Pages" 
date:   2024-04-11 11:30:00 +0200
---

**TLDR: Don't setup DNS wildcards for GitHub Pages. Spammers are paying to use GitHub Pages infrastructure and squatting those subdomains to deliver SEO spam.**

When I set up my blog a few years back, I wanted something that had the lowest maintenance possible. I didn't want to have to worry about updating servers, dealing with deployments and load balancing. [GitHub Pages](https://pages.github.com) seemed like the solution I was looking for.

Anticipating that I might have wanted to deploy to subdomains in the future, I setup a DNS wildcard A entry for GitHub Pages. While there's now [a warning against doing that](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site), that wasn't the case in 2016 when I set this up.

> Warning: We strongly recommend that you do not use wildcard DNS records, such as *.example.com. These records put you at an immediate risk of domain takeovers, even if you verify the domain. For example, if you verify example.com this prevents someone from using a.example.com but they could still take over b.a.example.com (which is covered by the wildcard DNS record). For more information, see "Verifying your custom domain for GitHub Pages."

To this day, there's nothing that prevents this to be setup either.

Earlier this week, I noticed strange URLs (eg. tasteful.fredericjacobs.com) being indexed by Google on my subdomains, that caused me to investigate and quickly linked it back to this DNS wildcard entry, that was allowing attackers to generate GitHub Pages on my subdomains.

I searched for the repositories for these GitHub Pages but they seem to be using **paid** GitHub accounts since it allows them to use GitHub Pages without having to open-source the repositories.

After reporting this to GitHub, they responded this is an intentional design decision, and is working as expected.

I have since removed the wildcard DNS entry, addressing the issue for my domain. But as far as I can tell, the accounts of the attackers are still paying GitHub for delivering spam pages on  many other domains. It would be nice to see GitHub take action and notify users who setup wildcards for GitHub Pages, and prevent these from being setup in the future.

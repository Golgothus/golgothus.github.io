---
title: "Google Dorking"
date: 2020-12-25T01:02:30-06:00
draft: false
author: "Golgothus"
description: "My attempt at completing the TryHackMe.com Beginner Path - Google Dorking room"
summary: "TryHackMe Beginner Path - Google Dorking write-up."
tags: ["TryHackMe","Linux"]
featuredImage: "/images/city.png"
categories: ["THM - Beginner Path"]
images: ["images/logo.jpg"]
#- name: featured-image-preview
# src: /assets/images/featured-image-preview.jpg
---

# Google Dorking

| Keyword | Function|
| ------------ | -----------|
| User-agent| Specify the type of "Crawler" that can index your site (the asterisk being a wildcard, allowing all "User-agents" |
| Allow | Specify the directories or file(s) that the "Crawler" can index |
| Disallow | Specify the directories or file(s) that the "Crawler" cannot index |
| Sitemap | Provide a reference to where the sitemap is located (improves SEO as previously discussed, we'll come to sitemaps in the next task) |

**user-agent:** **\***

**disallow:** /*.ini$

**sitemap:** www.mywebsite.com/sitemap.xml

> .ini files useful for Windows configuration \
> .CONF are useful for *nix configuration

| Term | Action |
| --- | --- |
| filetype | Search for a file by its extension (e.g. PDF) |
| cache | View Google's Cached version of a specified URL |
| intitle | The specified phrase MUST appear in the title of the page |

> -inurl:htm -inurl:html intitle:”index of” apk

Google Proxy:
> [Google Web Light](//https://googleweblight.com/?lite_url=http://example.com) \
>[Google Translate](//http://translate.google.com/translate?sl=ja&tl=en&u=http://example.com/)

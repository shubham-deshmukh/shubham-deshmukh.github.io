---
layout: post
title: "Vulnhub: Kioptrix level 2 Walkthrough"
subtitle: "Difficulty: Simple"
# cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [Vulnhub, Kioptrix, level 2,walkthrough]
---

Hi Folks...
Before moving forward, please try harder and come back later. Still you are stuck then let get started.

## Objective
Gain the root access.

## Algorithm
- Locate the target machine : Host Discovery
- Find the entry points : Enumeration
- Gain access to the machine : Finding vulnerability and exploitation
- Escalate the privileges : Privilege Escalation
- Clean up : Post-Exploitation

### Host Discovery
Generally we know the target IP address. Since lab is set up as internal network and addresses are assigned by DHCP server. We need to find the target IP address.
![sudo netdiscover -r \<ip-range\>](/assets/img/vulnhub/kioptrix/lvl2 "Host Discovery using netdiscover")

At the heart of the struggle are two very different ideas of success—survival-driven and soul-driven. For survivalists, success is security, pragmatism, power over others. Success is the absence of material suffering, the nourishing of the soul be damned. It is an odd and ironic thing that most of the material power in our world often resides in the hands of younger souls. Still working in the egoic and material realms, they love the sensations of power and focus most of their energy on accumulation. Older souls tend not to be as materially driven. They have already played the worldly game in previous lives and they search for more subtle shades of meaning in this one—authentication rather than accumulation. They are often ignored by the culture at large, although they really are the truest warriors.

A soulful notion of success rests on the actualization of our innate image. Success is simply the completion of a soul step, however unsightly it may be. We have finished what we started when the lesson is learned. What a fear-based culture calls a wonderful opportunity may be fruitless and misguided for the soul. Staying in a passionless relationship may satisfy our need for comfort, but it may stifle the soul. Becoming a famous lawyer is only worthwhile if the soul demands it. It is an essential failure if you are called to be a monastic this time around. If you need to explore and abandon ten careers in order to stretch your soul toward its innate image, then so be it. Flake it till you make it.

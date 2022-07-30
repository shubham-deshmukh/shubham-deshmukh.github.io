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

#### Host Discovery
Generally we know the target IP address. Since lab is set up as internal network and addresses are assigned by DHCP server. We need to find the target IP address. Host IP address is ```10.10.10.2```
```bash
sudo netdiscover -r 10.10.10.1/24
```
![sudo netdiscover -r \<ip-range\>](/assets/img/vulnhub/kioptrix/lvl2/kioptrix_lvl2_host_discovery.png "Host Discovery using netdiscover")

#### Enumeration
It is the most important step to achieve our objective. More the information about the target more the possibilities of attack vectors. Quick nmap scan reveals the information about the various common services running on the target.
```bash
nmap --vv -sV -sC -A -T5 -oN nmap_fast 10.10.10.4
```
![nmap results](/assets/img/vulnhub/kioptrix/lvl2/nmap-1.png "nmap scan")
![nmap results](/assets/img/vulnhub/kioptrix/lvl2/nmap-2.png "nmap scan")



A soulful notion of success rests on the actualization of our innate image. Success is simply the completion of a soul step, however unsightly it may be. We have finished what we started when the lesson is learned. What a fear-based culture calls a wonderful opportunity may be fruitless and misguided for the soul. Staying in a passionless relationship may satisfy our need for comfort, but it may stifle the soul. Becoming a famous lawyer is only worthwhile if the soul demands it. It is an essential failure if you are called to be a monastic this time around. If you need to explore and abandon ten careers in order to stretch your soul toward its innate image, then so be it. Flake it till you make it.

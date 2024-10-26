---
title: 'Hacking PermX'
description: 'In this blog I hack my way through PermX'
heroImage: '/permx/thumbnail.png'
draft: true
pubDate: '2021-07-08'
tags: ['ctf', 'hacking']
---

I will be hacking the ctf PermX. I will be writing this blog out as I hack this machine to properly document 
everything. The first thing I will start with is gathering as much information as I can (Reconassince). Once I have done that
I will look to see if any of the information I have gathered could be used to get foothold of the target. Once I get foothold my next goal is privilege escalation.

## Information Gathering

Starting with the ip address the first thing I do is run a port scan to see what I can find. After running the scan I notice that the target is possibly running a website on port 80.

![Initial port scan](/permx/nmap_initial2.png)

The website on port 80 led me to a site called ELearning. After looking through the source code I didn't find anything. I started to numerating directories after looking through the source code but came up empty handed.
Next I moved on to enumerating subdomains and I found the subdomain lms.

![subdomain enumeration results](/permx/ffuf_sub.png)

## Foothold  

When I visit the lms subdomain I find a chamillo login page. I first tried to guess common credentials like admin:admin or admin:password123 
but was not able to login. I started looking at the source code to see if I could maybe find a software version or a links to another page.
The source code told me I was running chamillo version 1. 

![Chamillo](/permx/chamillo_and_sourcecode.png)

A quick google search shows us that there is an exploit for unrestricted file uploads **(CVE-2023-4220)**. The exploit requires a php payload file for our reverse shell. It also provides additional information that's needed like the directory to visit the payload file once its uploaded.

![Exploit I found to use](/permx/exploit.png)

I started by creating my payload file and giving my poc script executable permissions. Once I did that I ran the script adding the other information
needed to run the exploit. And we finally have access to the target machine. I go ahead and upgrade to a tty terminal.

![Access](/permx/access.png)

## Privilege Escelation

Heading to the home directory I find the user mtz which I have no permissions to access. Taking note of the user I worked my way to the web servers root directory. After looking around the file system for a minute I found some credentials in the **(/var/www/chamilo/app/config/var/www/chamilo/app/config/configuration.php)** It had a username and password.

![Configuration File](/permx/creds.png)

I then tried to ssh as the mtz user we found earlier using the password I had found. Success! Im now logged in as the user mtz. My next objective is to see what I can find under this users home directory. From the user's home directory I get my first Flag.

![mtz user flag](/permx/flag1.png)

## Gaining Root Access

Now that we have gotten our first flag we need to escalate our privileges to root leve so we can get the next flag. I start by checking if there's any file that allows sudo access.  

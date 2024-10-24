---
title: 'Hacking PermX'
description: 'In this blog I hack my way through PermX'
heroImage: '/permx/thumbnail.png'
draft: false
pubDate: '2021-07-08'
tags: ['ctf', 'hacking']
---

I will be hacking the ctf PermX. I will be writing this blog out as I hack this machine to properly document 
everything. The first thing I will start with is gathering as much information as I can (Reconassince). Once I have done that
I will look to see if any of the information I have gathered could be used to get foothold of the target. Below we start with the targets ip address.

![target ip](/permx/target1.png)

## Information Gathering

Now that I have the ip address the first thing I do is run a port scan to see what I can find. After running the port scan I notice that the target is possibly running a website on port 80.

![Initial port scan](/permx/nmap_initial2.png)

Upon putting the ip address in the website field I got to a website called Elearning. Looking through the source code of the website I didn't find anything interesting. I started to click through the other availble links to see if I could find something on another page. Unfortunatly I don't find anything. 


![The discovered website](/permx/elearningsite.png)

I started enumerating directories to see if I could find any pages that were not made public but to my luck I hit another dead end.
Next I moved on to enumerating subdomains and I found the subdomain lms.

![subdomain enumeration results](/permx/ffuf_sub.png)




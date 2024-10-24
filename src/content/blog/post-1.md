---
title: 'Hacking PermX'
description: 'In this blog I hack my way through PermX'
heroImage: '/permx/thumbnail.png'
published: false
pubDate: '2021-07-08'
tags: ['ctf', 'hacking']
---

I will be hacking the ctf PermX. I will be writing this blog out as I hack this machine to properly document 
everything. The first thing I will start with is gathering as much information as I can (Reconassince). Once I have done that
I will look to see if any of the information I have gathered could be used to get foothold of the target. Below we start with the targets ip address.

![target ip](/public/permx/target1.png)

## Information Gathering

Now that we have the ip address the first I do is run a port scan to see what we can find. After running the port scan we notice that the target is running a webserver on port 80. So we know that when we enter the ip address in our browser we get a website.

![nmap initial scan](/public/permx/nmap_initial2.png)
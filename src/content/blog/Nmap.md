---
title: Nmap
description: Going over nmap and how it workd.
heroImage: /nmap/thumbnail.png
draft: true
pubDate: 2024-12-13
tags:
  - hacking
---

# Nmap 

Nmap is a very great tool for port scanning but before writing this post I didn't fully understand how Nmap worked. I feel I spent a lot of time trying to learn Nmap so this post is aimed at trying to explain Nmap quickly for anybody wanting to learn. 
# Host Discovery 

Nmap allows us to easily run a sweep scan against a network. A sweep-scan, in simple terms, is just a ping scan against all the devices on a network to see if they're up. On a local network, Nmap will default to an ARP scan. You can initiate a ping scan with the _**-sn**_ flag. The command below runs a sweep scan against the whole subnet but can be used for an individual host.


```
> nmap -sn 10.10.10.1/24
```

# Note About Ping Scans

 One thing to remember is that most windows firewalls block ICMP echo requests, and ping scans can be turned off with the ***-Pn*** flag.  Because by default most of the scans start with a ping scan we may have to disable the ping scan manually.

```
> nmap -Pn scanme.nmap.org
```

# Plain Scan 

When conducting a plain Nmap scan, you start by initiating Nmap with just the host as an argument. The host can be either an IP address or a domain. The default scan without sudo privileges is a full TCP connect scan, which we will get to in a minute. Below is an example of how the command would look. You can also run a full connect scan with the _**-sT**_ flag.

```
> nmap scanme.nmap.org                               
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-12-13 03:41 CST
Nmap scan report for scanme.nmap.org (45.33.32.156)
Host is up (0.071s latency).
Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
53/tcp   open  domain
80/tcp   open  http
9929/tcp open  nping-echo

Nmap done: 1 IP address (1 host up) scanned in 15.74 seconds
```

If we look at the scan results, we can break down what Nmap does under the hood line by line, in the following order:

1. Nmap starts the scan.
2. Nmap attempts to perform a DNS resolution, matching the domain scanme.nmap.org to its IP address.
3. Nmap performs a sweep-scan to see if the host is up.
4. Nmap attempts to connect to the target ports using a full three-way handshake.
## Full TCP Connect Scan/Full-Scan

For each port, Nmap will send a _**SYN**_ packet. When a port responds with a _**SYN-ACK**_, Nmap responds with a _**RST-ACK**_ packet to reset the connection and marks the port as open. An _**(ACK-RST)**_ packet combines both the _**RST**_ and **ACK** flags, acknowledging the connection and resetting it simultaneously. The port is marked as closed if it receives an _**RST**_ packet, and as filtered if there’s no response, possibly indicating a firewall.

#### How Nmap Handles Each Response For A Full-Scan

- **(SYN-ACK)**: Nmap completes the three-way handshake, resets the connection, and marks the port as open.
- **(RST)**: Nmap marks the port as closed.
- **(No response)**: Nmap marks the port as filtered.
## The SYN SCAN/Half-Scan

A half-scan is considered more stealthy than a full-scan because it does not complete the full three-way handshake. Because of this, it creates fewer logs, making it a little more difficult for firewalls to detect. A half-scan is the same as a full-scan with one key difference—it doesn't complete the connection. When Nmap receives a _**SYN-ACK**_, it doesn't respond with an _**ACK/RST**_ packet to complete the full three-way handshake. Instead, Nmap responds with an _**RST**_ packet, immediately resetting the connection. You can run a SYN scan by using the _**-sS**_ flag.

```
> nmap -sS scanme.nmap.org
```

#### How Nmap Handles Each Response For A SYN Scan

- **(SYN-ACK)**: Nmap resets the connection and marks the port as open.
- **(RST)**: Nmap marks the port as closed.
- **(No response)**: Nmap marks the port as filtered.

# Detecting Firewalls with the ACK-Scan

The ACK scan works by sending ACK flags to the target ports. When the ports receive the ACK flag, whether they're open or closed, they will respond with a reset flag. A response with the reset flag would mark the port as unfiltered. No response would mark it as filtered. In simple terms, a response means unfiltered, and no response means filtered. By running this scan, you can get an idea of firewall rules and what ports are blocked or not. You can run this scan with the ***-sA*** flag.

```
> nmap -sA scanme.nmap.org
```

# Firewall Evasion DNS Spoofing

A lot of firewalls ignore DNS traffic because DNS is essential for most services. Since DNS is a trusted protocol using port 53 **(the DNS port)** as the source port can help evade firewall detection. You can set the source port with the ***-g*** flag. 

```
> nmap -sS -g 53 scanme.nmpa.org
```

# Firewall Evasion Using Decoys

You can use Decoy IPS when running an Nmap scan making it harder for the firewall determine where the traffic is coming from. You can have up to 5 decoys at once. You can create decoys using the ***-D RND:<1-5>*** flag where in the brackets you have the option between one and five Decoys.

```
> nmap -sS -D RND:5
```

# Things I Didn't Cover


There are a few things I didn't get into, such as the version scan, which is exactly what it sounds like Nmap probing to get additional version information. There's also the script scan, which runs scripts against the ports to identify more details about them or even find vulnerabilities
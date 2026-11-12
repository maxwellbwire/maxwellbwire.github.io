---
title: "L2 MAC Flooding and ARP Spoofing: Exploring Layer 2 Attacks"
date: 2026-11-12 16:00:00 +0300
categories: [Cybersecurity, Network Security]
tags: [tryhackme, mac-flooding, arp-spoofing, mitm, ettercap, wireshark, tcpdump, nmap]
---

## Overview

As part of my cybersecurity training on TryHackMe, I completed the **L2 MAC Flooding & ARP Spoofing** room. This practical exercise focused on Layer 2 network attacks and demonstrated how weaknesses in network switching and Address Resolution Protocol (ARP) can be exploited to intercept and manipulate network traffic.

The lab helped me understand how attackers can position themselves between communicating devices, inspect traffic and potentially access information that should remain private.

## Tools Used

- **Nmap** – Network discovery and identification of active hosts.
- **tcpdump** – Capturing network traffic through the command line.
- **Wireshark** – Analysing captured packets and examining protocol details.
- **macof** – Generating random MAC addresses to demonstrate MAC flooding.
- **Ettercap** – Performing ARP spoofing and man-in-the-middle attacks, including traffic manipulation.

## 1. Network Discovery

I started by accessing the lab machine through SSH and inspecting its internal network interface.

```bash
ip addr show eth1
```

This helped me identify the internal IP address and network prefix. I then used Nmap to discover other active devices on the subnet.

```bash
nmap -sn 192.168.12.0/24
```

The scan identified other hosts on the network and provided a starting point for investigating their communication.

## 2. Passive Network Sniffing

Next, I used tcpdump to monitor traffic passing through the internal interface.

```bash
tcpdump -i eth1
```

I also saved packet captures for further analysis in Wireshark.

```bash
tcpdump -A -i eth1 -w /tmp/tcpdump.pcap
```

After transferring the PCAP file to my machine, I opened it in Wireshark to inspect the captured packets, identify communicating hosts and examine ICMP packet data.

This exercise showed me why packet analysis is useful when investigating network activity and troubleshooting communication between devices.

## 3. MAC Flooding

I then explored MAC flooding using `macof`.

The tool generates traffic with random source MAC addresses. This demonstrates how an attacker can attempt to overwhelm a switch's MAC address table and potentially cause traffic to be flooded beyond its intended destination.

I monitored the network with tcpdump while the flooding activity was running, then examined the captured traffic in Wireshark.

This helped me understand how MAC address table exhaustion can affect normal network switching behaviour.

## 4. ARP Spoofing and Man-in-the-Middle Attacks

The next exercise focused on ARP cache poisoning.

I used Ettercap to demonstrate an ARP spoofing attack:

```bash
ettercap -T -i eth1 -M arp
```

The first scenario demonstrated that ARP packet validation could prevent the MITM attack from succeeding. I then worked through another scenario where the attack was successful.

By analysing the intercepted traffic, I was able to identify HTTP requests, the requested file and authentication information being transmitted between hosts.

One important takeaway was how easily sensitive information can be exposed when services transmit data without adequate encryption.

## 5. Traffic Manipulation with Ettercap Filters

In the final section, I explored how Ettercap filters can be used to modify traffic rather than simply observe it.

I created a custom filter file and compiled it using:

```bash
etterfilter whoami.ecf -o whoami.ef
```

I then loaded the compiled filter when running Ettercap:

```bash
ettercap -T -i eth1 -M arp -F whoami.ef
```

The exercise demonstrated how an attacker with a MITM position could identify and replace specific commands in unencrypted traffic. I used this technique within the authorized lab environment to retrieve the final flag.

## Key Takeaways

This room helped me understand several important network security concepts:

- MAC flooding can affect how switches forward traffic when their MAC address tables are overwhelmed.
- ARP spoofing can allow an attacker to intercept communication between devices on a local network.
- Passive packet capture and analysis can reveal useful information about network activity.
- Unencrypted protocols can expose sensitive information, including authentication credentials.
- Ettercap filters demonstrate that intercepted traffic can be manipulated, not just monitored.
- ARP validation and encrypted communication help reduce the risk of these attacks.

## Final Thoughts

The biggest lesson from this exercise was understanding the difference between observing network traffic and actively manipulating it. Working with Nmap, tcpdump, Wireshark and Ettercap gave me practical experience investigating network behaviour and identifying weaknesses that could lead to information exposure.

As I continue developing my skills in cybersecurity, I want to build on this knowledge in network security assessments, vulnerability analysis and ethical hacking.

**Lab:** [TryHackMe – L2 MAC Flooding & ARP Spoofing](https://tryhackme.com/room/layer2)

*All activities were performed in the authorized TryHackMe lab environment.*

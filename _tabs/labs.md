
---
title: Labs
icon: fas fa-flask
order: 6
---

# Cybersecurity Labs & Practical Learning

This page documents my hands-on cybersecurity learning through Hack The Box Academy and TryHackMe. The labs have helped me develop practical skills in network reconnaissance, Linux, vulnerability assessment, web application security, penetration testing, and security analysis.

My approach is to understand how vulnerabilities work, practise identifying them in controlled lab environments, and document what I learn along the way.

## Hack The Box Academy

**Status: All listed modules completed**

### Getting Started
- **Focus:** Introduction to penetration testing and common security tools.
- **Tools:** Nmap, Gobuster, WhatWeb, SearchSploit, and Metasploit.
- **Skills developed:** Target reconnaissance, service enumeration, web directory discovery, and researching known vulnerabilities.

### Introduction to Networking
- **Focus:** Networking fundamentals and communication between systems.
- **Skills developed:** Understanding network protocols, IP addressing, and subnetting.

### Linux Fundamentals
- **Focus:** Linux command-line operations and system navigation.
- **Skills developed:** Working with the terminal, navigating the filesystem, and using common Linux commands.

### Vulnerability Assessment
- **Focus:** Identifying, analysing, and prioritising vulnerabilities.
- **Tools:** Nmap, Nessus, and OpenVAS/GVM.
- **Skills developed:** Network scanning, vulnerability discovery, understanding CVSS scores, and interpreting scan results.

### Metasploit Framework
- **Focus:** Learning the Metasploit Framework and its role in penetration testing.
- **Skills developed:** Understanding modules, auxiliary scanning, exploit workflows, and the importance of validating findings in an authorised lab.

## TryHackMe

**Status: All listed rooms and practical exercises completed**

### OWASP Top 10 2025 — Practical Web Security

Worked through practical exercises covering common web application security weaknesses.

- **Broken Access Control:** Tested insecure direct object references (IDOR) and unauthorised access to other users' data.
- **Authentication Failures:** Investigated account-handling and case-sensitivity weaknesses.
- **Logging and Alerting Failures:** Analysed access logs to identify suspicious activity and relevant request details.
- **Security Misconfigurations:** Examined API behaviour and error responses for information disclosure.
- **Software Supply Chain Failures:** Investigated application processing behaviour and debug-related information exposure.
- **Cryptographic Failures — AES-ECB:** Examined client-side JavaScript for exposed encryption keys and used CyberChef to decode and decrypt data.
- **Insecure Design:** Discovered API endpoints and examined how application assumptions could expose sensitive information.
- **Cryptographic Failures — XOR:** Analysed encrypted notes and tested the correct key to recover their contents.
- **Injection — Server-Side Template Injection (SSTI):** Tested template expressions and investigated server-side execution in a controlled environment.
- **Software or Data Integrity Failures:** Explored insecure Python pickle deserialization and how unsafe handling of serialized data can lead to security risks.

**Tools and techniques:** Browser developer tools, API inspection, CyberChef, source-code review, log analysis, and controlled input testing.

### Guided Pentest: Web
- **Focus:** Web application reconnaissance, testing, and vulnerability identification.
- **Tools:** Nmap and browser-based testing techniques.
- **Skills developed:** Service enumeration, investigating upload functionality, testing access controls, examining password-reset behaviour, and identifying information disclosure.

### Guided Pentest: Infrastructure
- **Focus:** Infrastructure penetration-testing methodology.
- **Skills developed:** Following a structured assessment process, investigating exposed services, and working through infrastructure-focused security testing in a controlled lab.

### Attacktive Directory
- **Focus:** Active Directory enumeration and Windows domain security.
- **Tools and techniques:** Nmap, Kerbrute, Impacket's GetNPUsers, Hashcat, SMB enumeration, and Evil-WinRM.
- **Skills developed:** Enumerating domain services, investigating Kerberos-related weaknesses, accessing exposed shares, and understanding domain privilege escalation concepts such as DCSync and NTDS.DIT.

### RecruitX
- **Focus:** Web application security and insecure file-upload handling.
- **Skills developed:** Inspecting application configuration, understanding how file extensions are handled, and investigating the security implications of server-side processing of uploaded files.

### Careers in Cyber
- **Focus:** Cybersecurity career pathways and the roles within the industry.
- **Skills developed:** Building a clearer understanding of cybersecurity specialisations and professional development paths.

### DNS in Detail
- **Focus:** Domain Name System fundamentals.
- **Skills developed:** Understanding DNS records, domain resolution, and the role DNS plays in network communication and reconnaissance.

### Passive Reconnaissance
- **Focus:** Collecting information about targets without directly interacting with their systems.
- **Tools and techniques:** Public information sources and Shodan.
- **Skills developed:** Identifying publicly exposed assets and understanding how reconnaissance supports security assessments.

### Threat Intelligence Tools
- **Focus:** Using threat intelligence resources to investigate suspicious indicators.
- **Skills developed:** Understanding how threat intelligence supports the investigation and assessment of potentially malicious infrastructure and activity.

### Python Basics
- **Focus:** Python programming fundamentals.
- **Skills developed:** Building foundational programming knowledge that supports scripting, automation, and cybersecurity tasks.

## Tools & Technologies Practised

The labs have given me practical exposure to the following tools and technologies:

- **Network reconnaissance:** Nmap, Gobuster, WhatWeb
- **Vulnerability assessment:** Nessus, OpenVAS/GVM, SearchSploit
- **Penetration testing:** Metasploit Framework
- **Web application security:** Browser developer tools, API testing, SSTI investigation
- **Active Directory and Windows security:** Kerbrute, GetNPUsers, Hashcat, SMB, Evil-WinRM
- **Cryptography analysis:** CyberChef, Base64 decoding, AES-ECB, XOR
- **Operating systems and scripting:** Linux command line, Python
- **Threat intelligence and reconnaissance:** Shodan and public intelligence resources

## Key Takeaways

These labs have helped me connect networking and technical support experience with practical cybersecurity concepts. I have gained experience investigating services, examining application behaviour, interpreting errors and logs, and identifying weaknesses in controlled environments.

Not every exercise worked on the first attempt. Troubleshooting errors, checking assumptions, and revisiting documentation have been important parts of my learning process.

I continue to build on this foundation through practical labs, technical write-ups, and cybersecurity projects.

**Note:** All security testing described here was conducted in authorised training environments.

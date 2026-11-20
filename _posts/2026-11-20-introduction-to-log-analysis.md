---
title: "Introduction to Log Analysis: Investigating Suspicious Activity with TryHackMe"
date: 2026-11-20 09:00:00 +0300
categories: [Cybersecurity, Blue Team]
tags: [log-analysis, tryhackme, linux, cyberchef, sigma, yara, soc]
---

# Introduction to Log Analysis

Log analysis is an important part of security monitoring and incident investigation. System and application logs record activity that can help analysts understand what happened, identify unusual behaviour, and investigate potential security incidents.

I recently completed the **Introduction to Log Analysis** room on TryHackMe as part of my Security Analyst training. The exercises gave me practical experience working with log files, extracting useful information, and recognising patterns that may indicate suspicious activity.

## What I worked on

### 1. Understanding logs and timelines

I explored different types of logs and their role in security investigations. I also learned how timelines help analysts reconstruct events by arranging activity from different sources in chronological order.

A consolidated timeline, sometimes called a *super timeline*, can help connect events and reveal patterns that may not be obvious when examining individual log files.

### 2. Analysing logs using Linux command-line tools

I practised using Linux commands to search, filter, extract, and count information in log files.

Some of the commands I used included:

```bash
# View the contents of a log file
cat apache.log

# Search for a specific pattern
grep "login.php" apache.log

# Extract requested URLs
cut -d ' ' -f 7 apache.log

# Count successful HTTP 200 responses
awk '$9 == 200' apache.log | wc -l

# Identify IP addresses generating the most requests
cut -d ' ' -f 1 apache.log | sort | uniq -c | sort -nr
```

These exercises helped me understand how command-line tools can be combined to investigate large amounts of log data without relying entirely on graphical tools.

### 3. Using regular expressions

I learned how regular expressions can identify specific patterns in log entries.

For example, this command searches for post IDs between 20 and 29:

```bash
grep -E 'post=2[0-9]' apache-ex2.log
```

I also practised using regular expressions to identify IP addresses and other patterns. One important lesson was that the format of the data matters. A pattern designed for colon-separated values will not necessarily match values separated by hyphens.

### 4. Analysing data with CyberChef

I used CyberChef to decode and extract information from log files.

The exercises included:
- Decoding Base64-encoded data.
- Extracting IP addresses using regular expressions.
- Searching for MAC address patterns.
- Using operations such as **From Base64** and **List matches**.

I also encountered a practical challenge when my TryHackMe AttackBox session expired. I downloaded the required files, transferred them to my Kali Linux virtual machine, and continued the exercises using CyberChef in my browser.

### 5. Introduction to Sigma and YARA

I explored two rule-based technologies used in security detection:

- **Sigma:** Uses YAML-based rules to describe suspicious events in logs and help security teams detect activity across supported SIEM platforms.
- **YARA:** Uses rules to identify patterns in files and other data, helping analysts recognise potentially suspicious content.

This introduced me to how detection rules can help automate parts of the threat identification process.

## Challenges and lessons learned

One of the challenges I faced was writing regular expressions that matched the exact format of the data. My initial MAC address pattern returned no results because it did not match the hyphen-separated format in the file. I adjusted the expression and tested it again until it returned matches.

I also had to work around an expired AttackBox session by moving the exercise files to my Kali Linux environment.

These experiences reinforced the importance of understanding the data format, checking command output, and troubleshooting instead of assuming a tool or command is correct.

## Key takeaways

This exercise improved my understanding of how log analysis supports security monitoring and incident investigations.

I gained practical experience with:

- Searching and filtering log entries using Linux commands.
- Identifying suspicious activity and common attack patterns.
- Extracting information using regular expressions.
- Decoding encoded data with CyberChef.
- Understanding how Sigma and YARA support rule-based detection.
- Using timelines to connect events during an investigation.

Most importantly, I learned that logs are more than records of system activity. When analysed properly, they provide valuable evidence that helps security analysts understand what happened and identify possible threats.

## What's next?

I plan to continue practising log analysis and explore how these techniques are applied in SIEM platforms and real-world Security Operations Centre (SOC) environments.

**One room at a time. One skill at a time.**

## Learning resource

- [TryHackMe – Introduction to Log Analysis](https://tryhackme.com/room/introtologanalysis)


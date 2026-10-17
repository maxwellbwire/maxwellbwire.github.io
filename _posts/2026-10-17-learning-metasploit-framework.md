---
title: "Introduction to Metasploit"
date: 2026-10-17
categories: [Cybersecurity, Hands-on Labs]
tags: [metasploit, hack-the-box, penetration-testing, ethical-hacking]
description: "My hands-on experience learning Metasploit through HTB Academy."
---

## Introduction

As I continue developing my cybersecurity skills, I recently worked through the **Using the Metasploit Framework** module on Hack The Box Academy. The goal was to understand how Metasploit can be used during penetration testing, from identifying relevant exploit modules to attempting exploitation and managing the access obtained.

Coming from a technical support and networking background, I am used to troubleshooting connectivity problems and investigating systems when something goes wrong. This module helped me apply that same systematic approach to security testing.

## Getting started with Metasploit

Metasploit is a penetration-testing framework that provides modules for vulnerability testing, exploitation, auxiliary tasks, and post-exploitation activities.

I started by learning how to navigate the console and search for modules relevant to particular vulnerabilities. The basic command to launch the framework is:

```bash
msfconsole
```

Once inside the console, I could search for modules using a vulnerability name or identifier. For example:

```text
search type:exploit name:ms17_010
```

This searches for exploit modules associated with MS17-010, a collection of SMB-related vulnerabilities affecting certain Windows systems.

One lesson I took from this process is that finding a potentially relevant exploit is only the beginning. I still need to understand the vulnerability, confirm that the target may be affected, and check whether the module is appropriate for the target environment.

## Selecting and configuring modules

After identifying a module, I learned how to select it and inspect its available options.

```text
info <module_path>
use <module_path>
show options
```

The `info` command displays information about a module, while `use` selects it for configuration. The `show options` command lists the settings that may need to be supplied before execution.

For example, a module may require the target IP address:

```text
set RHOSTS <TARGET_IP>
show options
```

`RHOSTS` identifies the remote target or targets. Depending on the selected module, other settings may include the target port, payload, or local callback address.

I learned why reviewing these options matters. Incorrect settings can cause an otherwise relevant module to fail, and choosing a module without understanding its requirements can waste time during an assessment.

## Exploring different vulnerabilities

The exercises introduced me to several vulnerability scenarios, including MS17-010 and EternalRomance, remote code execution involving Apache Druid, exploitation involving elFinder, and the Sudo Baron Samedit vulnerability.

These examples showed me that exploitation depends on the vulnerability and the environment in which it exists. A technique that applies to one service or operating system will not necessarily work against another.

I also began to appreciate the importance of enumeration and vulnerability research before attempting exploitation. Information about the target's operating system, running services, software versions, and configuration can help narrow down the appropriate testing approach.

For me, this connected the work I had previously done with network scanning and vulnerability assessment to the next stage of a penetration test.

## Running modules and interpreting results

Before running a module, I learned to review its settings and, where supported, use its vulnerability-checking functionality.

```text
show options
check
run
```

The `check` command is available only for modules that implement a check function. It may help determine whether a target appears vulnerable, but it does not guarantee that exploitation will succeed. The `run` command executes the selected module.

I learned not to treat the absence of an error as proof of success. Instead, I need to examine the output, understand what the module reports, and verify whether the expected result was actually achieved.

This is an important distinction in vulnerability assessment: a tool can produce useful information without successfully exploiting the target, and a failed exploitation attempt does not automatically mean the vulnerability is absent.

## Understanding sessions

Another important part of the module was learning about sessions. When an exploit or payload establishes an interactive connection, Metasploit may create a session that allows further interaction with the target.

Some useful session-management commands are:

```text
sessions
sessions -i <SESSION_ID>
```

The first lists available sessions, while the second attaches to a particular session using its ID.

Session management matters because penetration-testing workflows often involve switching between tasks and targets. Knowing how to identify and interact with the correct session makes the process easier to manage.

I also learned that the type of access obtained matters. A Meterpreter session, a command shell, and an elevated session do not provide identical capabilities. I need to understand what access I have rather than assume that establishing a connection means I have full control of the system.

## Challenges and troubleshooting

Not every part of the module was straightforward. I encountered challenges involving privilege escalation and session compatibility, which reminded me that successful exploitation depends on more than selecting the right module.

When an attempt does not work, I need to review the module configuration, check the target environment, understand the current session, and investigate the reason for the failure before trying another approach.

This reinforced a lesson from technical support: troubleshooting works best when I verify each step instead of making assumptions. In cybersecurity, that also means distinguishing between a vulnerability check, a successful exploit, an established session, and elevated privileges.

## What I learned

Working through this module improved my understanding of how Metasploit fits into a penetration-testing workflow. I became more familiar with searching for modules, reviewing their documentation, configuring options, executing supported checks, and managing sessions.

The biggest lesson was that Metasploit is not simply a collection of commands for gaining access to systems. It is a framework that requires an understanding of the underlying vulnerabilities, the target environment, and the limitations of each module.

I also learned the importance of verifying results and investigating unsuccessful attempts rather than assuming that a command worked as intended.

## Connecting this experience to my cybersecurity goals

My experience in technical support and network troubleshooting has given me a foundation in systematic investigation. I want to build on that foundation as I move further into cybersecurity, especially vulnerability assessment, penetration testing, and ethical hacking.

Learning Metasploit is one step in that process. I still need to strengthen my understanding of Windows and Linux internals, improve my enumeration skills, and learn more about the vulnerabilities behind the tools I use.

My next step is to keep practising in authorised lab environments and documenting what I learn, including the approaches that fail and the lessons I take from them. These exercises will help me develop both my technical skills and my ability to communicate security findings clearly.

## Final thoughts

The HTB Academy Metasploit module helped me connect vulnerability discovery with practical exploitation concepts. It also showed me why understanding a tool's behaviour is just as important as knowing its commands.

As I continue my cybersecurity journey, I want to focus on understanding why vulnerabilities exist, how they can be identified and verified, and what can be done to reduce the associated risks.

*All testing described in this post takes place within authorised learning environments.*

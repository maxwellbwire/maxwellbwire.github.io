---
title: "OWASP Top 10 2025: My Hands-On Web Security Learning Journey"
date: 2026-10-24
categories: [Cybersecurity, Web Security]
tags: [OWASP Top 10, TryHackMe, Web Security, IDOR, SSTI, Cryptography, Insecure Deserialization]
---

# OWASP Top 10 2025: My Hands-On Web Security Learning Journey

## Introduction

I recently worked through the OWASP Top 10 2025 practicals on TryHackMe as part of my cybersecurity learning journey. The exercises covered several common web application security weaknesses and gave me an opportunity to test how these issues can affect real application behaviour in controlled lab environments.

Coming from an IT support and networking background, I am used to troubleshooting connectivity issues, investigating problems, and working through evidence to find the cause. These labs challenged me to apply a similar approach to web security: inspect the application, test how it responds to different inputs, investigate unexpected behaviour, and understand the security implications.

Some tasks were straightforward, while others required research and additional guidance. Here are the main techniques I explored and what I learned from the experience.

## 1. Broken Access Control: Investigating IDOR

The first practical introduced me to Broken Access Control through an Insecure Direct Object Reference (IDOR) vulnerability.

An IDOR occurs when an application uses a user-controlled identifier to retrieve a resource without properly checking whether the current user is authorised to access it. For example, an application might use an account ID in a URL to display account information.

In the lab, I changed the account ID in requests and observed the information returned for different accounts. This allowed me to identify an account containing more than one million dollars and retrieve the associated note.

The important lesson was that authentication and authorisation are different. A user may be logged in successfully but should not automatically have access to another user's information.

**What I learned:** Access control must be enforced on the server for every protected resource. Changing an identifier should never be enough to gain access to another user's data.

## 2. Authentication Failures: Testing Account Handling

The authentication practical explored how weak account-handling logic can create opportunities for unauthorised access.

I registered an account using a different capitalisation of the name `admin` and then examined the account information available after registration. The application treated the differently capitalised username in a way that exposed the intended lab result.

This exercise showed me why applications need consistent username handling and clear rules for account registration and identity verification.

**What I learned:** Applications should enforce appropriate username uniqueness rules, handle case sensitivity consistently, and ensure that account identity cannot be confused or manipulated to bypass security controls.

## 3. Security Logging and Alerting Failures

Logging and alerting are important for detecting suspicious behaviour and investigating security incidents.

In this practical, I examined application logs to investigate activity that resembled a brute-force attack. Multiple denied access attempts provided a useful indication of suspicious activity. I then identified the IP address associated with those attempts, the account that was subsequently accessed, and the endpoint requested.

The exercise demonstrated how useful logs can be when they contain relevant information about requests and authentication events.

**What I learned:** Logs should record useful details such as timestamps, source IP addresses, account names, authentication outcomes, and requested endpoints. Security teams also need monitoring and alerting to help identify suspicious patterns before they lead to further compromise.

## 4. Security Misconfigurations: Investigating API Responses

For the Security Misconfigurations practical, I accessed the target application and examined the information it displayed about its API.

The page included a description of an endpoint used to retrieve user information by ID. Initially, I tried numeric user IDs, but the responses did not reveal what I was looking for. I then tested a non-numeric value and observed a different response that exposed the lab's flag.

This was a useful example of how unexpected input can reveal information about an application's error handling and internal logic.

**What I learned:** Developers should avoid exposing sensitive internal details through API responses or error messages. Input validation should be consistent, and errors should be handled safely without disclosing information that could help an attacker.

## 5. Software Supply Chain Failures: Examining Debug Behaviour

This practical required me to investigate how the application processed input through an API.

I started by checking the health endpoint and then tested the processing endpoint with different inputs. A numeric input caused a Python error because the application attempted to perform a string operation on a value of the wrong type.

The supplied source code provided another useful clue. It contained a debug condition that returned diagnostic information when a specific input was provided. Testing that condition exposed sensitive information, including the lab's flag.

This task took more investigation than I initially expected. I had to compare the responses to different inputs and connect the behaviour I observed with the relevant source code.

**What I learned:** Debug functionality should never expose secrets or internal configuration in a production application. Applications should validate input types, handle errors safely, and ensure that diagnostic features cannot be triggered by unauthorised users.

The exercise also reminded me to distinguish between a demonstrated application vulnerability and a vulnerability in a third-party dependency. Investigating supply chain security requires evidence about the components and trust relationships involved.

## 6. Cryptographic Failures: Recovering Protected Information

The Cryptographic Failures practical involved a document viewer that displayed encrypted content.

I inspected the page and found a JavaScript file containing information about the encryption configuration. The file exposed a hardcoded secret key and identified the encryption mode as AES-ECB.

I recognised ECB as a weak choice for protecting many types of structured data because identical plaintext blocks produce identical ciphertext blocks under the same key. However, identifying the algorithm and key was only part of the task.

I needed additional guidance to understand the decryption process, and this introduced me to CyberChef. I used the information recovered from the JavaScript file to configure the appropriate decoding and decryption operations and recover the confidential content.

**What I learned:** Encryption is only as secure as its implementation and key management. Hardcoded secrets and unsuitable encryption modes can undermine the protection that encryption is intended to provide. Developers should use trusted cryptographic libraries, appropriate authenticated encryption, and secure key storage practices.

## 7. Insecure Design: Exploring Application Endpoints

The Insecure Design practical involved a chat application that was presented as being intended for mobile users.

I initially tried accessing some common paths, but those attempts did not provide useful results. After reconsidering the application and its likely functionality, I investigated the user-related API endpoints and discovered a list of users.

I then examined the message endpoint associated with the administrator account. This exposed the information needed to complete the practical.

The experience reinforced the importance of looking beyond what an application's interface appears to allow. A restriction described on a page does not provide security if the underlying API does not enforce the same access rules.

**What I learned:** Security needs to be built into the application's design. Authentication and authorisation must be enforced by backend services, and sensitive messages must only be available to users who have the appropriate permissions.

## 8. Weak XOR Encryption: Finding the Correct Key

Another practical involved three encrypted notes protected using a short XOR key.

The application provided the encrypted notes in Base64 format and a hint about the key's structure. I initially misunderstood the hint and tried incorrect key variations.

After reconsidering the expected key format, I identified the correct key and recovered the contents of the notes, including the flag.

I also explored CyberChef during the process to understand the difference between Base64 decoding and decryption. Base64 is an encoding format, not an encryption algorithm. Decoding it reveals the underlying bytes, but those bytes may remain unreadable until the correct XOR operation and key are applied.

**What I learned:** Short or predictable keys can make encryption easier to break. I also learned to read hints carefully and verify the expected key format before spending too much time testing incorrect possibilities.

## 9. Injection: Exploring Server-Side Template Injection

The Injection practical introduced me to Server-Side Template Injection (SSTI).

SSTI occurs when an application processes user-controlled input as a template expression rather than treating it as ordinary text. Depending on the template engine and the application's configuration, this can expose internal information or potentially allow command execution.

I began by testing simple template expressions and observing whether the application evaluated them. After confirming that the input was being processed, I researched additional Python template-engine techniques and used the lab to investigate the execution context and locate the flag file.

This practical showed me how a seemingly simple input field can become a serious security risk when untrusted input is evaluated by the server.

**What I learned:** Applications should never treat untrusted input as executable template code. Developers should use secure template-rendering practices, avoid exposing powerful objects or operating-system functionality, and enforce appropriate permissions on the application process.

## 10. Software or Data Integrity Failures: Insecure Deserialization

The final practical focused on Python's `pickle` module and the risks of insecure deserialization.

Python pickle can reconstruct Python objects from serialized data. However, deserializing untrusted pickle data can execute code during object reconstruction, making it unsafe for handling input from untrusted sources.

The task required me to create a serialized payload that would read a file on the target system. My initial attempt did not work as expected, so I reviewed the provided solution and corrected my approach before submitting the payload to the vulnerable application.

This helped me understand the difference between creating a serialized payload locally and having the vulnerable application deserialize and execute it on the target system.

**What I learned:** Serialized data should not automatically be trusted simply because it is presented in a particular format. Applications should avoid deserializing untrusted pickle data and use safer formats and validation mechanisms where appropriate.

## Challenges and Lessons Learned

Not every practical worked on the first attempt. Some required me to revisit my assumptions, compare different application responses, inspect source code, or consult external learning resources.

The cryptography and insecure deserialization exercises were especially challenging because they introduced tools and concepts I had not used extensively before. The XOR exercise also reminded me that a small misunderstanding of a hint can lead to unnecessary trial and error.

I used documentation, tutorials, and additional guidance when I reached a point where I could not make progress independently. I still have more practice to do, but I now have a clearer understanding of the concepts and the steps involved in investigating them.

One approach I want to continue improving is documenting each test as I perform it: what I changed, what I expected to happen, what actually happened, and what the result tells me. This will make my investigations easier to reproduce and help me explain my findings more clearly.

## Conclusion

Working through these TryHackMe practicals helped me connect web security concepts with observable application behaviour. I gained experience testing access controls, inspecting API responses and application logs, examining JavaScript source code, using CyberChef for cryptographic analysis, and investigating injection and deserialization vulnerabilities.

The biggest lesson for me was that effective security testing is not just about finding a working payload. It is also about understanding why a weakness exists, what evidence supports the finding, and how the issue could be prevented.

As I continue developing my cybersecurity skills, I want to become more independent in my investigations and improve how I document vulnerabilities and communicate their impact.

This is another step in my journey from IT support and networking toward cybersecurity analysis and ethical hacking.

*All testing described in this post was performed in TryHackMe's controlled lab environments.*

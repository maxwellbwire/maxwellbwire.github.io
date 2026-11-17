---
title: "SQL Injection Fundamentals: From Authentication Bypass to Remote Code Execution"
date: 2026-11-17 09:00:00 +0300
categories: [Cybersecurity, Web Security]
tags: [sql-injection, hack-the-box, mysql, burp-suite, web-security, database-enumeration, penetration-testing]
---

## Introduction

SQL injection is one of the vulnerabilities that can have serious consequences for web applications when user input is not handled securely.

As part of Week 8 of the Cyber Shujaa Security Analyst programme, I completed the **SQL Injection Fundamentals** module on Hack The Box Academy. The module provided practical exposure to how SQL databases work, how vulnerable queries can be manipulated, and how attackers may exploit these weaknesses to access information beyond what an application is supposed to expose.

I worked through different SQL injection techniques, starting with basic SQL statements and gradually moving into authentication bypass, UNION-based injection, database enumeration, file operations, and remote code execution.

This post documents some of the techniques I explored, the challenges I encountered, and the lessons I took away from the exercises.

## Lab Overview

- **Platform:** Hack The Box Academy
- **Programme:** Cyber Shujaa Security Analyst
- **Module:** SQL Injection Fundamentals
- **Focus:** Database security and web application vulnerabilities
- **Tools:** MySQL Client, Burp Suite, Web Browser, Linux Terminal

## 1. Understanding MySQL and SQL Statements

Before exploring SQL injection, I needed to understand how databases work and how applications communicate with them.

I started by connecting to a remote MySQL database using the command-line client. One of the first challenges was that the default connection command failed because of an SSL compatibility issue with the remote server.

After researching the error, I used the `--skip-ssl` option to establish the connection successfully.

Once connected, I practiced several basic SQL commands:

```sql
SHOW DATABASES;
USE employees;
SHOW TABLES;
DESCRIBE employees;
```

These commands helped me understand how to identify databases, select a database, list its tables, and inspect their structures.

I also practiced the main SQL statements:

- `SELECT` – Retrieve records from a database.
- `INSERT` – Add new records.
- `UPDATE` – Modify existing records.
- `DELETE` – Remove records.
- `ALTER` – Modify table structures.
- `DROP` – Remove database objects.

This foundation was important because understanding normal SQL query behaviour makes it easier to recognize when an application is handling user input insecurely.

## 2. Filtering and Manipulating Query Results

The next part focused on controlling the information returned by SQL queries.

I worked with clauses and operators such as `WHERE`, `ORDER BY`, `LIMIT`, and `LIKE`.

For example, the following query retrieves employees whose first names begin with "Bar" and who were hired on a specific date:

```sql
SELECT *
FROM employees
WHERE first_name LIKE 'Bar%'
AND hire_date = '1990-01-01';
```

I also explored logical operators:

- `AND` requires all specified conditions to be true.
- `OR` returns records when at least one condition is true.
- `NOT` reverses a condition.

Understanding how these operators work became particularly important in the next section, where I started exploring SQL injection.

## 3. Subverting Query Logic and Authentication Bypass

This was where the practical SQL injection exercises became more interesting.

I learned how an application can become vulnerable when it directly combines user input with an SQL query without properly handling that input.

One of the first tests involved entering a single quote (`'`) into an input field to observe how the application responded.

A poorly constructed SQL query may allow specially crafted input to change the original query logic.

For example, an always-true condition can alter how an authentication query is evaluated:

```sql
' OR '1'='1' -- -
```

I used this concept in the lab to understand how authentication checks could be bypassed when the application was vulnerable.

I also explored SQL comment syntax, particularly:

- `-- -`
- `#`

These comments can cause the database to ignore the remaining portion of a query, depending on the query structure and database syntax.

One important lesson was that the placement of quotation marks, parentheses, and comments matters. A small syntax difference can determine whether an injection attempt works or produces an error.

## 4. UNION-Based SQL Injection

I then moved to UNION-based SQL injection.

The `UNION` operator combines the results of two or more SELECT statements. However, the queries must return compatible results, including the same number of columns and compatible data types in corresponding positions.

I first practiced this concept directly against the database by combining records from different tables.

I then applied it to a vulnerable web application.

One of the important steps was identifying how many columns the original query returned. I used `ORDER BY` tests, increasing the column number until the application returned an error.

After identifying the column count, I used placeholder values to determine which columns were displayed in the web application.

For example:

```sql
cn' UNION SELECT 1,2,3,4-- -
```

Once I understood the column structure, I explored how database functions could be used to retrieve information.

Examples included:

```sql
SELECT @@version;
SELECT USER();
SELECT DATABASE();
```

This demonstrated how a vulnerable application could expose database information through its normal response.

## 5. Database Enumeration

Database enumeration was another important part of the module.

I learned how the `INFORMATION_SCHEMA` database provides information about the structure of a MySQL database.

I focused on three important components:

- `SCHEMATA` – Information about available databases.
- `TABLES` – Information about tables within databases.
- `COLUMNS` – Information about columns within tables.

Through UNION-based injection, I practiced identifying database names, discovering tables, and examining their columns.

I also explored how an attacker could potentially retrieve information such as usernames, password hashes, and other sensitive records when a database query is vulnerable.

This exercise helped me understand that SQL injection is not necessarily limited to accessing the information displayed on the original webpage. Depending on the application's database permissions and query structure, it can expose information from other tables and databases.

## 6. Reading Files Through SQL Injection

One of the more interesting sections covered reading files from the underlying server.

I learned about the MySQL `LOAD_FILE()` function and how it can be abused when the database account has sufficient permissions.

The exercise involved checking the database user's privileges and understanding the FILE permission.

I then explored how an attacker could potentially read files from the web server, including application source code.

This was particularly important because application source code may contain references to configuration files and other sensitive resources.

In the lab, examining the application source helped me identify where additional configuration information was stored.

The main lesson was that a database vulnerability can sometimes expose information beyond the database itself.

## 7. Writing Files and Remote Code Execution

The next section explored the risks associated with writing files through SQL injection.

I learned about the MySQL `SELECT INTO OUTFILE` statement and the conditions required for successful file creation.

Some of the important requirements included:

- Appropriate database FILE privileges.
- The `secure_file_priv` configuration.
- Write permissions on the destination directory.

In the controlled lab environment, I explored how a vulnerable database query could be used to create files on the web server.

The exercise then demonstrated how an improperly protected web-accessible script could be abused to execute system commands.

I verified the execution context and explored how files could be listed and read through the lab environment.

This was one of the most important parts of the module because it demonstrated how a vulnerability initially affecting database queries could potentially lead to remote code execution.

It also reinforced the importance of restricting database privileges and controlling which directories database processes can access.

## 8. Final Skill Assessment

The final assessment brought together the techniques covered throughout the module.

I worked against a vulnerable web application and used Burp Suite to intercept and inspect HTTP requests.

My initial attempts to authenticate and register an account did not work as expected. I had to investigate the requests being sent by the application and understand how the underlying SQL query was structured.

I then worked through the following stages:

1. Examining the application's HTTP requests using Burp Suite.
2. Testing the SQL query structure and identifying the number of columns.
3. Enumerating the database and identifying relevant tables.
4. Extracting the requested database information.
5. Investigating database privileges and testing file access.
6. Identifying the application's root directory.
7. Testing file-writing capabilities and achieving remote code execution in the authorized lab.

The assessment required me to combine several techniques rather than rely on a single injection payload.

It also showed me how important it is to understand the application's behaviour before attempting to manipulate its database queries.

## Challenges and Lessons Learned

Not everything worked on the first attempt.

One of my initial challenges was connecting to the remote MySQL server. The default command failed because of SSL compatibility, and I had to research the issue before successfully connecting using `--skip-ssl`.

The final assessment was more challenging. I struggled with account registration, identifying the correct SQL injection payloads, and determining the application's root directory.

I used Burp Suite to inspect HTTP requests, revisited the module notes, and consulted online resources and walkthroughs to understand some of the techniques.

These resources helped me complete the assessment, but they also showed me areas where I need more practice, particularly developing the ability to troubleshoot SQL injection vulnerabilities independently.

One thing I took away from this experience is that understanding why a payload works is more valuable than simply copying and executing it.

## Security Takeaways

From a defensive security perspective, this module highlighted several important practices for protecting web applications:

- **Use parameterized queries:** Avoid constructing SQL statements by directly concatenating user input.
- **Apply least privilege:** Database accounts should only have the permissions required for their intended functions.
- **Restrict file operations:** Avoid granting unnecessary FILE privileges to application database accounts.
- **Protect application configuration:** Sensitive configuration files should not be exposed through the web server.
- **Control filesystem permissions:** Database and web server processes should not have unnecessary write access.
- **Monitor suspicious activity:** Log and investigate unusual database queries, authentication failures, and unexpected file operations.

These controls can reduce the impact of SQL injection vulnerabilities and help prevent a database-level issue from escalating into a full server compromise.

## Conclusion

Completing SQL Injection Fundamentals gave me a better understanding of how web applications interact with databases and how weaknesses in query handling can create serious security risks.

I progressed from basic MySQL commands to testing authentication bypass, UNION-based injection, database enumeration, file access, and remote code execution in an authorized environment.

The biggest takeaway for me was understanding how one vulnerability can have multiple layers of impact depending on the permissions and configuration of the affected system.

I still have more to learn, especially when it comes to independently identifying injection points and understanding how to construct and troubleshoot payloads.

As I continue developing my skills in vulnerability assessment and ethical hacking, this module has given me a stronger foundation for testing web application security and understanding how to identify and prevent SQL injection vulnerabilities.

**Learning by doing, one vulnerability at a time.**

---

*Lab completed as part of the Cyber Shujaa Security Analyst programme using Hack The Box Academy. All testing was conducted within the authorized training environment.*

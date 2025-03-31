---
categories:
  - Tryhackme
  - CTF
layout: post
image:
  path: hacking.png
media_subpath: /assets/posts/2025-03-31-THM-Lookup
tags:
  - CTF
  - Fundamentals
  - Tryhackme
title: THM - Lookup - (ongoing)
---
![](2025-03-31-19-17.png)
## Introduction

*Lookup offers a treasure trove of learning opportunities for aspiring hackers. This intriguing machine showcases various real-world vulnerabilities, ranging from web application weaknesses to privilege escalation techniques. By exploring and exploiting these vulnerabilities, hackers can sharpen their skills and gain invaluable experience in ethical hacking. Through "Lookup," hackers can master the art of reconnaissance, scanning, and enumeration to uncover hidden services and subdomains. They will learn how to exploit web application vulnerabilities, such as command injection, and understand the significance of secure coding practices. The machine also challenges hackers to automate tasks, demonstrating the power of scripting in penetration testing.*

## Solution


Starting with Nmap scan:

````
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-03-31 19:09 EEST
Nmap scan report for thm.machine
Host is up (0.061s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)

80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Did not follow redirect to http://lookup.thm
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
````
Metasploit did not contain any exploits for these services.

![](2025-03-31-19-30.png)

Website did not redirect to any site.

![](2025-03-31-19-15.png)

By adding to /etc/hosts file the website redirected correctly.

![](2025-03-31-19-14.png)

Landed on loging page. Let's try to inject some SQL into here (https://github.com/Sourabh-Sahu/SQL-Injection/blob/main/SQL-Injection-Auth-Bypass.md)

![](2025-03-31-19-37.png)

It did not work. Let's open up BurpSuite to analyse this site more.

![](2025-03-31-19-59.png)

At the same time, let's put Dirbuster to scan through the directories. Only the index and server-page was found, nothing interesting yet.

![](2025-03-31-20-27.png)

Checking CVE Database of this Apache version, there was a possible vulnerability of HTTP Request Smuggling and bypassing the authentication with crafted SSL-request. CVE-2024-40725 and CVE-2024-40898. These CVE:s are affecting Apache versions 2.4.0 to 2.4.61, (https://github.com/TAM-K592/CVE-2024-40725-CVE-2024-40898/tree/ALOK)






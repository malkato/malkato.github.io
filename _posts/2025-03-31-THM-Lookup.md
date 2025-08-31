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

Edit (nmap --script=vuln):

|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_http-vuln-cve2017-1001000: ERROR: Script execution failed (use -d to debug)
| http-enum: 
|_  /login.php: Possible admin folder
|_http-csrf: Couldn't find any CSRF vulnerabilities.
|_http-dombased-xss: Couldn't find any DOM based XSS
````
Metasploit and Nmap-script did not show any vulnerabilities for these services.

![](2025-03-31-19-30.png)

Website did not redirect to any site.

![](2025-03-31-19-15.png)

By adding to /etc/hosts file the website redirected correctly.

![](2025-03-31-19-14.png)

Landed on loging page. Let's try to inject some SQL into here [https://github.com/Sourabh-Sahu/SQL-Injection/blob/main/SQL-Injection-Auth-Bypass.md](https://github.com/Sourabh-Sahu/SQL-Injection/blob/main/SQL-Injection-Auth-Bypass.md)

![](2025-03-31-19-37.png)

It did not work. Let's open up BurpSuite to analyse this site more.

![](2025-03-31-19-59.png)

At the same time, let's put Dirbuster to scan through the directories. Only the index and server-page was found, nothing interesting yet.

![](2025-03-31-20-27.png)

Checking CVE Database of this Apache version, there was a possible vulnerability of HTTP Request Smuggling and bypassing the authentication with crafted SSL-request. CVE-2024-40725 and CVE-2024-40898. These CVE:s are affecting Apache versions 2.4.0 to 2.4.61, 
[https://github.com/TAM-K592/CVE-2024-40725-CVE-2024-40898/tree/ALOK](https://github.com/TAM-K592/CVE-2024-40725-CVE-2024-40898/tree/ALOK)

There was a great enumeration Python-script for this website, which detects from the response behaviour, if the username is valid even the password would be incorrect. [https://infosecwriteups.com/lookup-thm-walkthrough-fdced3367aa6](https://infosecwriteups.com/lookup-thm-walkthrough-fdced3367aa6) - 0verlo0ked

By modifying this script to use my wordlist and not informing about invalid requests, I was able to catch two usernames of the server. "admin" was found earlier in the section, where I tried to inject the SQL-commands into the form. Bruteforcing tool called Hydra could now break the password using these usernames.

![](images/2025-04-04-14-28.png)

Hydra : 

User "admin" took a lot of attempts and did not result in successful login, so I ended up then trying the another user.

````
hydra -l jose -P /usr/share/wordlists/rockyou.txt lookup.thm http-post-form "/login.php:username=^USER^&password=^PASS^:invalidpass" -V
````

![](images/2025-04-04-14-56.png)

In order to access to the redirected site, the subdomain had to add to the /etc/hosts file.


![](images/2025-04-04-14-02.png)

This service is called elFinder and Exploit Database has a exploit for this exact version, which can be used to gain a shell for this machine. It uses command injection vulnerability in the PHP connector to deliver the payload in to the target.

[https://www.exploit-db.com/exploits/46481](https://www.exploit-db.com/exploits/46481)

![](images/2025-04-04-14-20.png)
![](images/2025-04-04-14-47.png)

Metasploit Framework has also an exploit for this. Let's use it. s

![](images/2025-04-04-15-54.png)

At first I had a lot of problems not getting the shell but the localhost settings were invalid and therefore the shell could not establish any connection. By changing it to my VPN-tunnel address I was able to gain a meterpreter shell.

![](images/2025-04-04-15-59.png)

![](images/2025-04-04-15-03.png)

Meterpreter shows that we are now logged into "www-data", which usually does not have a lot of privileges on the system.

````
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
**************
/usr/sbin/pwm <----
**************
/usr/bin/at
/usr/bin/fusermount
/usr/bin/gpasswd
/usr/bin/chfn
````
Listing all the files, which have other privileges. "/usr/sbin/pwm" executes id command to extract username and user ID. It attempts to read the extracted user's ".password" file. Because the id command is not using the absolute $PATH, this can be manipulated to use our own malicious code. This is called Path Hijacking. Below screenshot shows, that there is bash syntax inserted into /tmp/id file and changing the file into executable. This path is then exported to $PATH-variable. Now the "pwm" is outputting the user think's .password file.

![](images/2025-04-04-15-06.png)
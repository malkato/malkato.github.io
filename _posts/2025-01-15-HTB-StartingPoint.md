---
categories:
- CTF
layout: post
image:
    path: Starting.gif
media_subpath: /assets/posts/2025-01-15-StartingPoint
tags:
- Basic
title: HTB - Starting Point Machines
---
### Meow
*Continuing my journey in HTB since 2021, with "Starting Point" -machines. Let's see. These are the easiest machines on this platform.*

*We need to telnet into target as root, in order to get the flag*

![](2025-02-14-23-30-29.png)

*We're in let's list and try to find the flag.*

![](2025-02-14-23-31-25.png)

### Fawn

*Which FTP version is running on the target ? :*

![](2025-02-14-23-41-28.png)

*Target system had anonymous login allowed on FTP-service and then there was flag.txt file in the directory, I downloaded it.*

![](2025-02-14-23-47-03.png)

### Dancing

*This is a Windows machine.*

Service name for port 445 ? : 

![](2025-02-14-23-54-05.png)

How many shares on the target machine ? : 

![](2025-02-14-23-57-18.png)

### Redeemer

Which TCP port is open on the target ? : Redis, It's an "in-memory database", these databases are normally much faster than traditional databases, because they use RAM and therefore the data retrieval is faster than secondary memory like HDD or SSD.

![](2025-02-15-00-30-20.png)

![](2025-02-15-00-36-57.png)

![](2025-02-15-00-37-22.png)

---
categories:
- Schoolwork
layout: post
media_subpath: /assets/posts/2025-03-08-Linux-Infra
tags:
- Basic
title: Linux - Infrastructure Services - School
---
# Topology

<img src="image3.png" style="width:6.69375in;height:4.39722in" alt="Random alt text 1" />

<span id="_Toc190694245" class="anchor"></span>Figure 1 VLE Laboratory Environment Topology Image

# Introduction

This document presents the laboratory work for the Linux Infrastructure Services course. The course is part of the Systems Maintenance module.

# Theory

##  

# Lab 1

In the first laboratory exercise, packages supporting the Nextcloud service are installed on a server named "Cloud" in the VLE environment. Additionally, the Apache service is configured and started.

## Implementation

<img src="image4.png" style="width:6.6875in;height:5.46181in" alt="Random alt text 2" />

<span id="_Toc190694246" class="anchor"></span>Figure 2 MariaDB installation

<img src="image5.png" style="width:6.6875in;height:3.61458in" alt="Random alt text 3" />

<span id="_Toc190694247" class="anchor"></span>Figure 3 PHP 7.4 installation

<img src="image6.png" style="width:6.63542in;height:2.1875in" alt="Random alt text 4" />

<span id="_Toc190694248" class="anchor"></span>Figure 4 Apache installation

<img src="image7.png" style="width:6.6875in;height:1.3125in" alt="Random alt text 5" />

<span id="_Toc190694249" class="anchor"></span>Figure 5 Nextcloud database creation

<img src="image8.png" style="width:6.69375in;height:1.36111in" alt="Random alt text 6" />

<span id="_Toc190694250" class="anchor"></span>Figure 6 Configuration error in the material

The configuration found in the materials may have changed its format, and some configuration parameters had changed places (Figure 6). After small changes, Apache restarted successfully (Figure 7).

<img src="image9.png" style="width:4.23958in;height:1.56944in" alt="Random alt text 7" />

<span id="_Toc190694251" class="anchor"></span>Figure 7 Corrected Virtualhost configuration

<img src="image10.png" style="width:6.6875in;height:2.91667in" alt="Random alt text 8" />

<span id="_Toc190694252" class="anchor"></span>Figure 8 Firewall rules and SELinux check

Firewall rules were added to the server for Apache to allow traffic on ports 80 (http) and 443 (https) (Figure 8).

**Why use virtualhost?**

This allows one server to host multiple domains. (Kiarie, 2021)

**Why is the virtualhost configuration in the format "01_nextcloud.conf"?**

Apache searches the content in numerical order, and this format is a good general practice to organize hosts. (Yang & Tagliaferri, 2022).

**Where can you find http and php logs?**

In the path "/var/log/httpd/error_log & /access_log". (Linux.fi, 2023)

# Lab 2

In this lab, extensions for PHP are installed and directories for Nextcloud are configured for SELinux.

## Implementation

<img src="image11.png" style="width:6.69375in;height:1in" alt="Random alt text 9" />

<span id="_Toc190694253" class="anchor"></span>Figure 9 Nextcloud download

<img src="image12.png" style="width:6.69375in;height:0.27639in" alt="Random alt text 10" />

<span id="_Toc190694254" class="anchor"></span>Figure 10 Extracted to path "/var/www/html"

<img src="image13.png" style="width:6.69375in;height:2.5in" alt="Random alt text 11" />

<span id="_Toc190694255" class="anchor"></span>Figure 11 Bash script

<img src="image14.png" style="width:6.27778in;height:0.69444in" alt="Random alt text 12" />

<span id="_Toc190694256" class="anchor"></span>Figure 12 Script execution

<img src="image15.png" style="width:6.69375in;height:5.10417in" alt="Random alt text 13" />

<span id="_Toc190694257" class="anchor"></span>Figure 13 PHP extensions script

<img src="image16.png" style="width:6.69375in;height:5.10417in" alt="Random alt text 14" />

<span id="_Toc190694258" class="anchor"></span>Figure 14 Script execution

Almost all extensions were installed using the script (Figures 13 and 14). Some optional extensions were installed manually.

<img src="image17.png" style="width:6.69375in;height:0.97889in" alt="Random alt text 15" />

<span id="_Toc190694259" class="anchor"></span>Figure 15 Environment variables

<img src="image18.png" style="width:6.69375in;height:1.02292in" alt="Random alt text 16" />

<span id="_Toc190694260" class="anchor"></span>Figure 16 Adding parameters

The instructions required changing file size-related variables. I couldn't find these in the path "/etc/php-fpm.d/www.conf/". By placing these values under "[www]", they point to a specific area in PHP, overriding the general settings. (Figure 16)

<img src="image19.png" style="width:6.04251in;height:2.68788in" alt="Random alt text 17" />

<span id="_Toc190694261" class="anchor"></span>Figure 17 Changing the hostname

I encountered an issue where I couldn't connect to the domain, so I changed the server's hostname using the (hostnamectl) tool to match my student ID. (Figure 17)

<img src="image20.png" style="width:6.69375in;height:5.68125in" alt="Random alt text 18" />

<span id="_Toc190694262" class="anchor"></span>Figure Web portal

An admin user was created with the credentials "admin:admin" for the Nextcloud service (Figure 18).

<img src="image21.png" style="width:6.69375in;height:4.14167in" alt="Random alt text 19" />

<span id="_Toc190694263" class="anchor"></span>Figure Nextcloud dashboard

The Nextcloud user did not have permissions to the "nextcloud" database, so I had to reassign the permissions. The parameter "nextcloud.*" was not sufficient to grant permissions to other users. I removed the dot and wildcard from the query, and after that, I was able to access the database with another created user and complete the service installation. (Figure 19)

**Why SEmanage instead of chcon?** Changes made with SEmanage are permanent, while chcon changes are temporary. (Das, 2020)

**Why is regex important in file paths? (/.*) ?** This ensures that changes within the specified path do not affect anything, as regex ensures that any file within the specified path belongs to that definition.

# Lab 3

In this lab exercise, LDAP authentication is configured for the NextCloud server installed in the previous lab.

## Implementation

At the start of the lab, I searched for applications from NextCloud and noticed that the "php-ldap" module was not installed on the server. I installed this with the command "dnf install php-ldap". (Figure 20)

<img src="image22.png" style="width:3.35463in;height:3.31296in" alt="Random alt text 20" />

Figure LDAP not found

<img src="image23.png" style="width:3.3338in;height:3.17753in" alt="Random alt text 21" />

Figure Successful LDAP installation

After this, the user must be configured for Windows-AD (DC-01) to use LDAP for authentication, as this is used as the host in the LDAP configuration.

<img src="image24.png" style="width:4.3131in;height:5.60495in" alt="Random alt text 22" />

Figure LDAP user creation

A new user was created in the domain's "Users" section. The problem with LDAP integration was that the usernames were incorrect according to the service. This was resolved by enabling the "Password never expires" setting in the user's settings. I understood that the new user had not yet logged in and changed the password, so it was impossible for this user to log in (Figure 22).

<img src="image25.png" style="width:6.69375in;height:3.89097in" alt="Random alt text 23" />

Figure Configuration OK

Now Nextcloud LDAP integration indicates that the configuration is correct (Figure 23).

<img src="image26.png" style="width:6.69375in;height:4.66944in" alt="Random alt text 24" />

Figure Domain Users

Fetch objects from AD and select domain users as the group (Figure 24).

<img src="image27.png" style="width:6.69375in;height:4.77222in" alt="Random alt text 25" />

Figure User verification

The new OU user "sales01" was found through LDAP by searching with the username (Figure 25).

<img src="image28.png" style="width:6.69375in;height:1.42014in" alt="Random alt text 26" />

Figure Login with user "Sales01"

Login was also successful to Nextcloud with the user (Figure 26).

# Lab 4

In this lab, a mail relay server is installed on the SMTP server.

## Implementation

<img src="image29.png" style="width:6.69375in;height:2.85556in" alt="Random alt text 27" />

Figure Postfix installation

<img src="image30.png" style="width:6.69375in;height:6.74167in" alt="Random alt text 28" />

Figure Postfix configuration

<img src="image31.png" style="width:6.69375in;height:1.21989in" alt="Random alt text 29" />

Figure Postfix network settings

<img src="image32.png" style="width:6.69375in;height:2.22222in" alt="Random alt text 30" />

Figure Postfix restart

**"What is the difference between the sendmail command and the sendmail program/package?"**

The sendmail command is a general interface supported by different MTAs (Mail Transfer Agents), while the program is a broader entity that handles emails, for example, routing and managing them. (Proofpoint, 2025)

<img src="image33.png" style="width:6.69375in;height:6.82083in" alt="Random alt text 31" />

Figure Dig "Lookout.vle.fi"

The command produces a result from the DNS registry for the mail relay server address because the parameter "MX" is used. (Figure 31).

<img src="image34.png" style="width:6.69375in;height:1.62764in" alt="Random alt text 32" />

Figure Setting the email in the profile

An email address obtained from VLE is set for the Nextcloud user (Figure 32).

<img src="image35.png" style="width:5.9626in;height:2.98889in" alt="Random alt text 33" />

Figure Email testing

Set the SMTP server address in the test field, and the message was successfully sent (Figure 33).

<img src="image36.png" style="width:6.69375in;height:4.34653in" alt="Random alt text 34" />

Figure Received email

# Sources

Das, Anwesha. 2020. Difference between chcon and semanage. Blog. Accessed 17.2.2025.  
<https://anweshadas.in/difference-between-chcon-and-semanage/>

Kiarie, James. 2021. How to Configure Apache Virtual Hosts on Rocky Linux. Tecmint. Blog. Accessed 17.2.2025.  
<https://www.tecmint.com/configure-apache-virtual-hosts-on-rocky-linux/>

Yang, Kong & Tagliaferri, Lisa. 2022. How To Set Up Apache Virtualhosts on Ubuntu 20.04. Digital Ocean. Accessed 17.2.2025.  
[https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-20-04#step-1-creating-the-directory-structure](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-20-04%23step-1-creating-the-directory-structure)

Linux.fi. 2023. Apache HTTPD. Wiki. Accessed 17.2.2025  
<https://linux.fi/wiki/Apache_HTTPD>

Proofpoint. What is Sendmail? Accessed 5.3.2025  
<https://www.proofpoint.com/us/threat-reference/sendmail>

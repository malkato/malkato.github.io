---
categories:
- Schoolwork
layout: post
image:
    path: windowsad.gif
media_subpath: /assets/posts/2025-03-03-Windows-Domain
tags:
- Schoolwork
title: Windows Domain Administration - School
---

## Topology

<img src="image3.png" style="width:6.69375in;height:4.39722in" alt="random string" />

<span id="_Toc192421661" class="anchor"></span>Figure 1 VLE Laboratory Environment Topology

## Introduction

This document presents the laboratory work for the Windows Domain Administration course. The course is part of the Systems Maintenance module.

## Theory

## Windows Domain

A Windows Domain is a centralized structure of various systems, users, resources, and their management. Devices can include, for example, printers and other connectable devices. These are managed from a single station used by system administrators. (Awati, 2025)

## Group Policy

Group Policy is a feature used by system administrators in Windows AD to set configurations for systems and users. These settings are objects that can control all Windows features of the target device, such as security, configuration, and user environment. (Simister, 2024)

## Lab 1

In this exercise, domains (DC-01 and DC-02) are established, and one workstation is connected to the domain.

## Implementation

<img src="image4.png" style="width:6.69375in;height:3.35972in" alt="random string" />

<span id="_Toc192421662" class="anchor"></span>Figure System Preparation Tool

The "Sysprep" tool removes system-specific information, such as drivers and identifiers. This effectively "cleans" the image. (Figure 2). This procedure is performed on both Windows AD devices in this exercise.

<img src="image5.png" style="width:6.69375in;height:6.67014in" alt="random string" />

<span id="_Toc192421663" class="anchor"></span>Figure 3 Assigning IP-addresses

After cleaning, the correct IP addresses are set for both domains. (Figure 3)

<img src="image6.png" style="width:5.52778in;height:4.75in" alt="random string" />

<span id="_Toc192421664" class="anchor"></span>Figure Configuration table

<img src="image7.png" style="width:6.69375in;height:4.22778in" alt="random string" />

<span id="_Toc192421665" class="anchor"></span>Figure Establishing the Domain

<img src="image8.png" style="width:6.69375in;height:4.95278in" alt="random string" />

<span id="_Toc192421666" class="anchor"></span>Figure Domain promotion

<img src="image9.png" style="width:6.69375in;height:4.20139in" alt="random string" />

<span id="_Toc192421667" class="anchor"></span>Figure Domain Controller Options

<img src="image10.png" style="width:6.69375in;height:4.89722in" alt="random string" />

<span id="_Toc192421668" class="anchor"></span>Figure Prerequisites Check

Add AD services from "Add roles and features". Then promote the device to a Domain Controller. (Figures 5-8)

<img src="image11.png" style="width:6.38889in;height:3.95833in" alt="random string" />

<span id="_Toc192421669" class="anchor"></span>Figure DC-01 Ready

Now DC-01 operates as the domain "studentid.zzzz.zzzz.fi". (Figure 9). Then add the DC-02 device to this created domain.

<img src="image12.png" style="width:6.69375in;height:2.51042in" alt="random string" />

<span id="_Toc192421670" class="anchor"></span>Figure Adding student ID as name

Before connecting DC-02, rename the DC-01 AD page with the student ID. (Figure 10)

<img src="image13.png" style="width:6.69375in;height:5.25833in" alt="random string" />

<span id="_Toc192421671" class="anchor"></span>Figure Adding features to DC-02

The previous steps for adding Domain services are also performed on the second server. (Figure 11)

<img src="image14.png" style="width:6.69375in;height:4.91944in" alt="random string" />

<span id="_Toc192421672" class="anchor"></span>Figure Domain-forest

Promotion is successful using the student ID in the domain name and logging in as an administrator to the domain (DC-01). (Figure 12)

<img src="image15.png" style="width:6.69375in;height:3.49028in" alt="random string" />

<span id="_Toc192421673" class="anchor"></span>Figure Replication

From additional settings, replicate the extra settings from (DC-01). (Figure 13)

<img src="image16.png" style="width:6.69375in;height:3.02917in" alt="random string" />

<span id="_Toc192421674" class="anchor"></span>Figure DNS settings

In DNS settings, find "\_msdcs". This is used to locate ADDC servers and can be used to replicate settings (Gopal, 2016).

<img src="image17.png" style="width:6.69375in;height:3.87639in" alt="random string" />

<span id="_Toc192421675" class="anchor"></span>Figure 15 Reverse Lookup Zone (DC-01)

<img src="image18.png" style="width:6.69375in;height:4.22222in" alt="random string" />

<span id="_Toc192421676" class="anchor"></span>Figure DC-01 Created zone

<img src="image19.png" style="width:3.38889in;height:1.33333in" alt="random string" />

<span id="_Toc192421677" class="anchor"></span>Figure 17 Adding other zones for future labs

<img src="image20.png" style="width:6.69375in;height:4.83681in" alt="random string" />

<span id="_Toc192421678" class="anchor"></span>Figure Creating PTR for DC01

<img src="image21.png" style="width:6.69375in;height:1.15903in" alt="random string" />

<span id="_Toc192421679" class="anchor"></span>Figure Replicating the entire domain to DC02

Creating zones for a DC in the domain and setting the "dynamic updates" option makes it easy to transfer information to all servers in the forest (Figure 19).

Add the workstation to the created domain using PowerShell and rename the station.

<img src="image22.png" style="width:4.73611in;height:2.375in" alt="random string" />

<span id="_Toc192421680" class="anchor"></span>Figure WS-01 DNS settings

<img src="image23.png" style="width:6.69375in;height:6.87431in" alt="random string" />

<span id="_Toc192421681" class="anchor"></span>Figure Connecting to the domain

When encountering a problem communicating with the domain, I noticed a typo in the domain name. DNS must point to the DC, but the domain itself is in the form "studentid.zzzz.domain.fi" (Figure 21). After this, the connection was successful (Figure 22)

<img src="image24.png" style="width:4.56944in;height:1.23611in" alt="random string" />

<span id="_Toc192421682" class="anchor"></span>Figure Successful connection

## Questions:

What are the ways to rename servers: PowerShell, Server Dashboard, and Control Panel, as I did for the workstation.

What other ways are there to connect a workstation to the domain: The way I did it, through the Control Panel, and another way is through PowerShell.

## Lab 2

In this lab, an organizational unit (OU) is created for the domain, defining different user groups and their permissions.

## Implementation

<img src="image25.png" style="width:5.75in;height:3.54167in" alt="random string" />

<span id="_Toc192421683" class="anchor"></span>Figure Organizational Units

User groups are created, and permissions are added using the "minimum policy" approach, meaning, for example, the sales group does not have permission to make system changes because they do not typically perform them (Figure 23). I wondered why computers are also defined separately in the OU, as I thought defining users in the domain would be sufficient. AI provided a good answer, emphasizing that for security and segmentation reasons, it is better to define both in a large organization, (ChatGPT, 2025).

<img src="image26.png" style="width:5.625in;height:3.68056in" alt="random string" />

<span id="_Toc192421684" class="anchor"></span>Figure New user in ICT

Create a new user in the ICT unit (Figure 24).

<img src="image27.png" style="width:4.18056in;height:1.34722in" alt="random string" />

<span id="_Toc192421685" class="anchor"></span>Figure Admin

<img src="image28.png" style="width:6.20833in;height:3.91667in" alt="random string" />

<span id="_Toc192421686" class="anchor"></span>Figure Remote user login

Define login for remote users to be allowed only during working hours (Figure 26).

Question: Why is the OU system used and what are its benefits?

Benefits include centralized management of all domain objects. Permissions and settings are easy to change and add using them. The system is used because an organization has multiple departments with multiple devices and users. In such a case, centralized management is an efficient way to handle changes. Maintenance is quite easy.

## Lab 3

In this lab exercise, group policies are defined from the Domain Controller. These can set specific configurations for either individual or multiple objects, such as a simple background image or permissions.

## Implementation

<img src="image29.png" style="width:6.12586in;height:1.60439in" alt="random string" />

<span id="_Toc192421687" class="anchor"></span>Figure New GPO

<img src="image30.png" style="width:6.69375in;height:2.71875in" alt="random string" />

<span id="_Toc192421688" class="anchor"></span>Figure Disabling background image at user level

<img src="image31.png" style="width:6.69375in;height:3.0625in" alt="random string" />

<span id="_Toc192421689" class="anchor"></span>Figure Initial policies in the domain 1

<img src="image32.png" style="width:6.69375in;height:1.93194in" alt="random string" />

<span id="_Toc192421690" class="anchor"></span>Figure Policies 2

The initial settings in the domain concern passwords and their requirements, user account locking, and login. Local settings also include network-related settings, such as not storing the hash value when changing the password. (Figures 29 and 30).

Next, I create a base for policies that I will set for all created groups in this lab series.

<img src="image33.png" style="width:6.0946in;height:4.71941in" alt="random string" />

<span id="_Toc192421691" class="anchor"></span>Figure Disabling Control Panel (HR)

<img src="image34.png" style="width:3.93805in;height:5.38617in" alt="random string" />

<span id="_Toc192421692" class="anchor"></span>Figure Disabling script execution (Sales)

<img src="image35.png" style="width:6.00153in;height:2.6885in" alt="random string" />

<span id="_Toc192421693" class="anchor"></span>Figure Disabling shutdown buttons (RemoteUsers)

<img src="image36.png" style="width:6.69375in;height:1.75694in" alt="random string" />

<span id="_Toc192421694" class="anchor"></span>Figure Password age is one month (Management)

<img src="image37.png" style="width:6.69375in;height:2.95625in" alt="random string" />

<span id="_Toc192421695" class="anchor"></span>Figure gpresult (sales)

<img src="image38.png" style="width:6.69375in;height:1.10139in" alt="random string" />

<span id="_Toc192421696" class="anchor"></span>Figure Testing

<img src="image39.png" style="width:6.69375in;height:2.88958in" alt="random string" />

<span id="_Toc192421697" class="anchor"></span>Figure Login attempt

It was good to notice that a user belonging to "RemoteUsers" could not log in outside working hours, so to test the policy, I reset the rule (Figure 37).

<img src="image40.png" style="width:6.69375in;height:1.90764in" alt="random string" />

<span id="_Toc192421698" class="anchor"></span>Figure Disabling shutdown (Remote)

<img src="image41.png" style="width:6.69375in;height:3.16528in" alt="random string" />

<span id="_Toc192421699" class="anchor"></span>Figure Verifying Control Panel disable (HR)

## Sources

Gopal (BDRSuite). 2016. “\_msdc dns records”. Spiceworks Forum Post. Accessed 19.2.2025  
<https://community.spiceworks.com/t/_msdc-dns-records/526978>

OpenAI. 2025.” Why do you need to specify also computers and users individually when doing OU's ?”. ChatGPT. Accessed 24.2.2025

Awati, Rahul. 2025. What is Active Directory Domain (AD Domain)? Techtarget. Accessed 9.3.2025  
<https://www.techtarget.com/searchwindowsserver/definition/Active-Directory-domain-AD-domain>

Simister, Aidan. 2024. What are Group Policy and GPO and What Role do they Play in Data Security? Lepide. Accessed 9.3.2025  
<https://www.lepide.com/blog/what-is-group-policy-gpo-and-what-role-does-it-play-in-data-security/>

---
categories:
- School
layout: post
media_subpath: /assets/posts/2025-03-15-Windows-Infra
tags:
- Schoolwork
title: Windows Infrastructure Administration
---

## Introduction

*This document presents the laboratory exercises for the Windows Infrastructure Administration course. The course is part of the Systems Maintenance module.*


## Topology

<img src="images/media/image3.png" style="width:6.69375in;height:4.39722in" alt="Random alt text 1" />

<span id="_Toc192934774" class="anchor"></span>Figure 1 VLE Laboratory Environment Topology



## Theory

## Windows Server

An operating system developed by Microsoft for servers, which can manage servers, their features, security, and scalability at an organizational level. Server Manager acts as a tool in Windows Server to perform these tasks. (Microsoft, 2022)

## Failover

Failover is a method to maintain the availability of systems and the services running under them. For example, if one system stops working, the workload is distributed to other servers in the environment. Servers operate in clusters, and an individual server in this context is called a node. This minimizes service availability interruptions and creates a more secure service environment. (Unitrends, 2023)

## WSUS

Windows Server Update Services is a server feature developed by Microsoft that distributes updates centrally to designated devices and acts as their manager. It serves as an intermediary server for Microsoft updates to endpoints. (Theo, 2025)

## Windows Admin Center

Windows Admin Center is a graphical web-based management service where maintenance and monitoring tasks can be performed for Windows servers and clusters. These tasks include monitoring events, logs, and performance. Azure integration features and essentially the same features offered by Windows Server Manager are also available in Admin Center. (Katie, 2023)

## Lab 1

In this laboratory exercise, a Failover Cluster and DHCP are configured. Their functionality is tested on the WS-01 endpoint.

## Implementation

First, both Failover servers are joined to the domain created in the Windows Domain Administration course, "zzzz.zzzz.vle.fi".

<img src="images/media/image4.png" style="width:6.69375in;height:4.39757in" alt="Random alt text 2" />

<span id="_Toc192934775" class="anchor"></span>Figure FO2 as part of AA4961 domain

<img src="images/media/image5.png" style="width:6.69375in;height:2.75347in" alt="Random alt text 3" />

<span id="_Toc192934776" class="anchor"></span>Figure 3 FO1 joining the domain

<img src="images/media/image6.png" style="width:6.69375in;height:5.40903in" alt="Random alt text 4" />

<span id="_Toc192934777" class="anchor"></span>Figure 4 Failover F01 installation

<img src="images/media/image7.png" style="width:6.69375in;height:4.12986in" alt="Random alt text 5" />

<span id="_Toc192934778" class="anchor"></span>Figure 5 Failover F02 installation

<img src="images/media/image8.png" style="width:5.50077in;height:6.49049in" alt="Random alt text 6" />

<span id="_Toc192934779" class="anchor"></span>Figure 6 iSCSI FO1

<img src="images/media/image9.png" style="width:6.69375in;height:4.70347in" alt="Random alt text 7" />

<span id="_Toc192934780" class="anchor"></span>Figure 7 Adding iSCSI drives F01

<img src="images/media/image10.png" style="width:5.76122in;height:5.47993in" alt="Random alt text 8" />

<span id="_Toc192934781" class="anchor"></span>Figure 8 Naming the drive "Q" F01

<img src="images/media/image11.png" style="width:6.69375in;height:4.83472in" alt="Random alt text 9" />

<span id="_Toc192934782" class="anchor"></span>Figure 9 New drives S and Q visible in the list

<img src="images/media/image12.png" style="width:6.67802in;height:4.66732in" alt="Random alt text 10" />

<span id="_Toc192934783" class="anchor"></span>Figure 10 Building the cluster F01

<img src="images/media/image13.png" style="width:6.69375in;height:4.78403in" alt="Random alt text 11" />

<span id="_Toc192934784" class="anchor"></span>Figure 11 Validation

<img src="images/media/image14.png" style="width:6.69375in;height:4.32083in" alt="Random alt text 12" />

<span id="_Toc192934785" class="anchor"></span>Figure 12 Successful validation

<img src="images/media/image15.png" style="width:6.69375in;height:4.46528in" alt="Random alt text 13" />

<span id="_Toc192934786" class="anchor"></span>Figure 13 Adding IP address to the cluster

<img src="images/media/image16.png" style="width:6.69375in;height:4.61806in" alt="Random alt text 14" />

<span id="_Toc192934787" class="anchor"></span>Figure 14 Configuring Quorum

<img src="images/media/image17.png" style="width:6.69375in;height:3.35069in" alt="Random alt text 15" />

<span id="_Toc192934788" class="anchor"></span>Figure 15 DHCP F01

<img src="images/media/image18.png" style="width:6.69375in;height:4.16111in" alt="Random alt text 16" />

<span id="_Toc192934789" class="anchor"></span>Figure 16 User accounts F01

<img src="images/media/image19.png" style="width:6.69375in;height:3.31181in" alt="Random alt text 17" />

<span id="_Toc192934790" class="anchor"></span>Figure 17 User accounts FO2

<img src="images/media/image20.png" style="width:6.69375in;height:3.54792in" alt="Random alt text 18" />

<span id="_Toc192934791" class="anchor"></span>Figure 18 Scope DHCP F01

Now that the DHCP scope is defined (Figure 18), let's try to enable automatic addressing on WS-01.

<img src="images/media/image21.png" style="width:6.69375in;height:6.50833in" alt="Random alt text 19" />

<span id="_Toc192934792" class="anchor"></span>Figure 19 WS01 network settings

Add a reservation to DHCP for the specific physical address (MAC) (Figure 19).

<img src="images/media/image22.png" style="width:4.58397in;height:4.26101in" alt="Random alt text 20" />

<span id="_Toc192934793" class="anchor"></span>Figure 20 Reservation F01

<img src="images/media/image23.png" style="width:6.69375in;height:6.08819in" alt="Random alt text 21" />

<span id="_Toc192934794" class="anchor"></span>Figure 21 Failover configuration

<img src="images/media/image24.png" style="width:5.24031in;height:5.77164in" alt="Random alt text 22" />

<span id="_Toc192934795" class="anchor"></span>Figure 22 Joining FO2

<img src="images/media/image25.png" style="width:4.52146in;height:3.68802in" alt="Random alt text 23" />

<span id="_Toc192934796" class="anchor"></span>Figure 23 Successful join

<img src="images/media/image26.png" style="width:6.69375in;height:2.29167in" alt="Random alt text 24" />

<span id="_Toc192934797" class="anchor"></span>Figure 24 Shutting down FO1 for testing

<img src="images/media/image27.png" style="width:6.69375in;height:3.50625in" alt="Random alt text 25" />

<span id="_Toc192934798" class="anchor"></span>Figure 25 WS01 ipconfig

When FO1 is shut down (Figure 24), WS01 received the address through FO2 (Figure 25).

## Lab 2

In this lab, we create file shares related to FO1 & FO2.

## Implementation

<img src="images/media/image28.png" style="width:6.69375in;height:3.22917in" alt="Random alt text 26" />

<span id="_Toc192934799" class="anchor"></span>Figure 26 FO1 Resource Manager configuration

<img src="images/media/image29.png" style="width:6.69375in;height:3.48958in" alt="Random alt text 27" />

<span id="_Toc192934800" class="anchor"></span>Figure 27 FO2 Resource Manager configuration

<img src="images/media/image30.png" style="width:6.69375in;height:4.51389in" alt="Random alt text 28" />

<span id="_Toc192934801" class="anchor"></span>Figure 28 Adding Fileserver role FO1

<img src="images/media/image31.png" style="width:6.69375in;height:1.80972in" alt="Random alt text 29" />

<span id="_Toc192934802" class="anchor"></span>Figure 29 Created role

<img src="images/media/image32.png" style="width:6.69375in;height:4.83194in" alt="Random alt text 30" />

<span id="_Toc192934803" class="anchor"></span>Figure 30 New share

<img src="images/media/image33.png" style="width:6.69375in;height:4.33333in" alt="Random alt text 31" />

<span id="_Toc192934804" class="anchor"></span>Figure 31 Management

<img src="images/media/image34.png" style="width:5.85498in;height:3.30254in" alt="Random alt text 32" />

<span id="_Toc192934805" class="anchor"></span>Figure 32 All drives

## Questions

**How can drives be created using PowerShell?**  
Using a similar command structure: `New-SmbShare -Name "MyDocs" -Path "C:\Users\Documents" -ChangeAccess "DOMAIN\Users" -FullAccess "DOMAIN\Admins"` (ChatGPT, 2025)

**What is File Server Resource Manager and why is it useful?**  
This management tool in Windows Server allows for the sharing of resources and their access rights conveniently within a domain. These resources include, for example, sharing storage space from an external ISCSI storage server or utilizing the resources of the Windows Server itself. Management can also limit how much certain user groups can use disk space.**

## Lab 3

In this lab exercise, WSUS (Windows Server Update Services) is installed, which I am already familiar with from the Cyber Defense module. Cluster updating is also implemented.

## Implementation

First, the same procedure is performed on the WSUS device as on all other servers in this module, i.e., sysprep.

<img src="images/media/image35.png" style="width:6.69375in;height:5.24028in" alt="Random alt text 33" />

<span id="_Toc192934806" class="anchor"></span>Figure WSUS sysprep

<img src="images/media/image36.png" style="width:6.69375in;height:4.89861in" alt="Random alt text 34" />

<span id="_Toc192934807" class="anchor"></span>Figure Joining the domain

<img src="images/media/image37.png" style="width:6.69375in;height:4.81597in" alt="Random alt text 35" />

<span id="_Toc192934808" class="anchor"></span>Figure Adding update feature

<img src="images/media/image38.png" style="width:6.69375in;height:4.96736in" alt="Random alt text 36" />

<span id="_Toc192934809" class="anchor"></span>Figure Installation

<img src="images/media/image39.png" style="width:6.31944in;height:4.63889in" alt="Random alt text 37" />

<span id="_Toc192934810" class="anchor"></span>Figure WSUS configuration

<img src="images/media/image40.png" style="width:6.20833in;height:4.36111in" alt="Random alt text 38" />

<span id="_Toc192934811" class="anchor"></span>Figure Connecting to update server

<img src="images/media/image41.png" style="width:6.69375in;height:3.13542in" alt="Random alt text 39" />

<span id="_Toc192934812" class="anchor"></span>Figure Configured WSUS

<img src="images/media/image42.png" style="width:6.69375in;height:4.01181in" alt="Random alt text 40" />

<span id="_Toc192934813" class="anchor"></span>Figure Domain default policy

<img src="images/media/image43.png" style="width:6.69375in;height:5.25972in" alt="Random alt text 41" />

<span id="_Toc192934814" class="anchor"></span>Figure Synchronization

<img src="images/media/image44.png" style="width:6.69375in;height:3.65556in" alt="Random alt text 42" />

<span id="_Toc192934815" class="anchor"></span>Figure Adding users to policy

<img src="images/media/image45.png" style="width:6.69375in;height:2.02083in" alt="Random alt text 43" />

<span id="_Toc192934816" class="anchor"></span>Figure Device list

For some reason, WSUS crashed and I reinstalled it because it no longer responded to reset or other actions. After this, when updates were fetched, the devices appeared in the list (Figure 43).

<img src="images/media/image46.png" style="width:6.69375in;height:1.53472in" alt="Random alt text 44" />

<span id="_Toc192934817" class="anchor"></span>Figure IIS Manager

"WsusPool" stopped working, and for this reason, the entire WSUS crashed (Figure 44).

## Question

**How do you update Failover servers with minimal downtime?**  
Using the Cluster-Aware Updating (CAU) feature. This allows for nearly downtime-free updating in clusters. (Perplexity, 2025)

## Lab 4

In this lab exercise, we explore Windows Admin Center from the workstation WS01.

## Implementation

<img src="images/media/image47.png" style="width:6.69375in;height:2.5in" alt="Random alt text 45" />

<span id="_Toc192934818" class="anchor"></span>Figure Downloading Admin Center

<img src="images/media/image48.png" style="width:6.69375in;height:4.28472in" alt="Random alt text 46" />

<span id="_Toc192934819" class="anchor"></span>Figure Login portal

<img src="images/media/image49.png" style="width:6.15711in;height:4.16725in" alt="Random alt text 47" />

<span id="_Toc192934820" class="anchor"></span>Figure Adding the server

Initially, the problem was logging in, but I noticed when reinstalling that the login method was not (NTLM / Kerberos), and thus it did not work. Now the server can be added to the service. (Figure 47)

<img src="images/media/image50.png" style="width:6.69375in;height:2.36458in" alt="Random alt text 48" />

<span id="_Toc192934821" class="anchor"></span>Figure DC01 statistics

<img src="images/media/image51.png" style="width:6.69375in;height:1.64375in" alt="Random alt text 49" />

<span id="_Toc192934822" class="anchor"></span>Figure Failover cluster

<img src="images/media/image52.png" style="width:6.69375in;height:1.61597in" alt="Random alt text 50" />

<span id="_Toc192934823" class="anchor"></span>Figure Cluster-Aware and Resources

<img src="images/media/image53.png" style="width:6.69375in;height:2.97847in" alt="Random alt text 51" />

<span id="_Toc192934824" class="anchor"></span>Figure Events

## Questions

**List 5 ways how and why you would use Windows Admin Center?**

1.  **Azure**

2.  **Security and monitoring**

3.  **IT support**

4.  **Cluster management**

5.  **Virtualization**

With Azure backup, the functionality of the environment could be ensured, as the agent installed on the devices would back up critical data to another server. Admin Center could be used for security monitoring, for example, monitoring device network traffic with Wireshark and firewall settings. IT support would be easily managed for a large domain of devices, as essentially all settings can be configured through this, just like with a Domain Controller. This service also supports clustering, and thus I added Failover clusters there as well (Figure 49). Updates, disk drives, network settings, and logs of these servers can be managed centrally through this (Figures 50–51). For Microsoft's Hyper-V virtualization, this would also be a handy tool for creating and managing them.

**How does Admin Center differ from Server Manager and why is it useful?**  
From my perspective, Admin Center simplifies and expands the features of Server Manager conveniently. It brings Azure features and endpoint management into the same package, making maintenance significantly easier than with Server Manager. Tasks can be handled in Admin Center but more straightforwardly, making it a much better environment.**

## References

ChatGPT. 2025. Question: “How to create shares with PowerShell? in windows server”. Accessed 12.3.2025

Perplexity. 2025. Question: “How to update Failover-cluster with no downtime? WSUS?”. Accessed 13.3.2025

Microsoft. 2022. Server Manager. Microsoft Learn Article. Accessed 15.3.2025  
<https://learn.microsoft.com/en-us/windows-server/administration/server-manager/server-manager>

Unitrends. 2023. Failover: What It Is and Its Importance in Business Continuity. Blog. Accessed 15.3.2025  
<https://www.unitrends.com/blog/failover/>

Theo. 2025. What is Windows Server Update Services (WSUS)?. Blog. Accessed 15.3.2025  
<https://tsplus.net/fi/what-is-windows-server-update-services-wsus-/>

Katie, Terrell Hanna. 2023. Definition Windows Admin Center?. Accessed 15.3.2025  
<https://www.techtarget.com/searchwindowsserver/definition/Microsoft-Project-Honolulu>

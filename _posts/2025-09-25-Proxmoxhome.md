---
categories:
  - Freetime
layout: post
image:
  path: hacking.png
media_subpath: /assets/posts/2025-09-25-Proxmoxhome/
tags:
  - Experiencing
  - Freetime
title: Homelab - Proxmox Cluster + Portainer
---

## My Current Homelab Topology – Proxmox Cluster + Portainer Stack

## Topology

![](2025-09-26-18-48-31.png)

## Story

The idea of making a Proxmox cluster came when talking to my friend about old devices.  
I realized I still had an old laptop lying around and thought: *why not turn it into something useful?*  
That’s how it started — with just a single Dell Latitude E5550 running Proxmox.  

From there, curiosity grew dramatically. One node wasn’t enough. I wanted to see what a bigger cluster could do, how it would behave, and how far I could push this small homelab setup. That snowball effect led me to add more nodes, experiment with metrics, and eventually land on the current setup you see here.  


## Background
- Last project: OpenWRT → InfluxDB logging on Raspberry Pi (later dismissed when I got proper nodes).  
- Old server blew up → recovered DB and migrated to new container deployment.  


## Current Setup

### Proxmox Cluster (3 nodes currently, 4th arriving soon)
- **Node 1**: Dell Latitude E5550 → running HAOS  
- **Node 2**: Fujitsu Esprimo → running Wazuh (will be migrated)  
- **Node 3**: HP Prodesk (Newest Node)
- **Node 4**: Lenovo Thinkcentre → newest node, Wazuh target  

![](rpmn4th46.png)

### Raspberry Pi 5 running Portainer
- Grafana (with Proxmox metrics dashboard from GrafanaLabs)  
- InfluxDB (storing cluster metrics)  
- Memos (notes, migrated from old server)  

![](1.png)

## Troubles Along the Way

Not everything went smoothly, of course.  

At one point, I had real issues with connecting devices over WiFi. My first thought was that it had to be some kind of legacy issue — old devices not supporting newer encryption methods, or maybe WPA quirks. I spent some time chasing that theory, changing settings, tweaking options, and generally overcomplicating it.  

Eventually, the real cause turned out to be much simpler: the OpenWRT WiFi interface wasn’t actually assigned to the LAN. Because of that, DHCP never reached the devices, so they just sat there without a proper network configuration. One small checkbox was enough to break the whole setup.  

It was a reminder of how these homelab projects often go — you learn by tripping over small but critical details. And once you fix them, the network suddenly feels like it’s *clicking* into place. Also set the MAC-Filter including only devices I personally own.

![](2025-09-25-16-03-06.png)

### Networking
- TP-Link Switch  
- OpenWRT as WiFi AP + WAN gateway  
- Windows PC for main management  
- Next step: OPNsense firewall between WAN and OpenWRT  


## Why Influx + Grafana?
- Proxmox has a built-in metrics option.  
- I wanted proper historical and visualized data of the datacenter.  
- Easy setup, ready-made dashboard.  

![](2025-09-25-16-00-00.png)

## Next Plans
- Add OPNsense firewall next week.  
- Tablet integration with HAOS → for dashboards, to-do lists, etc. 
- Playing around with this environment and implementing new ideas 



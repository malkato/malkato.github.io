---
categories:
  - Freetime
layout: post
image:
  path: hacking.png
media_subpath: /assets/posts/2025-07-18-SimpleSIEM
tags:
  - Experiencing
title: Homelab - Testing Lightweight Stack SIEM
---

## Introduction

![alt](/assets/posts/2025-07-18-SimpleSIEM/2025-07-18-21-29.png)

*This project started from a small curiosity. I was casually checking the logs on my OpenWrt router (while using BanIP), and I noticed that there's a surprising amount of traffic hitting my router — way more than I expected. It got me wondering: where is all this traffic coming from, and what kind of IP addresses are trying to reach my device?*

*To get a better understanding of this, I wanted to build a small monitoring and visualization system. The goal is to collect incoming traffic logs from the router, forward them to a separate PC, and use tools like Telegraf, InfluxDB, and Grafana to visualize the data. This way I can get a clearer picture of the patterns, countries of origin, and possibly suspicious sources that interact with my network.*

*It’s not a huge setup, but it gives me a hands-on way to learn more about the traffic flows around my router and build a simple yet useful traffic analysis system.*

### Installation

*Updating the Linux subsystem for Windows and installing the necessary programs (influxdb, telegraf and grafana)*

````
sudo apt update && sudo apt upgrade -y
sudo apt install influxdb telegraf grafana
````




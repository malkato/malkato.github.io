---
categories:
  - Freetime
layout: post
image:
  path: hacking.png
media_subpath: /assets/posts/2025-07-18-SimpleSIEM/
tags:
  - Experiencing
title: Homelab - Testing Lightweight Stack SIEM
---

## Introduction

![alt](2025-07-18-21-29.png)

*This project started from a small curiosity. I was casually checking the logs on my OpenWrt router (while using BanIP), and I noticed that there's a surprising amount of traffic hitting my router — way more than I expected. It got me wondering: where is all this traffic coming from, and what kind of IP addresses are trying to reach my device?*

*To get a better understanding of this, I wanted to build a small monitoring and visualization system. The goal is to collect incoming traffic logs from the router, forward them to a separate PC, and use tools like Fluentbit, InfluxDB, and Grafana to visualize the data. This way I can get a clearer picture of the patterns, countries of origin, and possibly suspicious sources that interact with my network.*

*It’s not a huge setup, but it gives me a hands-on way to learn more about the traffic flows around my router and build a simple yet useful traffic analysis system.*

*Tips: If you are too tired to write things down, just use AI to literature your own thoughts or instructions and paste them here.*

### Installation

*Updating the UbuntuVM and installing the necessary programs (influxdb, fluent-bit and grafana)*

````
sudo apt install influxdb grafana
curl https://raw.githubusercontent.com/fluent/fluent-bit/master/install.sh | sh --- One liner for fluent-bit install
````
*It's pretty straightforward to set up log forwarding from your OpenWrt router. You just need to point the logs to the correct IP address of your log receiver (e.g., your PC or server), make sure the destination port is open and available, and then choose UDP as the protocol for sending logs. Once that's set, your router will start forwarding logs in real-time to the target system. Picture from the system settings as an example.*

![](2025-07-18-21-41.png)

*Or via command line like this*
````
uci set system.@system[0].log_ip='192.168.1.100'  # Fluentbit/Grafana Server IPv4 address
uci set system.@system[0].log_port='6514'         # Port number
uci commit system
/etc/init.d/log restart
````

**Create database for the logs in influxdb:**
````
user@Ubuntu-20:~/Desktop$ influx
Connected to http://localhost:8086 version 1.6.4
InfluxDB shell version: 1.6.4
> CREATE DATABASE fluentbit;
> SHOW DATABASES
name: databases
name
----
_internal
fluentbit
> exit

````

**Configure Fluentbit (/etc/fluent-bit/fluent-bit.conf):**

````
[SERVICE]
    Flush        5
    Daemon       Off
    Log_Level    info
    Parsers_File parsers.conf

[INPUT]
    Name          syslog
    Mode          udp
    Listen        0.0.0.0
    Port          6514
    Parser        syslog-rfc3164
    Tag           syslog

[OUTPUT]
    Name          influxdb
    Match         *
    Host          127.0.0.1 #localhost
    Port          8086
    Database      telegraf
    Sequence_Tag  message
````

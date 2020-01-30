---
title: "MikroTik DHCP Lease Time [bad wlan working]"
date: 2020-01-30T02:15:27+02:00
draft: false
description: One of the most important MikroTik settings to stabilize the operation of WiFi clients
tags:
  - mikrotik
  - solving
---
 
One of the most important MikroTik settings to stabilize the operation of WiFi clients

If your iPhone or laptop that have a modern network card able to save energy, you may find that the router breaks the connection and your device does not connect to wlan very quickly

The thing is the lease time parameter in the DCHP server configuration, by default the microtic suggests 10 minutes of IP address lease time, it's hard for me to judge whether it's much or little, in practice it's little and you're getting problems.

The solution is to increase the rental time by at least a whole working day, on the home network can be more
![2020-01-30_02-04](/2020-01-30_02-04.png)

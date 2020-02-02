---
title: "MikroTik DHCP Lease Time [bad wlan working]"
date: 2020-01-30T02:15:27+02:00
draft: false
description: One of the most important MikroTik settings to stabilize the operation of WiFi clients
tags:
  - MikroTik
  - solving
---
 
There is one of the most important MikroTik setting to stabilize the operation with WiFi clients.

If your iPhone or laptop has a modern network card and able to save energy, you may find that the router breaks the connection. So, your device does not connect to wlan very quickly.

By default MikroTik suggests 10 minutes of IP address lease time. In practice it's not enough and you'll get problems. 

The solution is increasing rental time for at least a whole working day. In home network you can do it longer for comfortable using.

For example, this is the window where you can increase lease time (I've increased it until 4 days):

![2020-01-30_02-04](/2020-01-30_02-04.png)

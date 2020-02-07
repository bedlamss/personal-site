---
title: "MikroTik - how to change MAC address"
date: 2020-02-06T05:28:26+02:00
draft: false
description: It`s very simple
tags:
  - MikroTik
  - solving
---

​	In Ukraine Internet service providers frequently use MAC binding to configure client Internet on their routers.

​	In MikroTik routers it is impossible to change the MAC address by dint of wibfig, moreover, Winbox (a utility for craftsmanship tuning MikroTik products) also hasn't option to change the MAC address. There is only a “Reset MAC Address” button.

The MAC field is not editable:

![mikrotik-mac](/mikrotik-mac.png)



For changing the MAC you might run the terminal in Winbox and execute the command (where `ether1` is the name of the interface for which you need to replace the MAC address):

```
[admin@MikroTik] > interface ethernet set ether1 mac-address=00:11:d3:f4:01:f4
```

In order to return the factory settings of MAC, use the "Reset MAC Address" button.


---
title: "MikroTik Security tips"
date: 2020-02-06T05:46:50+02:00
draft: false
description: The most basic security settings you need to do!
tags:
  - MikroTik
  - solving
  - security
---

Sometimes I see in the logs information that someone from the Internet wants to hack my router:

![mikrotik-sec](/mikrotik-sec.png)



The most basic security settings you need to do!

In addition to using a randomly generated 12-digit password, you can delete the admin user.

*All actions are performed through the [winbox](https://mikrotik.com/download) interface *

System > Users

![mikrotik-users](/mikrotik-users.png)



- Delete the admin user and create a new one with a unique username. This will greatly complicate the task to burglars with a brute force password (because it will be necessary to pick up a login).
- Allow using a login only from local ip addresses, the range of which can be viewed in the settings of the DCHP server.

![mikrotik-users-dhcp](/mikrotik-users-dhcp.png)

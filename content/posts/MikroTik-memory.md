---
title: "MikroTik hAP lite [not enough space for update]"
date: 2020-01-29T04:21:18+02:00
draft: false
tags:
  - MikroTik
  - solving
    
---



Solution of the problem with updating packages in cheap MikroTik's routers.

I think, that 16MB even for budget home devices is not enough. The fact, in some situations, when we use standard packet sets, at certain points of time flash memory may simply not be enough for the next update. Especially if installed additional packages from the official site that implements specific things.

*p.s MikroTik hAP ac2 is the solution of another price segment and has the same pathetic 16 MB of flash memory*

My solution consists in removing some of the packets, further updating the OS, and installing them afterwards. This does not solve the problem at all, but makes possible upgrading.

There is an instruction for updating:

1. Make a backup settings (important!)

2. Disable all the packages, those you can, including these:

- hotspot

- ipv6

- mpls

- ppp

- routing

- wireless

- advanced tools

3. Reboot

4. Upgrade only these packages, has been taken from the all packages zip:

- system

- security

- dhcp

5. Reboot

6. If previous steps were succesful - upgrade firmware in system - routerboard

7. Reboot

8. Now install the additional packages. Only those you need

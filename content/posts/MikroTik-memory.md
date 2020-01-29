---
title: "MikroTik hAP lite [not enough space for update]"
date: 2020-01-29T04:21:18+02:00
draft: false
tags:
  - mikrotik
  - solving
    
---



 Solution of the problem when updating packages in the cheap routers of the company mikrotik.

I think that 16MB even for budget home devices is categorically low, the fact is that in some situations, when using standard packet sets, at certain points in time flash memory may simply not be enough at the next update. Especially since it is possible to install additional packages from the official site implementing specific things.

*p.s MikroTik hAP ac2 is the solution of another price segment and yet has the same pathetic 16 MB of flash memory*

My solution is to remove some of the packets then update the OS, and install them afterwards. This does not solve the problem in any way, it just makes it possible to perform an upgrade.

1. Make a backup settings (important)

2. Disable all the packages you can including this:

- hotspot

- ipv6

- mpls

- ppp

- routing

- wireless

- advanced tools

3. Reboot

4. Upgrade using only this packages taking them from the all packages zip:

- system

- security

- dhcp

5. Reboot

6. After successful upgrade proceed to upgrade routerboot in system routerboard upgrade

7. Reboot

8. now install the additional packages you need only

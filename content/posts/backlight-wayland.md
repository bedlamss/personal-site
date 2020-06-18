---
title: "Solution for scroll lock key who can activated back light in my keyboard"
date: 2020-06-18
draft: false
description: Solution by using systemd unit and timer
tags:
  - linux
  - solving
  - wayland
---

This button will never work by linux upstream bug 😒

    Create files: `backlight.service` in `/etc/systemd/system` -  *recommended place for user files* and `backlight.timer` for restating backlight.service

*restating needed because keyboard sometimes reinitialization after some action, like connected Bluetooth headset etc* 🤔

Find you input led id, in my keyboard it`s number 7, but in xorg session or other system it can be other: 

`sudo sh -c 'echo 1 > /sys/class/leds/input7::scrolllock/brightness'`

After we have found correct number, put that in `backlight.service `:

```
[Unit]
Description=Keyboard backlight  service

[Service]
Type=oneshot
User=root
ExecStart= sh -c 'echo 1 > /sys/class/leds/input7::scrolllock/brightness'

[Install]
WantedBy=multi-user.target
```

And backlight.timer:

```
[Unit]
Description=Keyboard backlight  service

[Timer]
OnBootSec=15sec
OnUnitActiveSec=5sec

[Install]
WantedBy=timers.target
```

So, enable that by: 

`sudo systemctl enable backlight.service`

`sudo systemctl enable backlight.timer`

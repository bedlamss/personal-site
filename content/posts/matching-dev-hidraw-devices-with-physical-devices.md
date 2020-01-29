---
title: Matching /dev/hidraw* devices with physical devices
date: '2019-10-04T00:07:30+03:00'
draft: false
tags:
  - bash
  - hidraw
  - sysfs
  - hid
  - tutorial
---
> During development of ContourUSB device driver (I' ve dedicated [a post](/project/contourusb/) for this one) I had a hard time detecting the hidraw device created for the meter. I' ve been suggested to just print all available hidraw devices (pseudo files), then plug the meter and re-print hidraw devices to detect which one was just created. While this worked I wasn't really thrilled with the approach. So I created this simple following script.

This is a short tutorial about how to match `/dev/hidraw*` devices to physical ones. For this purpose, we are going to use the sysfs Linux filesystem. Specifically, the following is a simple bash script that prints the hidraw device entries with their related device:

```bash
#!/bin/bash

FILES=/dev/hidraw*
for f in $FILES
do
  FILE=${f##*/}
  DEVICE="$(cat /sys/class/hidraw/${FILE}/device/uevent | grep HID_NAME | cut -d '=' -f2)"
  printf "%s \t %s\n" $FILE "$DEVICE"
done
```

Example output:

```bash
@arvchristos > bash hidrawmatch.sh
hidraw0 	 Logitech USB Receiver
hidraw1 	 Logitech K400 Plus
hidraw2 	 Bayer HealthCare LLC Contour USB
```

We used the `/sys/class` directory that includes device classes registered with the kernel. In fact, we navigated to the `.hidraw` subdirectory because we are seeking information about hidraw interfaced devices.

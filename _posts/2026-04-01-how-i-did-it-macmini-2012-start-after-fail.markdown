---
title:  "How I did it: MacMini 2012 - DietPi - Autostart after power fail"
date:   2026-04-01 16:00:00
categories: [dietpi]
tags: [dietpi, linux, powerfail, macmini, how-i-did-it]
---


```bash
setpci -s 0:1f.0 0xa4.b=0
```

To make this work, let's add this to the root cron on every reboot, run `sudo crontab -e` and add the following

```bash
@reboot /usr/bin/setpci -s 00:1f.0 0xa4.b=0
```
you can verify the content of the root cron with

```bash
sudo crontab -l
```

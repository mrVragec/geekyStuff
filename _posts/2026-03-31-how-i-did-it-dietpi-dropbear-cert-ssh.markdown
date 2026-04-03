---
title:  "How I did it: Dietpi - Dropbear SSH With Certificates"
date:   2026-03-31 16:00:00
categories: [dietpi]
tags: [dietpi, linux, ssh, dropbear, cert, how-i-did-it]
---

So, it happened again and it a never-learning-story! But, at this point I took it little bit more seriously as I'm already sick of doing it again and again. So, yeah, my server got hacked :(, nothing new, same old story! Someone just run the crypto coin minner on my MacMini 2012 :), probably didn't earn a lot!

So this time, and as always in the past, I decided to reinstall [DietPi](https://dietpi.com/). You might wonder why? I think it's a easy-to-install, easy-to-maintain linux distribution and gives everything that I need without huge effort to install. 
This time, I decided to make servers little bit more secure, so will use VPN, disable SSH password login (allow cert only) and expose only ports that are really in use an no more direct DMZ (lession learned). 

## 1. Preparation
Assuming that you have a RaspberryPi or any other SBC or even x86 with installed DietPi, if not yet, feel free to follow this [how-to](). 

## 2. Dropbear configuration
I tried to google about the Dropbear and DietPi but no luck with direct how-to. So I digg deeper little bit on the configuration with dietPi based on this [link](https://dietpi.com/docs/software/ssh/#dropbear-restrict-root-logins) where I thought might be usefull. You can see that all the usefull thinks might be already there but still litlle bit confusing how to do all with `dietpi.txt`. I can tell you, it's pretty simple, when you know how to do it :D. 

### 2.1 Disable SSH with password
To disable SSH login using password there is a simple chnage in the `/boot/dietpi.txt` to be done. 
``` bash
SOFTWARE_DISABLE_SSH_PASSWORD_LOGINS = 1
```
After that, as the `dietpi.txt` is not used at startup or when the Dropbear service would restart, this needs to be applyed to the config. 
``` bash
/boot/dietpi/func/dietpi-set_software disable_ssh_password_logins
```
Prompting the previous command, without any attribute, would read it from the `dietpi.txt` and apply configuration the the current configs. When you apply the command, you should see something like this 
``` bash
[  OK  ] DietPi-Set_software | Setting in /etc/default/dropbear adjusted: DROPBEAR_EXTRA_ARGS="-s"
[  OK  ] DietPi-Set_software | systemctl restart dropbear
[  OK  ] DietPi-Set_software | Desired setting in /boot/dietpi.txt was already set: SOFTWARE_DISABLE_SSH_PASSWORD_LOGINS=1
[  OK  ] disable_ssh_password_logins (1) | Completed
```
WARNING: Do not terminate the SSH session if you don't have direct access to the remove machine! Follow the guide till the end!

### 2.2 Add custom public certificate info
For this, we will use `AUTO_SETUP_SSH_PUBKEY` that would add our publict cert info to all the users (root/dietpi and any other user on the system).

```bash
AUTO_SETUP_SSH_PUBKEY=ssh-ed25519 AAAAAAAA111111111111BBBBBBBBBBBB222222222222cccccccccccc333333333333 mySSHkeyName
```
When this is added, let's apply this similar as was done in the previous step. With the following command, the public cert info will be added to the configuration
```bash
/boot/dietpi/func/dietpi-set_software add_ssh_pubkeys
```
You should see an outpus something like this 
``` bash
[ INFO ] DietPi-Set_software | Root privileges required. Retrying with sudo: sudo /boot/dietpi/func/dietpi-set_software add_ssh_pubkeys

 DietPi-Set_software
─────────────────────────────────────────────────────
 Mode: add_ssh_pubkeys

[  OK  ] DietPi-Set_software | chmod 0400 /root/.ssh/authorized_keys
[  OK  ] DietPi-Set_software | Added setting ssh-ed25519 AAAAAAAA111111111111BBBBBBBBBBBB222222222222cccccccccccc333333333333 mySSHkeyName to end of file /root/.ssh/authorized_keys
[  OK  ] DietPi-Set_software | mkdir -p /home/dietpi/.ssh
[  OK  ] DietPi-Set_software | chmod 0400 /home/dietpi/.ssh/authorized_keys
[  OK  ] DietPi-Set_software | chown -R dietpi /home/dietpi/.ssh
[  OK  ] DietPi-Set_software | Added setting ssh-ed25519 AAAAAAAA111111111111BBBBBBBBBBBB222222222222cccccccccccc333333333333 mySSHkeyName to end of file /home/dietpi/.ssh/authorized_keys
[  OK  ] add_ssh_pubkeys | Completed
```

### 2.3 Change Dropbear port
To add additional level of security, I will change the default port `22` to something more non-default like `2245` hoping to increase work for hackers for cca. 2 min nowadays with AI. 
In `/etc/default/dropbear` just change `DROPBEAR_PORT` to something like 
```bash
DROPBEAR_PORT=2245
```

## Test it out
We came to the last point, where we just need to restart the Dropbear service and test all.
Restart the Dropbear with
```bash
systemctl restart dropbear
```
and test if you can connect with the following command
```bash
ssh -i .ssh/myCert dietpi@192.168.0.222 -p 2245
```

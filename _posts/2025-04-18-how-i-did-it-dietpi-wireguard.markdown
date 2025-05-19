---
title:  "How I did it: Dietpi Wireguard VPN"
date:   2025-04-18 23:00:00
categories: [dietpi]
tags: [dietpi, linux, wireguard, vpn, how-i-did-it]
---

Long time no see, but lot of work was done in the mean time. I will try to write down all the things that I did in the past in some how "How I did it" posts but more about this little bit later.

This post is about how I setup the Wireguard VPN on Dietpi using Raspberry Pi 3.

Usually, I don't use VPN so much, but after a discussion with one of my colleges and especially together with Pi-Hole DNS blocker, I really love it!

First things first, so while ago I had lot of the RasPis at home but didn't know what to do with them, sadly. Then I found out the RasPi dedicated linux distribution called [DietPi](https://dietpi.com/) that changed everything for me! It's amagins what the guys did and how managing the software on it as an easy task!

So, let get the work done!

## 1. Preparation
To start with this, you will need Raspberry Pi or any other SBC or even MacMini or VM would work. From this [link](https://dietpi.com/#download) you can download DietPi image suitable for you needs and install it.

## 2. Install Software
I used Wireguard VPN in my case, as they claim it's very simple and fast modern VPN  (and I think it's true).


### 2.1 Follow the crowd
`dietpi-software` -> `Search software` -> `PiVPN`

### 2.2 Get there faster
The following command will install PiVPN.
```console
dietpi-software reinstall 117
```

## 3. Configuration

## 4. Client
In my case, I use the VPN on the iPhone and iPad just to make my surfing experience better as I don't see ADs (this, why not will be written donw in another post) and just to make my feeling better. :D 


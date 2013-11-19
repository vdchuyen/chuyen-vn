---
layout: post
title:  "Install Openwrt on Dlink DIR-505"
date:   2013-11-12 14:17:42
categories: blog
---

Knowing OpenWRT via some custom firmwares: ddwrt, tomato...but to be a Cisco fan, dd-wrt is always my choice. 

For a travel wifi router, beside Apple Airport Extreme, Dlink also has similar products, one of them I own here is the Dlink 505.

![md-image](http://twimgs.com/informationweek/byte/reviews/2012-July/d-link-angle.jpg)

But the world is not a safe place, Dlink firmwares have some ['bonus'](http://www.devttys0.com/2013/10/reverse-engineering-a-d-link-backdoor/) feature that I don't want to run the original firmware. 

Fortunately, OpenWRT has a [detail wiki](http://wiki.openwrt.org/toh/d-link/dir-505) about this product. And some guys at OpenWRT has push a [working image](http://downloads.openwrt.org/snapshots/trunk/ar71xx/openwrt-ar71xx-generic-dir-505-a1-squashfs-factory.bin) to trunk. 

You can directly flash this image via original web admin interface. Wait for a while and try to ping the default IP: 192.168.1.1. If you have reply packets, congrats. Your Dlink router has just run on new Openwrt firmware.
There're some steps to get fully functional OpenWRT firmware. You can read the 1st step on wiki, I don't mention these steps here.

The tricky part of the process is how to get the internet for your Dlink to get luci and other packages. That's what I need to describe here. Firstly, connect your laptop/computer to internet then plug the wire from the Dlink router to your computer. Then your computer become a gateway for the router to go outside. Needless to say, turn on the forwarding with sysctl, set a nat MASQUARADE rule via iptables and you're done. From router console, just install packages and you're done.

The freedom of installing additinal feature/software on the router is so interesting...even you can emulate an "Airplay"-function router via shairport. 

Update: installed mpd, plugged 2 USB drivers and i've been using this little wifi router as a 24/7 music player ;)

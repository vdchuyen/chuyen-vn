---
layout: post
title:  "Low-power 24/7 music player "
date:   2013-11-03 16:45:42
categories: blog
---

I have one [plug computer]({% post_url 2012-12-19-rescue-the-plug-computer %}) and I use this for all-day music player and torrent leecher ! 

It has only one Ethernet and one USB port. So to get enough storage for music and USB audio card, I have to use a USB hub which has 4 additional ports. The USB sound card is the Creative X-Fi Go Pro which has great sound with the price. You can find it on [ebay](http://www.ebay.com/itm/Creative-SoundBlaster-X-Fi-Go-Pro-USB-SB1290-Sound-Card-70SB129000000-/350750801433?pt=US_Sound_Card_External&hash=item51aa607e19) with is around 18 bucks. The plug kernel should load module snd_usb_audio when you plug that USB sound card. To force the kernel always set the default sound card, you have to add this line to /etc/modprobe.d/alsa-base.conf

{% highlight bash %}

options snd-usb-audio index=0

{% endhighlight %}


I have two USB thumb drives. One is from Kingmax, another is from 
Kingston. In order to get two thumb drives to be automounted, you have 
to get to Uboot interface. Connect the serial port to the plug, set 
root_delay=5 or 10s with bootargs_console so that the system has enough time waiting for usb_sub starts. All you have to do now is adding mount point for two thumbdrives in /etc/fstab.

The last thing is setup mpd on the plug. The mpd client I use for mobile is MPDroid which has great interface.


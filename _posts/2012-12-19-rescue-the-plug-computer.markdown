---
layout: post
title:  "Rescue the plug computer"
date:   2012-12-19 02:26:42
categories: jekyll
---

I had one [plugcomputer](http://www.ionics-ems.com/plugcomputer.html) and oneday I messed up the rootfs then it became a brick.

I was planning to unbrick it but at that time but the libusb and ftdi drivers are not built for 64bit then I let it unbrick for such long time… until the raspberry pi are produced massively :))

I searched around and found this [guy](http://mikelev.in/2010/09/unbrick-sheevaplug). He has done it successfully so I began trying but it didn’t work for me. So I repeated and try other steps to make it work, and here are the steps:

uboot version:

The lastest stock version will be 3.4.27, you can get it on [http://www.plugcomputer.org/downloads/plug-basic](http://www.plugcomputer.org/downloads/plug-basic)

I have upgraded uboot to U-Boot 2011.12 (Mar 11 2012 - 18:59:46) but the EISA installer does not work, then have to restore uboot, more information can be found at: [http://wiki.slimdevices.com/index.php/SqueezePlug](http://wiki.slimdevices.com/index.php/SqueezePlug)

Be careful with your usb devices, preparing at least two usb devices to work with EISA, be sure to partition only one primary partition VFAT on your USB or EISA won’t load it.

Plug one usb to your computer to by pass the EISA have-to-copy install image to USB before install to NAND. The other contains EISA app plug to your plugcomputer. You have to try several times to get it work because sometimes the plug does not recognize your usb.

Check carefully the USB VendorID and ProductID by using USBDeview the device should be USB Serial Converter A and B, the sheeva-installer does not recognized your ftdi if you don’t set the right value.

Finally, you can check it at this url. If you can see the picture that means it’s running well.

*All these steps run on Windows XP 32bit VMWare guest and a Windows 7 64bit computer host*

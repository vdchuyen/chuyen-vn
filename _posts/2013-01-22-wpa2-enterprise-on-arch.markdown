---
layout: post
title:  "WPA2 Enterprise on Arch"
date:   2013-01-22 20:59:42
categories: blog
---

My company have two wlan networks, one is for Public access and another is for private Corporate using WPA2 Enterprise, it is automatically connects on Windows but not in Linux.

After struggling with googling around and debugging, finally I have a working config WPA2 Enterprise WLAN on Linux Arch.

The working configuration sample is below, save this config with any name in /etc/network.d and use netcfg name to connect.

{% highlight bash %}

CONNECTION='wireless'
DESCRIPTION='Automatically generated profile by wifi-menu'
INTERFACE='wlan0'
SECURITY='wpa'
ESSID=your_wpa2_enterprise_ssid
IP='dhcp'
SECURITY='wpa-configsection'
CONFIGSECTION='
 ssid="your_wpa2_enterprise_ssid"
 key_mgmt=WPA-EAP
 eap=PEAP
 pairwise=CCMP TKIP 
 group=CCMP TKIP 
 identity="user@domain"
 password="Your_pass"
 priority=1
 scan_ssid=1
 ca_path="/etc/ssl/certs"
 phase2="auth=MSCHAPV2"
 '
{% endhighlight %}

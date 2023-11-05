---
layout: post
title: Khi bạn già đi
date: 2021-07-06 09:07:02 +0530
categories: #build
---

Centos 7 is too old so while you need collectd with plugin write metrics to Prometheus, no way you can do that on current packages. First, it’s a bug with collectd older than 5.12 and all you have to do is to build the package your self.

To build with flag --enable-write_prometheus, you must get two dependencies packages:

{% highlight bash %}
yum install protobuf-c-devel libmicrohttpd-devel
{% endhighlight %}

Then just grab latest src and make.

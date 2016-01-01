---
layout: post
title:  "Build Volatility profile on Centos 5"
date:   2016-01-01 07:58:48
categories: blog
---

Redhat has done so many backports features from 3.x kernel to their 2.6.x, this is good for most of the time but it's hard to build the kernel modules because the data structures is hard to identify.

To build profile for Centos you need two steps:

* Build dwarfdump, these steps belows works for me:


	yum install elfutils-devel
	wget 'http://www.prevanders.net/libdwarf-20140413.tar.gz'	
	make
	cp dwarfdump /usr/local/sbin

* Create vtypes (kernel's data structures) so that volatility can analyze:

	svn checkout https://github.com/volatilityfoundation/volatility/trunk/tools/linux
	cd linux

Without patching:

	make -C //lib/modules/2.6.18-398.el5/build CONFIG_DEBUG_INFO=y M="/tmp/linux" modules
	make[1]: Entering directory `/usr/src/kernels/2.6.18-398.el5-x86_64'
	CC [M]  /tmp/linux/module.o
	/tmp/linux/module.c:204: error: redefinition of 'struct module_sect_attr'
	/tmp/linux/module.c:211: error: redefinition of 'struct module_sect_attrs'
	/tmp/linux/module.c:353:5: warning: "STATS" is not defined
	/tmp/linux/module.c:369:5: warning: "DEBUG" is not defined
	make[2]: *** [/tmp/linux/module.o] Error 1
	make[1]: *** [_module_/tmp/linux] Error 2
	make[1]: Leaving directory `/usr/src/kernels/2.6.18-398.el5-x86_64'
	make: *** [dwarf] Error 2

Apply this patch:

	--- linux/module.c	2016-01-01 07:04:46.000000000 +0700
	+++ linuxx/module.c	2015-12-31 20:15:39.000000000 +0700
	@@ -198,7 +198,7 @@
	#if LINUX_VERSION_CODE == KERNEL_VERSION(2,6,18)
	#define OUR_OWN_MOD_STRUCTS
	#endif
	-
	+/*
	#ifdef OUR_OWN_MOD_STRUCTS
	struct module_sect_attr
	{
	@@ -221,7 +221,7 @@
	struct module_sections module_sect_attrs;
	
	#endif
	-
	+*/
	struct module_kobject module_kobject;
	
	#ifdef CONFIG_SLAB
 
	make

* Finally, zip both files to Profile

	zip /tmp/Cen-2.6.18-398.el5-x86_64.zip module.dwarf /boot/System.map-2.6.18-398.el5 
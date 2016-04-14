---
layout: post
title: Kernel kompilieren
modified:
categories: blog
description: A fast forward to compile a linux kernel
tags: [kernel kompilieren]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
---

Just a fast forward for me, maybe useful for others too.
I need all space available in /usr/src for compiling so that i save the kernelsource in my home dir and set a softlink to the /usr/src/linux dir.

For this blog entry i use debian wheezy.


Install the dependencies

    apt-get install bzip2 fakeroot kernel-package libncurses5-dev discover

if not already in there, go to your homedirectory

    cd ~

get the kernel from kernel.org

    wget https://www.kernel.org/pub/linux/kernel/v3.x/linux-x.xx.tar.bz2

unzip/untar

    tar xvjf linux-x.xx.tar.bz2

if doesnt exist create compiledir linux

    mkdir /usr/src/linux

go to /usr/src/

    cd /usr/src/

set a softlink

    ln -s /home/yourdirectory/linux-x.xx /usr/src/linux

cd into the linux dir

    cd linux/linux-3.x.xx

load the configuration and modules from the installed kernel and build the .config file with:

    make localmodconfig

here we can accept the settings and if we need some more options afterwards:

    make menuconfig 

---

Note:
after this, make sure in your .config is the following line:

**CONFIG_DEVTMPS=y**

its under Generic Device Drivers in menuconfig there you can enable the automatic mount /dev too

and that the Kernelcompression is set to bzip2

---

cleanup

    make-kpkg clean

build the kernel

    fakeroot make-kpkg --initrd --revision=1.0 kernel_image

after this go back to you homedir

    cd ~

and now we got a new .deb package in there, what we can install with dpkg

    dpkg -i linux-image-x.xx.x_1.0_amd64.deb

if you want to uninstall it type

    dpkg -l

look for the image and type

    dpkg -r linux-image-x.xx.x


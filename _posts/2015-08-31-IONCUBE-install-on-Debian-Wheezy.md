---
layout: post
title: IONCUBE install on Debian Wheezy (7.8)
modified: 2016-04-11
categories: howto
description: IONCUBE install on Debian Wheezy
tags: [php, ioncube]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
---
Make sure you have php 5.4 installed

    php -v

PHP 5.4.39-0+deb7u2 (cli) (built: Mar 25 2015 08:33:29)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2014 Zend Technologies

check that your extension directory is the same than mine (/usr/lib/php5/20100525)

    php -i | grep extension_dir

if yes you can use the skript whitout modification, if not edit the path to your fit

As root / su go to home directory and create a file e.g. ioncube.sh

    sudo su  

    cd ~  

    nano ioncube.sh

and paste the following into it
*(Attention its for amd64 version .. for x86 see at the end of the post)*

    ?#!/bin/bash
    cd /root
    wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
    tar xzf ioncube_loaders_lin_x86-64.tar.gz
    cp ioncube/ioncube_loader_lin_5.4.so /usr/lib/php5/20100525/
    file=/usr/lib/php5/20100525/ioncube_loader_lin_5.4.so
    chmod 644 $file
    echo zend_extension=$file > /etc/php5/conf.d/20-zend_extensions.ini
    chmod 644 $file
    chown 0:0 $file
    service apache2 reload

Make it executable

    chmod +x ioncube.sh

Run it

    ./ioncube.sh

Now check if it works

    php -v

PHP 5.4.39-0+deb7u2 (cli) (built: Mar 25 2015 08:33:29)
Copyright (c) 1997-2014 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2014 Zend Technologies
**with the ionCube PHP Loader v4.7.5**, Copyright (c) 2002-2014, by ionCube Ltd.

For use with x86 System you can use this skript

    ?#!/bin/bash
    cd /root
    wget http://downloads3.ioncube.com/loader_downloads/ioncube_loaders_lin_x86.tar.gz
    tar xzf ioncube_loaders_lin_x86.tar.gz
    cp ioncube/ioncube_loader_lin_5.4.so /usr/lib/php5/20100525/
    file=/usr/lib/php5/20100525/ioncube_loader_lin_5.4.so
    chmod 644 $file
    echo zend_extension=$file > /etc/php5/conf.d/20-zend_extensions.ini
    chmod 644 $file
    chown 0:0 $file
    service apache2 reload

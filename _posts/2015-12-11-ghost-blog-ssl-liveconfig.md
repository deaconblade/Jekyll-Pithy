---
layout: dark-post
title: Ghost Blog SSL – Liveconfig
modified:
categories: blog
description:
tags: [ghost, SSL, liveconfig©]
image:
  feature:
  credit:
  creditlink:
comments: false
share: false
---

First enable Apache2_mod headers

    a2enmod headers
    
and restart Apache2

    service apache2 restart
    
Enable SSL in the Liveconfig Domain settings


Put the following into the custom .httpd.conf file located in /var/www/’yourcustomer-i.e.web1’/ if the file does’nt exist create it and chown it to root:root

##### httpd.conf

    Redirect permanent / https://example.com/
    ProxyPass / http://127.0.0.1:2368/
    ProxyPassReverse / http://127.0.0.1:2368/
    ProxyPreserveHost   On
    RequestHeader set X-Forwarded-Proto "https"
    <IfModule mod_ssl.c>
      SSLProxyEngine On
    </IfModule>

In Liveconfig go to Serververwaltung / Web and save again so that the httpd.conf loads via include.

Now Edit the Ghost config.js and change http into https

##### config.js

    production: {
        url: 'https://example.com',
        forceAdminSSL: true,

restart Ghost
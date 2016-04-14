---
layout: post
title: Webseitenredirect mittels PHP
modified: 2016-04-11
categories: howto
description: Webseiten redirect made easy mittels PHP
tags: [redirect, php]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
---

Um sich tueftelein im Apache/Nginx oder mit einer .htaccess zu sparen kann man ganz simpel im z.b. /var/www/ Verzeichnis eine index.php mit folgendem Inhalt erstellen welche dann auf das gewuenschte Ziel weiterleitet.

    <?php
    header ('HTTP/1.1 301 Moved Permanently');
    header("Location: http://meine.adresse.tld/meinverzeichnis");
    header("Connection: close");
    ?>

---
layout: dark-post
title: Icinga2 Client Setup (Debian Jessie)
modified:
categories: howto
description: Icinga2 Client Setup (Debian Jessie)
tags: [icinga2, debian jessie]
image:
  feature:
  credit:
  creditlink:
comments: false
share: false
---

Port 5665 freigeben: 

    sudo ufw allow 5665

Repository hinzufuegen:

     wget -O - http://packages.icinga.org/icinga.key | apt-key add -   

Pakete neu einlesen:

    sudo apt-get update

Icinga2 und Nagios-plugins installieren: 

    sudo apt-get install icinga2 nagios-plugins

Client mit dem Master verbinden:

    sudo icinga2 node wizard
    
![icinga2wizard](/images/howto/icinga2client/1.jpg)  

![icinga2wizard-1](/images/howto/icinga2client/2.jpg)

Icinga2 neustarten:

    service icinga2 restart

Saemtliche Voreinstellungen koennen local auf dem Client eingestellt werden.

Braucht man den ssh check oder sonstwas nicht die /etc/icinga2/conf.d/services.conf  entsprechend anpassen.
Danach icinga2 reloaden oder neustarten.

Auf den Master laden wir jetzt noch die Configs damit diese auch auf der Webseite angezeigt werden. 
(das muss bei jeder Configaenderung gemacht werden) 

    sudo service icinga2 reload
    
- 

    sudo icinga2 node update-config 


[Gastlogin](/portfolio/icinga2-icingaweb2/)



Dokumentation gibts <a href="http://docs.icinga.org/icinga2/latest/doc/module/icinga2/chapter/getting-started" target="_blank">hier</a>


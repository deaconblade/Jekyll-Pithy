---
layout: post
title: XenServer und VMs bei SoYouStart
modified:
categories: portfolio
description: Xenserver at SoYOuStart
tags: [xenserver, soyoustart]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
---

SoYouStart ?
------------

<a href="https://www.soyoustart.com/de/" target="_blank">So You Start</a> ist ein Markenname von <a href="https://www.ovh.de/" target="_blank">OVH</a> welche dezidierter Rootserver im „mittelklasse Segment“ anbieten.

Sofern der Server bei Bestellung verfuegbar ist, kann dieser meist schon innerhalb von den „beruehmten“ 120 Sekunden bereit gestellt werden.

Der Webmanager zum erstellen des Betriebssystems, beantragung von IP Adressen und Reverse DNS Eintraegen ist simpel gehalten und intuitiv zu verwenden.
(TIPP um die FailOver-IPs [wegen RIPE] zu beantragen machen Sie das telefonisch in Saarbruecken .. geht ruck zuck .. via TicketSystem ist das, naja ne laengere Geschichte.)

Ich habe mich fuer mein Projekt fuer den XenServer entschieden, da ich zum einen in meinem Heimnetzwerk VMware vsphere verwende und zum anderen mal was neues ausprobieren wollte.

Ein klick im Webmanager auf Xenserver installieren genuegt und nach einer Weile erhaelt man die eMail mit den Zugangsdaten. Den XenCenter muss man sich noch von der https://Haupt-IP herunterladen.

Das eigendlich „komplizierte“ am Xenserver ist die Geschichte mit dem localen ISO Ordner. Hier gibt es nicht die Moeglichkeit ganz einfach seine *.iso Datein hochzuladen um dann seine virtuellen Maschinen zu erstellen.

XenServer local ISO Storage gibt <a href="http://riverlite.co.uk/blog/xenserver-creating-a-local-iso-library/" target="_blank">hier</a> eine Hilfestellung zum Erstellen des Ordners.

Wenn das die gewuenschte ISO Datei dann hochgeladen wurde und man sich an das Erstellen der virtuellen Maschine macht gibt es noch ein zwei Dinge zu beachten.

Bevor wir loslegen muss im Webmanager eine IP ausgeaehlt werden und dort auf der rechten Seite MAC-Adresse zuweisen klicken und VMWARE auswaehlen. Diese MAC-Adresse geben wir dann bei der virtuellen Maschine ueber den XenCenter bei der Netzwerkkarte an.

**Debian/Ubuntu keine netinstall verwenden!**  
Wir muessen eine Vollversion (Bei Debian die DVD 1) verwenden und lediglich die Standard Installation durchfuehren. Bei der Konfiguration des Netzwerkes muessen wir „Netzwerk unkonfiguriert lassen“ auswaehlen. Das hat den Hintergrund das wir bei der Installation „Failover-ip“ netmask 255.255.255.255 und Gateway „Haupt-ip.254“ eingeben muessten, was der Installer uns aber nicht akzeptieren wird.
Das Netzwerk wird spaeter mittels /etc/network/interfaces eingerichtet.

Die IPV6 Gatewayadresse herausfinden ist <a href="http://craptalk.de/2015/06/soyoustart-und-ipv6/" target="_blank">hier Top</a> beschrieben

meine /etc/network/interfaces – aus copy & paste schutz Gruenden teilweise mit xxx versehen.

    auto lo
    iface lo inet loopback
    auto eth0
    iface eth0 inet static
    address 46.105.xxx.58
    netmask 255.255.255.255
    broadcast 46.105.xxx.58
    post-up route add 91.121.xxx.254 dev eth0
    post-up route add default gw 91.121.xxx.254
    pre-down route del 91.121.xxx.254 dev eth0
    pre-down route del default gw 91.121.xxx.254

    iface eth0 inet6 static
    address 2001:xxxx:1:xxxx::10
    netmask 64
    up ip -6 route add 2001:xxxx:1:aFF:FF:FF:FF:FF dev eth0
    up ip -6 route add default via 2001:xxxx:1:aFF:FF:FF:FF:FF dev eth0
    down ip -6 route del 2001:xxxx:1:aFF:FF:FF:FF:FF dev eth0
    down ip -6 route del default via 2001:xxxx:1:aFF:FF:FF:FF:FF dev eth0

Meine Maschinen auf dem XenServer sind unter anderem:

1x Sophos UTM Home Edition with site-2-site vpn to my home LAN  
1 Debian Desktop internal LAN  
1 Windows 7 internal LAN

Dank Sophos HTML5 VPN Portal kann ich mich (sofern Inet und Browser vorhanden) von ueberall aus mit den Desktops verbinden. Und das kostenlos :-)
---
layout: post
title: Sophos UTM 9 – pfSense – IPsec Site2Site VPN
modified:
categories: howto
description:
tags: [Sophos UTM, pfsense, IPsec, Site2Site VPN]
image:
  feature:
  credit:
  creditlink:
comments: false
share: false
---

***Fuer das VPN sollten die beiden LAN’s unterschiedliche Subnetze aufweisen.***

#### Hier ist mein basic Setup:

- Sophos : **192.168.60.0/24** – WAN hat feste IP
- pfSense hinter Speedport-Hybrid Router : **192.168.64.0/24** – WAN dynDNS  
(Am Speedport muss natuerlich UDP 500 und 4500 an die pfSense weitergeleitet werden)

##### SOPHOS UTM
In der Sophos UTM sollten wir unbedingt eine eigene Regel verwenden, denn wenn man eine vorhandene auf seine Werte abaendert funktioniert das nicht – keine Ahnung wieso das so ist hat mich aber letzendlich 3 Stunden sinnlose Zeit gekostet.


Also zuerst: **Site-to-Site-VPN –> IPsec –> Richtlinien** auswaehlen

Neue IPSec-Richtline und folgendes eingeben:

Name: custom  
IKE-Verschluesselungsalg.: AES 256  
IKE-Authentifizierungsalg.: SHA2 256  
IKE-SA-Lebensdauer 3600  
IKE-DH-Gruppe: Gruppe 14: MODP 2048  
IPsec-Verschlüsselungsalgorith.: AES 256  
IPsec-Authentifizierungsalgorithmus: SHA2 256  
IPsec-SA-Lebensdauer: 3600  
IPsec-PFS-Gruppe: Gruppe 14: MODP 2048  
Strike Richtline: nicht aktiviert  
Komrimierung: nicht aktiviert  

Speichern

![utm1](/images/howto/sophos/1.png)

Jetzt richten wir das entfernte Gateway ein: **Site-to-Site-VPN –> IPsec –> Entfernte Gateways**

Neues entferntes Gateway und folgendes eingeben:

Name: Remote-GW  
Gateway-Typ: Verbindung initieren  
Gateway: Hier die oeffentliche IP der pfSense bzw. den dynDNS Host eingeben  
Authentifizierungs: Verteilter Schluessel  
Schluessel: Ig656gnd9009was@34krasses(Creativ-sein)  
Wiederholen: Ig656gnd9009was@34krasses(Creativ-sein)  
VPN-ID-Typ: IP-Adresse  
VPN-ID (optional): leer lassen  
Entfernte Netzwerke: das LAN der pfSense 192.168.64.0/24  

Speichern

Jetzt richten wir die Verbindung ein: **Site-to-Site-VPN –> IPsec –> Verbindungen**

Neue IP-Sec-Verbindung waehlen und folgendes eingeben:

Name: Remote VPN  
Entferntes Gate: Remote-GW auswaehlen  
Lokale Schnittst.: External (WAN)  
Richtlinie: custom  
Lokale Netzwerke: Internal (Network)  
Automatische Firewallregeln: aktivieren  
Striktes Routung: nicht aktivieren  
Tunnel an lokale Schnittstelle binden: nicht aktivieren

Speichern

Die Verbindung brauchen wir noch nicht aktivieren, da wir erst die pfSense einrichtung vornehmen.

##### PFSENSE
Erstmal IPSec aktivieren: **VPN –> IPsec –> enable IPsec –> Save**

Rechts auf das + klicken um eine neue Phase 1 zu erstellen und folgendes eingeben:

phase 1  
General Information  
Key Exchange Version: V1  
Internet Protokoll: IPv4  
Interface: WAN  
Remote gateway: Hier die oeffentliche IP der Sophos bzw. den dynDNS Host eingeben
Description: Remote-GW

Phase 1 proposal (Autentication)  
Authentication method: Mutual PSK  
Neogation mode: Main  
My identifier: My IP address  
Peer identifier: Peer IP address  
Pre-Shared Key: Ig656gnd9009was@34krasses(Creativ-sein)

Phase 1 proposal (Algorithms)  
Encryption algorithm: AES – 256 bits  
Hash algorithm: SHA256  
DH key group: 14 (2048 bit)  
Lifetime: 3600

![pfsense1](/images/howto/sophos/2.png)

Advanced  
Disable Rekey: nicht aktiviert  
Responder Only: nicht aktiviert  
Nat Traversal: In meinem Fall Force – aber Auto sollte es auch tun  
Dead Peer Detection: aktivieren -> 10seconds -> 5 retries  

Speichern

Jetzt aktivieren wir die Phase 2 indem wir unter Phase 1 auf das + klicken (nicht auf das Rechte +) und geben folgende Werte ein:

phase 2  
Mode: Tunnel IPv4

Local Network:

Type: LAN subnet  
Type: None  
Remote Network:  

Type: Network  
192.168.60.0/24  
Phase 2 proposal (SA/Key/Exchange)  
Protocol: ESP  
Encryption algorithms: erstes Kaestchen aktiveren (AES) und 256 bits auswaehlen  
Hash algorithms: drittes Kaestchen aktivieren (SHA256)  
PFS key group: 14 (2048 bit)  
Lifetime: 3600  

Advanced Options  
Automatically ping host: IP eines Geraetes im Sophos LAN z.b. 192.168.60.10

Speichern

Jetzt muessen wir noch die Firewall Einstellungen in der pfSense vornehmen

**Firewall –> Rules –> IPsec –> +**

Zu Testzwecken kann man hier eine allow-any-any regel erstellen

![pfsense2](/images/howto/sophos/3.png)

Speichern und aktivieren

Jetzt noch pruefen ob pfSense automatisch Regeln fuer das WAN interface erstellt hat, wenn nicht sollten hier noch die Ports UDP 500 – 4500 – ESP freigegeben werden.

![pfsense3](/images/howto/sophos/4.png)

Speichern und aktivieren

Zusaetzlich habe ich noch eine LAN Regel erstellt:

![pfsense4](/images/howto/sophos/5.png)

Speichern und aktivieren

Verbinden

Jetzt koennen wir wieder zur *Sophos* wechseln und unter **Site-to-Site-VPN –> IPsec –> Verbindungen** den Schieberegler aktiveren

Wenn das ganze funktioniert jubel!!, aber bitte bedenken das dieses How-To nur dem Basic setup entspricht, diverses Sicherheitstuning sollte noch veranlasst werden.
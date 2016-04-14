---
layout: post
title: Ghost unter ISPConfig3 - Apache2 - Ubuntu installieren
modified: 2016-04-11
categories: howto
description: In diesem howto zeige ich wie man Ghost mit ISPConfig3 zum laufen bekommt
tags: [ubuntu, ispconfig3, ghost]
image:
  feature:
  credit:
  creditlink:
comments: true
share: false
---

# Voraussetzung

+ Ubuntu 14.04
+ Apache 2.4
+ ISPConfig3
+ Rootrechte

Eine Anleitung zum Setup finden Sie <a href="https://www.howtoforge.com/perfect-server-ubuntu-14.04-apache2-php-mysql-pureftpd-bind-dovecot-ispconfig-3" target="_blank">hier</a>

---

In dieser Anleitung wird Ghost nicht in eine Subdomain installiert.

Ueberall wo hier im Text **thoje.it** steht, muessen Sie Ihren Domainnamen einsetzen.

### ISPConfig3

Gehen Sie auf Webseiten und klicken "Neue Webseite hinzufügen"

SSL kann bei Bedarf spaeter noch hinzugefuegt werden.
Sie koennen bei Optionen schauen welchen Linuxbenutzer ISPConfig fuer die Seite angelegt hat

**Konsole**

    sudo apt-get install nodejs

und den dazugehoerigen Programmanager

    sudo apt-get install npm

Wichtig ist jetzt noch folgender Befehl (ohne den bekommen Sie spaeter Probleme)

    sudo update-alternatives --install /usr/bin/node node /usr/bin/nodejs 10

Falls nicht schon geschehen installieren Sie noch curl

    sudo apt-get install curl

***Rootrechte***

    sudo su

Da ich mit ISP Config 3 bereits meine Webseite mit der Domain thoje.it angelegt habe wechsle ich nun in folgendes Verzeichnis

    cd /var/www/thoje.it/web

Wir laden uns Ghost in das Verzeichnis

    curl -L https://ghost.org/zip/ghost-latest.zip -o ghost.zip

Und entpacken die zip (es wird in den Ordner ghost entpackt)

    unzip -uo ghost.zip -d ghost

Jetzt muessen wir zu erst einmal die Rechte anpassen, da ISPConfig diese in Form web1:client1
vergeben hat. Wir gehen einen Ordner zurueck, so dass wir im Domain verzeichnis sind

    cd ..

um den Eigentuemer fuer das Verzeichnis /var/www/thoje.it/web nun herauszufinden

    ls -l

Schauen sie welcher Benutzer hier die Rechte hat (In meinem Fall web1 client2)

Da wir als root eben im web/ Verzeichnis rumgewurschtelt haben, hat unser Ghost Ordner den falschen Eigentuemer.

**Achtung** passen Sie web1 und client2 entsprechend an

    chown -R web1:client2 web/

Jetzt gehen wir in den entpackten Ghost Ordner

    cd web/ghost

Die beispiel Configurationsdatei noch umbenennen

    cp config.example.js config.js

und installieren Ghost

    npm install --production

Wenn das ohne Fehler durchmaschiert ist die halbe Miete schon drin.

Wir starten Ghost

    npm start

Theoretisch sollte Ghost jetzt auf Port 2368 erreichbar sein, aber das nuetzt uns nicht wirklich was wir wollen ja unser Blog via <a href="https://thoje.it/" target="_blank">https://thoje.it</a> aufrufen und nicht mittels <a href="https://thoje.it:2368/">https://thoje.it:2368</a>

Beenden Sie Ghost mit Strg +C

Falls nicht schon geschehen noch die Apache Proxy-Module laden

    a2enmod proxy_http proxy

Jetzt koennen wir den Proxy in ISPConfig 3 einstellen

    ProxyPreserveHost On ProxyPass / http://127.0.0.1:2368/

Wir wechseln wieder in unser Ghost Verzeichnis

    cd /var/www/thoje.it/web/ghost/

Uns starten Ghost

    npm start

Ghost sollte nun unter <a href="https://thoje.it/" target="_blank">https://thoje.it</a> bzw. Ihrer Domain erreichbar sein.

Leider sind wir noch nicht fertig, denn sobald Sie die Konsole schliessen ist auch Ghost weg.

Schliessen Sie Ghost durch druecken von STRG +C

### Startup und Start Stop

Wir verwenden pm2 fuer unser Vorhaben (immernoch im Ghost Ordner)

    npm install pm2 -g

nun starten wir Ghost

    NODE_ENV=production pm2 start index.js --name "Ghost"

Startup erzeugen

    pm2 startup

und das ganze speichern

    pm2 save


Als erstes sollten Sie <a href="http://ihre-domain.net/ghost">http://ihre-domain.net/ghost</a> aufrufen damit der Blog und ihr Konto eingerichtet werden kann.

Wenn alles klappt sollten Sie dringenst noch ein paar Anpassungen an der config.js im Ghost Ordner vornehmen. Die wichtigste befindet sich im productions block

    nano /var/www/thoje.it/web/ghost/config.js

aendern Sie hier die URL entsprechend ab

Sie koennen auch noch statt der sqlite3 eine mysql datenbank verwenden, die Sie ueber ISPConfig3 anlegen. Auch wegen SSL schauen sie sich die unten stehenden Links an.

Beachten Sie das wenn Sie in ISPConfig etwas an der Webseite aendern wird die vhost-datei ueberschrieben so das sie erneut den Eintrag anpassen muessen.
**Hinweis: Ich uebernehme hier keine Garantie oder sonstiges**

Hier noch ein paar gute Seiten

<a href="http://support.ghost.org/deploying-ghost/" target="_blank">http://support.ghost.org/deploying-ghost/</a>
<a href="http://www.allaboutghost.com/" target="_blank">http://www.allaboutghost.com/</a>
<a href="http://docs.ghost.org/" target="_blank">http://docs.ghost.org/</a>

Themes
<a href="http://www.allghostthemes.com/" target="_blank">http://www.allghostthemes.com/</a>

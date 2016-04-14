---
layout: post
title: Rootserver SSH Login nur mit SSH-Key
modified:
categories: howto
description:
tags: [rootserver, SSH-Login, putty, kitty]
image:
  feature:
  credit:
  creditlink:
comments: false
share: false
---

Ein Sicherheitsaspekt bei Rootservern ist der Login via SSH.  
Im Normalfall gibt man seinen Benutzernamen und sein Passwort ein um Zugriff zu erhalten. Hier sollte natuerlich zusaetzlich fail2ban installiert werden damit ein Angreifer nach einer gewissen Anzahl von fehlversuchen automatisch gebannt wird. Um das ganze aber noch sicherer zu machen sollte man eine Anmeldung mittels Benutzernamen und Passwort direkt verbieten.

Ich zeige es hier auf wie man das ganze von einer Windows-Maschine aus machen kann.


Zu Erst benoetigt man den <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">PuTTYgen</a>.

Nachdem man diesen heruntergeladen hat, startet man die das Programm puttygen.exe und klickt dort auf „Generate“

![puttygen1](/images/howto/ssh/1-Key-Generator.jpg)

(Waerend der Schluessel generiert wird, sollte man etwas die Maus etwas hin und her bewegen.)

Wenn die Schluessel generiert wurden kann man bei „Key comment“ seine E-Mail Adresse eingeben oder so lassen. 
Dann sollte man noch „Key passphrase“ und „Confirm passphrase“ ein Passwort vergeben. 
Dies ist zwar nicht notwendig, aber wenn man hier kein Passwort eingibt und jemand kommt in den Besitz des Privaten-SSH-Keys kann er sich ohne Eingabe einen Passwortes mit dem Rootserver verbinden, und erlangt im schlimmsten Falle sofort Rootrechte.
Desshalb wuerde ich sowieso keinen Key fuer den Root erstellen sondern fuer einen normalen Benutzer mit sudo Rechten („sudoer“). 
Ok, wir speichern jetzt den public und den private key an einem sicheren Ort.

***Den PuTTYgen lassen wir noch geoeffnet.***

![puttygen2](/images/howto/ssh/2-Key-Generator.jpg)

Wir wechseln zu PuTTy/KiTTy und verbinden uns mit unserem Rootserver.

Nun installieren wir erstmal SSH

    sudo aptitude install ssh
    
und wechseln in unseren Home Ordner:

    cd ~

Sofern man noch keine Keys erstellt hatte oder das System frisch aufgesetzt wurde hat man noch nicht den benoetigten Ordner und die entsprechende Datei .

    mkdir .ssh

-

    cd .ssh

-

    nano authorized_keys

in diese Datei fuegen wir nun den Public-Schluessel aus unserem noch geoeffneten PuTTygen ein.
Bitte darauf achten alles aus dieser Box zu kopieren 😉

![puttygen3](/images/howto/ssh/3-Key-Generator.jpg)  

![insertkeyinterm](/images/howto/ssh/4-Key-Generator.jpg)

Die Datei speichern und schliessen.

Jetzt vergeben wir noch die Rechte, damit kein Unbefugter damit was anfangen kann.

    chmod 0600 authorized_keys

wir gehen einen Schritt zurueck

    cd ..

und passen die Rechte des Ordners an

    chmod 0700 .ssh

Jetzt testen wir ob die Verbindung mittels SSH-Key klappt, bevor wir den normalen Login sperren.

In PuTTy/KiTTy sind folgende Einstellungen vorzunehmen:

Wir klicken auf „Data“

![kitty1](/images/howto/ssh/1-KiTTY-Configuration.jpg)

Und geben dort unseren Benutzernamen ein.

![kitty2](/images/howto/ssh/2-KiTTY-Configuration.jpg)

Nun klicken wir auf SSH und waehlen dort Auth aus wo wir dann mittels „Browse“ unseren Private-Schluessel den wir zu Beginn an einen sicheren Ort gespeichert haben auswaehlen:

![kitty3](/images/howto/ssh/3-KiTTY-Configuration.jpg)

Nun gehen wir wieder zurueck zu Session und speichern alles ab.

![kitty4](/images/howto/ssh/4-KiTTY-Configuration.jpg)

Jetzt stellen wir eine Verbindung mit unserem Rootserver her.
Hier sollte nun folgende Meldung erscheinen:

![loginterminal](/images/howto/ssh/login-thoje-it-KiTTY.jpg)

Wenn nach Eingabe des Passwortes welches wir fuer den Schluessel erstellt haben alles funktioniert koennen wir nun den normalen Zugang sperren.

Hierzu editieren wir die sshd.conf

    sudo nano  /etc/ssh/sshd_config 

Nun suchen wir nach folgender Passage:

    #PasswordAuthentication yes

und aendern diese in (Achtung bitte auch die Raute # entfernen)

    PasswordAuthentication no

und speichern und schliessen die Datei.

Jetzt noch ssh neu starten

    sudo service ssh restart

Wenn man nun eine Verbindung ohne SSH-Key herstellt wird man bei der Anmeldung nach Eingabe des Benutzernamens und Betaetigung der Entertaste eiskalt abserviert.

![nologin](/images/howto/ssh/2-login-thoje-it-KiTTY.jpg)

Anmerkung:
Passen Sie darauf auf das Sie sich nicht selbst ausschliessen :-)
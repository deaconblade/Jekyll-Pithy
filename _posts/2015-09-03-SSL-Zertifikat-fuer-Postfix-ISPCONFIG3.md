---
layout: post
title: SSL Zertifikat fuer Postfix (ISPConfig3 Multiserver)
modified: 2016-04-11
categories: howto
description: Ein SSl-Zertifikat fuer den Postfix unter ISPConfig3 Multiserver einbauen.
tags: [postfix, ispconfig3, multiserver, ssl-zertifikat]
image:
  feature:
  credit:
  creditlink:
comments: true
share: true
---

# CSR erstellen

Auf dem Mailserver einloggen und den RSA sowie CSR erstellen

1.

    cd ~

2.

    openssl req -nodes -newkey rsa:2048 -keyout smtpd.key -out mail.csr

Hier ist nun der Common-Name wichtig geben Sie den kompletten FQDN Ihres Mailservers z.b. mail.ihredomain.tld ein

Den Inhalt aus der eben erstellen mail.csr kopieren und beim Zertifikatsausteller Einfuegen (Apache2 Zertifikat o.ae. auswaehlen)

Nachdem Sie Ihr Zertifikat erhalten haben zurueck in der Konsole vom Mailserver sichern wir uns die erstmal die Datein

3.

    mv /etc/postfix/smtpd.key /etc/postfix/smtpd.key.bak

4.

    mv /etc/postfix/smtpd.cert /etc/postfix/smtpd.cert.bak

nun die am Anfang mit openssl erstellte Datei reinkopieren

5.

    cp ~/smtpd.key /etc/postfix/

nun den Inhalt des neuen Zertifikates welches wir per eMail erhalten haben kopieren und per

6.

    nano /etc/postfix/smtpd.cert

einfuegen.

Postfix neustarten

7.

    service postfix restart

und Dovecot neustarten

8.

    service dovecot restart

---
layout: post
title: Ghost Blog vs. Jekyll 
modified:
categories: blog
description:
tags: [ghost, jekyll, blog]
image:
  feature:
  credit:
  creditlink:
comments: true
share: false
---

#### Welches moderne Blogsystem moechte ich verwenden ?

<a href="https://ghost.org/" target="_blank">Ghost</a> oder <a href="http://jekyllrb.com/" target="_blank">Jekyll</a> das ist hier die Frage.

Ich verwende nun schon seit ein paar Monaten Ghost und bin sehr zufrieden damit.
Da ich aber auch gerne mal etwas anderes ausprobiere habe ich mich mal mit dem ultra hippen Jekyll befasst.

Wie mit allen, wenn man sich damit auskennt ist es super einfach. Ich gehe jetzt von mir aus und halte es wie unsere Kanzlerin fuer mich war es #Neuland.

##### Fangen wir mit Jekyll an:
Die Webseite von <a href="http://jekyllrb.com/" target="_blank">Jekyll</a> ist auf English was dem ganzen keine Abbruch tun sollte, allerdings ist die Beschreibung etwas ich sags mal „niveauvoll“ gehalten, so das man da wirklich erstmal rumdoktern muss um das ganze local laufen zu lassen. Zumal noch einige Vorbedingungen erfuellt sein muessen. Z.B. sollte man Linux oder Mac OS X laufen haben sowie Ruby, Rubygems und natuerlich Nodejs installiert haben.
Nur wenn diese Vorbedingungen erfuellt sind kann man diesen Blog wie es die Webseite verspricht innerhalb von Sekunden zum laufen bringen.

![jekyll](/images/blog/jekyll/Jekyll1.png)

Der Vollstaendigkeit halber sei noch erwaehnt das es auch moeglich ist Jekyll local mit Windows zu betreiben was aber nicht unbedingt das gelbe vom Ei ist. Ein how-to gib es <a href="http://jekyll-windows.juthilo.com/" target="_blank">hier</a>.

Des Weiteren gibt es noch die moeglichkeit das ganze auf github hosten zu lassen, was fuer einen kompletten Blog kostenlos ist. Der Blog ist dann unter der Adresse username.github.io erreichbar, und man kann sofern man eine eigene Domain hat diese per CNAME auch dorthin weiterleiten. Ich empfehle als Einstieg das ganze erstmal auf <a href="https://github.com/" target="_blank">github</a> zu hosten und etwas rumzuprobieren.
Sie koennen sich auf github kostenlos registrieren. Dann schauen sie auf <a href="http://www.jekyllnow.com/" target="_blank">jekyllnow</a> vorbei, dort wird in einer kurzen verstaendlichen (englischen) Anleitung erklaert wie man Jekyll ganz ohne ohne localer installation usw. auf github innerhalb von Sekunden zum laufen bekommt.

Fazit:
Als Neuling in der Materie Jekyll benoetigt man doch eine gewisse Zeit, wenn alles laeuft und man tiefer drinne ist, fasziniert einen Jekyll schon ungemein. Ich denke Jekyll ist nicht zu unrecht so beliebt. Zumal Jekyll keinerlei Datenbank benoetigt und sofern man auf github hostet auch noch voellig kostenfrei.

##### Kommen wir nun zu Ghost:
Auch die Webseite von Ghost ist in English. Ist halt so, ist modern. Ghost an sich ist kostenlos, allerdings benoetigt man einen eigenen Webserver oder man laesst es von Ghost selbst hosten, beides kostet (in der Regel) Geld, wenn auch nicht die Welt. Aber nicht jeder Webhoster wird Ihnen eine nodejs Umgebung bereitstellen, so dass Sie wahrscheinlich beim self-hosting um einen Rootserver nicht vorbei kommen.
Benoetigt wird Linux sowie nodejs. Um nodejs gescheit zu installieren folgt man am besten dieser Anleitung: <a href="https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager/" target="_blank">Nodejs</a>.

Sie koennen unter <a href="https://ghost.org/de/" target="_blank">https://ghost.org/de/</a> Ghost fuer 14 Tage kostenlos ausprobieren. Ist eine tolle Sache um reinzuschnuppern.
Sollten Sie sich zum self-hosting entscheiden kann ich Ihnen <a href="https://blog.novatrend.ch/2014/12/22/ein-blog-mit-ghost-und-node-js/" target="_blank">Ein Blog mit Ghost und Node.js</a>, oder auch [meinen Artikel](howto/Ghost-unter-ISPConfig3-Apache2-installieren/) ans Herz legen, dort ist auf Deutsch gut beschrieben wie die Installation unter Ubuntu von statten gehen kann. Einmal installiert und unter http://ihre.adresse.ltd/ghost ihr Administratorkonto eingerichtet ist Ghost absolut selbsterklaernd. Auch wenn Sie das Theme aendern wollen ist dies mit ein paar Handgriffen erledigt.

Fazit:
Wenn Ghost laeuft, ist es wirklich kinderleicht zu bedienen. Blogs erstellen in Markdown ist simple, das Theme aendern ist einfach, Backups erstellen ein klacks.

Zusammenfassend sei gesagt:
Wer bloggen will „like a hacker“ nimmt Jekyll, wers einfach und schick zugleich haben moechte greift zu Ghost.
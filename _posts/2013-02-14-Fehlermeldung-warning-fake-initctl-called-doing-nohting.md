---
layout: post
title:  "Fehlermeldung *Warning: Fake initctl called, doing nothing.* unter Ubuntu"
date:   2013-03-23 13:34:21 +0200
categories: mysql ubuntu
---
Als ich in den letzten Tagen einen MySQL-Server auf einem Ubuntu-Gerät installieren wollte, kam mir folgende Fehlermeldung unter: *Warning: Fake initctl called, doing nothing.* Das Problem war nicht der MySQL-Server, dieser wurde korrekt installiert und lies sich auch direkt über die eigene binary starten. Nur der Start über die INIT-Skripte war nicht möglich. Wie sich herausstellte lag der Fehler an upstart von Ubuntu, einem event-basierten INIT-Daemon, der Tasks unter Ubuntu verwaltet. Irgendwas musste bei der Installation schief gelaufen sein. Der Fehler war aber schnell behoben. Ein *sudo apt-get purge mysql-server* Anschließend ein *sudo apt-get install upstart --reinstall* und zuletzt ein *sudo apt-get install mysql-server* hat die Fehler behoben und der MySQL-Server lies problemlos starten.
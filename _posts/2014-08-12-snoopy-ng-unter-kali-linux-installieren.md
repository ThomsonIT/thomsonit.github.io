---
layout: post
title:  "snoopy-ng unter kali linux installieren"
date:   2014-08-12 19:50:00 +0200
categories: virtualbox kali snoopyng
---
Ich habe mir gerade snoopy-ng nochmal in einer VirtualBox unter Kali Linux installiert.  Das Installationsskript ist dabei mehrfach abgebrochen. Der Fehler lag nicht, wie zuerst vermutet, am fehlenden _pip_ sondern am fehlenden _xslt_. Ein *apt-get install libxslt1-dev* behebt das Problem und die Installation läuft dann durch.
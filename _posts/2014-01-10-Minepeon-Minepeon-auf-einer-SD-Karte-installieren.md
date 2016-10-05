---
layout: post
title:  "Minepeon: Minepeon auf einer SD-Karte installieren"
date:   2014-01-10 13:34:21 +0200
categories: raspberrypi mining minepeon
---
Da ich mich technisch etwas mit dem Bitcoin Mining auseinandersetzen wollte, habe ich mir einen ASIC Block Erupter besorgt.
Dabei handelt es sich um einen kleinen USB-Stick, der für nichts anderes gedacht ist als SHA256 basierte Coins zu generieren.
Wer daran Interesse hat findet alle nötigen Informationen auf [Google](https://www.google.de/#q=asic+block+erupter). 
Um diesen Block Erupter zu betreiben kann man nun Software auf dem Mac installieren und diesen dann damit betreiben. 
Da ich das ganze aber lieber Stand-Alone betreiben möchte und sowieso einen Raspberry PI in der Ecke liegen habe,
bietet es sich an auf dem PI ein Linux zu installieren und die Mining-Software darauf zu betreiben.

![Bitcoin Block Erupter am Rasperry PI](http://i43.tinypic.com/302ctau.jpg)  
Nach einer kurzen Suche bei Google fand ich heraus, dass es ein fertiges Paket mit dem Namen *MinePeon* gibt.
Diese ist exakt dafür gedacht um spezielle ASIC-Hardware mit dem Raspberry PI zu betreiben.
Minepeon auf einer SD-Karte zu installieren ist leicht. Das Vorgehen (auf dem Mac) ist wie folgt:  
Zunächst lädt man die aktuelle Version von der [Minepeon Website](http://minepeon.com/index.php/Main_Page) herunter und entpackt das Paket.
Man erhält dann eine *.img Datei.

Nachdem die im Raspberry Pi zu verwendende SD-Karte in den Mac eingesteckt ist, öffnet man ein Terminal Fenster und gibt den Befehl *diskutil list* ein.
Man bekommt dann eine Auflistung mit allen Partitionen die derzeit gemountet sind. In meinem Fall ist die Partition der SD-Karte mit disk1s1 bezeichnet.
Mit *diskutil unmountDisk /dev/disk1* wird die Partition getrennt. Anschließend kann man die SD-Karte mit folgendem Befehl beschreiben:
{% highlight bash %}sudo dd bs=1m if=/Pfad/zum/Minepeon/image of=/dev/NameDerSDKarte{% endhighlight %}
In meinem Fall lautet der Befehl komplett:
{% highlight bash %}sudo dd bs=1m if=Downloads/Minepeon-0.2.4.3.img of=/dev/disk1{% endhighlight %}
Für eine Fortschrittsanzeige kann man auch pv verwenden:
{% highlight bash %}sudo dd bs=1m if=Downloads/Minepeon-0.2.4.3.img | pv -s 8g | sudo dd bs=1m of=/dev/disk1{% endhighlight %}
Nachdem der Schreibvorgang beendet ist, kann man die SD-Karte mit `diskutil eject /dev/disk1` auswerfen.
Danach kann man die SD-Karte in den Raspberry Pi stecken und das System starten.
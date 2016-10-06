---
layout: post
title:  "Mac OS X Mavericks in VirtualBox installieren. Kleine Gedächtnishilfe..."
date:   2014-08-11 22:59:00 +0200
categories: macosx mavericks virtualbox
---
Ich habe auf meinem Macbook die Entwicklerversion von Mac OS X 10.10 Yosemite installiert. So schön wie das auch ist, manches läuft darunter _noch_ nicht. So zum Beispiel die Software [Umsatz von MOApp](http://umsatz-programm.de). Da ich diese gerade aber sehr gut gebrauchen kann (Steuern und so...), habe ich mich dazu entschlossen mal ein OS X in einer VirtualBox zu installieren.

Nachdem man das Installationspaket aus dem AppStore geladen hat, kann man die Installation einfach abbrechen. Im /Applications Ordner finden sich dann trotzdem noch die Installationsdateien. 

Da man die aber nicht einfach so als Startmedium einbinden kann, muss man sich daraus noch eine *.dmg generieren. Dazu hat [Edward Shaw](http://ntk.me/about/) ein tolles kleines Tool gebastelt. Man könnte das per [Github](https://github.com/ntkme/iesd) installieren aber es gibt einen einfacheren Weg. Per *gem*.
{% highlight bash %}
sudo gem install iesd
{% endhighlight %}
In die Konsole eintippen, Passwort rein und fertig. Nach der Installation kann man dann mit iesd ein *.dmg zur Installation generieren. 
{% highlight bash %}
iesd -i /Volumes/Install\ OS\ X\ Mavericks/Install\ OS\ X\ Mavericks.app -o Mavericks.dmg -t BaseSystem
{% endhighlight %}
In meinem Fall lagen die Installationsdaten noch auf einer SD-Karte, daher habe ich andere Pfade. Beim laden aus dem AppStore liegen die Daten zum installieren in  */Applications/Install\ OS\ X\ Mavericks.app*

Für andere Systeme, Lion zum Beispiel, oder wenn man ECC Arbeitsspeicher besitzt, gibt es noch weitere Befehle. Hierzu noch der Artikel von einem Blog dessen Anleitung ich auch genutzt habe: [How to install OS X on VirtualBox](http://www.robertsetiadi.net/install-os-x-virtualbox/)

**Update 12.08.2014:** Zu früh gefreut. Die Installation läuft nicht durch. Keine Ahnung aber mitten im Start geht es nicht mehr weiter. Ich hab ein bisschen mit den Einstellungen rum gespielt aber hat nicht funktioniert. Wenn jemand Mavericks in VirtualBox auf einem Yosemite Host ans laufen bekommen hat, würde ich mich über eine kleine Info freuen.
---
layout: post
title:  "snoopy-ng auf dem Raspberry Pi installieren"
date:   2014-01-17 22:59:00 +0200
categories: raspberrypi snoopy-ng
---
Ich hab schon wieder eine ganze weile nichts mehr geschrieben...  
Heute habe ich auf Twitter von snoopy-ng erfahren. Wer mehr darüber erfahren möchte, dem empfehle ich [diesen Artikel](http://www.heise.de/newsticker/meldung/Gratis-Software-macht-Drohnen-zu-WLAN-Schnuefflern-2289596.html). Snoopy-ng selbst findet man auf [Github](https://github.com/sensepost/snoopy-ng). Ich war direkt verblüfft und wollte das ausprobieren. Also den Raspberry Pi aus der Schublade und los gehts.

Zuerst das Paket mit wget von Github laden und per unzip entpacken. Ein 
{% highlight bash %}sudo bash install.sh{% endhighlight %}
startet die installation. Direkt der erste Dämpfer:  
Die installation bricht unmittelbar nach dem Zeitablgleich per ntp ab. Der Fehler ist schnell gefunden. In Zeile 20 in der install.sh steht:
{% highlight bash %}/etc/init.d/ntp start{% endhighlight %}
Wenn ntp aber nicht dort liegt, fliegt das Skript direkt raus. Also Zeile 20 mit
{% highlight bash %}
sh
if [ -f /etc/init.d/ntp ]; then
	/etc/init.d/ntp start
fi
{% endhighlight %}
ersetzen und die sache läuft.

Dämpfer Nummer 2: 
Abbruch bei der Installation von lxml. Keine Ahnung woran es liegt aber ein 
{% highlight bash %}sudo apt-get install libxml2-dev libxml-dev{% endhighlight %}
hat es behoben.

Jetzt läuft die Installation durch und ich kann snoopy das erste mal starten. Ich habe es direkt mal mit WLAN probiert:
{% highlight bash %}
sudo snoopy -v -m wifi:mon=True,iface=wlan0 -d myDrone -l Remscheid
{% endhighlight %}
Und ZACK, Exception... Fehler findet sich schnell, die Karte kann nicht in den Monitor Mode, da snoopy-ng dafür auf airmon-ng setzt und dieses nicht installiert ist.

Also schnell die aircrack suite laden und per *make* und *make install* installieren.  
Läuft!

Ich habe jetzt einen Capture mit etwa 140 Access Points und 14 Devices. Wobei von den 14 Devices wahrscheinlich mindestens die hälfte auf mich selbst fällt :)

Ich installiere gerade eine VM mit Kali Linux um snoopy dort auch nochmal zu installieren. Zum ansehen wird Maltego empfohlen und die Transform-Skripte benötigen sqlalchemy und dergleichen und das will ich mir nicht auf dem Mac installieren.

Wenn das alles läuft und ich mir die Auswertung mal angesehen habe, mache ich einen neuen Post.
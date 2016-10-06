---
layout: post
title:  "snoopy-ng SSID-Datensalat in der snoopy.db"
date:   2014-08-18
categories: snoopyng
---
Mir ist aufgefallen dass in der SQLite Datenbank von snoopy-ng sehr viele Access Points eingetragen werden die entweder keine sind oder deren Daten unvollständig sind. Es sind viele Einträge mit falscher oder gar nicht vorhandener MAC-Adresse und auch die SSIDs sind oft mit vollkommen absurden Einträgen versehen.
![snoopy.db wifi_ap_ssids](https://www.dropbox.com/s/sv1rpj07qglczcw/Screenshot%202014-08-17%2014.04.24.png?dl=1)
Da der Code von snoopy-ng offen ist, kann ich mir diesen natürlich auch anschauen. Da ich aber wenig bis kein Python kann, hilft mir das nicht viel. Ich habe mir das trotzdem mal zur Brust genommen und glaube den Grund zu kennen. Wenn diesen Text jemand liest der Python kann und feststellt, dass das alles Blödsinn ist was ich schreibe, würde ich mich über eine Nachricht freuen.

Also das wifi Plugin von snoopy-ng selbst führt erstmal keine Aufgaben durch. Das machen alles die sub-plugins die in dem mod80211/ Ordner liegen. Dort gibt es das Plugin wifi_aps.py, dieses ist wohl dafür da alle Daten eines Access-Points zurückzuliefern. Das sind derzeit die MAC-Adresse, die SSID, den Hersteller und einen timestamp.

Der Funktion *proc_packet* wird ein Paket des Sniffers [scapy](http://www.secdev.org/projects/scapy/) übergeben. Da ich keine Ahnung habe welche Datentypen Python kennt, nenne ich das jetzt mal ein Dictionary. Die Funktion prüft zunächst ob das Dictionary einen Bereich mit dem Namen *Dot11Beacon* besitzt, dass dort das Feld *info* im Bereich *Dot11Elt* einen Inhalt besitzt und über einen Regulären Ausdruck ob die Mac-Adresse korrekt formatiert ist. In Zeile 66 gibt es dann diese Zuweisung:
{% highlight python %}
ssid = p[Dot11Elt].info.decode('utf-8', 'ignore')
{% endhighlight %}
Diese weist der Variable ssid den Namen des Access Point aus dem Bereich 'Dot11Elt' zu.

Wenn man sich aber mal den Inhalt eines solchen Pakets anschaut sieht man, dass schon von scapy ein ganz schöner Datensalat kommt. Um mir den Inhalt des Paketes anzuschauen habe ich in Zeile 17 ein:
{% highlight python %}
from pprint import pprint
{% endhighlight %}
eingefügt und nach Zeile 57 ein:
{% highlight python %}
pprint(p)
{% endhighlight %}
Dann bekommt man so etwas als Ausgabe:
{% highlight xml %}
<RadioTap  version=0 pad=0 len=26 present=TSFT+Flags+Rate+Channel+dBm_AntSignal+Antenna+b14 notdecoded='\xf9\xad+\x00\x00\x00\x00\x00P\x02l\t\xc0\x00\xa4\x00\x00\x00' |
	<Dot11  subtype=8L type=Management proto=0L FCfield= ID=0 addr1=ff:ff:ff:ff:ff:ff addr2=00:11:22:33:44:55 addr3=00:11:22:33:44:55 SC=17792 addr4=None |
		<Dot11Beacon  timestamp=4651827584 beacon_interval=100 cap=short-slot+ESS+privacy+short-preamble |
			<Dot11Elt  ID=SSID len=11 info='APSSID' |
			<Dot11Elt  ID=Rates len=8 info='\x82\x84\x8b\x96\x0c\x12\x18$' |
			<Dot11Elt  ID=DSset len=1 info='\x01' |
			<Dot11Elt  ID=TIM len=4 info='\x00\x01\x00D' |
			<Dot11Elt  ID=ERPinfo len=1 info='\x00' |
			<Dot11Elt  ID=RSNinfo len=20 info='\x01\x00\x00\x0f\xac\x02\x01\x00\x00\x0f\xac\x04\x01\x00\x00\x0f\xac\x02\x00\x00' |
			<Dot11Elt  ID=vendor len=22 info='\x00P\xf2\x01\x01\x00\x00P\xf2\x02\x01\x00\x00P\xf2\x02\x01\x00\x00P\xf2\x02' |
			<Dot11Elt  ID=ESRates len=4 info='0H`l' |
			<Dot11Elt  ID=vendor len=24 info="\x00P\xf2\x02\x01\x01\x86\x00\x03\xa4\x00\x00'\xa4\x00\x00BC^\xaag\xba?/" |
			<Dot11Elt  ID=36 len=223 info="\xee\x13\x1b\xff\xe6v\x0e\x11\xff\x16\x1f5\xb4}\t\xa0o\x04\x11m(\xd6\xf8\xcfe\xf6\xff\x01b0I\xbb*1Yr\xe4\xb7\x13W\xf5\x92tXm\x82\x140\x00\xc8\x00\x02\xc0(\xdf|'(.\xcd\x82X\x04I\x8f\x94\xc0`2\x08\xaa\x06\x02\n\x96\xc4\x02\x00\xe0\x91D\xa1p\x12\x99\xa0Q\x8aH\xee\x0cd\xb6" |>>>>>>>>>>>>>

{% endhighlight %}
Auf Github findet man in den Issues zu snoopy-ng ebenfalls User die davon betroffen sind. glennzw schreibt dazu selbst, dass das ein Problem von scapy zu sein scheint und gerade bestimmte WLAN-Karten dafür anfällig sind. Darunter fällt leider die TPLINK TL-WN722N die ich auch zum testen verwendet habe. Ich habe noch einen Stick mit rtl8192 Chipsatz und die im MacBook integrierte Airport Extreme(?). Ich werde das ganze nochmal damit testen und dann Berichten ob eine davon besser funktioniert.
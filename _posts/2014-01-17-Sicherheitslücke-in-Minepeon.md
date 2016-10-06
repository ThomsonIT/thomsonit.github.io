---
layout: post
title:  "Sicherheitslücke in Minepeon"
date:   2014-01-17 22:59:00 +0200
categories: raspberrypi minepeon mining
---
Minepeon weist in der aktuellen Version 0.2.4 eine gravierende Sicherheitslücke auf. Die Vorraussetzungen dafür ist, dass Minepeon aus dem Internet erreichbar ist. Diese Möglichkeit haben sich aber sicherlich viele eingerichtet, um jederzeit nachsehen zu können ob alles in Ordnung ist. Auch ich hatte Minepeon durch einen lokalen Tunnel ins Internet freigegeben...

Minepeon bietet eine API an mit der man Daten vom Miner abrufen und Einstellungen tätigen kann. Diese API lauscht auf Port 4028 und ist **ohne Anmeldung nutzbar!**

Ich habe tcpdump auf meinem Minepeon installiert und diesen auf Port 4028 gestellt und die Ausgabe in eine Log-Datei umgeleitet. Bereits nach wenigen Stunden war ein anderer Miner eingestellt und ich hatte haufenweise Befehle einer russischen IP-Adresse im Log, die meine Pools mit anderen überschrieben hat.

Die Lösung:
Die Konfigurationsdatei */opt/minepeon/etc/miner.conf* in einem Editor öffnen und die Zeile api-allow bearbeiten. Das *W:0\/0* muss man durch 127.0.0.1 ersetzen. Dann ist die API nurch durch den localhost zu erreichen. Die Datei sollte dann so aussehen (meinen Pool habe ich entfernt. Seinen eigenen sollte man natürlich drin lassen):
{% highlight json %}
	{
    "http-port": "8337",
    "algo": "c",
    "api-listen": true,
    "api-port": "4028",
    "expiry": "120",
    "hotplug": "5",
    "log": "5",
    "no-pool-disable": true,
    "queue": "1",
    "scan-time": "60",
    "shares": "0",
    "kernel-path": "\/opt\/minepeon\/bin",
    "api-allow": "127.0.0.1",
    "pools": [
        {
        }
    ]
	}
{% endhighlight %}
Anschließend kann man die Datei speichern und den Miner neu starten. Die API ist dann nicht mehr von anderen beschreibbar.

**Update vom 08.02.14:** Ich habe gerade im Github-Repository gesehen, dass seit dem 08. Januar 2014 bereits 127.0.0.1 als api-allow eingetragen ist. Wenn man seinen Minepeon danach aufgesetzt hat sollte man also bereits auf der sicheren Seite sein ;-)
---
layout: post
title:  "Minepeon: Größe der SD-Karte voll ausnutzen"
date:   2014-01-10 15:42:00 +0200
categories: raspberrypi minepeon mining
---
Die Minepeon Partition hat nach dem schreiben auf die SD-Karte eine Größe von ungefähr 2GB. Wenn man eine SD-Karte verwendet die größer ist als 2GB, kann man eine weitere Partition erstellen um den vollen Umfang der Karte nutzen zu können.

Dazu benötigt man ein Programm das Partitionen verändern kann. Ich habe mich für [parted](https://www.gnu.org/software/parted/) entschieden. Dabei handelt es sich um einen relativ bekannten Partitionsmanager.

Minepeon basiert auf Arch Linux und dieses Linux hat eine Paketverwaltung mit dem Namen pacman. Damit ist es normalerweise möglich Pakete und deren Abhängigkeiten automatisch installieren zu lassen. Nachdem man per SSH mit dem Raspberry Pi verbunden ist, öffnet man mit *sudo -i* eine Superuser Konsole. Anschließend kann man mit *pacman -S parted* das Paket installieren lassen.

Leider hat das bei mir aus unbekannten Gründen nicht funktioniert, wie man am nächsten Screenshot sehen kann:  
![pacman installation von parted](http://i44.tinypic.com/2r3jf2u.png)  
Die Datei ist auf dem Server mirror.archlinuxarm.org nicht verfügbar obwohl pacman durchaus der Meinung ist, dass das Paket dort verfügbar sein sollte.
Daher habe ich zur Installation einen kleinen Umweg gewählt. Ich habe bei Google nach dem Paketnamen parted-3.1-2-armv6h.pkg.tar.xz gesucht und einen Mirror gefunden wo das Paket vorhanden ist. Den Direktlink zu dieser Datei habe ich kopiert und das Paket mit *wget http://server.tld/link/zum/paket* direkt in das root-Verzeichnis geladen. Anschließend kann man das Paket mit *pacman -U /root/parted-3.1-2-armv6h.pkg.tar.xz* installieren.

Jetzt geht es ans Partitionieren der SD-karte. Dazu gibt man {% highlight bash %}parted{% endhighlight %} ein. Da die SD-karte das einzige Speichergerät ist, dass dem System bekannt ist, wird diese automatisch gewählt.
{% highlight bash %}
minepeon ~ # parted
GNU Parted 3.1
Using /dev/mmcblk0
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)
{% endhighlight %}
Damit die Einheiten etwas besser zu nutzen sind, gibt man {% highlight bash %}unit chs{% endhighlight %} ein. Anschließend kann man mit {% highlight bash %}print{% endhighlight %} eine Liste der Partitionen anzeigen lassen.
{% highlight bash%}
(parted) unit chs                                                         
(parted) print                                                            
Model: SD SMI (sd/mmc)	
Disk /dev/mmcblk0: 15221,63,31
Sector size (logical/physical): 512B/512B
BIOS cylinder,head,sector geometry: 15222,64,32.  Each cylinder is 1049kB.
Partition Table: msdos
Disk Flags: 

Number  Start     End          Type      File system  Flags
 1      1,0,0     90,63,31     primary   fat16        lba
 2      91,0,0    1790,63,31   extended
 5      92,0,0    1790,63,31   logical   ext2	

(parted)
{% endhighlight %}
Man sieht hier das Ende der letzten logischen Partition liegt bei 1790,63,31 und die SD-Karte selbst geht bis 15221,63,31. Also ist da noch eine Menge Platz.
Mit {% highlight bash %}mkpart primary 1791,63,31 15221,63,31{% endhighlight %} legt man eine neue Partition an. Beim ersten Wert geht man eine Stelle höher als das Ende der letzten Partition und beim zweiten bis zum Ende der SD-Karte. Das Ende der SD-Karte sieht man in der Zeile die mit "Disk" anfängt. Mit *print* bekommt man die aktualisierte Tabelle angezeigt.
{% highlight bash %}
(parted) print                                                            
Model: SD SMI (sd/mmc)
Disk /dev/mmcblk0: 15221,63,31
Sector size (logical/physical): 512B/512B
BIOS cylinder,head,sector geometry: 15222,64,32.  Each cylinder is 1049kB.
Partition Table: msdos
Disk Flags:

Number  Start     End          Type      File system  Flags
 1      1,0,0     90,63,31     primary   fat16        lba
 2      91,0,0    1790,63,31   extended
 5      92,0,0    1790,63,31   logical   ext2
 3      1791,0,0  15221,63,31  primary   fat16

(parted)
{% endhighlight %}
An dieser Stelle sollte man sich die Nummer der neu erstellen Partition merken. Anschließend wird parted mit {% highlight bash %}quit{% endhighlight %} beendet und die Partition mit {% highlight bash %}mkfs.ext4 /dev/mmcblk0p3{% endhighlight %} erneut partitioniert. Mit {% highlight bash %}e2label /dev/mmcblk0p3 data{% endhighlight %} bekommt die Partition noch den Namen data zugewiesen.

Damit die Partition bei jedem Start des Systems automatisch eingehangen wird, muss man noch die datei */etc/fstab* anpassen. Dazu öffnet man die Datei mit {% highlight bash %}nano /etc/fstab{% endhighlight %} im Editor und fügt eine neue Zeile ein:
{% highlight bash %}
/dev/mmcblk0p3  /data           ext4    defaults        1       2
{% endhighlight %}
Vorhandene Zeilen auf keinen Fall verändern, sonst funktioniert das System nicht mehr!

Nachdem die Datei gespeichert ist, wird mit {% highlight bash %}mkdir /data{% endhighlight %} der Einhängepunkt im Verzeichnisroot angelegt. Zuletzt kann man die Partition mit {% highlight bash %}mount /data{% endhighlight %} einhängen.

Fertig!
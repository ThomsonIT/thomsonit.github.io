---
layout: post
title:  "Minepeon: Passwörter ändern!"
date:   2014-01-10 15:01:00 +0200
categories: raspberrypi minepeon mining
---
Da es sich bei Minepeon um ein fertiges System handelt, sind alle Passwörter schon vorher gesetzt. Die Standardkombination sowohl für die htaccess-Abfrage als auch für SSH lautet *minepeon:peon*.
Das wissen leider auch alle anderen Personen die das System kennen... Und da dieses System prinzipiell dafür genutzt wird um Geld durch das Mining zu verdienen, ist es natürlich ein beliebtes Angriffsziel.
Daher sollte man unmittelbar nach dem ersten Start des Systems die Passwörter ändern. Zunächst kann man sich in das Webinterface einloggen und unter dem Reiter Settings ein neues Passwort für die htaccess-Abfrage vergeben.

![](http://i39.tinypic.com/97pssi.png)  
Anschließend sollte man sich per SSH verbinden und sowohl das Passwort des Standardnutzers minepeon als auch das root-Passwort ändern.

Und das geht so:  
Einfach ein neues Terminal Fenster öffnen und *ssh minepeon@IP.ADRESSE.VON.MINEPEON* eingeben.
Nach Eingabe des Passworts *peon* ist man verbunden. Nun gibt man *passwd* ein. Dadurch wird das Passwort des Benutzers minepeon geändert.  
![](http://i40.tinypic.com/2a7snxk.png)  
Der root-Benutzer hat kein Standard-Passwort und darf auch nicht per SSH verbinden. Trotzdem sollte man für dieses wichtige Konto ein eigenes Passwort vergeben. Dazu gibt man *sudo passwd* ein. Dies führt dazu, dass man zunächst das eigene Passwort eingeben muss und anschließend das Passwort des Superusers ändert.
Anschließend kann man die Verbindung mit *logout* trennen und die Passwörter sind erfolgreich geändert.
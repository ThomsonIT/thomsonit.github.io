---
layout: post
title:  "MySQL-Passwort zurücksetzen unter Mac OS X 10.8.2"
date:   2012-03-23 13:34:21 +0200
categories: mysql macosx
---
Diese Anleitung beschreibt, wie man unter Mac OS X 10.8.2 das MySQL-Root-Passwort zurücksetzen kann. Ich habe selbst lange Zeit nach einer Lösung gesucht nachdem mir das MySQL-Passwort abhanden gekommen ist. Da ich keine funktionierende Lösung gefunden habe, habe ich mich selbst in dem MySQL-Manual auf die Suche gemacht und mir die Schritte selbst zusammengesucht. Folgende Schritte waren nötig um das Passwort auf ein beliebiges zu setzen:

Wer nicht den ganzen Text lesen möchte hat hier die benötigten Befehle in der Übersicht:
{% highlight bash %}
sudo /bin/sh
/usr/local/mysql/bin/mysqld stop
touch /usr/local/mysql/bin/resetpass
echo "UPDATE mysql.user SET Password=PASSWORD('passwort') WHERE User='root';" > /usr/local/mysql/bin/resetpass
echo "FLUSH PRIVILEGES;" >> /usr/local/mysql/bin/resetpass
/usr/local/mysql/bin/mysqld_safe --init-file=/usr/local/mysql/bin/resetpass &
CTRL+C
/usr/local/mysql/bin/mysqld stop
rm /usr/local/mysql/bin/resetpass
exit
exit
{% endhighlight %}

Wer sich nicht so gut auskennt, dem empfehle ich die Schritt für Schritt Anleitung.
-----

Zunächst öffnen wir das Terminal und wechseln in eine neue Shell um mit &gt;echten&lt; Root-Rechten zu arbeiten. Dazu gibt man *sudo /bin/sh* in die Konsole ein. Nach dem eingeben des Passworts (Die Eingabe ist nicht sichtbar) landet man in einer Shell die als Root ausgeführt wird. Überprüfen kann man das noch indem man *whoami* in die Konsole tippt.

{% highlight bash %}
sudo /bin/sh
sh-3.2#
sh-3.2# whoami
root
sh-3.2#
{% endhighlight %}

Als nächstes muss der MySQL-Server gestoppt werden. Dazu geben wir den Befehl */usr/local/mysql/bin/mysqld stop* in die Konsole ein. Der MySQL-Server wird dann gestoppt.

{% highlight bash %}
sh-3.2# /usr/local/mysql/bin/mysqld stop
130211 21:13:45 [Warning] Setting lower_case_table_names=2 because file system for /usr/local/mysql-5.5.20-osx10.6-x86_64/data/ is case insensitive
130211 21:13:45 [Note] ./mysqld: Shutdown complete
sh-3.2#
{% endhighlight %}

Jetzt brauchen wir eine neue Datei, die dem MySQL-Server beim nächsten Start übergeben wird. Dazu geben wir *touch /usr/local/mysql/bin/resetpass* in die Konsole ein. Anschließend müssen wir die Datei noch mit Inhalt füllen. Das erledigen wir mit den folgenden Zeilen:

{% highlight bash %}
echo "UPDATE mysql.user SET Password=PASSWORD('passwort') WHERE User='root';" > /usr/local/mysql/bin/resetpass
echo "FLUSH PRIVILEGES;" >> /usr/local/mysql/bin/resetpass
{% endhighlight %}

Wer aufmerksam gelesen hat, sieht bereits, dass das neue Passwort in den runden Klammern hinter PASSWORD steht. Dort kann man jetzt ein beliebiges Passwort einsetzen. Dabei muss darauf geachtet werden, dass die Anführungszeichen erhalten bleiben.

Mit *cat /usr/local/mysql/bin/resetpass* können wir den Inhalt der Datei nochmals überprüfen:

{% highlight bash %}
sh-3.2# cat /usr/local/mysql/bin/resetpass
UPDATE mysql.user SET Password=PASSWORD('passwort') WHERE User='root';
FLUSH PRIVILEGES;
sh-3.2#
{% endhighlight %}

Als letzten Schritt müssen wir den MySQL-Server noch einmal mit der neuen Init-Datei starten. Dazu nutzen wir den Befehl */usr/local/mysql/bin/mysqld_safe --init-file=/usr/local/mysql/bin/resetpass &*:

{% highlight bash %}
sh-3.2# /usr/local/mysql/bin/mysqld_safe --init-file=/usr/local/mysql/bin/resetpass &
[1] 86379
sh-3.2# 130211 21:29:25 mysqld_safe Logging to '/usr/local/mysql/data/Thomass-MacBook-Air.local.err'.
130211 21:29:25 mysqld_safe Starting mysqld daemon with databases from /usr/local/mysql/data
{% endhighlight %}

Das Terminal ist jetzt erstmal gesperrt. Um wieder in das Terminal tippen zu können, müssen wir die Tastenkombination *CTRL+C* betätigen. Anschließend ist das Terminal wieder benutzbar. Das Passwort ist nun geändert und eigentlich sind wir bereits fertig. Allerdings wollen wir ja auch alles wieder so hinterlassen wie wir es aufgefunden haben, daher beenden wir die Instanz nun zunächst mit */usr/local/mysql/bin/mysqld stop* wieder.

{% highlight bash %}
sh-3.2# /usr/local/mysql/bin/mysqld stop
130211 21:35:19 [Warning] Setting lower_case_table_names=2 because file system for /usr/local/mysql-5.5.20-osx10.6-x86_64/data/ is case insensitive
130211 21:35:19 [Note] /usr/local/mysql/bin/mysqld: Shutdown complete
sh-3.2#
{% endhighlight %}

Anschließend löschen wir die Datei resetpass mit dem Befehl *rm /usr/local/mysql/bin/resetpass*. Zuletzt verlassen wir die Superuser-Konsole wieder indem wir *exit* eintippen. Man befindet sich dann wieder in der "sicheren" Konsole. Diese verlassen wir ebenfalls mit dem Befehl *exit* wieder oder schließen einfach das Fenster.

Jetzt ist alles wieder aufgeräumt und man kann den MySQL-Server wieder starten. Wie auch immer man möchte, kann man dazu entweder den Computer neu starten, das PreferencePane von MySQL installieren und den Server darüber starten, oder eben über die Konsole. Der Server ist dann wieder einsatzbereit.
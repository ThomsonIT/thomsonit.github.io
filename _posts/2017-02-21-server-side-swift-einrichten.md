---
layout: post
title:  "Server-side swift mit vagrant auf dem Mac einrichten"
tags: swift
excerpt: "Ein Projekt an dem ich mitarbeite erfordert einen serverseitigen Daemon. Da ich beim Programmieren ohnehin Swift verwende und dieses inzwischen auch für den Server zur Verfügung steht, dachte ich mir ich verwende Swift für dieses Projekt!"
---
Ein Projekt an dem ich mitarbeite erfordert einen serverseitigen Daemon. Da ich beim Programmieren ohnehin Swift verwende und dieses inzwischen auch für den Server zur Verfügung steht, dachte ich mir ich verwende Swift für dieses Projekt!

Um auf dem Mac zu testen brauche ich eine virtuelle Maschine. Diese ist mit Vagrant schnell eingerichtet:
{% highlight ruby %}
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
  end

  config.vm.define "swift", primary: true do |swift|
    swift.vm.hostname = "swiftbox"
    swift.vm.network "private_network", ip: "192.168.10.22"
    swift.ssh.forward_agent = true
  end
end
{% endhighlight %}

Auf dem Server werden noch ein paar Pakete benötigt:
{% highlight bash %}
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install clang libicu-dev
{% endhighlight %}
Anschließend Swift herunterladen:
{% highlight bash %}
cd /home/vagrant
wget https://swift.org/builds/swift-3.0.2-release/ubuntu1404/swift-3.0.2-RELEASE/swift-3.0.2-RELEASE-ubuntu14.04.tar
tar -zxvf swift-3.0.2-RELEASE-ubuntu14.04.tar
echo „PATH=/home/vagrant/swift-3.0.2-RELEASE-ubuntu14.04/usr/bin:\“$(PATH)\““ >> .profile
{% endhighlight %}
Sobald man sich erneut per SSH angemeldet hat steht Swift zur Verfügung:
{% highlight bash %}
vagrant@swiftbox:~$ swift --version
Swift version 3.0.2 (swift-3.0.2-RELEASE)
Target: x86_64-unknown-linux-gnu
{% endhighlight %}
Um jetzt mit einem neuen Projekt zu beginnen erstellt man einen neuen Ordner, wechselt in diesen und gibt folgenden Befehl ein:
{% highlight bash %}
swift package init --type executable
{% endhighlight %}
Der Befehl erstellt die benötigte Ordnerstruktur um ein Projekt vernünftig zu verwalten. Wenn man nun Änderungen an den Dateien vorgenommen hat, kann man das Projekt kompilieren indem man folgenden Befehl eingibt:
{% highlight bash %}
swift build
{% endhighlight %}
Nun ist es zwar ganz nett im Terminal mit Swift zu arbeiten aber Code möchte man doch lieber in einer richtigen IDE wie Xcode bearbeiten. Daher besteht natürlich die Möglichkeit eine Xcodeproj-Datei zu erstellen, welche man mit Xcode öffnen kann:
{% highlight bash %}
swift package generate-xcodeproj
{% endhighlight %}
Zum Stand heute (21.02.2017) sieht es so aus, dass Änderungen in Xcode, beispielsweise das hinzufügen von Dateien oder das ändern irgendwelcher Abhängigkeiten, oft dazu führt, dass Xcode diese nicht richtig einliest.
Ich hatte selbst das Problem, dass nach dem erstellen einer neuen Klasse diese nicht zur Verwendung in anderen Klassen oder sogar in main.Swift zur Verfügung stand. 
Glücklicherweise wurde ich von loganwright auf ios-developers.slack.com darauf hingewiesen, dass die Xcodeproj in diesem und vielen anderen Fällen immer wieder neu generiert werden muss.
Vor allem wenn man irgendwann spezielle Compiler-oder Linker-Anweisungen benötigt kann es sehr nervig sein das Kommando immer wieder komplett eingeben zu müssen. Daher hier noch ein kleiner Tipp für ein Alias:
{% highlight bash %}
alias gx='swift package generate-xcodeproj'
{% endhighlight %}
Diesen Alias kann man erweitern (darauf komme ich in einem der nächsten Blogposts zu sprechen) falls nötig und muss dann nicht immer das komplette Kommando eingeben.

Noch eine kleine Anmerkung:
Möchte man das Projekt in Xcode bauen lassen, muss man noch das aktive Scheme vom Projekt auf die ausführbare Datei setzen. Sonst kompiliert Xcode nicht!
![Xcode edit scheme](https://blog.tboes.de/files/xcode_scheme.png)












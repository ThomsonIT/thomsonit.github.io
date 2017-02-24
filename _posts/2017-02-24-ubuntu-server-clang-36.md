---
layout: post
title:  "clang-3.6 unter Ubuntu 14.04 installieren"
tags: swift ubuntu clang
---
Auf der Vagrant-VM mit der ich für Swift entwickle ist Ubuntu 14.04 installiert. Wenn man dort den Befehl `sudo apt-get install clang` eingibt, bekommt man aus den Standard-Paketen clang-3.4 installiert. Das ist zwar grundsätzlich okay, aber der 3.0.2-Release Build von Swift wirft beim kompilieren die Meldung, dass mindestens clang-3.6 empfohlen wird. Jetzt kommt man mit einem einfachen `sudo apt-get install clang-3.6` leider nicht sehr weit, weil das binary anschließend tatsächlich nicht mehr clang sondern clang-3.6 heisst.

Hiermit kann man den Fehler beheben:

    sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-3.6 100
    sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-3.6 100

Danach geht alles seinen gewohnten Gang und man kann wieder kompilieren.

    
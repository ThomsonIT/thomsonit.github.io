---
layout: post
title:  "Der Interface Builder und Linkshänder"
date:   2014-10-15 20:29:32
categories: apple Xcode
---
Entwickler die Xcode verwenden und Linkshänder sind werden das Problem kennen: Beim ziehen der Verbindungen vom Interface Builder in den Quelltext, hat man die Arme über kreuz um mit der rechten Hand die Control-Taste zu drücken, während man mit der linken das Trackpad bedient. Für dieses Problem habe ich heute eine Abhilfe gefunden: [Karabiner](https://pqrs.org/osx/karabiner/)

Das kleine Programm erlaubt es, Tasten beliebig zu remappen.

In den Einstellungen kann man unter *Misc&Uninstall* den Button *open private.xml* auswählen und dort eigene Eintrage erstellen. Für genau diesen Anwendungsfall habe ich bereits einen Eintrag hinzugefügt:
{% highlight xml %}
<item>
    <only>XCODE</only>
    <name>Remap COMMAND_R to CONTROL_L</name>
    <appendix>Change the right Command to Control</appendix>
    <appendix>Allows left handed people to Use CTRL+Drag in Interface Builder</appendix>
    <identifier>private.remap_controll_to_optionr</identifier>
    <autogen>__KeyToKey__ KeyCode::COMMAND_R, KeyCode::CONTROL_L</autogen>
</item>
{% endhighlight %}
Wenn man diesen in die private.xml kopiert und anschließend aktiviert, ist in Xcode die rechte Command-Taste zur Control-Taste gemapped. Für mich sehr angenehm.
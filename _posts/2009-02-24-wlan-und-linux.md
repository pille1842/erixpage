---
title: WLAN und Linux -- vereint!
location: Muggensturm
---
Endlich ist mir gelungen, was so lange Jahre stets zum Scheitern verurteilt war. Ich konnte mit meinem Rechner aus dem Keller heraus unter Linux (openSUSE 10.3) eine WLAN-Verbindung zu unserem Netzwerk herstellen. Es ist geschafft!

Zunächst aber ein Rückblick. Was geschah, damit es so weit kommen konnte?

Ich entschied mich am Sonntag, einen erneuten Anlauf zu starten, Linux auf meinem Rechner zu installieren. Seit einigen Monaten war bereits ein [Ubuntu](https://www.ubuntu.com/) 8.03 mit 40 meiner 250 GB versorgt, und dieses Betriebssystem wurde zunächst auf Version 8.10 gebracht, bevor es dann losging. Das Procedere war theoretisch klar: Der WLAN-USB-Stick lässt sich nicht mit einem Linux-Treiber verwenden, weil es einen solchen nicht gibt, also muss man auf die Methode mit dem Programm "[ndiswrapper](https://sourceforge.net/projects/ndiswrapper/)" und einem Windows-Treiber zurückgreifen. Dabei wird die .inf-Datei des Windows-Treibers mit zugehörigen .sys-Dateien dem Ndiswrapper verfüttert, der sich als Kernelmodul einschaltet und so den Betrieb des Adapters möglich macht.

Machen wir es kurz. Die Verbindungsherstellung unter Ubuntu scheiterte. Ndiswrapper konnte mit keinem der verfügbaren Windows-Treiber (98, XP/2000, Vista) etwas anfangen. Im Internet riet man zur Verwendung von Ndiswrapper Version 1.47 (installiert war bei mir die neueste Version, 1.54). Die Installation dieser Ndiswrapper-Version scheiterte jedoch daran, dass der GNU-Compiler sich strikt weigerte, so eine Antiquität zu kompilieren. Schade.

Das Projekt Ubuntu, das meinen kompletten Sonntag in Anspruch genommen hatte, war also gescheitert. Am Montag ging ich jedoch frisch ans Werk und dachte mir: Vielleicht schafft es ja das bewährte SuSE Linux!

Schnurstracks ging’s also ins Arbeitszimmer, [openSUSE 10.3](https://www.opensuse.org/)-DVD holen, einlegen, installieren. Kein großes Ding.

Es folgte ein weiterer vergeblicher Versuch, den USB-WLAN-Adapter zum Laufen zu bringen (es handelt sich dabei übrigens um einen Siemens Gigaset USB Adapter 108). Im Systemlog vermerkte Ndiswrapper kühl: “Windows driver could not initialize device.” Mööp.

Am Montag Abend kam mir dann die Idee, die WLAN-PCI-Karte meines Bruders statt meines eigenen USB-Adapters auszuprobieren. Das erwies sich als gute Idee. Die Karte vom Typ NETGEAR WG311 v3 ließ sich zwar auch nicht problemlos, aber wenigstens überhaupt in Betrieb nehmen. Die Schritte waren die gleichen: Windows-Treiber (für XP) mit Ndiswrapper laden, Ndiswrapper in den Kernel holen, Netzwerkeinstellungen treffen. Nach einigen Anläufen klappte die Konfiguration der WPA-PSK-Verschlüsselung und die Verbindungsherstellung zum LAN.

Ich konnte also zum ersten Mal unter Linux einen Ping entsenden. Die Freude erwies sich aber als verfrüht. Problemlos ließ sich zwar mit allen Rechnern im lokalen Netzwerk kommunizieren, von der Außenwelt war ich aber nach wie vor durch die lapidare Meldung des Ping-Befehls abgeschnitten: “Network is unreachable.”

Eine weitere Nacht musste vergehen, bevor mein ehrenwerter Alter Herr auf die Idee kam, statt DHCP einmal die statische Konfiguration von IP-Adresse, DNS-Server etc. zu verwenden. Und: Zack! Es läuft. Demnächst wird Ndiswrapper nun also in die Startkonfiguration zum automatischen Starten eingestellt.

Ein kleiner Ausblick: morgen abend wird das neue openSUSE 11.1 installiert. Dann die ganze Schweinerei nochmal wiederholen, Dateien von Windows auf Linux migrieren und die Windows-Partition dann löschen, vorausgesetzt, alles klappt wie geplant. Dann wird Windows bei mir zukünftig nur noch als virtuelle Maschine laufen, wenn es notwendig ist. :)

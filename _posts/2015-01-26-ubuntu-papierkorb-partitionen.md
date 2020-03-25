---
title: "Ubuntu 14.04: Papierkorb auf anderen Partitionen einrichten"
location: Rastatt
---
Mein PC verfügt über eine Solid State Disk, auf der das Betriebssystem (die Root-Partition) beheimatet ist. Die Partitionen /home, /var und /srv liegen jedoch allesamt auf der ebenfalls eingebauten normalen Festplatte (HDD). Standardmäßig ist es nicht möglich, auf /var und /srv den Papierkorb zu benutzen – man kann Dateien nur endgültig löschen. Das liegt an der Funktionsweise des Papierkorbs. Auf jeder Partition (außer /home) legt Ubuntu einen Ordner ab, der als Papierkorb fungiert. Dieser setzt sich zusammen aus dem Wort ".Trash" und der User-ID des jeweiligen Benutzers, also z.B. ".Trash-1000" für den User mit der ID 1000.

Da aber die Verzeichnisse /var und /srv für normale Benutzer nicht beschreibbar sind, kann das System den Papierkorb-Ordner nicht anlegen und verweigert es daher, Dateien in den Papierkorb zu legen.

Dass für jede Partition ein eigener Papierkorb-Ordner angelegt wird, ist übrigens durchaus sinnvoll: Sonst müssten Dateien, die in den Papierkorb verschoben werden, tatsächlich in das Home-Verzeichnis des Benutzers und damit ggf. über Partitionsgrenzen hinweg kopiert werden, was den “Löschvorgang” unnötig verlangsamt.

Wie sorgt man nun also dafür, dass auch auf anderen Partitionen ein Papierkorb zur Verfügung steht?

## Verzeichnisrechte ändern

Einfachste Möglichkeit: Die entsprechenden Verzeichnisse mit Schreibrechten für andere Benutzer ausstatten (die Verzeichnisse /var und /srv gehören dem Benutzer root und sind von normalen Systembenutzern nicht beschreibbar). Dazu genügt es, folgenden Befehl auszuführen:

```
sudo chmod o+w /var /srv
```

Natürlich muss man diese “Lücke” nicht dauerhaft geöffnet lassen. Es genügt, in beiden Verzeichnissen jeweils eine Datei “test.txt” anzulegen und diese anschließend mit dem Dateimanager in den Papierkorb zu werfen. Das “Trash”-Verzeichnis ist dann angelegt und die Schreibrechte können wieder zurückgestellt werden:

```
sudo chmod o-w /var /srv
```

## Papierkorb-Ordner manuell anlegen

Natürlich kann man auch einfach für jeden Systembenutzer manuell einen Papierkorb-Ordner anlegen. Die Nummer “1000” ist bei den folgenden Befehlen jeweils durch die User-ID des betreffenden Benutzers auszutauschen. Um die ID eines Benutzers herauszufinden, benutze einfach den folgenden Befehl:

```
cat /etc/passwd | grep benutzername
```

Die Ausgabe sollte in etwa so aussehen:
```
benutzername:x:1000:1000:Vorname Nachname,,,:/home/benutzername:/bin/bash
```

Die Zahl nach "x:" ist die User-ID. Um nun auf den Partitionen /var und /srv jeweils einen Papierkorb-Ordner anzulegen, tipp ein:

```
sudo mkdir /{srv,var}/.Trash-1000
sudo chown benutzername:benutzername /{srv,var}/.Trash-1000
```

Der Papierkorb sollte jetzt auch auf diesen Partitionen funktionieren. Viel Spaß damit. 😉

---
title: "Ubuntu 14.04: Fehler beim Software-Update beheben"
location: Rastatt
---
Seit einigen Tagen kommt bei manueller Auslösung von "apt-get update" zur Erneuerung der Paketquellen unter Ubuntu 14.04 ein Fehler auf, der sich zuletzt beim Update-Versuch einiger Repositories in der Meldung äußerte: "Failed to fetch http://some-ubuntu-server/some-repo: Hash Sum mismatch"

Die entsprechende Quelldatei wurde nicht erneuert, so dass potenziell Updates nicht ausgeliefert wurden. Der Fehler scheint bis heute zu bestehen, im entsprechenden Bugreport (#1373598) ist bereits ein Lösungsansatz aufgetaucht: Einfach von den deutschen Ubuntu-Servern auf Schweizer Server wechseln, die funktionieren nämlich nach wie vor.

Dazu führen wir im Terminal folgende Befehle aus. In der Datei "sources.list" sind die Paketquellen verzeichnet; wir legen zunächst eine Kopie unter dem Namen sources.list.ch an und benennen sources.list dann in sources.list.de um. Dann legen wir einen Softlink an, mit dem man beim Aufruf von sources.list zu sources.list.ch weitergeleitet wird. Diese Datei (sources.list.ch) bearbeiten wir dann.

```
$ cd /etc/apt
$ sudo cp sources.list sources.list.ch
$ sudo mv sources.list sources.list.de
$ sudo ln -s sources.list.ch sources.list
$ sudo nano sources.list.ch
```

In der Datei sources.list.ch ersetzen wir nun jedes Auftauchen von "de.ubuntu." durch "ch.ubuntu.". Damit sind wir effektiv auf die Schweizer Server gewechselt.

Um wieder auf deutsche Server zu wechseln, genügt folgendes. Wir löschen den Link "sources.list" und legen einen neuen an, diesmal mit Verweis auf "sources.list.de".

```
$ cd /etc/apt
$ sudo rm sources.list
$ sudo ln -s sources.list.de sources.list
```

Und um alle Änderungen rückgängig zu machen: Den Link "sources.list" und die Datei "sources.list.ch" löschen und die "sources.list.de" wieder in "sources.list" umbenennen.

```
$ cd /etc/apt
$ sudo rm sources.list sources.list.ch
$ sudo mv sources.list.de sources.list
```

Damit können wir alle Updates herunterladen, obwohl bei den deutschen Servern momentan irgendetwas nicht in Ordnung zu sein scheint.

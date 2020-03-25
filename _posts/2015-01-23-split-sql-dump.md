---
title: "The sky is the limit: Große SQL-Dateien zum Hochladen splitten"
location: Rastatt
---
Ach, wie sehr vermisst man doch den Root-Shellzugang, wenn man ihn einmal nicht hat. Um einen Kunden umzuziehen, musste ich heute auf einen phpMyAdmin zurückgreifen, der Uploads nur bis 8MB erlaubt. Was tun, wenn selbst der gezippte SQL-Dump aber 10MB groß ist?

![]({% link assets/images/sqldump.gif %})

*Im Bild: Programmierer wird von riesigem SQL-Dump erschlagen.*

Klar, die Datei teilen. Anstatt einen Editor zu bemühen, der versucht, diese Riesen-Textdatei (entpackt ca. 85MB) in den Speicher zu laden, kann man auch bequem auf jahrzehntealte Unix-Tools zurückgreifen. Die hier beschriebene Methode lässt sich bestimmt noch sehr verbessern, aber da es funktioniert hat, sei es dennoch erklärt.

## Schritt 1: Die Datei in der Mitte teilen.
Dafür verwenden wir das Kommando split:

```
split -n 2 dump.sql dump_
```

Die Option "-n 2" sagt dem Befehl, dass er die Datei in zwei gleich große Teile spalten soll. "dump.sql” ist unser MySQL-Dump. Der letzte Parameter gibt das Präfix vor, das split vor die erzeugten Dateien setzen soll. Wir erhalten zwei neue Dateien: dump_aa und dump_ab.

## Schritt 2: Evtl. abgeschnittene Kommandos reparieren.
Beim Zerteilen kann es natürlich passieren, dass wir ein SQL-Statement zersägen. Darum inspizieren wir die letzte Zeile der ersten Teildatei mit tail:

```
tail -n 1 dump_aa
```

Wenn das Ende des ausgegebenen Textes kein Semikolon (und damit ein abgeschlossenes SQL-Statement) ist, müssen wir reparieren. Und das ist wahrscheinlich der Fall. Mit ein bisschen Glück fängt der ausgegebene Text mit einem neuen Statement wie "INSERT INTO” oder "CREATE TABLE” an. Ansonsten muss man sich eben durch Veränderung des Arguments "-n 1" die letzten paar Zeilen ausgeben lassen und den folgenden Schritt anpassen. (Zum Beispiel "-n 5", um die letzten fünf Zeilen zu inspizieren und zu entscheiden, wo das letzte Statement der Datei beginnt.)

Hier sei einmal vorausgesetzt, dass wirklich nur die letzte Zeile kein vollständiges Statement ist. Diese letzte Zeile schneiden wir jetzt ab und speichern sie temporär in einer dritten Datei:

```
tail -n 1 dump_aa > temp.sql
```

Die erste Teildatei muss natürlich um diese letzte Zeile gekürzt werden. Dafür eignet sich das Gegenstück zu tail: head.

```
head -n -1 dump_aa > PART1.sql
```

Dieser Befehl kopiert die ganze erste Teildatei mit Ausnahme der letzten Zeile (-1) in die neue Datei PART1.sql.

Jetzt hängen wir die in temp.sql gespeicherte letzte Zeile ganz vorne an die zweite Teildatei (dump_ab) dran, und zwar mit einem simplen cat:

```
cat temp.sql dump_ab > PART2.sql
```

Den Schluss der ersten und den Anfang der zweiten Teildatei kann man abschließend noch auf eventuell aufgetretene Fehler überprüfen:

```
tail -n 1 PART1.sql
head -n 1 PART2.sql
```

## Schritt 3: Aufräumen und zippen.
Wenn wir sicher sind, dass alles glattgelaufen ist, löschen wir die überflüssigen temporären Dateien (rm).

```
rm temp.sql dump_aa dump_ab
```

Und zum Abschluss packen wir jeden der erzeugten Teil-Dumps in ein eigenes GZip-Archiv, um ihn ins phpMyAdmin hochladen zu können.

```
gzip PART1.sql
gzip PART2.sql
```

Fertig!

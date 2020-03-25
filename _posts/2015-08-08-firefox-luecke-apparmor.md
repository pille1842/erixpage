---
title: "Firefox-Sicherheitslücke: Zugriff auf sensible Dateien per AppArmor verhindern"
location: Rastatt
---
Vor zwei Tagen veröffentlichte Mozilla in seinem Sicherheits-Blog [Informationen über eine Sicherheitslücke in Firefox](https://blog.mozilla.org/security/2015/08/06/firefox-exploit-found-in-the-wild/), die es über den eingebauten PDF-Viewer erlaubt, den Computer des Benutzers nach Dateien zu durchsuchen und diese ins Internet hochzuladen. Zuvor war auf einer russischen Nachrichtenseite eine Werbeanzeige entdeckt worden, über die Dateien wie der Inhalt des .ssh-Verzeichnisses (SSH-Schlüsselpaare usw.), die mysql_history (Historie der zuletzt eingegebenen MySQL-Befehle) etc. pp. auf einen Server in der Ukraine hochgeladen wurden.

Mozilla hat sofort reagiert und einen Fix veröffentlicht – wer noch nicht auf die Firefox-Version 39.0.3 geupdated hat, sollte das dringend tun.

Ganz abseits von diesem konkreten Fall macht es aber deutlich, dass Firefox als weit verbreiteter Browser auch auf Linux-Systemen ein Einfallstor für Schadsoftware ist. Die Frage ist also: Wie kann man sensible Informationen wie SSH- und GnuPG-Schlüssel, den GNOME-Keyring und ähnliche Dateien vor dem Zugriff durch den Browser schützen?

## AppArmor-Profil für Firefox in Ubuntu standardmäßig deaktiviert

Ubuntu liefert ein AppArmor-Profil aus, über das der Zugriff von Firefox auf einige sensible Bereiche des Systems verhindert wird. Dieses Profil ist jedoch standardmäßig nicht aktiviert, weil es unter Umständen zu Problemen mit einigen Erweiterungen (z.B. solchen, die GnuPG benutzen) kommen kann. Das kann man dem technisch unbedarften Benutzer eben nicht zumuten. Das Profil ist im Ubuntu-Wiki ausführlich spezifiziert und erklärt. Wer keine Firefox-Erweiterungen benutzt, die GnuPG benötigen, kann es vermutlich ohne Probleme aktivieren.

Ich musste dazu zunächst das Paket apparmor-utils installieren:

```
sudo apt-get install apparmor-utils
```

Im Anschluss steht das Programm "aa-enforce" zur Verfügung, mit dem sich AppArmor-Profile aktivieren lassen. Dieses Programm muss wiederum mit Administratorrechten ausgeführt werden, um das mitgelieferte Firefox-Profil zu aktivieren:

```
sudo aa-enforce /etc/apparmor.d/usr.bin.firefox
```

Nun sollte man alle Firefox-Instanzen beenden. Starte nun einen Firefox und gib folgenden Befehl ein, um zu prüfen, ob das Profil aktiviert ist:

```
$ sudo aa-status
25 profiles are in enforce mode.
   /usr/lib/firefox/firefox{,*[^s][^h]}
   /usr/lib/firefox/firefox{,*[^s][^h]}//browser_java
   /usr/lib/firefox/firefox{,*[^s][^h]}//browser_openjdk
   /usr/lib/firefox/firefox{,*[^s][^h]}//sanitized_helper
8 processes are in enforce mode.
   /usr/lib/firefox/firefox{,*[^s][^h]} (8490)
```

(Einige Zeilen habe ich hier ausgelassen.) Wenn unter den Profilen Firefox vorkommt, ist das schon mal gut. Ein laufender Firefox sollte dann auf jeden Fall unter den Prozessen im "enforce mode" gelistet werden.

Du kannst nun versuchen, in der URL-Eingabezeile von Firefox per "file:///home/BENUTZERNAME/.ssh/id_rsa.pub" auf Deinen öffentlichen SSH-Schlüssel zuzugreifen – wenn alles geklappt hat, dann wird die Datei in Firefox nicht angezeigt werden. Zumindest Deine SSH-Schlüssel und Deine GnuPG-Dateien sind nun sicher vor Angriffen.

## Was kann man noch tun?

Am sichersten ist es natürlich immer, Firefox in einer virtuellen Maschine auszuführen, die nicht auf Dateien außerhalb der VM zugreifen kann – das ist jedoch gleichzeitig auch ein sehr aufwendiger und ressourcenfressender Weg, die eigenen Dateien zu schützen. Eine andere Möglichkeit wäre, Firefox unter einem anderen Benutzernamen auszuführen, mit eigenem Home-Verzeichnis und ohne Leserechte auf sensible Bereiche des Systems und die Benutzerdateien.

Es gibt auch diverse Sandboxing-Möglichkeiten, um Firefox in einer Umgebung auszuführen, die vom Rest des Systems abgeschnitten ist. Ein guter Ausgangspunkt für die Lektüre ist [diese StackExchange-Seite](http://security.stackexchange.com/questions/56703/best-method-to-sandbox-x-applications-in-ubuntu).

Wie immer gilt: Man sollte nicht übermäßig paranoid sein. Es macht natürlich nachdenklich, dass man sich über den Browser unter Umständen auch unter Linux Leute auf den heimischen Computer holt, die dort nichts verloren haben. Die Aktivierung des AppArmor-Profils ist nicht mit viel Aufwand verbunden und schützt schon einmal einige sensible Bereiche des Systems vor den Argusaugen des Browsers. Wer mehr tun will, muss sich tiefer reinknien. Auch für Kubuntu-, Xubuntu- und sonstige Derivatsbenutzer ist die Aktivierung des AppArmor-Profils noch mit ein bisschen Recherche verbunden (z.B. für den Schutz von KWallet unter KDE).

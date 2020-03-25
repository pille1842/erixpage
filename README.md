Dieses Repository enthält die Quelldateien meiner Website [erixpage.de](http://www.erixpage.de/).

## Build
Funktionsfähige Ruby-Entwicklungsumgebung vorausgesetzt. Zunächst Jekyll und Bundler installieren:

```
gem install jekyll bundler
```

Anschließend das Repository klonen. Im Verzeichnis können dann alle Jekyll-Befehle ausgeführt werden, z.B.:

```
bundle exec jekyll serve
```

Zum Deployment muss lediglich der Inhalt des Verzeichnisses `_site/` ans Ziel hochgeladen werden.

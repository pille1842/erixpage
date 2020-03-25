---
title: "Output all the things: PHP-Debug mit printvar"
location: Rastatt
---
Manche Leute haben ziemlich schicke PHP-Debugger zur Verfügung, setzen in ihren Eclipse-Arbeitsumgebungen einen Breakpoint und inspizieren die Programmvariablen. Das weicht leider von einer Arbeitswirklichkeit als "Mädchen für alles" ab, wo man in der Regel am offenen Herzen per FTP an den Kundendomains arbeitet und tatsächliche Prozesse prüfen muss.

Jeder, der so mit PHP arbeitet, nutzt vermutlich in irgendeiner Form print_r() oder var_dump(), um sich auch die Inhalte von Arrays und Objekten anzeigen zu lassen.

Von meinem Arbeitgeber abgeschaut und auch bei privaten Projekten gerne im Einsatz ist folgende Funktion namens printvar(), die per print_r() die übergebene Variable ausgibt und dabei noch einen <pre>-Block darum setzt, damit das Ganze hübsch im Monospace-Font gedruckt wird.

{% highlight php lineno %}
function printvar($var) {
    echo '<pre>';
    print_r($var);
    echo '</pre>';
}
{% endhighlight %}

Das ist schön und gut, wenn man sich mal kurz eine Variable auf den Bildschirm werfen will, um zu verstehen, was nun schon wieder schiefläuft. Manchmal will man aber auch in einem Aufwasch den Inhalt mehrerer Variablen anzeigen. Man kann dann entweder mehrere printvar()-Aufrufe hintereinander setzen oder aus den Bestandteilen ein Array formen:

{% highlight php %}
printvar(array($var1, $var2, $var3));
{% endhighlight %}

Hübscher ist es, wenn man sich zunutze macht, dass PHP auch eine variable Anzahl von Funktionsparametern unterstützt. Printvar 2.0 sieht folgendermaßen aus:

{% highlight php lineno %}
function printvar() {
    echo '<pre>';
    $args = func_get_args();
    foreach ($args as $a) {
        print_r($a);
    }
    echo '</pre>';
}
{% endhighlight %}

Der Aufruf erfolgt dann einfach nur noch mit:

{% highlight php %}
printvar($var1, $var2, $var3);
{% endhighlight %}

Der ultimative Debug-Kick stellt sich ein, wenn man auch noch eine irgendwie global gesetzte Debug-IP einbindet, damit der unbedarfte Kunde nicht auf einmal den halben Programminhalt vorgeworfen kriegt. Die finale Version von Printvar sieht dann so aus (vorausgesetzt, die Debug-IP wird als globale Konstante DEBUG_IP definiert):

{% highlight php lineno %}
function printvar() {
    if (DEBUG_IP == $_SERVER['REMOTE_ADDR']) {
        echo '<pre>';
        $args = func_get_args();
        foreach ($args as $a) {
            print_r($a);
        }
        echo '</pre>';
    }
}
{% endhighlight %}

Voila! Aber Vorsicht: Man gewöhnt sich so schnell an das Vorhandensein von printvar(), dass man bald nicht mehr ohne kann 😉

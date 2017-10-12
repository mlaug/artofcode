---
id: 901
title: 'Skalierung im Startup &#8211; müssen es immer Server sein?'
date: 2013-11-23T12:18:54+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=901
permalink: /2013/11/23/skalierung-im-startup-muessen-es-immer-server-sein/
categories:
  - Allgemein
---
Ich bin mittlerweile 4 Jahre bei <a href="http://www.lieferando.de" target="_blank" class="broken_link">Lieferando</a> (yd. yourdelivery GmbH) und habe alle Phasen eines Startups mitgemacht. Zu Anfang bedeutet das in der IT vor allem eins: _kaum Geld_ == _kaum Server_. Und später muss gespart werden, d.h. werden auch nicht sonderlich mehr. Aus der Not wurde bei mir jedoch schnell eine Tugend. Neue Implementierungen wurden klar auf ihre Skalierbarkeit geprüft und nicht erst unser <a title="Software in Schichten – die neue Seite von Lieferando" href="http://www.artofcode.de/2013/05/16/software-in-schichten-die-neue-seite-von-lieferando/" target="_blank">aktuelles Konzept</a> ist voll darauf ausgelegt selbst bei großem Ansturm mit nur einer Handvoll Server auszukommen &#8211; sprichwörtlich! Das gibt uns nicht nur im Vergleich zur Konkurrenz einen großen Vorteil, da mehr Kunden bei uns nicht mit mehr Servern einhergehen. Unser Software läuft daher nicht nur &#8222;with scale&#8220; sondern gleichzeitig auch &#8222;at scale&#8220;, d.h. wir arbeiten in der IT extrem Kosteneffizient.

Zum Erreichen dieses Ziels kristallisierten sich im Laufe der Zeit ein paar Regeln:

## Perfomance is a Feature! &#8211; do you feel the need for speed?

Der Leitsatz von Jeff Atwood (Stackoverflow, Discourse) ist bei mir eingeschlagen wie eine Offenbarung. Er bringt es auf den Punkt, dass die Performanz einer Applikation seine Zeit braucht und genau so viel Einsatz benötigt wie die Neuentwicklung eines neuen Features. Dabei fängt Skalierung nicht erst in der Architektur der Software an, sondern schon viel früher in der Auswahl der Technologie. Gute Skalierung beginnt auf dem planen und skizzieren der Software.

Nicht jeder hat die Zeit und das Geld die Entscheidung PHP weiterhin zu verwenden durch solch eine Technologie wie [HipHop](https://www.facebook.com/notes/facebook-engineering/speeding-up-php-based-development-with-hiphop-vm/10151170460698920) zu kompensieren. Lieber gleich zu Anfang die richtige Entscheidung treffen&#8230;

Stellt das hier eine <a href="https://www.youtube.com/watch?v=CUpwLhZh66A" target="_blank">typische Szene </a>bei euch im Büro dar? Dann ist das Team auf dem richtigen Weg!

## Testcases to the rescue

Bei Lieferando achte ich akribisch auf die Qualität der Software und sehe Testcases als festen Bestandteil der Entwicklung. Ob es nun unbedingt der Test-First Ansatz sein muss, lasse ich mal im Raum stehen. Mir ist letztendlich wichtig, dass Testcases geschrieben werden und das diese eine genau so hohe Qualität haben müssen wie die Implementierung selber. Erst wenn eine ordentliche Testabdeckung erreicht wurde, können Fehler schnell erkannt werden und auch Flaschenhälse ermittelt werden. In JUnit aber auch in anderen Test Frameworks hilft hier die Annotierung mit &#8222;_timeout&#8220; _um zu prüfen, ob die eigenen Implementierungen auch innerhalb einer definierten Zeit durchlaufen.

<pre>@Test(timeout=1000)
public myTest(){
    // ....
}</pre>

Arbeitet mit solchen Testcases wird dem Team eines sehr schnell klar: Performanz ist Teil der Implementierung, zu einem Gewissen Anteil auch Bestandteil von Korrektheit des gesamten Programms. Eine langsame Applikation muss korrigiert werden, benötigt Anpassungen.

Wer aber doch lieber nach Gründen sucht, warum er keine Testcases schreiben sollte, kann sich einfach <a href="http://www.javacodegeeks.com/2013/10/10-reasons-why-you-should-not-write-unit-test-cases.html" target="_blank">hier</a> austoben&#8230;

## Lieber 7 Zwerge als Optimus Prime &#8211; Klasse statt Masse

Wenn es hier um die Filmauswahl gehen würde, sähe meine Entscheidung wahrscheinlich genau anders herum aus. Aber in der Entwicklung präferiere ich es viele kleine Komponenten zu haben die im Verbund miteinander zusammen arbeiten als eine monolitische hochkomplexe Maschine. Die Abhängigkeiten je Komponente beschränkt sich damit auf ein Minimum und die Analyse der Performanz erfordert keine teuren Tools. Ameisen haben sich evolutionär nunmal auch vor den Dinosauriern durchgesetzt und die Natur hat in der Regel recht &#8230;

[None](http://i.imgur.com/ViPceht.jpg){.embedly-card}
  


## Fremde Köche versalzen die Suppe &#8211; Sorge für Redundanz

Man kann sich noch so lange den Kopf über die perfekte Architektur zerbrechen. Irgendwann kommt der Punkt und man muss Schnittstellen von anderen Anbietern implementieren. Und schon hat man den Salat. Fremde Software, fremde Server, &#8230; alles meist schon kein Spass bei den Eigenen. Bietet jede einzelne Komponenten eine Garantierte Verfügbarkeit von 99,9% an, bedeutet dies bei 10 Fremdabhängigkeiten schon nur noch eine allgemeine Verfügbarkeit von 98,9% des eigenen Services in seiner Gänze. Das sind 96,4 Stunden Probleme oder Ausfall im Jahr, oder 4 Tage! Aaaaaargh 🙁 So viel hält kein Techniker-Herz aus, also heißt es Redundanzen aufzubauen. Jeder Service, der mehrfach verfügbar ist, sollte auch redundant eingebaut werden. Wechsel zwischen den Diensten sollte automatisch erfolgen, nie in manueller Arbeit. Andernfalls könnten die Probleme im Support bereits eskalieren.

[<img alt="viprinet_redundanz_desktop_1680_1050" src="http://www.artofcode.de/wp-content/uploads/2013/10/viprinet_redundanz_desktop_1680_1050.jpg" width="710" height="443" />](http://www.artofcode.de/wp-content/uploads/2013/10/viprinet_redundanz_desktop_1680_1050.jpg)

Mehr Server bedeutet für mich jedoch nicht, dass die IT ineffizient arbeitet. Sobald sich aber manifestiert das mit jeder Wachstumsstufe der Firma auch deutlich mehr Server benötigt werden, läuft meiner Meinung nach etwas falsch.
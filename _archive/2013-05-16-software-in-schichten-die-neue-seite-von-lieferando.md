---
id: 733
title: 'Software in Schichten &#8211; die neue Seite von Lieferando'
date: 2013-05-16T08:15:53+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=733
permalink: /2013/05/16/software-in-schichten-die-neue-seite-von-lieferando/
image: /wp-content/uploads/2013/05/IMG_9921.jpg
categories:
  - Allgemein
  - Pagespeed
  - Teamarbeit
  - Webentwicklung
---
[
  
](http://www.artofcode.de/wp-content/uploads/2013/05/IMG_9921.jpg) Wenn ich gerne auf etwas zurück schaue, dann ist es der Moment, bei dem ich die erste Skizze der neuen Architektur mehrmals vor mich hingekritzelt habe. Unabhängige Komponenten, komplett losgelöst von fremden Abhängigkeiten, die über einen Bus kommunizieren. Buzzwords die ein technisches Herz höher hüpfen lassen. Aber auch ein Konzept, was sich immer stärker verbreitet und auch nicht sonderlich neu ist. Unser System sollte aber ein paar zusätzliche Anforderungen erfüllen, die leider in vielen Planungen eher selten berücksichtigt werden.

  1. <a name="topic1"></a>Software Skalierung, keine naive Skalierung mit Hardware
  2. <a name="topic2"></a>Kosteneffizient, Punkt 1 soll mit nur **einer** Handvoll Server erreicht werden
  3. <a name="topic3"></a>Unabhängige Komponenten &#8211; mit Schnittstellen zum Erfolg

Ich war und bin stets ein Freund der Softwareskalierung. Ich kriege Magengeschwüre bei dem Gedanken eine Farm von 25 Servern zu managen. Auch bei der Verwendung von Puppet gibt es hier eine zusätzliche Fehlerquelle, wenn so viele Server zusammen arbeiten müssen. Es gibt gute Gründe Server um sich zu scharen, aber für uns haben wir beschlossen, dass es unnötig ist: Wir haben keine hochdynamischen Inhalte, sondern erfahren ein sehr humanes Intervall von Datenänderung auf der Seite. Warum also die Seite konstant dynamisch halten? Richtig &#8230; Schwachsinn!

Von da an war es noch ein langer Weg, bis das System, wie wir es jetzt auf [lieferando](http://www.lieferando.de){.broken_link} vorfinden, seine endgültige Gestalt angenommen hat und die oben genannten Bedingungen erfüllt.

<!--more-->

## Der Weg ist das Ziel &#8211; der Prototyp

Wir haben 2009 mit PHP angefangen. PHP ist eine einfache Sprache und Entwickler lassen sich für die Zusammenstellung eines Teams schnell finden. Schnell hielt die Software jedoch nicht mehr dem wachsenden Erfolg der Firma stand und zeigt in 2011 erstmals Schwächen in der Skalierung zu Peakzeiten. Ab diesem Zeitpunkt haben wir technisch eine andere Marschroute gesetzt, die uns erfolgreich bis in das Jahr 2012 schiffte. Skalierung wurde als Feature und als solches auch mit entsprechenden Manntagen eingeplant und umgesetzt. Was wir Mitte 2012 vorfanden war dann eine Software, die vorerst mit den Unternehmensanforderungen zurecht kam, jedoch vor uns wie ein ausgewrungener Schwamm lag. Wir sahen wenig Potential für weitere Optimierungen was auch daran lag, dass die Software mitnichten ein Schwamm sondern ein Betonklotz war. Der gesamte Softwarestack erstreckte sich von unten bis oben in einem Guss. Ein Zustand, den wir ändern wollten.

Nach einigen Gesprächen mit verschiedenen Leuten*, die diesen Weg auch gehen mussten, war die Richtung klar. Die Technologien noch lange nicht&#8230;

Das Trennen von zwei Systemen verlangt immer einen Kommunikationsbus um Informationen zwischen den einzelnen Komponenten zu gewährleisten. Hier haben wir uns sehr schnell für <a href="http://www.rabbitmq.com" target="_blank">RabbitMQ</a> entschieden. Einfach zu installieren, gute Tutorials und bekannte Stabilität und Skalierung. Jetzt wussten wir also, wie miteinander gesprochen wird. Aber wer soll hier eigentlich miteinander sprechen? Eine Phase des Experimenterens begann und fand ihren Höhepunkt in einer groben technischen Skizze.

<p style="text-align: center; position: relative;">
  <a href="http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-15-um-15.53.50.png"><img class=" wp-image-800 aligncenter" alt="Bildschirmfoto 2013-05-15 um 15.53.50" src="http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-15-um-15.53.50.png" width="640" height="484" srcset="http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-15-um-15.53.50.png 1142w, http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-15-um-15.53.50-300x226.png 300w, http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-15-um-15.53.50-1024x774.png 1024w" sizes="(max-width: 640px) 100vw, 640px" /></a>
</p>

Mit der hier dargestellten Trennung der Komponenten war dann auch die Wahl der Programmiersprache nach einigen Diskussionen gefunden. [Punkt 3: check](#topic3)

  * **Worker: **PHP mit <a href="http://framework.zend.com" target="_blank">ZF2</a>
  * **API: **Java mit <a href="http://www.springsource.org" target="_blank">Spring</a>
  * <span style="line-height: 13px;"><strong>Caching: </strong>Javascript mit <a href="http://nodejs.org" target="_blank">NodeJS</a></span>
  * <span style="line-height: 13px;"><strong>Frontend: </strong>Javascript mit <a href="http://canjs.com" target="_blank">CanJS</a></span>

Jede einzelne Komponente ist im MVC Stil aufgebaut worden. Ein Zustand, der besonders in Browserapplikationen selten vorgefunden wird. Mit der Wahl von Technologie und Struktur begann im Oktober 2012 die erste Entwicklung der Software und ich muss gestehen, dass wir teilweise etwas blauäugig gestartet sind. Wir haben an allen Stellen vollständig neue Technologien eingesetzt, bei denen die meisten nicht mehr als die Dokumentation gelesen haben. So sollte es nicht lange auf sich warten lassen, dass wir auf erste technische Probleme stießen, die in Retrospektive jetzt eher klein aussehen, uns vor 6 Monaten aber noch vor Probleme gestellt haben.

## [<img class="alignleft size-full wp-image-759" alt="Bildschirmfoto 2013-05-05 um 17.40.24" src="http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.40.241.png" width="562" height="428" srcset="http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.40.241.png 562w, http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.40.241-300x228.png 300w" sizes="(max-width: 562px) 100vw, 562px" />](http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.40.241.png)

Das größte und umfassendste Problem stellte dabei der Caching Mechanismus dar. Bei der Vorstellung der Technologien dürfte man sich nämlich fragen, was NodeJs mit Caching zu tun hat. Die in NodeJs implementierte Software ist der Kleber zwischen der Frontend Applikation und der API. Da wir sehr stark abhängig von der Sichtbarkeit bei Google sind, können wir es uns nicht leisten, dass die Seiten über Abfrage der API erst generiert werden, wenn der Benutzer sie besucht. So hatten wir die Idee, dass die Applikation in NodeJs interpretiert wird, so vorgeneriert wird und hiermit von Google u.a. direkt einlesbar ausgegeben wird. NodeJs war jedoch nicht unsere Erste Wahl &#8211; leider Gottes &#8211; sondern wir wollten API nah in Java implementieren.

Java stehlt hier den JavaScript Interpreter <a href="https://developer.mozilla.org/de/docs/Rhino" target="_blank">Rhino</a> von Mozilla zur Verfügung, der uns fast zur Verzweiflung gebracht hat. Ich habe selten eine so nutzlose Implementierung gesehen, die zwar funktioniert, aber so dramatisch langsam ist, dass es mich wunderte, dass es überhaupt Einzug in die Standard Bibliothek von Java gefunden hat. Rhino, wenn auch stabil, ist mehr für Experimente gedacht, weniger aber für den produktiven Einsatz. Warum? Unsere Referenzseite im <a href="http://www.lieferando.de/lieferservice-berlin-10115" target="_blank" class="broken_link">Liefergebiet 10115</a> benötigte für die Generierung in Rhino ehrfürchtige 17 Minuten. Wir selbst haben uns als obere Grenze für die Generierung von 1.000.000 Seiten ein Limit von 6 Stunden gesetzt. Bei 17 Minuten für eine Seite, die in dieser Größe so auch häufiger vorkommt, sind wir von diesem Ziel weit entfernt. Genau so weit entfernt, wie das aktuelle Resultat in Stunden zu bemessen.

Aber trauern wir keinen verlorenen Kämpfen hinterher. Wir haben um Weihnachten herum auf NodeJs umgesattelt und hatten schnell die gewünschten Resultate. <a href="http://code.google.com/p/v8/" target="_blank">Googles V8</a> sei dank waren die Zeiten zum Generieren einer Seite endlich innerhalb eines akzeptablen Rahmens.

## Schichten schön und gut, aber was ist jetzt das Besondere?

Eine absolut berechtigte Frage. Was ist nun wirklich besonders an der neuen Software? Wir haben immer schon großen Wert darauf gelegt, dass wir nicht mit Servern, sondern mit Software skalieren. Ein Konzept, dass mitlerweile so gut aufgegangen ist, dass wir seit 2 Jahren mit der gleichen Anzahl von Servern auskommen. Insgesamt waren es nie mehr als 5 Server und das soll sich in naher Zukunft auch nicht groß ändern.

Dem zur Folge haben wir als integralen Bestandteil der Software einen ausgeklügelten Caching-Mechanismus eingebaut. Ein Mechanismus der nicht nur das Kernstück darstellt sondern auch den technischen Stolz der IT ausmacht und zusammen mit den CDN Spezialisten von <a href="https://myracloud.com/de/ddos-schutz/ " target="_blank">Myracloud</a> umgesetzt wurde. Caching ist hier bei uns kein integraler Bestandteil der API, des Frontends oder einer anderen Komponente sondern ebenfalls eine eigenständige Komponente. Auf diese Weise konnten wir wesentlich mehr Einfluss auf das Caching nehmen und haben damit gewaltige Resourcen auf unseren Servern frei geschafft, wie der Munin Graph mit einem Blick auf die Auslastung der Datenbank deutlich macht. Mit einer so geringen Last konnten wir nun sogar bestehende Maschinen abschalten und so Kosten reduzieren. [Punkt 2 check!](#topic2)

[<img class="alignnone  wp-image-751" alt="Bildschirmfoto 2013-05-05 um 17.25.56" src="http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.25.56.png" width="699" height="497" srcset="http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.25.56.png 998w, http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.25.56-300x213.png 300w" sizes="(max-width: 699px) 100vw, 699px" />](http://www.artofcode.de/wp-content/uploads/2013/05/Bildschirmfoto-2013-05-05-um-17.25.56.png)

Die NodeJS Applikation lädt hierbei die JS Applikation (und das ist die große Magie) in seinen Kontext und generiert die <a href="http://embeddedjs.com" target="_blank">EJS Templates</a> in direkter Zusammenarbeit mit der API, die die Daten liefert. Was also eigentlich beim Kunden im Browser passiert, geschieht bei uns ebenfalls auf dem Server. Dabei haben wir darauf geachtet, dass auch jedes Template einer Seite aus einzelnen Komponenten besteht, die wir Jig (aus dem englischen &#8222;Schablone&#8220;) benannt haben. Jedes Jig besteht aus einem Set von Konfigurationsdateien, die der NodeJS Applikation alle nötigen Informationen zur Verfügung stellt, die benötigt werden, um aus dem EJS Template HTML zu generieren und dieses dann in das Seitentemplate zu giessen. Das daraus final entstehende HTML laden wir in CDN hoch und haben es so hochverfügbar. Ein Seitenaufruf benötigt dabei nur noch konstanten Aufwand, da keine zusätzlichen Ressourcen geladen werden müssen. Die Auswirkungen auf die von Google genommenen Messungen zur Geschwindigkeit der Seite waren somit schon nach Tagen sichtbar und haben unsere wildesten Träume übertroffen.

[<img class="size-full wp-image-758 alignnone" alt="Google Perfomanz" src="http://www.artofcode.de/wp-content/uploads/2013/05/skitch.png" width="774" height="310" srcset="http://www.artofcode.de/wp-content/uploads/2013/05/skitch.png 774w, http://www.artofcode.de/wp-content/uploads/2013/05/skitch-300x120.png 300w" sizes="(max-width: 774px) 100vw, 774px" />](http://www.artofcode.de/wp-content/uploads/2013/05/skitch.png)

Das ganze kann von Jedermann auch über die Seite <a href="http://www.pagespeed.de" target="_blank">Pagespeed</a> geprüft werden. Wir erreichen hier 80/100 Punkte, wobei das Ergebnis noch negativ für uns ausfällt, da wir keinen Ping erlauben und somit 5 Punkte auf der Straße liegen lassen. In Anbetracht der anderen Werte können wir uns das jedoch durchaus leisten. Da wir diese Werte nun unabhängig davon erreichten ob gerade Hauptgeschäftszeit oder nichts los war, zeigte, dass die Software tatsächlich nach unseren wünschen skalierte und nicht mehr von der Besucheranzahl abhängig ist. [Punkt 1 check!](#topic1)

## &#8230; na dann erstmal ein Bier

Und so bleibt es nur noch diese Leute erwähnen, die bei all den Unwägbarkeit, den vielen zusätzlichen Stunden und vielen schlaflosen Nächten, als Team zusammen gehalten haben und mir meine in den letzten Tagen nervöse Art verziehen haben. Ich möchte jenen danken und bin mir bewusst, dass ohne dieses großartige Team eine solche Umstellung in so kurzer Zeit nicht möglich gewesen wäre.

Ich darf euch präsentieren: Die **IT von Lieferando****, ein junges Team (teilweise in Jahren, teilweise im Geiste :)), dass in den letzten 6 Monaten eine Menge gelernt und unglaubliches in kürzester Zeit parallel zum Tagesgeschäft vollbracht hat.

[<img class="   " alt="IMG_9921" src="http://www.artofcode.de/wp-content/uploads/2013/05/IMG_9921.jpg" width="810" height="540" />](http://www.artofcode.de/wp-content/uploads/2013/05/IMG_9921.jpg) Von links nach Rechts: Daniel (PHP, Salesforce), Lukasz (Iphone), Maciej (Android), Vincent (Javascript, PHP), Andre (PHP), Matthias (CTO), Allen (PHP), Jens (Lead Frontend), Benjamin (CSS/HTML), Alex (PHP), Ismael (Java), Francesc (Java). Es fehlen noch Oliver (CSS) und Toni (Javascript) 

&nbsp;

* _hier auch nochmals ein persönlicher Dank an <a href="https://www.xing.com/profile/Rafael_Otero" target="_blank">Rafael Otero</a> für die Zeit und Mühen und vielen Gesprächen, die wir geführt haben. Für die regelmässigen Reviews und die teilweise schmerzhafte aber helfende Ehrlichkeit, wenn wir mit unseren Ideen zu ihm gegangen sind._

_** Du willst Teil dieses Team werden? Du kennst dich in Javascript, Java, PHP aus und fühlst dich bei dessen Anwendung wie ein Ninja? Wir suchen Leute die für die Sache brennen und die Dinge einfach fertig kriegen. Bewirb dich noch heute unter jobs@lieferando.de, wir freuen uns auf deine Bewerbung!_
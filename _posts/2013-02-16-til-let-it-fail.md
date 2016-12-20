---
id: 696
title: 'TIL &#8211; Let it fail'
date: 2013-02-16T16:15:13+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=696
permalink: /2013/02/16/til-let-it-fail/
categories:
  - Allgemein
  - Datenbank
---
Woran denkt ein Entwickler, wenn man ihn nach der Applikation fragt? Ich denke die meisten denken an ihre Codebasis und stellen diese auch (meist) zu Recht als ihren ganzen Stolz in den Vordergrund. Bei jeder Applikation die von mehr als einer Person benutzt wird sind zumeist auch mehr als nur ein Entwickler am Werk. Womit wird dann begonnen? Codingstandards&#8230; aber wir alle wissen was mit diesen unter Zeitdruck passiert. Nur das Ziel in den Augen wird basierend auf eine beschränkte Menge von Usecases entwickelt. Das Resultat ist meistens verheerend.  Die Applikation läuft zwar aber nach ein paar Wochen bemerkt man Schlimmes. Die Daten sind völlig inkonsistent und produzieren in anderen Teilen der Applikation Fehler, die nicht erklärbar sind. Das durfte ich in letzter Zeit am eigenen Leib erfahren und musste mir eingestehen, dass ich viel zu wenig Zeit darin investiert habe, dass auch die Datenbank sich um einen konsistenten Datensatz kümmert. Die Applikation sollte hier nicht die alleinige Verantwortung bekommen. Über die Datenbankspezifikation und vernünftige Constraints ist es möglich die gesamte Gruppe von Entwicklern auch unter Zeitdruck dazu zu bringen, dass ihr Codebasis, so schlimm sie auch aussieht, zu sauberen Daten zu zwingen. Sollte dies nicht passieren, schlägt die Applikation fehlt. Aber was ist schlimmer als ein invalider Datensatz.

Seit kurzem erst gehe ich also bei allen Weiterentwicklungen klar nach einen Prinzip vor: **Let it fail!**

Was soll das jetzt? Die Applikation soll nicht funktionieren und fehlschlagen? Ganz im Gegenteil. Das Ziel einer jeden Applikation sollte es weiterhin sein allen Standards an Sicherheit und Stabilität zu genügen, aber wenn das mal nicht zutrifft, so diese lieber eine Fehlermeldung werfen, als zu versuchen auf irgendeine Weise Fehlerkorrekturen vorzunehmen.

Da sind mir direkt ein paar Beispiele eingefallen. Mit Vorliebe habe ich Applikationen in PHP mit [Magics](http://php.net/manual/en/language.oop5.magic.php) ausgestattet um so eine einfache und saubere Objektgestaltung zu gewährleisten. Ein Array von Daten wurden dann einfach mit ___set_ und ___get_ abgefragt. Bei der Implementierung habe ich in einer Zeile einen gravierenden konzeptionellen Fehler gemacht

#### Coding Beispiel (ein ganz dummes)

<pre class="brush:php">public function __get($method){

    // ... try to find $method inside an array property otherwise return null

    return null

}</pre>

Aber warum zur Hölle sollte man _NULL_ zurück geben? Damit keine Exceptions geworfen werden? Als ganz großer Mist entpuppte sich das, wo doch nun jede Methode, die nicht definiert war in den Magic lief und null zurück gab. Keine Exceptions, gar nichts.

<pre class="brush:applescript">$this-&gt;methodDefined(); // definiert
$this-&gt;methdoeDifned(); // nicht definiert, returned null</pre>

#### Und was ist mit der Datenbank?

Na die Datenbank sollte auch ein wenig Verantwortung bekommen. Und damit meine ich nicht Prozeduren und Trigger. Beides Dinge, die ich ungern in einer Datenbank habe. Du überlegt, ob du Trigger brauchst? Wenn man sich diese Frage stellt, braucht man sie nicht! Ein Augenmerk will ich aber auf Constraints legen. Jenes schöne Konstrukt in Datenbanken, die eine Art Raster über die Tabelle legen und so nur ein gewisses Set von Daten auch persistieren. Da ich in der Regel auf MySQL setze, war ich doch stark verwundert, dass Constraints hier nur syntaktischer Zucker sind. Schreibt man den Query mit ihnen falsch, so bekommt man eine Fehlermeldung, einen Effekt haben die Contraints jedoch nicht auch wenn [W3Resource was anderes behauptet](http://www.w3resource.com/mysql/creating-table-advance/constraint.php). In MySQL muss man sich [mit Triggern aushelfen](http://stackoverflow.com/questions/9734920/can-a-mysql-trigger-simulate-a-check-constraint) und so selber seine Contraints schreiben. Ein deutlich größere Aufwand in Aufbau und Pflege. Da hat man es in Oracle und Postgres doch einfacher.

Wie auch immer man es gewährleisten will: **Eine Applikation, die abbricht, wenn sie falsche Daten bekommt, ist keine schlechte Applikation!**
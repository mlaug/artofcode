---
id: 667
title: OpenSource stärkt die Software Qualität in kommerziellen Firmen
date: 2012-09-25T19:21:31+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=667
permalink: /2012/09/25/opensource-starkt-die-software-qualitat-in-kommerziellen-firmen/
image: /wp-content/uploads/2012/09/open_source_initiative.jpg
categories:
  - Allgemein
  - Netzgemeinde
tags:
  - coding
  - community
  - opensource
  - Qualität
---
[<img class="alignleft size-full wp-image-672" title="open_source_initiative" src="http://www.artofcode.de/wp-content/uploads/2012/09/open_source_initiative.jpg" alt="" width="200" height="229" />](http://www.artofcode.de/wp-content/uploads/2012/09/open_source_initiative.jpg)OpenSource und kommerzielle Unternehmen gehen vor allem in Deutschland selten Hand in Hand durch den Park spazieren. Erst ab einer bestimmten Größe erkennt ein Unternehmen, dass es der Community, auf dessen Tools es lange zeit kostenfrei zugegriffen hatte, auch wieder ein Stück zurück geben muss. Unternehmen wie Facebook und Twitter machen dies im großen Stil und stellen ihre Technologien auf GitHub zur Verfügung. <a href="http://de.wikipedia.org/wiki/HipHop" target="_blank">HipHop</a> von Facebook ist dabei eins der meiner Meinung nach besten Beispiele. Ich will nicht wissen, wie viele verdammt gute Entwickler wochenlang an dieser Technologie gesessen haben. Und das ganze kostenlos zur Verfügung stellen???

Kleine Unternehmen scheuen gerade diesen Schritt, sorgen sich um den nötigen Zeitaufwand. An ihnen nagt auch der Gedanke, dass sie Software, wo Entwickler ein paar Tage oder gar Wochen dran gesessen haben kostenfrei zur Verfügung zu stellen. Aber auch Entwickler selber sehen meist nicht den Drang ihre Tools frei zur Verfügung zu stellen. Und hier liegt der Hund begraben! Jeden Entwickler, den ich kenne, hat ein großes Interesse daran für sein Werk gehuldigt zu werden. Man will guten Code schreiben und diesen mit anderen Teilen. Aber halt nur integriert in die eigene Plattform.

Bei OpenSource geht es darum ein loses Stück Software zur Verfügung zu stellen, dass sich in jede Software (meist allerdings gleicher Programmiersprache) einbetten lässt.

Was muss also passieren?

  * **sauber Schnittstellen definieren **&#8211; oh mein Gott, unsere Software ist ein großer Block, da gibt es keine Schnittstellen nur ein riesiger Haufen von Methodenaufrufe

<div>
</div>

  * **Dokumentation schreiben** &#8211; andere sollen meinen Code verstehen? Ich habe doch alles annotiert&#8230;

<div>
</div>

  * **Komponenten konfigurierbar bereit stellen **&#8211; oh je, ich verwende doch überall feste Zahlen und keine Konstanten

<div>
</div>

  * **Abhängigkeiten entfernen oder über <a href="http://de.wikipedia.org/wiki/Dependency_Injection" target="_blank">Dependency Injection</a> lösen** &#8211; jetzt hört es aber auf, ich verwende immer meine eigene Datenbankklasse und die ist überall dabei

<div>
</div>

  * **Testcases** &#8211; tilde&#8230;

Alles Dinge die einem Programmierer unter Zeitdruck die Haare zu Berge stehen lassen? Alles Dinge, die man doch aus Zeitgründen vermeiden will und dies auch tut, da der Produktchef selten danach fragt, wenn er überhaupt Ahnung von diesen Dingen hat. Alles wird hochintegrativ in eine Plattform eingebettet. Wer hier noch vernünftig Testcases schreiben will hat halt Pech gehabt.

Was aber die Bedingungen für ein gutes OpenSource Projekt sind, sind gleichzeitig Vorraussetzungen für gute Softwarequalität. So muss man sich meist noch einmal Gedanken über die Architektur und Struktur seiner Software machen, wenn man einzelne Komponenten der Community zur Verfügung stellen will. Du erkennst bei deiner Software keine Komponenten, keine Module? Zeit für ein großes Refactoring!

Du hast Angst vor dem Feedback der Community? Dann ist dies vielleicht schon vorweg ein Eingeständnis, dass deine Software GET Parameter direkt in SQL einbettet&#8230;
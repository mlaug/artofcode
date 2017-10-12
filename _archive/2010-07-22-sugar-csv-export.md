---
id: 64
title: Sugar CSV Export
date: 2010-07-22T19:50:50+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=64
permalink: /2010/07/22/sugar-csv-export/
categories:
  - Allgemein
  - Php
  - Webentwicklung
tags:
  - csv
  - eport
  - Php
  - sugar
---
Ich hatte letztens Probleme bei dem Export von CSV Dateien bei der Applikation [Sugar](http://www.sugarcrm.com/crm/). Diese wurden völlig zerstört angezeigt und konnten in keinem der bekannten Tabellenverarbeitungen vernünftig angezeigt werden. Meine erste Vermutung war es, dass der exportierte Text (z.B. aus den Kommentaren) ebenfalls mit Kommata gespickt wäre und so keine vernünftigen CSV Export erlauben.

Das Problem bestand in Wirklichkeit jedoch an den Zeilenumbrüchen. Das ganze war also schnell durch eine kleine Anpassung in der Datei include/export_utils.php gelöst:

<pre class="php">$value = str_replace(',',' ',$value);
$value = str_replace(chr(13).chr(10),' ', $value);
</pre><fb:like href="http://www.artofcode.de/2010/07/22/sugar-csv-export/" layout="button\_count" show\_faces="false" width="450" action="like" colorscheme="light" style="margin-top:5px;" class="fb\_edge\_widget\_with\_comment fb\_iframe\_widget"></fb:like>
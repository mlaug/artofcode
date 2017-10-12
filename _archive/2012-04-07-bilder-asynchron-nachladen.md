---
id: 612
title: Bilder asynchron nachladen
date: 2012-04-07T14:27:39+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=612
permalink: /2012/04/07/bilder-asynchron-nachladen/
categories:
  - Allgemein
  - Javascript
  - JQuery
  - Webentwicklung
---
Ich habe schon länger nach einem Plugin gesucht, dass mir auf einfache Art und Weise Bilder nachlädt, sobald sie der Benutzer erst in seinem Browserfenster zu sehen bekommt. Vorher soll das Bild nicht geladen werden und somit Ressourcen auf dem Webserver schonen. Die meisten Möglichkeiten, die ich gefunden habe waren jedoch eher kompliziert und umständlich. Mit Hilfe des <a href="https://developer.mozilla.org/en/Canvas_tutorial/Using_images" target="_blank" class="broken_link">canvas Elements</a> und dem <a href="http://remysharp.com/2009/01/26/element-in-view-event-plugin/" target="_blank">inview Plugin für Jquery</a> ist es nun ziemlich einfach.

Dazu definiert man ein Canvas Element im HTML Code

<pre class="brush:xml">&lt;canvas class="canvas-image-loader" id="canvas-image-loader-1" width="100" height="100" data-url="URL TO YOUR IMAGE"&gt;&lt;/canvas&gt;</pre>

und setzt auf jedes dieser canvas Elemente das _inview_ Event. Sobald ein Canvas in den Sichtbereich des Benutzers kommt wird das Canvas mit dem Bild aus _data-url_ gefühlt
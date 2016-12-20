---
id: 489
title: 'SSI &#8211; Webserver und ihre alten Schätzchen'
date: 2011-02-20T14:36:12+00:00
author: mlaug
layout: post
guid: http://www.artofcode.de/?p=489
permalink: /2011/02/20/ssi-webserver-und-ihre-alten-schatzchen/
categories:
  - Php
  - Sicherheit
---
Server Side Includes sind alt. Uralt! Aber trotzdem findet man sie auch noch heute in modernen Webserverimplementierungen wie Lighttpd oder Nginx. Brauchen tut man sie eigentlich nicht mehr, sind sie doch nicht ansatzweise so performant wie die heute eingesetzten Websprachen. Seit CGI sind SSI eigentlich überflüssig. Aber für manch ein Szenario findet sich auch heute noch ein guter Einsatz.

Lange habe ich darüber nachgedacht wie man mit einfachsten Mitteln den Wert eines Cookies absichern kann. Setzt man z.B. eine statische Landingpage ein um Server Ressourcen zu sparen, möchte später aber trotzdem über den im Cookie gespeicherten Wert eine Aktion in PHP auslösen, z.B. einen Gutschein im Bestellprozess hinzufügen, so sollte man den Informationen vertrauen können. Auch sollte es nicht möglich sein den Wert des Cookies zu rekonstruieren bzw. diesen zu speichern und wieder zu verwenden.

Also nochmal zusammengefasst:

  * Der Inhalt des Cookies muss verifizierbar sein, dass dieser vom System generiert wurde und nicht von Dritten
  * Der Inhalt des Cookies soll innerhalb eines definierten Zeitraumes &#8222;ablaufen&#8220; und so nicht mehr gültig sein

Und genau hier bin ich auf die SSI gekommen. Warum? Weil das verschlüsseln von Daten mit Javascript keinen Sinn macht, lässt sich die eingesetzte Algorithmik im Code doch mit einfachsten Mitteln auslesen! Und PHP (oder andere Script Sprachen) will ich aus Perfomanzgründen nicht einsetzen um die Seite auch bei hoher Frequentierung Erreichbar zu halten.

Die einfachen Anweisungen von SSI sind schonend zu den Serverresourcen und ermöglichen alles, was man brauch

<pre class="brush:js">var token = '&lt;!--#exec cmd=""--&gt;';</pre>

Und schon haben wir einen Wert generiert den wir auf der Gegenseite später im Bestellprozess mit PHP wieder verifizieren können

<pre class="brush:php">&lt;?php
$cookie = $_COOKIE['token']; //enrypted value
$value = mcrypt($cookie);</pre>

Oder habt ihr ne bessere Idee 🙂

Was ich später noch behandeln werde ist eine perfomante und sichere Lösung für das Kommando, dass in den Include eingebaut werden sollte